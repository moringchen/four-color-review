# Risk Scoring Reference

## Score meanings

Use a 0-10 health score for each issue:
- 0-3: high risk
- 4-5: clear risk
- 6-7: medium risk
- 8-9: light risk
- 10: healthy

## High-risk triggers

Lower the score when any of these are true:
- no clear responsible party
- no terminal state
- key rule missing
- exception path missing
- state cannot be implemented clearly
- development would be blocked without more detail

## Final score rollup

After all issue scores are collected, convert the full set into a 0-100 final health score and summarize:
- total risks
- unresolved risks
- high-risk items below 6/10
