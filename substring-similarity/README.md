# Substring Overlap Replication Guide — Corpus-Level Contamination (Data-Level)

This folder reproduces the **corpus-level benchmark data contamination** analysis described in  
**“Benchmark Data Contamination in Underrepresented Languages: A Comprehensive Analysis Using Brazilian Data”** (Anonymous submission, 2025).

The method detects contamination between evaluation benchmarks and pretraining corpora using the **50-character substring overlap** strategy originally proposed in the [GPT-4 Technical Report (OpenAI, 2023)](https://arxiv.org/abs/2303.08774).  
Our implementation extends this approach for large-scale datasets integrated into a forked version of the [LLMSanitize (NTU NLP)](https://github.com/ntunlp/LLMSanitize).

-----

## 🎯 Objective

The substring overlap module measures **benchmark data contamination (BCD)** at the **corpus level**, detecting direct textual overlaps between benchmark items and pretraining data.  
It provides a standardized, white-box-compatible method to estimate potential data leakage in **open corpora**.

The forked LLMSanitize version used here integrates an efficient **streaming implementation of the 50-character substring method**, enabling scalable analysis of large datasets on limited hardware.

🔗 **Fork link:** [LLMSanitize-fork](https://anonymous.4open.science/r/LLMSanitize-1D10)

-----

## ⚙️ Method Overview

The procedure follows the GPT-4 contamination protocol with three key steps:

1.  **Benchmark Sampling:**

      * Each benchmark instance is normalized by removing non-alphanumeric characters.
      * Up to three random **50-character substrings** are sampled per instance (or the full text if shorter).

2.  **Corpus Streaming and Parallel Matching:**

      * The corpus is loaded using the Hugging Face `datasets` streaming mode.
      * Data is divided into manageable batches (e.g., 6,000 examples).
      * Multiple worker processes scan each batch independently, checking for exact substring matches between benchmark samples and the corpus.

3.  **Aggregation and Reporting:**

      * The main process aggregates all overlaps across workers.
      * Final metrics are computed and saved automatically.

This streaming architecture avoids building a full substring index, drastically reducing memory and disk usage while maintaining the precision of exact-match detection.

-----

## 🧩 Folder Structure

```

substring-similarity/
├── README.md ← This file
└── results/ ← Raw overlap outputs (JSONL reports)

```

-----

## 🚀 Running the Analysis

To execute the corpus-level contamination check:

1.  **Install and configure LLMSanitize (forked version)**

    ```bash
    git clone [llmsanitize-fork]
    cd llmsanitize
    pip install -r requirements.txt
    ```

2.  **Run the contamination analysis**
    Example command:

    ```bash
    python main.py \
      --train_data_name "Itau-Unibanco/aroeira" \
      --eval_data_name "eduagarcia/oab_exams" \
      --eval_set_key "train" \
      --stream_train_data \
      --text_key "text" \
      --method "gpt-4-stream" \
      --n_eval_data_points 6000 \
      --num_proc 100 \
      --seed 42
    ```

    **Main arguments:**

      * `--train_data_name`: Pretraining corpus on Hugging Face Hub (e.g., `"Itau-Unibanco/aroeira"`).
      * `--eval_data_name`: Benchmark dataset (e.g., `"eduagarcia/oab_exams"`).
      * `--stream_train_data`: Enables memory-efficient streaming mode.
      * `--method "gpt-4-stream"`: Specifies the substring overlap procedure.
      * `--n_eval_data_points`: Number of benchmark samples to test.
      * `--num_proc`: Number of parallel workers.
      * `--seed`: Ensures reproducibility of substring sampling.

3.  **Monitor and recover progress**
    The script periodically saves intermediate progress to `progress_*.json`, allowing safe resumption in case of interruptions.

-----

## 🧠 How It Works

The GPT-4 streaming protocol produces two complementary indicators:

  * **Benchmark Leakage Rate (BLR):**
    The percentage of benchmark items that appear at least once in the corpus.

  * **Contamination Density (CD):**
    The proportion of the corpus content overlapping with benchmark substrings.

-----

## 🧱 Implementation Details

This implementation extends LLMSanitize with:

  * **Streaming support** via `datasets.load_dataset(..., streaming=True)`
  * **Batch-level substring indexing** for controlled memory usage
  * **Parallel execution** using Python’s `multiprocessing.Pool`
  * **Resilience** through automatic checkpointing (`progress_*.json`)
  * **Reproducibility** using seeded substring sampling

### Key source files:

  * `llmsanitize/open_data_methods/gpt4_stream.py` — Main streaming contamination logic
  * `llmsanitize/utils/string_utils.py` — Substring sampling and overlap utilities
  * `llmsanitize/open_data_contamination_checker.py` — High-level method orchestration
  * `main.py` — CLI entry point

-----

## 📊 Results Provided

This directory’s `results/` folder contains the raw JSONL reports from the analysis.

After a successful run, the following output files are generated in the root directory of the tool:

```
contaminated_{BENCHMARK}_vs_{CORPUS}.jsonl  ← Texts of contaminated benchmark items
progress_{BENCHMARK}_vs_{CORPUS}.json       ← Execution state (for resuming)
report_{BENCHMARK}_vs_{CORPUS}.json         ← Summary metrics (BLR, CD)
```

-----

## 📄 Reference

> OpenAI. (2023). *GPT-4 Technical Report.*
> arXiv preprint arXiv:2303.08774.
> [https://arxiv.org/abs/2303.08774](https://arxiv.org/abs/2303.08774)