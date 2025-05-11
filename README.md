Testing LLM ability to detect injections

External repos:
- https://github.com/liu00222/Open-Prompt-Injection
- https://github.com/centerforaisafety/HarmBench
- https://github.com/sherdencooper/PromptFuzz

Core parts:
- large sample of prompts
  - each prompt is fanned out to:
    - include a prompt injection
      - multiple types of injections
    - include a prompt injection but with an increased context size
      - increase with random text (before, after, or both)
- subset of prompt injection detection techniques
- evauation if the detection was successful
- a selection of LLMs to test against
  - OpenAI
