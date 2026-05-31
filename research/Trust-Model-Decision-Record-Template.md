# Trust Model — Decision Record Template

**Purpose**: Use this template to formally record the team's answers to the critical open questions in the Trust Model. Once filled out, this becomes the canonical reference for the actual trust model in use.

---

## Decision Record: IQC Token System Trust Model

**Date**: _______________  
**Decision Makers**: _______________  
**Version of Trust Model this record applies to**: v0.3

---

### Decision 1: Long-term Ownership Structure

**Question**: What is the target long-term ownership/governance structure?

**Decision**:
- [ ] 4-of-7 Multisig (with specific signer entities below)
- [ ] Other: ____________________

**Signer Entities** (required):
1. ____________________ (role / organization)
2. ____________________
3. ____________________
4. ____________________
5. ____________________ (if 5+)
6. ____________________
7. ____________________

**Threshold**: ____ of ____

**Timelock Delay**: ____ hours / days

**Rationale**:

---

### Decision 2: Slashing Authority

**Question**: Who has the authority to slash stakers, and under what process?

**Decision**:
- [ ] Remains with the owner (timelocked multisig)
- [ ] Moves to a separate on-chain governance process
- [ ] Other: ____________________

**Process Details**:

**Rationale**:

---

### Decision 3: Allocation Contract Philosophy

**Question**: Should the TokenAllocation contracts remain owner-controlled long-term, or trend toward more autonomy?

**Decision**:
- [ ] Keep current `release()` + `recoverERC20()` model (with strengthened protections)
- [ ] Add vesting schedules + remove/restrict `recoverERC20` on IQC
- [ ] Move to fully autonomous / timelocked release logic over time
- [ ] Other: ____________________

**Details**:

**Rationale**:

---

### Decision 4: Emergency / Key Loss Procedures

**Question**: What is the recovery plan if the controlling multisig is compromised or keys are lost?

**Decision**:
- [ ] No formal recovery mechanism (loss is catastrophic)
- [ ] Social recovery / guardian process (details below)
- [ ] Other: ____________________

**Details**:

**Rationale**:

---

### Decision 5: Renouncing Ownership of IQCToken

**Question**: Will ownership of IQCToken be renounced after `lockMintingForever()`?

**Decision**:
- [ ] Yes — renounce after minting is permanently locked
- [ ] No — retain limited owner functions
- [ ] Other: ____________________

**Rationale**:

---

## Sign-Off

| Role                  | Name / Entity          | Date       | Signature / Confirmation |
|-----------------------|------------------------|------------|--------------------------|
| Deploying Party       |                        |            |                          |
| Technical Lead        |                        |            |                          |
| Legal / Compliance    |                        |            |                          |

---

*This Decision Record, once signed, should be published alongside the final Trust Model document.*