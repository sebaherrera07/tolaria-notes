---
type: AirOps
_organized: true
---
# Expose Quality Score via API (AIR-21280) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Surface the persisted `quality_score_breakdown` JSONB column (computed by AIR-21217) in both score API endpoints with full per-check detail including database `id` and human-readable `label`.

**Architecture:** The `quality_score_breakdown` column is already returned by both score endpoints — `as_json` is called without `only:`/`except:` in both controllers, so all columns are included automatically. No controller or model changes are needed. The only missing fields in the per-check sub-array are `id` (the Check record's database ID, for cross-referencing the response's `checks` array) and `label` (human-readable display name). We add these in `Scoring::QualityScore#check_contributions` so the stored JSONB already contains them at persist time — zero recomputation at request time.

**Tech Stack:** Ruby on Rails, RSpec, JSONB column on PostgreSQL

***

## Response Shape (Contract)

The `quality_score_breakdown` key is already present in both endpoints. Its shape when computed:

```json
{
  "quality_score_breakdown": {
    "total": 62,
    "total_out_of": 100,
    "brand_adherence": {
      "status": "scored",
      "score": 33,
      "out_of": 50,
      "passed": 2,
      "ran": 3
    },
    "ai_discoverability": {
      "status": "scored",
      "score": 29,
      "out_of": 50,
      "passed": 6,
      "ran": 14
    },
    "checks": [
      {
        "id": 1234,
        "check_type": "keyword_h2",
        "label": "Keyword in Headings",
        "dimension": "ai_discoverability",
        "passed": true,
        "weight": 3,
        "potential_gain": 0
      }
    ]
  }
}
```

- `quality_score_breakdown` is `null` when the `OUTPUT_QUALITY_SCORE` flag was off at scoring time or scoring hasn't finalized yet — frontend handles null.
- When a dimension has no checks: `{ "status": "not_applicable", "score": null, "out_of": 50, "passed": 0, "ran": 0 }`.
- `total_out_of` is 50 when one dimension is `not_applicable`, 100 when both are `scored`.

***

## File Map

| Action     | File                                                                                                     |
| ---------- | -------------------------------------------------------------------------------------------------------- |
| **Modify** | `packages/rails_app/app/services/content_quality/scoring/quality_score.rb`                               |
| **Modify** | `packages/rails_app/spec/services/content_quality/scoring/quality_score_spec.rb`                         |
| **Modify** | `packages/rails_app/spec/requests/api/content_quality/scores_spec.rb`                                    |
| **Modify** | `packages/rails_app/spec/requests/api/content_quality/scores_show_spec.rb`                               |
| **Modify** | `packages/rails_app/spec/requests/api/playbooks/sessions/artifacts/content_quality/scores/index_spec.rb` |

No controller changes. No model changes. No migrations.

***

## Task 1: Add `id` and `label` to QualityScore per-check output

**Files:**

- Modify: `packages/rails_app/app/services/content_quality/scoring/quality_score.rb`
- Modify: `packages/rails_app/spec/services/content_quality/scoring/quality_score_spec.rb`
- [ ] **Step 1: Write the failing spec**

In `spec/services/content_quality/scoring/quality_score_spec.rb`, inside the `'per-check contribution (feeds "+X" affordance)'` describe block, add after the existing examples:

```ruby
it 'includes id and human-readable label in each per-check item' do
  checks = [
    ContentQuality::Check.new(check_type: ContentQuality::CheckTypes::KEYWORD_H2, passed: true),
    ContentQuality::Check.new(check_type: ContentQuality::CheckTypes::WRITING_RULES, passed: false)
  ]
  result = described_class.new(checks).as_json

  expect(result[:checks]).to all(include(:id, :label))
  keyword = result[:checks].find { |c| c[:check_type] == ContentQuality::CheckTypes::KEYWORD_H2 }
  writing = result[:checks].find { |c| c[:check_type] == ContentQuality::CheckTypes::WRITING_RULES }
  expect(keyword[:label]).to eq('Keyword in Headings')
  expect(writing[:label]).to eq('Writing Rule')
end
```

