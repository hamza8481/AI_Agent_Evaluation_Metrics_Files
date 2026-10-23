AI Agent Evaluation Metrics: Answer Relevance

An automated AI testing and evaluation workflow built in n8n. This pipeline is designed to test the performance of LLM Agents by calculating a mathematical Relevance Score (using cosine similarity) between a users original question and the agents output.

Business and Operational Value
1. Automated QA for AI: Stops AI drift. Automatically tests how well your agent answers questions based on a predetermined dataset.
2. Hallucination Detection: Detects noncommittal answers (e.g. I do not know) and penalizes the score. If the agent goes off-script or hallucinates irrelevant info, the cosine similarity drops.
3. Metric Tracking: Automatically saves all evaluation metrics (Relevance Scores) directly to a Google Sheet for CI/CD reporting and model tuning.

Tech Stack and Key Nodes
1. Framework: n8n Evaluation Triggers and Langchain Nodes.
2. Models Used: gpt-4o-mini (For text generation and answer analysis) and text-embedding-3-small (For generating vectors to compare question similarity).
3. Core Nodes:
   a. Evaluation Trigger: Pulls test dataset rows from Google Sheets.
   b. Answer Relevance (LLM Chain): Acts as an interrogator, generating a new question based on the agents answer.
   c. Cosine Similarity Code Node: A custom JavaScript node that calculates the mathematical distance between two vectors.

Execution Flow
1. Dataset Fetch: The Evaluation Trigger pulls a row from a Google Sheet containing a test input (question).
2. Execution: The AI Agent receives the test question and generates a response (answer).
3. Re-Questioning: The Answer Relevance LLM reads the generated answer and attempts to formulate a new question that this answer solves. It also flags if the answer is noncommittal.
4. Vectorization: Both the original user question and the newly generated question are sent to OpenAI to be converted into Embeddings (vectors).
5. Similarity Math: A custom JavaScript node calculates the Cosine Similarity between the two vectors.
6. Scoring: The system calculates the final Relevance Score. If the questions are similar, relevancy is high. If the answer was noncommittal, the score is multiplied by 0.
7. Record: The final score and output are written back to the Google Sheet.
