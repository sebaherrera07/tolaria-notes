---
type: AirOps
_organized: true
---
# ./bin/aws-run staging

> ./bin/aws-run staging "bash -c 'cd packages/rails_app && bundle exec rake content_quality:backfill_keyword_intro_check'"

### Run in STG 2026-06-09

[backfill_keyword_intro_check] GridTables::ScoreConfig config#1 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled

```text
[backfill_keyword_intro_check] GridTables::ScoreConfig: enabled keyword_intro on 1 configs, 1 failed

[backfill_keyword_intro_check] Playbooks::ArtifactScoreConfig: enabled keyword_intro on 1 configs, 0 failed\

GridTables::ScoreConfig: enabled keyword_intro on 1 configs\

Playbooks::ArtifactScoreConfig: enabled keyword_intro on 1 configs\

Total: 2 configs updated
```

### Run in PROD 2026-06-09

```text
[backfill_keyword_intro_check] GridTables::ScoreConfig config#3 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
..[backfill_keyword_intro_check] GridTables::ScoreConfig config#42 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#47 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
.[backfill_keyword_intro_check] GridTables::ScoreConfig config#59 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#75 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#81 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#82 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#84 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured, A Brand Kit or Site Domain is required when link count checks are enabled
.[backfill_keyword_intro_check] GridTables::ScoreConfig config#91 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured, A Brand Kit or Site Domain is required when link count checks are enabled
[backfill_keyword_intro_check] GridTables::ScoreConfig config#93 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#94 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured, A Brand Kit or Site Domain is required when link count checks are enabled
[backfill_keyword_intro_check] GridTables::ScoreConfig config#95 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#99 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#100 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#101 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#102 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured, A Brand Kit or Site Domain is required when link count checks are enabled
[backfill_keyword_intro_check] GridTables::ScoreConfig config#103 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#107 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#108 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#109 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#111 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#114 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#126 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
[backfill_keyword_intro_check] GridTables::ScoreConfig config#144 failed: Validation failed: Keyword is required by one or more enabled checks but has not been configured
.........[backfill_keyword_intro_check] GridTables::ScoreConfig config#290 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled
[backfill_keyword_intro_check] GridTables::ScoreConfig config#299 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled
.....[backfill_keyword_intro_check] GridTables::ScoreConfig config#387 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled
...[backfill_keyword_intro_check] GridTables::ScoreConfig config#443 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled
....[backfill_keyword_intro_check] GridTables::ScoreConfig config#498 failed: Validation failed: A Brand Kit or Site Domain is required when link count checks are enabled
..............
[backfill_keyword_intro_check] GridTables::ScoreConfig: enabled keyword_intro on 398 configs, 29 failed

[backfill_keyword_intro_check] Playbooks::ArtifactScoreConfig: enabled keyword_intro on 4 configs, 0 failed

GridTables::ScoreConfig: enabled keyword_intro on 398 configs

Playbooks::ArtifactScoreConfig: enabled keyword_intro on 4 configs

Total: 402 configs updated
```
