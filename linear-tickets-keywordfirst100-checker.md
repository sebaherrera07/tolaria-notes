---
type: AirOps
_organized: true
---
# Linear Tickets: `keyword_first_100` Checker

## Ticket 1 — Backend

**Title:** `[Content Quality] Implement keyword_first_100 checker (backend)`

**Description:**

Add a new synchronous content quality checker that verifies the primary keyword appears within the first 100 words of the article body. This check is part of the SEO & AEO category and follows the same pattern as `KeywordDensityChecker` and `KeywordH2Checker`.

**Scope**

- Register a new check type constant `KEYWORD_FIRST_100 = 'keyword_first_100'` in `check_types.rb`
- Implement `KeywordIntroChecker` service class inheriting from `BaseChecker`
- Register the checker in the `KEYWORD_CHECKERS` hash in `checker_job.rb`
- Write RSpec unit tests covering all pass/fail/edge-case branches

**Acceptance criteria**

- [ ] `keyword_first_100` appears in `CheckTypes::ALL`
- [ ] Checker passes when any keyword variant (primary, secondary, or inflected form) is found within the first 100 words of body content
- [ ] Checker fails with evidence listing all keywords checked and their position window, plus a suggestion to move the keyword earlier
- [ ] Checker returns `passed: true` with an explanatory note when no keywords are available from classification (not a content failure)
- [ ] Checker returns `passed: false` with an explanatory note when body content is empty
- [ ] Checker is dispatched by `CheckerJob` via the `KEYWORD_CHECKERS` registry (tracing and error logging are handled automatically by `CheckerJob`)
- [ ] All spec branches green

**References**

- Python implementation: `content_quality_score_m1/tools/content_quality_scorer_m1/scoring/v1/checkers/rules/keyword_first_100.py`
- Closest Ruby analogs: `keyword_density_checker.rb`, `keyword_h2_checker.rb`

***

## Ticket 2 — Frontend

**Title:** `[Content Quality] Add keyword_first_100 to content quality UI`

**Description:**

Wire the new `keyword_first_100` check type into the frontend so it appears in the checks configuration panel under the SEO & AEO category, with a user-facing label and any necessary type definitions.

**Scope**

- Add `'keyword_first_100'` to the `ContentQualityCheckType` TypeScript union
- Add metadata entry to `CHECK_METADATA` in `check-metadata.ts` (category, label, learnMoreUrl)
- Add to `CATEGORY_FOR_CHECK` in `checks-config-panel/helpers.ts` under `"SEO & AEO Discoverability"`

**Acceptance criteria**

- [ ] `keyword_first_100` appears as a valid `ContentQualityCheckType` with no TS errors
- [ ] Checker appears in the configuration panel under "SEO & AEO", grouped alongside `keyword_density` and `keyword_h2`
- [ ] Label reads: "Check keyword in intro (first 100 words)"
- [ ] No existing checks are broken or reordered

**Dependencies**

- Blocked by Ticket 1 (backend must define the check type first)
