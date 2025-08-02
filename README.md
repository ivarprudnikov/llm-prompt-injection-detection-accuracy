
# Evaluating Prompt Injection Detection Using LLMs

## Overview

This project evaluates the ability of large language models (LLMs) to detect prompt injections, and investigates how detection performance changes as the context size increases. The hypothesis is that LLMs' ability to identify prompt injections deteriorates with larger input contexts, as suggested in some recent research. The results do not show such correlation but instead suggests that a slight context increase could improve the detection performance. The results also reinforce the idea of a reinforced prompt by reminding at the end of the input about the original instruction which improves the detection performance.

## Repository Structure

- **data/**: Contains all data files used for evaluation but also the outputs.
  - `basic_attacks.json`: Sample prompt injection attacks.
  - `basic_inputs.json`: Valid (non-malicious) prompt samples.
  - `detections.json`: List of detection prompts to evaluate against the samples.
  - `increases/`: Zipped HTML files representing context increases (news articles) at different token sizes (1k, 5k, 10k, 50k), each split in half and zipped.
  - `output_*.json`: Model evaluation outputs for various models and context sizes.
  - `overview.html`: HTML overview of results.
- **Notebooks**:
  - `run_<model>_<context>.ipynb`: Notebooks to run tests for each model and context size combination (e.g., `run_gpt4_10k.ipynb`).
  - `results_overview.ipynb`: Parses results and generates a summary report.
- **requirements.txt**: Python dependencies.
- **README.md**: Project documentation (this file).

## Models under evaluation

- OpenAI GPT-4.1 (1.7T params)
- Mistral ministral-3b-2410 (3B params)
- Mistral mistral-large-2411 (123B params)
- Llama Llama-4-Maverick-17B-128E-Instruct-FP8 (17B params)
- Gemini gemma3:12b (12B params)

## Data and Evaluation

- **Attacks**: Extracted from [PromptFuzz](https://github.com/sherdencooper/PromptFuzz).
- **Detections**: Two detection prompts to test model detection capabilities.
- **Context Increases**: Real news articles (unseen by models at the time), zipped and split for context expansion experiments. 

## How to Use

> **Warning:** Running 50k context size tests is expensive (approx. $100 for 2,000 requests).

### Prerequisites

- Python 3.12+
- Jupyter Notebook
- API keys for the relevant LLMs, set in `.env.local` (overrides `.env`)
  - For GPT4 model tests
    - `OPENAI_API_KEY` - get it from Open AI but also make sure to be on Tier 2 to get enough throughput without rate limiting.
  - For Mistral model tests
    - `MISTRAL_API_KEY` - get it from Mistral AI platform
  - For Llama Maverick model tests
    - Deploy the model on Azure AI Foundry and set the following environment variables:
      - `AZURE_AI_URL=https://<Specific to deployment>.services.ai.azure.com/models`
      - `AZURE_AI_KEY=****<Deployment key>****`
      - `AZURE_AI_API_VERSION=2024-05-01-preview`
  - For Gemma3 model tests
    - For `0k` tests download and use `ollama` to run the Llama model locally `ollama run gemma3:12b`
    - For other tests `GEMINI_API_KEY` is necessary - get it from Google AI Platform. The rate limit is very low no matter which pricing Tier you are on.

### Setup

1. **Create a virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use .venv\Scripts\activate
   ```
2. **Install dependencies:**
   ```bash
   pip install --upgrade pip  # May be needed for genai package
   pip install -r requirements.txt
   ```
3. **Configure API keys:**
   - Copy the template `cp .env .env.local`
   - Place your API keys in a `.env.local`. This will override values in `.env`.

### Running Experiments

1. **Open the desired notebook** for the model and context size you wish to test (e.g., `run_gpt4_10k.ipynb`).
2. **Run all cells** to execute the evaluation.
3. **To parse results and generate a report**, run the [results_overview.ipynb](results_overview.ipynb) notebook.

## References & Related Work

- [PromptFuzz](https://github.com/sherdencooper/PromptFuzz): Source of attack data.
- [Open-Prompt-Injection](https://github.com/liu00222/Open-Prompt-Injection): Evaluation framework for prompt injection detection.
- [HarmBench](https://github.com/centerforaisafety/HarmBench): Model evaluation for harmful output detection.

## Troubleshooting

- **API Rate Limits:** If you encounter rate limit errors, check your API tier and consider reducing request frequency by increasing the `sleep` between test runs.
- **Missing Keys:** Ensure `.env.local` is correctly configured and sourced.
- **Dependency Issues:** Run `pip install --upgrade pip` before installing requirements. And make sure your `venv` is used and selected as a kernel in Jupyter.

## License

This project is licensed under the MIT License.

Feel free to use and modify it for your own purposes, but please attribute the original authors, including PromptFuzz where the attack data came from.

