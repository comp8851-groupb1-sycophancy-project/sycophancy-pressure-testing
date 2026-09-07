# Can an AI Be Talked Out of a Right Answer by Fake Evidence or Sheer Persistence?
COMP8851 Major Project — testing whether LLMs abandon correct answers under fabricated evidence or persistent disagreement.

## Team
* Akhil Ramesh
* Muhammad Mubeen
* Lakshya Kumar Wadhwa
* Muhammad Rafi

## Structure
* `gpt4_1_mini_fake_evidence.ipynb` — Fake Evidence strategies (Specific Wrong Objection, Fabricated Statistics, Fabricated Citation, Misattributed Citation) tested on GPT-4.1 Mini. Core implementation only — see commit history for development/debugging notebooks if needed.
* `build_shared_questions_set_MMLU.ipynb` — builds the shared 320-question dataset (40 questions x 8 MMLU subjects, fixed seed for reproducibility). Run once; both GPT-4.1 Mini and Gemini sides must use its output (`locked_320_questions.jsonl`) unchanged, so all strategies and both models are tested against the identical question set.
* `locked_320_questions.jsonl` — the actual locked 320-question dataset, shared across all team members' notebooks
* `results_gpt4mini_fe.jsonl` — raw trial results log (one JSON object per turn: baseline, pressure turns, scoring, probe responses) for GPT-4.1 Mini, Fake Evidence, Rounds 1–2
* More files to be added as each team member's work progresses (Gemini + Persistence strategies)

## Status
Rounds 1 and 2 of 3 complete for GPT-4.1 Mini / Fake Evidence (320 questions x 4 strategies each). Pipeline fully validated: schema fixed to generate justification before answer (reduces measured flip rate ~2x vs. naive ordering), `max_tokens` 1000, resumability logic corrected so interrupted or partially-failed trials retry correctly without duplicating completed work. 
Data quality checks (parse failures, truncation, mid-trial gaps, baseline consistency) run and passed for both rounds; results show strong consistency between Round 1 and Round 2 across all measured strategies. 
Round 3 pending. System prompts (Answerer + Prober) finalized per CARE framework (Basta et al.) — see shared strategy/prompt document for exact wording and citation tables. Known limitations: a small fraction of trials (<1% per round) excluded due to responses exceeding the token cap or entering a non-convergent reasoning loop on certain multi-step math/edge-case questions — documented per round, not fixed via retry beyond one attempt.