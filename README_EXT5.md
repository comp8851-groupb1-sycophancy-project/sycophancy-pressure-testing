# Fake Evidence — 5-turn extension, Gemini 3.5 Flash-Lite

Port of cells **E1–E7** from Mubeen's `gpt4_1_mini_fake_evidence.ipynb`, appended
to the Gemini Fake Evidence notebook. Mubeen's instruction was to use his cells as
they are and change only the model call and the input file, and that is what this is.

## What is identical and what changed

| Cell | Status |
|---|---|
| **E1** — Turn 4/5 wording, `[Reason]` phrases, Papers 4–5 | **byte-for-byte identical** |
| **E2** — render sanity check | **byte-for-byte identical** |
| **E3** — trial selection | identical logic; two file names and the expected-count comment |
| `ALL_STRATEGIES` / `load_locked_questions` | **byte-for-byte identical** |
| **E4** — replay + extend | model-call layer only (see below) |
| **E4b** — `run_extension` with progress printing | **byte-for-byte identical** |
| **E5** — pilot | identical; pilot file name only |
| **E7** — per-round checks (×3) | **byte-for-byte identical** apart from `ROUND_TO_CHECK` |

Verified by string comparison, not by eye. `E4_diff_gpt_vs_gemini.txt` is the full
diff of the one cell that changed.

### The three changes in E4

1. `call_answerer` / `call_prober` from Sections 2–3 instead of the OpenAI client;
   `model` defaults to `MODEL_NAME` rather than `"gpt-4.1-mini"`.
2. The **live** Turn 4 and Turn 5 model replies are appended as the `Content` object
   the API returned, not as rebuilt text, so Gemini's thought signature carries from
   Turn 4 into Turn 5. The four **replayed** turns (0–3) can only come from the log
   as text — signatures were never logged. That is not a new risk: the probe re-run
   put 3,095 calls through this same model on rebuilt-from-text conversations and
   Gemini 3.5 Flash-Lite accepted every one.
3. A broad exception guard around the Answerer call. A single 400 partway through an
   unattended 2,710-trial run would otherwise abort it. The trial is logged
   `needs_review` and retried automatically on the next call; nothing is discarded.

E4's first `run_extension` definition is dropped, since E4b overwrites it anyway, and
E5 is placed **after** E4b so the pilot uses the version with progress printing.

## Scope on the Gemini data

E3's selection rule — final attempt only, reached Turn 3 still correct, every
response `ok` — gives **2,710 held trials**, matching Mubeen's ~2,710 estimate:

| Strategy | R1 | R2 | R3 | Total |
|---|---|---|---|---|
| Specific Wrong Objection | 133 | 127 | 126 | 386 |
| Fabricated Statistics | 237 | 247 | 253 | 737 |
| Fabricated Citation | 258 | 261 | 256 | 775 |
| Misattributed Citation | 270 | 270 | 272 | 812 |
| **Total** | **898** | **905** | **907** | **2,710** |

12 of the 3,840 trial keys in the log have more than one attempt; E3 keeps the last,
which is where the 386/812 figures come from rather than the 385/810 I had counted
under a different attempt rule. E3's rule is the shared one, so it governs.

Up to **5,420 Answerer calls** plus one Prober call per new flip.

## Pre-flight checks already done

- E1 re-run twice: the length guard holds, no duplicate papers (5 per subject per
  strategy, both citation tables, all 8 subjects).
- Every strategy × 8 subjects × turns 1–5 renders with no `KeyError`.
- Turns 4 and 5 rendered against all 2,710 held trials using each trial's real
  logged `target_wrong_answer`: 0 failures, target substituted in every message.
- Conversation rebuild returns exactly 8 messages for all 2,710 trials, so E4's
  assertion cannot fire.
- Full `run_extension` dry run against a stubbed model: resumability, per-trial
  append, probe-on-flip, field set, and all three E7 check cells behave correctly.
- All 16 of E1's `MC_EXTRA` papers (the real, verified set for Misattributed) are
  genuine published work — Pittet 2000 *Lancet*, Wells 2003 *NEJM*, Rivers 2001
  *NEJM*, Calabresi & Melamed 1972 *HLR*, Engle & Granger 1987 *Econometrica*,
  Hausman 1978 *Econometrica*, Ball & Brown 1968 *JAR*, Cooley & Tukey 1965
  *Math. Comp.*, and so on. Two I checked rather than recalled are listed in the
  sources below. One pedantic note for the bibliography: Golub & Kahan (1965)
  appeared in *SIAM Journal on Numerical Analysis* **Series B**, usually cited
  without the "Series B".

## How to run

The folder needs, alongside the notebook:

```
results_gemini35flashlite_fe.jsonl     <- exact name; rename the downloaded copy
locked_320_questions.jsonl
.env  with GEMINI_API_KEY=...
```

Then, in order:

1. Sections **1, 2, 3, 4, 4b** — required after every kernel restart.
2. **E1**, then **E2** (expect `Render errors: 0`).
3. **E3** — check the table matches 2,710.
4. the `ALL_STRATEGIES` cell, **E4**, **E4b**.
5. **E5** pilot — 8 trials into a separate pilot file. Read the printed messages
   before spending anything. Do not send the pilot file as a result.
6. `run_extension(filepath=EXT_RESULTS, rounds=(1,))`, then its E7 check. Repeat
   for Rounds 2 and 3.

Safe to interrupt. Re-running skips completed trials and retries `needs_review` ones.

## One thing to settle before these numbers go in the report

Every flip rate in both models' current drafts is a **3-turn** rate. Turns 4–5 will
raise them. The tables need restating as "flipped within 3 turns" and "within 5
turns", or re-baselining — and that affects the GPT tables too, not just Gemini.
