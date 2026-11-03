# Learn Hebrew — Specialist Agent Roles

Personal product. **No Marketing role.** Agents are focused specialists in a tight plan → build → review loop.

Related: [`learn-hebrew-plan.md`](./learn-hebrew-plan.md) · patterns: [`from-tsukaeru.md`](./from-tsukaeru.md) · research doc missing (screenshots in `media/research/`).

When expanding process rules, merge useful bits from Tsukaeru via [`from-tsukaeru.md`](./from-tsukaeru.md) (see plan §11).

---

## Roles

### 1. Planner

**Owns:** goals, MVP cuts, IA, AI stack decisions, data model, roadmap, open questions, Tsukaeru adoption checklist.

**Delivers:** plan/docs updates, scoped tickets (clear acceptance criteria), explicit out-of-scope.

**Does not:** ship feature UI, invent marketing copy, expand scope mid-loop without recording a decision.

**Done when:** implementers can build without guessing product intent; open questions are listed or resolved.

---

### 2. UI/UX implementer

**Owns:** visual system, layout, typography, motion, RTL/LTR, responsive mobile **and** desktop, quick-touch controls (mic, sheets, nav).

**Delivers:** production-feel screens and components; empty/loading/error states; accessibility basics (focus, labels, contrast).

**Does not:** redesign product goals; wire AI providers beyond UI stubs unless paired with Feature coder; casual “good enough for a demo” chrome.

**Done when:** screens feel native-like at phone and desktop widths; interactions respond immediately; matches plan quality bar (and Tsukaeru design system when available).

---

### 3. Feature coder

**Owns:** speak pipeline (STT → LLM → TTS), vocab/roots persistence, story harvest, API routes, morph lookup wiring, env/secrets hygiene.

**Delivers:** working loops with typed contracts; local persistence; graceful provider failures.

**Does not:** ignore UX specs; leave secrets in client; ship unreviewed provider sprawl.

**Done when:** acceptance criteria from Planner pass on real devices/browsers; Hebrew+English speak path works end-to-end for the scoped slice.

---

### 4. Production reviewer

**Owns:** quality gate before merge — correctness, UX regressions, a11y, perf, security, Hebrew/RTL breakage, “would I use this daily?”.

**Delivers:** structured review (blockers vs nits); required fixes list; approve only at production personal-use bar.

**Does not:** rewrite features casually; approve student-project polish; add Marketing scope.

**Done when:** blockers cleared; remaining nits tracked; branch is safe to merge for the scoped slice.

---

## Loop and handoff

```
Planner → (ticket) → UI/UX implementer ⇄ Feature coder → Production reviewer
                ↑________________ fail blockers _________________|
```

1. **Planner** writes a thin ticket: problem, acceptance criteria, out-of-scope, AI/provider constraint if any.  
2. **UI/UX** and **Feature** may work in parallel once contracts (routes, component props, data shapes) are agreed.  
3. Handoff package: what changed, how to try it (mobile + desktop), known gaps, screenshots/recording if UI.  
4. **Reviewer** rejects on quality-bar failure, missing RTL, broken mic UX, or secret leaks — not on taste nits alone.  
5. Failures return to the owning specialist; Planner only if scope must change.

### Handoff checklist (every pass)

- [ ] Acceptance criteria quoted and checked  
- [ ] Mobile **and** desktop smoke  
- [ ] Mic / permissions / error path tried  
- [ ] Hebrew RTL + English gloss checked  
- [ ] No client-exposed API keys  
- [ ] Out-of-scope items not sneak-shipped  

---

## Explicitly excluded

| Role | Why excluded |
| --- | --- |
| Marketing / growth / SEO / launch copy | Product is personal; not for sale now |
| Casual “full-stack do-everything” agent | Dilutes quality; prefer specialists |

---

## Tsukaeru overlay

See [`from-tsukaeru.md`](./from-tsukaeru.md) for concrete paths on the local machine. Prefer this file’s roles until scaffolding; then borrow only matching process rules (plans-in-MD, issue workflow, quality gates).
