# LLM based prompt injection detection accuracy and its relationship to the context size

## Abstract

This project investigates the efficacy of large language models (LLMs) in detecting prompt injection attacks, with particular focus on how detection performance varies with increasing context size. While recent literature suggests that LLMs' ability to detect code bugs deteriorate with larger input contexts, our empirical findings do not support this hypothesis in the context of prompt injection detection. Instead, our results indicate that moderate context expansion may enhance detection performance. Furthermore, our experiments demonstrate that reinforcement strategies—specifically, reiterating original instructions at the end of the input—significantly improve detection accuracy.

## Repository Structure

- **`data/`**: Contains experimental datasets and output files
  - `basic_attacks.json`: Curated prompt injection attack samples
  - `basic_inputs.json`: Benign prompt samples for baseline comparison
  - `detections.json`: Detection prompt templates evaluated in experiments
  - `increases/`: Context augmentation data (news articles) compressed at various token lengths (1k, 5k, 10k, 50k tokens)
  - `output_*.json`: Experimental results for different model-context configurations
  - `overview.html`: Visual summary of experimental findings
- **Experimental Notebooks**:
  - `run_<model>_<context>.ipynb`: Evaluation pipelines for specific model-context combinations
  - `results_overview.ipynb`: Analysis notebook for aggregating and visualizing results
- **`requirements.txt`**: Python package dependencies
- **`README.md`**: Project documentation

## Models Evaluated

Our study encompasses a diverse set of state-of-the-art language models:

- **OpenAI GPT-4.1** (1.7T parameters)
- **Mistral ministral-3b-2410** (3B parameters)
- **Mistral mistral-large-2411** (123B parameters)
- **Llama-4-Maverick-17B-128E-Instruct-FP8** (17B parameters)
- **Google Gemma3:12b** (12B parameters)

## Dataset and Methodology

- **Attack Vectors**: Prompt injection samples sourced from [PromptFuzz](https://github.com/sherdencooper/PromptFuzz)
- **Detection Strategies**: Two distinct detection prompt architectures evaluated for robustness
- **Context Augmentation**: News articles (post-training cutoff) utilized for controlled context expansion experiments

## Experimental Setup

> **Note:** Experiments with 50k token contexts incur significant computational costs (approximately $100 for 2,000 API calls).

### Prerequisites

- Python 3.12 or higher
- Jupyter Notebook environment
- API credentials for respective LLM providers (configured via `.env.local`)

### Environment Configuration

1. **Initialize virtual environment:**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

2. **Install dependencies:**
   ```bash
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

3. **Configure API credentials:**
   ```bash
   cp .env .env.local
   ```
   
   Required environment variables:
   - **GPT-4 experiments**: `OPENAI_API_KEY` (Tier 2 access recommended for sufficient throughput)
   - **Mistral experiments**: `MISTRAL_API_KEY`
   - **Llama experiments** (via Azure AI Foundry):
     - `AZURE_AI_URL=https://<deployment-specific>.services.ai.azure.com/models`
     - `AZURE_AI_KEY=<deployment-key>`
     - `AZURE_AI_API_VERSION=2024-05-01-preview`
   - **Gemma experiments**:
     - 0k context: Local deployment via `ollama run gemma3:12b`
     - Larger contexts: `GEMINI_API_KEY` (note: rate limits apply regardless of pricing tier)

### Running Experiments

1. Select your target model (e.g., `gpt4`)
2. Always start with the `0k` notebook, i.e. `run_<model>_0k.ipynb` as it generates the baseline output file used in other model tests (e.g., `run_gpt4_0k.ipynb`)
3. Execute all cells to perform the evaluation
4. Generate comprehensive results analysis using `results_overview.ipynb`

## Related Work

- [PromptFuzz](https://github.com/sherdencooper/PromptFuzz): Comprehensive prompt injection benchmark
- [Open-Prompt-Injection](https://github.com/liu00222/Open-Prompt-Injection): Framework for systematic evaluation of prompt injection defenses
- [HarmBench](https://github.com/centerforaisafety/HarmBench): Benchmark for evaluating harmful content detection in LLMs

## Troubleshooting

- **Rate Limiting**: Verify API tier status and adjust request intervals in notebooks as needed
- **Authentication Errors**: Ensure `.env.local` contains valid API keys and is properly loaded
- **Dependency Conflicts**: Update pip before installation and verify correct virtual environment activation

## Citation

If you use this work in your research, please cite:

```bibtex
@misc{llm-prompt-injection-detection-accuracy-2025,
  title={LLM based prompt injection detection accuracy and its relationship to the context size},
  author={Aivaras Prudnikovas},
  year={2025},
  publisher={GitHub},
  url={https://github.com/ivarprudnikov/llm-prompt-injection-detection-accuracy}
}
```

## License

This project is licensed under the MIT License. Attribution is required for derivative works, including acknowledgment of the PromptFuzz dataset authors.

