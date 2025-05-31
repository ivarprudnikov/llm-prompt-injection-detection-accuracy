Evaluating prompt injection detection using LLMs
========================

# About

Testing LLM ability to detect injections and how/if it deteriorates when the context size is increased.

The assumption is that LLMs ability to "see" the prompt injection in ever larger text will deteriorate. This was drawn from the observations in some research papers talking about it in the context of the ability to detect bugs in the code.

Core parts:
- number of sample attacks under `data/basic_attacks.json`
- few samples of valid prompts `data/basic_inputs.json`
- a list of detections to evaluate against the above samples under `data/detections.json`
- a set of prompt context increases to avaluate in addition to evaluating the above prompts
  - there are few increases with an approximate token size: 1k, 10k, 50k (50k tests are expensive!)
  - an increases are html content of recent news articles, the ones LLMs could not have seen before
  - each increase is split in half and is zipped
- a notebook for every model and context size combination to run the tests
  - models are:
    - OpenAI gpt-4.1 (1.7T params)
    - Mistral ministral-3b-2410 (3B params)
    - Mistral mistral-large-2411 (123B params)
    - Llama Llama-4-Maverick-17B-128E-Instruct-FP8 (17B params)
    - Gemini gemma3:12b (12B params)
- a notebook to parse the results and generate a report

External references and related work:
- Attack data was extracted from https://github.com/sherdencooper/PromptFuzz
- Related as it provides a way to evaluate detections and attacks against the model https://github.com/liu00222/Open-Prompt-Injection
- Related as it contains model evaluation examples to check for harmful output https://github.com/centerforaisafety/HarmBench

# Usage

**Note** that running 50k context size tests is expensive, it is around $100 on any platform for 2k requests that tests need.

Prerequisites:
- Python 3.12+
- Jupyter Notebook
- Your AI keys set in `.env.local` that override the values in `.env`

Prepare environment:
- Create a virtual environment:
  ```bash
  python -m venv venv
  source venv/bin/activate  # On Windows use .venv\Scripts\activate
  ```
- Install dependencies:
  ```bash
  pip install --upgrade pip # might be needed for genai package
  pip install -r requirements.txt
  ```
- Open the Jupyter notebook and run it
- To parse the results and generate a report, run the [results_overview.ipynb](results_overview.ipynb) notebook