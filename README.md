1. Write the prompt
Create prompt.md inside the task folder.
Add the exact question or instruction you want to evaluate.


2. Get ChatGPT’s response
Copy the contents of prompt.md.
Ask ChatGPT to answer it.
Save the answer as:
chatgpt_response.md


3. Get Grok’s response
Use the exact same prompt.
Ask Grok to answer it.
Save the answer as:
grok_response.md


4. Generate a rubric from ChatGPT’s response
Generate rubric.json
This defines:
evaluation dimensions
weights
ideal answer notes
scoring scale


5. Evaluate both models using the same rubric
Evaluate Grok
inputs:
rubric.json
prompt.md
grok_response.md
Output:
eval_grok.json
Evaluate ChatGPT
inputs:
rubric.json
prompt.md
chatgpt_response.md
Output:
eval_chatgpt.json

Each eval file includes:
score per dimension
rationale for the score
weighted total score
overall feedback

6. Aggregate results
Use all eval_*.json files to produce:
Per-task comparisons
Which model scored higher on each dimension
Which model produced the overall best answer for that task
Per-dimension averages across all tasks
Average correctness, clarity, depth, usefulness, etc.
Model trends (strengths/weaknesses)
Results are saved as:
summary_by_task.csv
summary_by_dimension.csv