- [ ] **Step 2: Run spec to verify it fails**

```bash
cd packages/rails_app && bundle exec rspec spec/services/content_quality/scoring/quality_score_spec.rb --format documentation 2>&1 | tail -15
```

Expected: FAIL — `expected {:check_type=>"keyword_h2", :dimension=>...} to include :id and :label`

- [ ] **Step 3: Add** `LABELS` **constant to QualityScore**

In `packages/rails_app/app/services/content_quality/scoring/quality_score.rb`, after the closing `.freeze` of `WEIGHTS` (around line 47), insert:

```ruby
LABELS = {
  ContentQuality::CheckTypes::FIRST_PARTY_INSIGHT         => 'First Party Insight',
  ContentQuality::CheckTypes::SOURCES_CITED_WITH_EVIDENCE => 'Sources Cited with Evidence',
  ContentQuality::CheckTypes::KEYWORD_H2                  => 'Keyword in Headings',
  ContentQuality::CheckTypes::KEYWORD_INTRO               => 'Keyword in Introduction',
  ContentQuality::CheckTypes::HEADINGS_DESCRIPTIVE        => 'Descriptive Headings',
  ContentQuality::CheckTypes::PARAGRAPH_LENGTH            => 'Paragraph Length',
  ContentQuality::CheckTypes::SENTENCE_LENGTH             => 'Sentence Length',
  ContentQuality::CheckTypes::HIERARCHY_CONSISTENCY       => 'Heading Hierarchy',
  ContentQuality::CheckTypes::INTERNAL_LINK_COUNT         => 'Internal Links',
  ContentQuality::CheckTypes::BROKEN_LINKS                => 'Broken Links',
  ContentQuality::CheckTypes::KEYWORD_DENSITY             => 'Keyword Density',
  ContentQuality::CheckTypes::LIST_PARALLELISM            => 'List Parallelism',
  ContentQuality::CheckTypes::BULLET_BREVITY              => 'Bullet Brevity',
  ContentQuality::CheckTypes::EXTERNAL_LINK_COUNT         => 'External Links',
  ContentQuality::CheckTypes::WRITING_RULES               => 'Writing Rule'
}.freeze
```

> Note: label copy is a first pass — adjust with design/frontend before shipping to users.

- [ ] **Step 4: Update** `check_contributions` **to include** `id` **and** `label`

In `packages/rails_app/app/services/content_quality/scoring/quality_score.rb`, update the return hash at line 138 in `check_contributions`:

```ruby
# Before:
{ check_type: check.check_type, dimension:, passed: check.passed, weight:, potential_gain: gain }

# After:
{
  id: check.id,
  check_type: check.check_type,
  label: LABELS[check.check_type],
  dimension:,
  passed: check.passed,
  weight:,
  potential_gain: gain
}
```

- [ ] **Step 5: Run spec to verify it passes**

```bash
cd packages/rails_app && bundle exec rspec spec/services/content_quality/scoring/quality_score_spec.rb --format documentation 2>&1 | tail -20
```

Expected: all examples passing

- [ ] **Step 6: Commit**

```bash
git add packages/rails_app/app/services/content_quality/scoring/quality_score.rb \
        packages/rails_app/spec/services/content_quality/scoring/quality_score_spec.rb
git commit -m "feat(AIR-21280): add id and label to per-check quality score breakdown"
```

***

## Task 2: Request spec — grid scores index endpoint

**Files:**

- Modify: `packages/rails_app/spec/requests/api/content_quality/scores_spec.rb`
- [ ] **Step 1: Add quality_score_breakdown contexts**

Inside the existing `'when scores exist for the cell'` context (after the `'with limit=1'` context), add:

