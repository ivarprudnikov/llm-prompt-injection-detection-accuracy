Testing LLM ability to detect injections and how it deteriorates when the context size is increased.

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

External references and related work:
- Attack data was extracted from https://github.com/sherdencooper/PromptFuzz
- Related as it provides a way to evaluate detections and attacks against the model https://github.com/liu00222/Open-Prompt-Injection
- Related as it contains model evaluation examples to check for harmful output https://github.com/centerforaisafety/HarmBench


