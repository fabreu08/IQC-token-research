# Trust Model — Team Review Notes (v0.2)

**Reviewer**: Internal / Multi-model synthesis synthesis  
**Date**: June 2026

## Summary of Changes from v0.1

- Added explicit adversary table with likelihood × impact.
- Strengthened recommendations with more concrete milestones.
- Made the mapping between trust model and audit findings more precise.
- Added emphasis that several "High" findings are only High because of the current single-EOA ownership.
- Improved structure and readability.

## Remaining Open Questions (Highest Priority)

1. **Long-term ownership structure** — What is the target end state (multisig + timelock? On-chain governance? Hybrid?)?

2. **Slashing authority** — Should this remain an owner action, or should it require a separate (documented) governance process?

3. **Allocation contract philosophy** — Are these meant to be long-term owner-controlled, or should they trend toward more autonomy?

4. **Emergency procedures** — What happens if the controlling multisig is compromised or keys are lost? Is there a social recovery / guardian process?

5. **Renouncing ownership** — Is full renunciation of `IQCToken` after `lockMintingForever()` the goal?

## Suggested Improvements for Next Version

- Add a short "What the owner **cannot** do" section for balance.
- Include specific signer requirements or operational security expectations for the multisig.
- Add a simple RACI-style matrix (who is Responsible, Accountable, Consulted, Informed for each privileged action).
- Decide whether slashing should require on-chain governance even in the short term.

## Recommendation

Treat v0.2 as the working draft for internal team discussion. Once the Open Questions above have answers, produce v1.0 as the canonical public version.

This document should be published and referenced in any future audit reports or deployment announcements.

---

*Review notes for internal use.*