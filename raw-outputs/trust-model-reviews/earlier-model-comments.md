# Earlier Model Comments on Trust Model / Ownership Issues

This file aggregates relevant comments about trust model, ownership, and related risks from earlier rounds of the multi-model audit (before dedicated Trust Model reviews began).

## From Opus 4.8 (Round 1)

- Strongly emphasized that the single EOA controlling the allocations created a de facto "rug vector" on 99% of supply.
- Noted that purpose strings are meaningless if the owner can release everything immediately.

## From Grok 4.3 web (Round 1)

- One of the earliest and strongest voices on the need for a clear trust model.
- Highlighted that severity of `recoverERC20` and slashing depends entirely on who holds the keys.
- Warned about the governance risk of tokens at DEAD address counting toward vote weight if ERC20Votes is enabled.

## From Kimi 2.5 (with agents, Round 2)

- Raised early concerns about the lack of defined ownership and governance processes.
- Noted that many "High" findings were actually "High only under current weak ownership."

## From Opus 4.7 (earlier rounds)

- Repeatedly stressed that the Trust Model must be specified before severity ratings can be finalized.
- Pointed out that "transfer to multisig" is not a solution without signer details.

---

*These comments were extracted from the broader conversation history and earlier model responses. They predate the dedicated Trust Model document but directly influenced its creation.*

*Full original text available in conversation history.*