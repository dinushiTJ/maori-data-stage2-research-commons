# Māori Data Identification in University of Waikato Research Commons (Stage 2)

[![Hugging Face datasets](https://img.shields.io/badge/Hugging%20Face-Datasets-FFD21E?logo=huggingface&logoColor=black)](https://huggingface.co/collections/dinushiTJ/mdf-research-commons-datasets)
[![Stage 1 repository](https://img.shields.io/badge/GitHub-Stage%201-181717?logo=github&logoColor=white)](https://github.com/dinushiTJ/maori-data-stage1-hansard)
[![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebooks-F9AB00?logo=googlecolab&logoColor=white)](https://colab.research.google.com/)
[![Kaggle](https://img.shields.io/badge/Kaggle-Notebooks-20BEFF?logo=kaggle&logoColor=white)](https://www.kaggle.com/)
![Python](https://img.shields.io/badge/Python-3-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebooks-F37626?logo=jupyter&logoColor=white)
![Gemini](https://img.shields.io/badge/Gemini-Labelling-8E75B2?logo=googlegemini&logoColor=white)
[![EmbeddingGemma](https://img.shields.io/badge/Model-EmbeddingGemma-4285F4?logo=google&logoColor=white)](https://huggingface.co/google/embeddinggemma-300M)
![Gemma 3 4B](https://img.shields.io/badge/Model-Gemma--3--4B%20LoRA-4285F4?logo=google&logoColor=white)
[![Source: Research Commons](https://img.shields.io/badge/Source-Research%20Commons-BE0F34)](https://researchcommons.waikato.ac.nz/)

Research code for finding and classifying Māori data in the [University of Waikato Research Commons](https://researchcommons.waikato.ac.nz/), the university's open-access repository of theses and research papers.

The work has two parts:

- **Stage 2.1** scrapes the metadata (title, abstract, subjects and so on) of every Research Commons record. An LLM labels each record as **Māori-related** (`maori_origin`) or **not Māori-related** (`non_maori_origin`). The labels are then used to train and compare cheaper classifiers: TF-IDF baselines, [EmbeddingGemma](https://huggingface.co/google/embeddinggemma-300M) and a LoRA fine-tuned Gemma-3-4B. The labels are also checked against other LLM providers.
- **Stage 2.2** takes the records labelled `maori_origin`, downloads the full text of each paper, and extracts quoted evidence of whether and how the paper contains Māori data.

## Related repositories

This repository is part of the University of Waikato Māori Data Findability Project. The project is split into stages, and each stage has its own repository:

| Stage | Repository | Source |
|---|---|---|
| Stage 1 | [maori-data-stage1-hansard](https://github.com/dinushiTJ/maori-data-stage1-hansard) | New Zealand Parliament debates (Hansard) |
| Stage 2 | [maori-data-stage2-research-commons](https://github.com/dinushiTJ/maori-data-stage2-research-commons) (this repository) | University of Waikato Research Commons |

Stage 2 reuses the Stage 1 approach (LLM labelling followed by a fine-tuned EmbeddingGemma classifier) and the same `maori_origin` / `non_maori_origin` labels. It then adds baselines, Gemma-3-4B fine-tuning, checks against other LLM providers and full-text evidence extraction.

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

The published datasets are in the [MDF Research Commons Datasets](https://huggingface.co/collections/dinushiTJ/mdf-research-commons-datasets) collection on the Hugging Face Hub.

## Authors

All authors are at the School of Computing and Mathematical Sciences, University of Waikato, Hamilton, New Zealand.

| Name | Email | ORCID |
|---|---|---|
| Dinushi Jayasinghe | [dinushi.jayasinghe@waikato.ac.nz](mailto:dinushi.jayasinghe@waikato.ac.nz) | [![ORCID](https://img.shields.io/badge/ORCID-0009--0005--6042--9137-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0009-0005-6042-9137) |
| Paul T. Brown | [paul.brown@waikato.ac.nz](mailto:paul.brown@waikato.ac.nz) | [![ORCID](https://img.shields.io/badge/ORCID-0000--0001--7501--3971-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0000-0001-7501-3971) |
| Nick Lim | [nick.lim@waikato.ac.nz](mailto:nick.lim@waikato.ac.nz) | [![ORCID](https://img.shields.io/badge/ORCID-0000--0003--4690--5780-A6CE39?logo=orcid&logoColor=white)](https://orcid.org/0000-0003-4690-5780) |

## Acknowledgements

The records and papers come from [Research Commons](https://researchcommons.waikato.ac.nz/), the institutional repository of the University of Waikato. Copyright in each item stays with its authors or rights holders. Check the licence on each item before reusing its content.
