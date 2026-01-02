## Appendix: Success Metrics

### How to Measure Real TDD (Not Cosplay)

#### Leading Indicators
- Invariants identified per sprint
- Time to first integration test green
- Frequency of priority queue re-ranking

#### Lagging Indicators
- Production incidents from invariant violations
- Time to resolve design ambiguity
- Ratio: Tests written / Assumptions settled

#### Anti-Metrics (Cosplay Detection)
- Test count without invariant confirmation
- Coverage % without design change
- Green CI without learning

### When to Worry

If you see:
- Tests increasing, but design stagnant
- High coverage, but frequent production issues
- Fast green, but unclear what was settled

You may be cosplaying.

---
