# Learn Hebrew — First Product Plan

**Status:** Research + plan only (no app scaffold yet)  
**Repo:** [bfpronaga/learn-hebrew](https://github.com/bfpronaga/learn-hebrew) — empty starter  
**Detail research:** `docs/research/hebrew-app-research.md` — **missing after handoff** (never present in Project Context); use plan § + [`media/research/`](../media/research/) until rewritten.  
**Agent roles:** [`docs/agent-roles.md`](./agent-roles.md)  
**Screenshots:** [`media/research/`](../media/research/)  
**Local handoff:** 2026-11-03 — durable docs live in this GitHub repo; Cursor Cloud Project Context is no longer required.

---

## 1. Goals

Build a **personal** Hebrew speaking practice web app (Next.js) that:

1. Lets you **speak** Hebrew (and English when stuck) and get an AI that **talks back**.
2. Goes beyond plain ChatGPT chat with **intentional vocabulary**: saved word lists, AI prompts that use those words, and speech practice against them.
3. Treats Hebrew seriously: vocab is rooted in **shoresh (root) + base forms** before gender / infinitive / tense conjugations.
4. Climbs a **content ladder** over time: picture-book / children’s-story texts → social posts / news for discussion + vocab.

Inspiration comes from speaking-first products (e.g. Fluently *patterns*), **not** cloning brand, copy, assets, or UI chrome.  
This is **not for sale** now — no marketing surface, no growth funnel work.

---

## 2. Users

**Primary (v1):** You — English-dominant adult learning Modern Israeli Hebrew, daily short speaking sessions on phone **and** desktop.

**Secondary (later):** Friends / family with the same goal.

**Not for v1:** Classroom LMS, kids’ product, Biblical-Hebrew seminary track, employer analytics, public launch marketing.

---

## 3. Quality bar

Ship **production-ready**, modern product quality — not student-project polish.

| Bar | Expectation |
| --- | --- |
| **Feel** | Native-like on **mobile and desktop** in one Next.js web app (responsive layout, not “mobile page shrunk”). |
| **UI/UX** | Intentional typography, motion, density, and RTL/LTR coexistence; clear hierarchy; no generic AI-purple template look. |
| **Interaction** | Quick-touch first: large hit targets, hold-to-talk / slide-to-lock, instant feedback states, &lt;100ms UI response to taps. |
| **Performance** | Fast first paint on phone; streaming where useful; no janky mic/permission flows. |
| **Engineering** | Typed, reviewed, accessible enough for daily personal use; errors recoverable; secrets server-side. |
| **Reference** | Pull patterns from local Tsukaeru checkout (see §11 and [`from-tsukaeru.md`](./from-tsukaeru.md)). Do not invent Tsukaeru specifics. |

---

## 4. Working model (specialist agents)

Heavy loop of **focused specialists** — narrowly responsible, professional — not casual generalists. **No Marketing role** (personal product).

| Role | Owns | Does not own |
| --- | --- | --- |
| **Planner** | Scope, IA, stack decisions, open questions, roadmap cuts | Shipping UI code |
| **UI/UX implementer** | Visual system, layouts, motion, responsive + RTL, micro-interactions | Backend AI wiring (unless blocked) |
| **Feature coder** | Speak pipeline, vocab/roots data, story lane, API routes | Visual redesign; marketing copy |
| **Production reviewer** | Quality gate: a11y, perf, security, regression, “would I ship this to myself?” | Feature invention |

Handoff rules, definition of done, and loop cadence: **[`docs/agent-roles.md`](./agent-roles.md)**.

---

## 5. MVP scope (first shippable slice)

**Working title:** Speak + Roots (name TBD).

### In scope

| Piece | What ships |
| --- | --- |
| **Speak session** | Hold-to-talk (or tap-to-lock) mic → STT (he + en) → LLM reply → Hebrew TTS. Optional English gloss / transliteration. |
| **Vocab list** | Local saved words: Hebrew, gloss, **shoresh**, lemma/base, optional binyan, notes. |
| **Root-aware prompts** | Active set (5–10 words); AI weaves base forms into questions; user answers aloud. |
| **Light correction** | On demand or end-of-turn: said → natural rephrase → short why. No fake CEFR / “% native”. |
| **Beginner text lane** | 1–3 picture-book / children’s-story passages; read → harvest words → speak about the story. |
| **Persistence** | `localStorage` / IndexedDB only — no auth, no cloud DB. |
| **Shell** | Next.js App Router; bottom nav on mobile + desktop sidebar/top nav twin; Practice / Vocab / Story. |

### Out of scope

- Auth, sync, payments, streaks-as-product, social, marketing site
- Full CEFR assessment, live 3D avatar tutors, games, interview simulator
- News / social content lane (Phase 2+)
- Full conjugator product, Anki export, native iOS/Android
- Cloning Fluently (or any app) visuals / copy

---

## 6. AI stack — decision-ready comparison

**Important:** ChatGPT Plus/Pro and Claude Pro/Max are **consumer chat subscriptions**. They do **not** replace **API** usage for a Next.js app. For the product, budget **API (usage)**; subscriptions are optional for *human* planning / eval.

Prices below are **approximate public rates as of research (late 2026)** — verify before committing spend. Hebrew quality notes from Scribeil/ivrit benchmarks, TTS arenas, and learner reviews (detail in research doc).

### 6.1 Speech-to-text (English + Hebrew)

| Option | He quality | En quality | Pricing (approx.) | Next.js fit | Notes |
| --- | --- | --- | --- | --- | --- |
| **ivrit-ai Whisper** (hosted or self-host CT2) | **Best** on Scribeil (~6.8 WER avg) | Good if same Whisper family | Infra / host cost; not a flat consumer sub | Route Handler → GPU/host API | Top Hebrew accuracy; ops heavier if self-host |
| **Soniox** | Strong (#3 ~7.6 WER) | Strong multilingual | ~$0.10/h async, ~$0.12/h realtime STT | REST/WS from server | Simple pricing; good default *hosted* He STT |
| **OpenAI Whisper / gpt-4o-transcribe** | Mediocre He (~12–16+ WER on benches) | Excellent | Whisper ~$0.006/min; gpt-4o-transcribe ~$0.006/min + tokens | Excellent SDK | Fine for **English** turns; weak as sole He STT |
| **ElevenLabs Scribe** | Mid (~13 WER v1 benches; realtime dearer) | Good | ~$0.22/h batch class; realtime ~$0.39/h | API | Convenient if already on ElevenLabs for TTS |
| **Google / Azure Speech** | Weaker He on public benches (Google ~26 WER on Scribeil) | Excellent | Cloud STT ~$16/1k min class (Chirp STT listed) | Cloud SDKs | Prefer for EN or enterprise; not He-first |
| **Browser Web Speech** | Unreliable He | OK EN | Free | Client-only | Dev fallback only |

### 6.2 Text-to-speech / talk-back (Hebrew)

| Option | He quality | Pricing (approx.) | Latency | Notes |
| --- | --- | --- | --- | --- |
| **ElevenLabs v3/v4** (`language_code: "he"`) | Good–strong | ~$0.08/1k chars (v4); Turbo cheaper ~$0.04/1k | Good; Turbo best latency | **Do not** use Multilingual v2 for Hebrew |
| **Google Chirp 3 HD (he-IL)** | Good; streaming-friendly | ~$30/1M chars listed | Strong streaming | Watch Hebrew word-boundary / nikud issues historically |
| **Gemini Flash TTS** | Competitive in arenas | Via Google AI / Cloud | Good | Pair with Gemini LLM if one-vendor preferred |
| **Soniox TTS** | Competitive | ~$0.70/h generated speech | Streaming | Same vendor as strong He STT |
| **Deepdub Phantom Z** | Top arena claims | Sales / custom | Fast streaming claimed | Overkill for personal MVP unless trial |
| **OpenAI TTS / Realtime** | Mid on He arenas vs specialists | TTS-1 ~$15/1M chars; Realtime audio tokens higher | Realtime convenient | Great DX; validate He ears before sole TTS |
| **Edge TTS (Avri/Hila)** | Good enough / free | $0 | OK | Cheap prototype voice |

### 6.3 Conversational LLM

| Option | Hebrew chat / morph explain | Voice path | API pricing (approx.) | Consumer sub (optional, human use) |
| --- | --- | --- | --- | --- |
| **Anthropic Claude (Sonnet / Opus)** | **Best “why”** for binyan / grammar | No first-class product voice in-app | Sonnet ~$2–3/MTok in / ~$10–15/MTok out (check current Sonnet 4.x/5.x); Opus higher | Claude Pro ~$20/mo; Max ~$100; Ultra ~$200 (+ API credits on Max/Ultra) |
| **OpenAI GPT-4o / GPT-5 family** | Strong conversation; occasional gender slip | Realtime / Advanced Voice for *ChatGPT*; API Realtime for apps | GPT-4o ~$2.50 / $10 per 1M; GPT-5 ~$1.25 / $10; mini/nano cheaper | ChatGPT Plus ~$20; Pro ~$100–$200 — **≠ API** |
| **Google Gemini** | Solid; great multimodal (photo text later) | Live / TTS depending on product | Usage-based AI Studio / Vertex | Google AI Premium ~$20 — **≠ app API** |
| **Open models (self-host)** | Variable He | DIY | GPU only | Not for v1 quality bar |

### 6.4 Hebrew morphology / roots

| Option | What it gives | Cost | Fit |
| --- | --- | --- | --- |
| **dicta-il/wordlist** (+ optional DictaBERT-lex) | root, lemma, binyan, POS | Free (dataset / model compute) | **v1 backbone** for shoresh fields |
| **YAP / HebPipe** | Parse / lemma / some binyan | Self-host | Optional enrichment |
| **LLM-only morph** | Convenient guesses | Token cost | Augment + verify; never sole source of truth |

### 6.5 Recommended default (pending your decision)

| Layer | **Recommended default** | **Cheaper alternative** | **Higher-quality alternative** |
| --- | --- | --- | --- |
| STT He | **Soniox** realtime/async | OpenAI Whisper (accept worse He) | **ivrit-ai** Whisper (best WER; more ops) |
| STT En | OpenAI Whisper / gpt-4o-transcribe | Web Speech EN | Same Soniox multilingual |
| TTS He | **ElevenLabs v3/v4** + `he` | Edge TTS | Chirp 3 HD or Deepdub trial |
| LLM | **Claude Sonnet** (tutor + morph explain) | GPT-4o-mini / Haiku for cheap turns | Claude Opus / GPT-5 for hard grammar |
| Morphology | **dicta-il wordlist** local lookup | LLM guess | DictaBERT-lex + lexicon filter |
| Orchestration | Next.js pipeline STT→LLM→TTS | Single-vendor OpenAI Realtime (after He A/B) | Split best-of-breed as above |

**Suggested personal monthly envelope (light daily use):** API-only on the order of **tens of USD** if sessions are short; subscriptions only if you already use Claude/ChatGPT for planning. Re-estimate after first 50 turns.

**You decide** which row to lock before implementation.

---

## 7. UX principles (mobile + desktop web)

1. Voice-first primary control; text is scaffolding.  
2. Never leave the learner frozen — hint / suggested reply (He + En).  
3. Corrections on demand or end-of-turn; honest, not theatrical scores.  
4. Slow TTS control for beginners.  
5. Code-switching allowed; tutor rephrases into Hebrew using target roots.  
6. Every vocab card shows **shoresh + base** before conjugations.  
7. One job per screen; desktop gets room without becoming a dashboard.  
8. RTL Hebrew + LTR English without layout thrash.  
9. Progress = roots practiced + minutes spoken.  
10. Privacy-simple: turn audio to providers only as needed; wipe local data in Settings.

---

## 8. Information architecture

```
/                 → Today: start Practice (active vocab)
/practice         → Speak session
/vocab            → Lists → word detail (root, base, examples)
/vocab/add        → Add word (+ morph suggest)
/story            → Short text → harvest → speak
/settings         → TTS speed, correction mode, STT bias, wipe data
```

**Responsive:** mobile bottom nav; desktop persistent nav + wider transcript/vocab panes. Same routes.

---

## 9. Data model sketch (local)

```ts
type VocabEntry = {
  id: string;
  hebrew: string;
  hebrewNikud?: string;
  glossEn: string;
  shoresh?: string;
  lemma?: string;
  pos?: "verb" | "noun" | "adj" | "other";
  binyan?: string;
  notes?: string;
  source?: "manual" | "story" | "session";
  createdAt: string;
  lastPracticedAt?: string;
  practiceCount: number;
};
```

Normalize harvests to **lemma/base** via Dicta when possible; conjugations live as examples under the base.

---

## 10. Phased roadmap

| Phase | Focus |
| --- | --- |
| **0 — Plan** | This doc + research + agent roles |
| **1 — Speak + Roots MVP** | Pipeline speak + local vocab/roots + 1–3 stories + production UI shell |
| **2 — Story polish** | More texts, nikud toggle, TTS cache |
| **3 — News / social** | Short posts/news → discuss + vocab |
| **4 — Depth** | Light SRS on roots; optional sync |
| **5 — Quality** | He STT/TTS A/B on *your* voice |

---

## 11. Adopt from Tsukaeru

**Local path (JAC-ITD-RAY):** `c:\Users\ronaga\Desktop\workspace\tsukaeru-app`  
**Inventory (paths only):** [`docs/from-tsukaeru.md`](./from-tsukaeru.md)  
Borrow process / engineering taste — **not** Tsukaeru auth, billing, or domain schemas. Hebrew v1 stays personal + local persistence.

| Area | Status after handoff |
| --- | --- |
| **Design system** | Still TBD for Hebrew; Tsukaeru worship/presenter apps are reference, not a shared package |
| **UI patterns** | See Tsukaeru apps; reimplement — do not share DOM components |
| **Agent rules** | Start from [`agent-roles.md`](./agent-roles.md) + `from-tsukaeru.md` (`AGENTS.md`, plan-documents, issue-workflow) |
| **PR / review standards** | Follow Tsukaeru taste lightly; this repo can stay simpler |
| **Responsive / interaction** | Window-class idea in Tsukaeru `multi-window-form-factors` — optional later |
| **Next.js architecture** | Prefer App Router + server secrets (aligns with Tsukaeru web apps) |
| **Testing / quality gates** | Production-ready personal bar in §3; formal CI still TBD |

**Process:** Use `from-tsukaeru.md` when scaffolding; deepen §11 only when adopting a concrete pattern.

---

## 12. Success metrics (personal MVP)

- First speak → talk-back in &lt;2 minutes.  
- ≥3 sessions/week for 2 weeks.  
- ≥70% of AI prompts include ≥1 active base form (spot-check).  
- ≥80% of new verb entries have shoresh + lemma.  
- Subjective clarity (understood / TTS slow) ≥4/5.  
- UI: usable one-handed on phone; comfortable on desktop.

---

## 13. Open questions

1. Lock AI default table (§6.5) — confirm or swap rows.  
2. Pipeline vs early OpenAI/Gemini Realtime after He A/B?  
3. Hosted Soniox vs self-host ivrit-ai?  
4. Nikud default on or off?  
5. Product name / visual direction (await Tsukaeru)?  
6. Story licensing / original short texts?  
7. When (if ever) auth for multi-device sync?  
8. ~~Provide Tsukaeru repo URL~~ — local path known; see §11 / `from-tsukaeru.md`.

---

## 14. Next step (when authorized)

Scaffold Next.js to the quality bar above + local vocab + one Practice pipeline — **not started in this handoff**. Work in a normal local Cursor chat on this folder.