```ruby
context 'when the score has a quality_score_breakdown' do
  let!(:scored_score) do
    score = create(:content_quality_score, score_config:, grid_cell:, score_percentage: 75)
    check1 = create(:content_quality_check, score:,
                    check_type: ContentQuality::CheckTypes::KEYWORD_H2, passed: true)
    check2 = create(:content_quality_check, score:,
                    check_type: ContentQuality::CheckTypes::WRITING_RULES, passed: false)
    breakdown = ContentQuality::Scoring::QualityScore.new([check1, check2]).as_json
    score.update_column(:quality_score_breakdown, breakdown)
    ContentQuality::ScoreEvent.emit(score, :scoring_completed)
    score
  end

  it 'includes quality_score_breakdown with dimension totals' do
    perform_request

    matching = json.find { |s| s['id'] == scored_score.id }
    breakdown = matching['quality_score_breakdown']
    expect(breakdown).to include(
      'total' => an_instance_of(Integer),
      'total_out_of' => 100,
      'brand_adherence' => include('status' => 'scored', 'out_of' => 50),
      'ai_discoverability' => include('status' => 'scored', 'out_of' => 50)
    )
  end

  it 'includes per-check entries with id, label, dimension, passed, weight, potential_gain' do
    perform_request

    matching = json.find { |s| s['id'] == scored_score.id }
    qs_check = matching['quality_score_breakdown']['checks'].first
    expect(qs_check.keys).to include('id', 'check_type', 'label', 'dimension', 'passed', 'weight',
                                     'potential_gain')
    expect(qs_check['id']).to be_an(Integer)
    expect(qs_check['label']).to be_a(String).and be_present
  end
end

context 'when the score has no quality_score_breakdown' do
  it 'returns quality_score_breakdown as null' do
    perform_request

    score_json = json.find { |s| s['id'] == latest_score.id }
    expect(score_json).to have_key('quality_score_breakdown')
    expect(score_json['quality_score_breakdown']).to be_nil
  end
end
```

- [ ] **Step 2: Run the spec**

```bash
cd packages/rails_app && bundle exec rspec spec/requests/api/content_quality/scores_spec.rb --format documentation 2>&1 | tail -20
```

Expected: all examples passing

- [ ] **Step 3: Commit**

```bash
git add packages/rails_app/spec/requests/api/content_quality/scores_spec.rb
git commit -m "test(AIR-21280): assert quality_score_breakdown shape in grid scores index"
```

***

## Task 3: Request spec — grid scores latest endpoint

**Files:**

- Modify: `packages/rails_app/spec/requests/api/content_quality/scores_show_spec.rb`
- [ ] **Step 1: Add quality_score_breakdown contexts**

Inside the existing `'when scores exist for the cell'` context (after the existing examples), add:

```ruby
context 'when the score has a quality_score_breakdown' do
  let!(:qs_check) do
    create(:content_quality_check, score: latest_score,
           check_type: ContentQuality::CheckTypes::KEYWORD_H2, passed: true)
  end

  before do
    breakdown = ContentQuality::Scoring::QualityScore.new([qs_check]).as_json
    latest_score.update_column(:quality_score_breakdown, breakdown)
  end

  it 'includes quality_score_breakdown with dimension status' do
    perform_request

    expect(json['quality_score_breakdown']).to include(
      'total' => an_instance_of(Integer),
      'total_out_of' => 50,
      'ai_discoverability' => include('status' => 'scored'),
      'brand_adherence' => include('status' => 'not_applicable')
    )
  end

  it 'includes per-check items with id and label' do
    perform_request

    qs_checks = json['quality_score_breakdown']['checks']
    expect(qs_checks.first).to include(
      'id' => qs_check.id,
      'label' => 'Keyword in Headings',
      'dimension' => 'ai_discoverability',
      'passed' => true,
      'potential_gain' => 0
    )
  end
end

context 'when quality_score_breakdown is nil' do
  it 'returns quality_score_breakdown as null' do
    perform_request

    expect(json).to have_key('quality_score_breakdown')
    expect(json['quality_score_breakdown']).to be_nil
  end
end
```

