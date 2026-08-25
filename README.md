\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*PROJECT GROUP 1B\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*



\# Can an AI Be Talked Out of a Right Answer by Fake Evidence or Sheer Persistence?



COMP8851 Major Project — testing whether LLMs abandon correct answers 

under fabricated evidence or persistent disagreement.



\## Team

\- Akhil Ramesh

\- Muhammad Mubeen

\- Lakshya Kumar Wadhwa

\- Muhammad Rafi



\## Structure

\- `gpt4\_1\_mini\_fake\_evidence.ipynb` - Fake Evidence strategies (False Consensus, 

&#x20; Specific Wrong Objection, Fabricated Citation, Misattributed Citation) tested 

&#x20; on GPT-4.1 Mini

\- `results\_gpt4mini\_fe.jsonl` - raw trial results log (one JSON object per turn: 

&#x20; baseline, pressure turns, scoring, probe responses)

\- `results\_gpt4mini\_fe.csv` - same results, exported as a spreadsheet-friendly 

&#x20; table for manual review/analysis

\- More files to be added as each team member's work progresses (Gemini + 

&#x20; Persistence strategies)



\## Status



Prototype stage — pipeline confirmed working end-to-end (Answerer, Opponent, 

trial loop, logging, analysis). 



Sampling of 320-question MMLU run in progress.

