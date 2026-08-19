# Replication Package — Benchmark Data Contamination in Underrepresented Languages: A Comprehensive Analysis Using Brazilian Data

This repository provides the full replication package for the paper **“Benchmark Data Contamination in Underrepresented Languages: A Comprehensive Analysis Using Brazilian Data”** (Anonymous submission, 2025).  
It includes all resources and raw outputs required to reproduce the results presented in the study.

## 📘 Overview

This study investigates **benchmark data contamination in underrepresented languages**, focusing on **Brazilian Portuguese (PT-BR)** as a case study.  
We analyze contamination along two complementary dimensions:

1. **Behavioral contamination (model-level)** — assessed with the **TS-Guessing** protocol, implemented within the [HELM framework (Stanford)](https://crfm.stanford.edu/helm/).  
   This method measures whether a model recalls benchmark content rather than reasoning from context.  

2. **Corpus-level contamination (data-level)** — detected using the **50-character substring overlap** technique implemented via [LLMSanitize (NTU NLP Group)](https://github.com/ntunlp/LLMSanitize).  
   This analysis identifies verbatim benchmark traces embedded in large-scale pretraining corpora.

## 🗂 Repository Structure

```
├── README.md ← This file
├── ts-guessing/ ← Behavioral contamination (model-level)
│ ├── README.md ← Instructions for running TS-Guessing on HELM
│ └── results/ ← Raw and aggregated behavioral contamination outputs
├── substring-similarity/ ← Corpus-level contamination (data-level)
│ ├── README.md ← Instructions for running the 50-character substring method
│ └── results/ ← Raw overlap outputs from corpus analysis
```

## 🚀 Reproducing the Results

To replicate the study:

1. Follow the setup and execution steps in  
   [`ts-guessing/README.md`](ts-guessing/README.md) to reproduce **behavioral (model-level)** contamination.  
   [`substring-similarity/README.md`](substring-similarity/README.md) to reproduce **corpus-level (data-level)** overlap detection.  
2. Compare your results against the provided raw outputs stored in each `results/` directory.

## 📊 Data and Outputs

- **Benchmarks**:  
  - [BLUEX](https://huggingface.co/datasets/portuguese-benchmark-datasets/BLUEX)  
  - [ENEM Challenge](https://huggingface.co/datasets/eduagarcia/enem_challenge)  
  - [HealthQA](https://huggingface.co/datasets/Larxel/healthqa-br)  
  - [OAB Exams](https://huggingface.co/datasets/eduagarcia/oab_exams)

- **Pretraining corpora**:  
  - [Aroeira](https://huggingface.co/datasets/Itau-Unibanco/aroeira)  
  - [GigaVerbo](https://huggingface.co/datasets/TucanoBR/GigaVerbo)  
  - [mC4-PT](https://huggingface.co/datasets/eduagarcia/mc4-pt)

- **Models (Specialized PT-BR)**:
  - [Bode-13B-Alpaca-PT-BR](https://huggingface.co/recogna-nlp/bode-13b-alpaca-pt-br-no-peft)
  - [Cabrita-7B](https://huggingface.co/22h/cabrita_7b_pt_850000)
  - [Gemma-3-Gaia-PT-BR-4B-it](https://huggingface.co/CEIA-UFG/Gemma-3-Gaia-PT-BR-4b-it)
  - [Gervásio-7B](https://huggingface.co/PORTULAN/gervasio-7b-portuguese-ptbr-decoder)
  - [Sabia-3.1](https://docs.maritaca.ai/api/pt/list-models)
  - [Sabia-7B](https://huggingface.co/maritaca-ai/sabia-7b)
  - [Tucano-2B4](https://huggingface.co/TucanoBR/Tucano-2b4)

- **Models (Multilingual)**:
  - [Gemma-3-12B-it](https://huggingface.co/google/gemma-3-12b-it)
  - [Granite-3.3-8B-Instr.](https://huggingface.co/ibm-granite/granite-3.3-8b-instruct)
  - [GPT-4o](https://platform.openai.com/docs/models/gpt-4o)
  - [Llama-3.1-8B](https://huggingface.co/meta-llama/Llama-3.1-8B-Instruct)
  - [Llama-4-Maverick-17B](https://huggingface.co/meta-llama/Llama-4-Maverick-17B-128E-Instruct)
  - [Ministral-8B-Instr.](https://huggingface.co/mistralai/Ministral-8B-Instruct-2410)
  - [Phi-4](https://huggingface.co/microsoft/phi-4)
  - [Qwen3-8B](https://huggingface.co/Qwen/Qwen3-8B)

All raw outputs are available in **JSONL** format for verification and independent inspection.

## 📄 Citation

If you use this package, please cite the associated paper:

> Vilar, I. S. M. de M., Maia, D. C., Brunet, J., Morais, F., & Marinho, L. B. (2026). *Benchmark Data Contamination in Underrepresented Languages: A Comprehensive Analysis Using Brazilian Data.* In Proceedings of the Fifteenth Language Resources and Evaluation Conference (LREC 2026), pp. 4765–4777. ELRA. https://doi.org/10.63317/39wbjvajnh7t

BibTeX and citation metadata are also available in [`CITATION.cff`](./CITATION.cff).

---

For inquiries related to replication or data inspection, please open an issue in this repository.