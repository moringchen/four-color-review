# Four-Color Modeling Reference

## Default color meanings

Use these four groups by default:
- Role objects: actors that initiate, approve, receive, or intervene.
- Core business objects: entities that carry business state and are created, changed, or completed.
- Rule/description objects: policies, constraints, thresholds, categories, and descriptive business rules.
- Time/event objects: events, milestones, status changes, deadlines, and timeout points.

## Extraction checklist

For each requirement, check:
- which role starts the flow
- which object carries the core business state
- which rules constrain the flow
- which event changes the state or closes the loop
- whether any rule or event lacks an owning object
