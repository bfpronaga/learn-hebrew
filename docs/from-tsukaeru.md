# Reusable patterns from Tsukaeru

Local monorepo (sibling folder): `c:\Users\ronaga\Desktop\workspace\tsukaeru-app`  
GitHub: use whatever remote that checkout uses. **Do not copy secrets, `.env`, Clerk keys, or Supabase service roles.**

Learn Hebrew is a separate personal product. Borrow **process and engineering taste**, not Tsukaeru auth/billing/domain tables.

## High-value references (paths + one-line summary)

| Path | Summary |
| --- | --- |
| `AGENTS.md` | Agent/human workflow: Issue → plan MD → implement → verify; AI does not ship; Japanese default for that product |
| `.cursor/rules/issue-workflow.mdc` | Confirm target app before coding; plan in MD; high-risk gates need human OK |
| `.cursor/rules/plan-documents.mdc` | Plans live under `docs/<product>/plans/YYYY-MM-DD-*.md`; chat is not the source of truth |
| `.cursor/rules/web-native-dual-platform.mdc` | Domain in `packages/*`; UI reimplemented per platform; no DOM→RN paste |
| `.cursor/rules/react-native-production.mdc` | Native production bar (skills + rules) — useful if Hebrew later grows a native client |
| `.cursor/rules/multi-window-form-factors.mdc` | Design by window class (compact / medium / expanded), not device name |
| `.cursor/rules/monorepo-build-scripts.mdc` | Prefer root `package.json` scripts; pnpm; avoid ad-hoc filters |
| `.cursor/rules/model-api-usage.mdc` | BYOK OpenAI OK; do not route through Vercel AI Gateway |
| `.cursor/rules/i18n-locales-consistency.mdc` | Shared locale package as source of truth; platform diffs in namespaces |
| `docs/plans/2026-11-02-web-native-dual-platform.md` | Full dual-platform design note |
| `docs/agent-company/AGENT_COMPANY.md` | Specialist “AI company” loop / gates (adapt roles; Hebrew has no Marketing role) |
| `packages/tsukaeru-layout` | Window-class layout helpers (if adopting the same form-factor model) |

## What *not* to copy blindly

- Clerk Application / ツカえる account model (`docs/tsukaeru-account.md`) — Hebrew v1 is personal, local persistence.
- Worship/presenter Supabase schemas and RLS.
- Store / IAP / subscription limit machinery.
- App-specific Cursor rules under `apps/*/.cursor/`.

## Suggested Learn Hebrew adaptations

1. Keep a short root `AGENTS.md` (or expand this repo’s README) once scaffolding starts.
2. Put implementation plans under `docs/plans/YYYY-MM-DD-*.md`.
3. Prefer Next.js App Router + server-side secrets for STT/LLM/TTS keys (matches plan).
4. Use specialist roles from `docs/agent-roles.md` instead of a full Tsukaeru agent-company loop until the app exists.