> Note: `total_out_of` is 50 here because only the AI Discoverability dimension ran (one `keyword_h2` check, no writing_rules). Brand Adherence is `not_applicable`.

- [ ] **Step 2: Run the spec**

```bash
cd packages/rails_app && bundle exec rspec spec/requests/api/content_quality/scores_show_spec.rb --format documentation 2>&1 | tail -20
```

Expected: all examples passing

- [ ] **Step 3: Commit**

```bash
git add packages/rails_app/spec/requests/api/content_quality/scores_show_spec.rb
git commit -m "test(AIR-21280): assert quality_score_breakdown shape in grid scores latest"
```

***

## Task 4: Request spec — playbooks artifact scores index endpoint

**Files:**

- Modify: `packages/rails_app/spec/requests/api/playbooks/sessions/artifacts/content_quality/scores/index_spec.rb`
- [ ] **Step 1: Add quality_score_breakdown context**

Inside the existing `'when scores exist for the draft artifact (T12 on-demand path)'` context (after the `'with limit=1'` context), add:

```ruby
context 'when the score has a quality_score_breakdown' do
  let!(:qs_check) do
    create(:content_quality_check, score: latest_score,
           check_type: ContentQuality::CheckTypes::KEYWORD_INTRO, passed: true)
  end

  before do
    breakdown = ContentQuality::Scoring::QualityScore.new([qs_check]).as_json
    latest_score.update_column(:quality_score_breakdown, breakdown)
  end

  it 'includes quality_score_breakdown in the score response' do
    perform_request

    score_json = json.find { |s| s['id'] == latest_score.id }
    expect(score_json['quality_score_breakdown']).to include(
      'total' => an_instance_of(Integer),
      'ai_discoverability' => include('status' => 'scored')
    )
  end

  it 'includes per-check items with id and label' do
    perform_request

    score_json = json.find { |s| s['id'] == latest_score.id }
    qs_check_json = score_json['quality_score_breakdown']['checks'].first
    expect(qs_check_json).to include(
      'id' => qs_check.id,
      'label' => 'Keyword in Introduction'
    )
  end
end
```

- [ ] **Step 2: Run the spec**

```bash
cd packages/rails_app && bundle exec rspec spec/requests/api/playbooks/sessions/artifacts/content_quality/scores/index_spec.rb --format documentation 2>&1 | tail -20
```

Expected: all examples passing

- [ ] **Step 3: Run the full quality score test suite**

```bash
cd packages/rails_app && bundle exec rspec \
  spec/services/content_quality/scoring/quality_score_spec.rb \
  spec/requests/api/content_quality/scores_spec.rb \
  spec/requests/api/content_quality/scores_show_spec.rb \
  spec/requests/api/playbooks/sessions/artifacts/content_quality/scores/index_spec.rb \
  --format progress 2>&1 | tail -10
```

Expected: all passing, 0 failures

- [ ] **Step 4: Commit**

```bash
git add packages/rails_app/spec/requests/api/playbooks/sessions/artifacts/content_quality/scores/index_spec.rb
git commit -m "test(AIR-21280): assert quality_score_breakdown shape in playbooks artifact scores"
```

***

## Verification

1. Enable `FeatureFlags::OUTPUT_QUALITY_SCORE` for a user in a dev console.
2. Trigger a score run (either grid or playbook artifact) so `ScoreFinalizer` populates `quality_score_breakdown`.
3. Hit `GET /api/grid_tables/:id/grid_cells/:id/scores/latest` — verify `quality_score_breakdown` is present with `total`, both dimension blocks, and a `checks` array where each item has `id`, `check_type`, `label`, `dimension`, `passed`, `weight`, `potential_gain`.
4. Verify `total_out_of: 50` appears for a score where writing rules weren't configured (brand adherence = not_applicable).
5. For a score produced before the flag was enabled: `"quality_score_breakdown": null` — confirm frontend handles it without error.
