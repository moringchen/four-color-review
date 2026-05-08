# Risk Scoring Reference

## Health Score meanings

Use a 0-10 Health Score for each issue:
- 0-3: high risk
- 4-5: clear risk
- 6-7: medium risk
- 8-9: light risk
- 10: healthy

Treat scores below 6/10 as the high-risk follow-up threshold for continuation and unresolved-item review.

## High-risk triggers

Lower the Health Score when any of these are true:
- no clear responsible party
- no terminal state
- key rule missing
- exception path missing
- state cannot be implemented clearly
- development would be blocked without more detail

## Final Health Score rollup

After all issue Health Scores are collected:
- add all issue scores
- divide by the number of issues to get the average Health Score
- multiply the average Health Score by 10 and round to the nearest whole number to get the Final Health Score

Summarize:
- total risks
- unresolved risks
- high-risk follow-up items below 6/10
