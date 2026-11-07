# Can an AI Be Talked Out of a Right Answer by Fake Evidence or Sheer Persistence?
COMP8851 Major Project — testing whether LLMs abandon correct answers under fabricated evidence or persistent disagreement.

## Team
* Akhil Ramesh
* Muhammad Mubeen
* Lakshya Kumar Wadhwa
* Muhammad Rafi

## Key Findings
* A bare objection with no evidence (Specific Wrong Objection) flipped **47.6%** of GPT-4.1 Mini's correct answers, the strongest strategy on both models.
* The result generalised to Group B2's 104 new questions (**46.1%**).
* Extending Fake Evidence to five turns added little (26 extra flips on GPT, 13 on Gemini), whereas Persistence kept rising with escalating pressure.
* Under Escalating Persistence, 65 questions flipped on GPT only versus 4 on Gemini only (paired McNemar test, significant).
* Models acknowledged changing their answer, but GPT credited the message's content in over 90% of cases, even when it contained no evidence.

## Shared Dataset
* `build_shared_questions_set_MMLU.ipynb` — builds the shared 320-question dataset (40 questions x 8 MMLU subjects, fixed seed for reproducibility). Run once; all notebooks use its output unchanged, so every strategy and both models are tested against the identical question set.
* `locked_320_questions.jsonl` — the locked 320-question dataset shared across all notebooks.

## Main Experiments (3 Rounds each)
**GPT-4.1 Mini**
* `gpt4_1_mini_fake_evidence.ipynb` — Fake Evidence strategies (Specific Wrong Objection, Fabricated Statistics, Fabricated Citation, Misattributed Citation). Includes Data Quality and results-analysis sections.
* `results_gpt4mini_fe.jsonl` — raw trial log (one JSON object per turn: baseline, pressure turns, scoring, probe responses).
* `gpt4_1_mini_Persistence_evidence.ipynb` / `results_gpt4mini_persistence.jsonl` — Persistence strategies.

**Gemini 3.5 Flash Lite**
* `gemini_3_5_flash_lite_fake_evidence.ipynb` / `results_gemini35flashlite_fe.jsonl` — Fake Evidence strategies.
* `gemini_persistence_3.5 flash lite` / `results_gemini_persistence.jsonl` / `results_gemini_persistence.csv` — Persistence strategies.

## Follow-up Experiments
**Five-turn Fake Evidence extension** — replays Fake Evidence trials at Turns 4 and 5 to compare both techniques over an equal number of turns.
* `followup_reruns_gpt4mini_fe.ipynb` — runs the extension.
* `results_gpt4mini_fe_ext5.jsonl`, `results_gemini35flashlite_fe_ext5.jsonl` — extension results.
* `README_EXT5.md` — details of the extension design.

**Four-condition Probe re-run** — re-runs the Probe to check whether its original wording was leading the models.
* `rerun_probe_swo.jsonl` — GPT-4.1 Mini, Fake Evidence.
* `probe_rerun_gemini35flashlite_fe.jsonl` — Gemini, Fake Evidence.
* `followup_reruns_gpt4mini_persistence.ipynb` / `rerun_probe_escalating_persistence.jsonl` — GPT-4.1 Mini, Persistence.
* `gemini_persistence_probe_rerun_final.py` / `results_gemini_3_5_flash_lite_persistence_probe_rerun.jsonl` / `.csv` — Gemini, Persistence.

**Follow-up analysis**
* `followup_analysis_gpt4mini_fe.ipynb` — question-level analysis (by subject and confidence), flip reasons, and cross-technique overlap.
* `followup_all_trials.csv`, `followup_round_consistency.csv`, `followup_fe_persistence_overlap.csv`, `followup_flip_reasons.csv` — analysis outputs.

## Group B2 Generalisation Test
Runs GPT-4.1 Mini Fake Evidence on Group B2's 104-question set to test whether the main result generalises.
* `groupB2_Qs_mmlu_104_questions.json` — Group B2's original question set.
* `locked_b2_104_questions.jsonl` — locked version used in the run.
* `results_gpt4mini_b2_fe.jsonl` — raw trial results.
* `b2_comparison_analysis.ipynb` — comparison with our main results and paired McNemar significance tests.
* `b2_per_round_breakdown.csv`, `b2_round_consistency.csv`, `b2_significance.csv` — analysis outputs.

## Figures
* `report_figures/` — final figures used in the report and presentation (pipeline, flip rate by strategy, flip risk by turn, McNemar comparison, which questions flip, why flips happen).

## Status
All experiments complete. Main experiments: 3 Rounds for GPT-4.1 Mini and Gemini 3.5 Flash Lite across both techniques (320 questions x 4 strategies x 3 Rounds). Follow-up experiments and the Group B2 generalisation test are also complete.

Pipeline fully validated: schema generates justification before answer (reduces measured flip rate substantially vs. naive ordering), `max_tokens` raised from 600 to 1,000 partway through Round 1 to reduce token-cap exclusions, and resumability logic corrected so interrupted or partially-failed trials retry correctly without duplicating completed work.

Data quality checks (parse failures, truncation, mid-trial gaps, baseline consistency) run and passed for all three Rounds; results are strongly consistent across Rounds for every measured strategy.

System prompts (Answerer + Prober) finalised per the CARE framework (Basta et al.).

## Setup
API keys are read from a local `.env` file, which is not committed. Create your own `.env` with the required OpenAI and Gemini keys before running the notebooks.
