# Can an AI Be Talked Out of a Right Answer by Fake Evidence or Sheer Persistence?

COMP8851 Major Project — testing whether LLMs abandon correct answers 
under fabricated evidence or persistent disagreement.

## Team
- Akhil Ramesh
- Muhammad Mubeen
- Lakshya Kumar Wadhwa
- Muhammad Rafi

## Structure
- `gpt4_1_mini_fake_evidence.ipynb` — Fake Evidence strategies (Specific Wrong 
  Objection, Fabricated Statistics, Fabricated Citation, Misattributed Citation) 
  tested on GPT-4.1 Mini
- `build_shared_questions_set_MMLU.ipynb` — builds the shared 320-question 
  dataset (40 questions x 8 MMLU subjects, fixed seed for reproducibility). 
  Run once; both GPT-4.1 Mini and Gemini sides must use its output 
  (`locked_320_questions.jsonl`) unchanged, so all strategies and both models 
  are tested against the identical question set.
- `locked_320_questions.jsonl` — the actual locked 320-question dataset, 
  shared across all team members' notebooks
- `results_gpt4mini_fe.jsonl` — raw trial results log (one JSON object per turn: 
  baseline, pressure turns, scoring, probe responses)
- `results_gpt4mini_fe.csv` — same results, exported as a spreadsheet-friendly 
  table for manual review/analysis
- More files to be added as each team member's work progresses (Gemini + 
  Persistence strategies)

## Status
Prototype stage — pipeline confirmed working end-to-end (Answerer, Opponent, 
trial loop, logging, analysis). System prompts restructured per CARE framework 
(Basta et al.) — Role/Context/Objective/Instructions format, verified working. 
Shared 320-question dataset built and locked. Full 320-question MMLU run in 
progress.