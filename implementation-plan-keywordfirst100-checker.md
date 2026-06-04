---
type: AirOps
_organized: true
---
# [Suggestions] Code reviews

- Right now the `e-pull-request` channel posts a message any time a PR is opened, which makes it quite noisy when actually some PRs are not ready for review
  - linters and unit tests are still running
  - greptile hasn't finished or left comments
- Maybe we can adjust the automatic workflow so that the message is posted to the channel ONLY when something indicates that it is ready for review?
  - Something like commenting in the PR with a command like `/ready-review` , or adding some tag.
- Another suggestion might be having everyone to enable GitHub scheduled reminders, and start using GitHub review assignments.
- Discuss many small PRs vs fewer and
