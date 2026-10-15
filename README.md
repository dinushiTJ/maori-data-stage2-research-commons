# Māori Data Identification in University of Waikato Research Commons (Stage 2)

Research code for finding and classifying Māori data in the [University of Waikato Research Commons](https://researchcommons.waikato.ac.nz/), the university's open-access repository of theses and research papers.

The work has two parts:

- **Stage 2.1** scrapes the metadata (title, abstract, subjects and so on) of every Research Commons record. An LLM labels each record as **Māori-related** (`maori_origin`) or **not Māori-related** (`non_maori_origin`). The labels are then used to train and compare cheaper classifiers: TF-IDF baselines, [EmbeddingGemma](https://huggingface.co/google/embeddinggemma-300M) and a LoRA fine-tuned Gemma-3-4B. The labels are also checked against other LLM providers.
- **Stage 2.2** takes the records labelled `maori_origin`, downloads the full text of each paper, and extracts quoted evidence of whether and how the paper contains Māori data.

## Notebooks

Each stage has a `notebooks/` folder. Notebooks are numbered in the order they should be run (`NN_` is a step and `NN.M_` is a sub-step). Most were written for Google Colab. The `08.1` and `08.3` notebooks were written for Kaggle.

### Stage 2.1: Classification (`stage2.1_research_commons/notebooks/`)

| Notebook | What it does |
|---|---|
| `00`–`02` | Tests the scraper, scrapes Research Commons, then cleans and hashes the dataset |
| `03`–`04` | Builds the classification prompt, labels each record with Gemini and reviews low-confidence labels |
| `05.1`–`05.2` | Prepares the datasets for the embedding models and for inference |
| `06.1`–`06.5` | TF-IDF and traditional classifier baselines |
| `07.1`–`07.8` | EmbeddingGemma zero-shot and fine-tuned models (`07.8` retrains without test leakage) |
| `08.1`–`08.4` | Gemma-3-4B base and LoRA fine-tuned inference and evaluation |
| `09.1`–`09.7` | Checks the Gemini labels against Llama (Groq), OpenAI GPT-5.4-mini and GPT-OSS |
| `10_paper_charts` | Figures for the paper |

### Stage 2.2: Evidence extraction (`stage2.2_evidence_extraction/notebooks/`)

| Notebook | What it does |
|---|---|
| `01_maori_data_evidence_extraction` | Downloads the full text of `maori_origin` papers, extracts evidence with Gemini, checks every quote against the source text and saves a Māori-data inventory |

## Running

Open a notebook in [Google Colab](https://colab.research.google.com/) (or Kaggle for `08.1` and `08.3`) and run the cells in order. Each notebook installs its own dependencies.

The notebooks read API keys from Colab Secrets or Kaggle Secrets. Depending on the notebook you will need:

- `HF_TOKEN`, a Hugging Face token, for the models and datasets
- `GOOGLE_API_KEY` or `GEMINI_API_KEY`, for Gemini
- `GROQ_API_KEY` and `OPENAI_API_KEY`, for the cross-provider checks
- `WANDB_KEY`, a Weights & Biases key, for logging training runs

## Data

The datasets are too large to include in this repository. Each stage expects its local data in a `data/` folder, which git ignores. Notebook 01 scrapes the records again, and the later notebooks read the labelled datasets from the Hugging Face Hub (`dinushiTJ/nz_research_commons_*`).

## Acknowledgements

The records and papers come from [Research Commons](https://researchcommons.waikato.ac.nz/), the institutional repository of the University of Waikato. Copyright in each item stays with its authors or rights holders. Check the licence on each item before reusing its content.
