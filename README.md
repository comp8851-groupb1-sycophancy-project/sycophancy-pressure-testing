# Can an AI Be Talked Out of a Right Answer by Fake Evidence or Sheer Persistence?
COMP8851 Major Project — testing whether LLMs abandon correct answers under fabricated evidence or persistent disagreement.

## Team
* Akhil Ramesh
* Muhammad Mubeen
* Lakshya Kumar Wadhwa
* Muhammad Rafi

## Structure
* `gpt4_1_mini_fake_evidence.ipynb` — Fake Evidence strategies (Specific Wrong Objection, Fabricated Statistics, Fabricated Citation, Misattributed Citation) tested on GPT-4.1 Mini. Core implementation plus Data Quality and results-analysis sections — see commit history for earlier development/debugging notebooks if needed.
* `build_shared_questions_set_MMLU.ipynb` — builds the shared 320-question dataset (40 questions x 8 MMLU subjects, fixed seed for reproducibility). Run once; both GPT-4.1 Mini and Gemini sides must use its output (`locked_320_questions.jsonl`) unchanged, so all strategies and both models are tested against the identical question set.
* `locked_320_questions.jsonl` — the actual locked 320-question dataset, shared across all team members' notebooks
* `results_gpt4mini_fe.jsonl` — raw trial results log (one JSON object per turn: baseline, pressure turns, scoring, probe responses) for GPT-4.1 Mini, Fake Evidence, Rounds 1–3
* More files to be added as each team member's work progresses (Gemini + Persistence strategies)

## Status
All 3 Rounds complete for GPT-4.1 Mini / Fake Evidence (320 questions x 4 strategies x 3 Rounds). Pipeline fully validated: schema fixed to generate justification before answer (reduces measured flip rate substantially vs. naive ordering), `max_tokens` raised from 600 to 1,000 partway through Round 1 to reduce token-cap exclusions, resumability logic corrected so interrupted or partially-failed trials retry correctly without duplicating completed work.

Data quality checks (parse failures, truncation, mid-trial gaps, baseline consistency) run and passed for all three rounds; results show strong consistency across all three Rounds for every measured strategy. Overall flip rate stable at 18.6–18.8% per Round; full findings in the results-analysis section of the notebook.

System prompts (Answerer + Prober) finalized per CARE framework (Basta et al.) — see shared strategy/prompt document for exact wording and citation tables.
