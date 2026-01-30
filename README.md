# 🎨 Thinking with Comics

**Enhancing LLM Generative Capabilities through Structured Visual Storytelling**

[![Paper](https://img.shields.io/badge/Paper-ICML%202026-blue)](https://arxiv.org/abs/xxxx.xxxxx)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

<p align="center">
  <img src="figures/overview.png" alt="Thinking with Comics Overview" width="800">
</p>

## 📖 Overview

**Thinking with Comics (TwC)** is a novel reasoning paradigm that leverages sequential comic panels to enhance the reasoning capabilities of Vision-Language Models (VLMs). Unlike traditional "Thinking with Text" (Chain-of-Thought) or "Thinking with Video" approaches, our method uses comics as an intermediate reasoning medium that:

- 🎯 **Preserves temporal logic** through panel sequences
- 📝 **Embeds text naturally** via speech bubbles and narration
- 💡 **Balances information density** between static images and videos
- 🔍 **Provides interpretable reasoning** through visual storytelling

### Key Features

- **Two Implementation Paths:**
  - **Path I (End-to-End):** Direct comic generation as the reasoning process
  - **Path II (VLM-Assisted):** Comic as conditioning context for downstream inference

- **Multiple Task Support:**
  - Mathematical reasoning (GSM8K, MATH-500, MathVista)
  - Document understanding (DocVQA)
  - Cultural knowledge (CulturalBench)
  - Comic translation

## 🏗️ Project Structure

```
ThinkWithComics-OpenSource/
├── code/
│   ├── config.py                    # Configuration (API, paths, parameters)
│   ├── utils.py                     # Utility functions
│   ├── evaluate_math_comics.py      # GSM8K & MATH-500 evaluation
│   ├── evaluate_mathvista_comics.py # MathVista evaluation
│   ├── evaluate_docvqa_comics.py    # DocVQA evaluation
│   ├── evaluate_cultural_comics.py  # CulturalBench evaluation
│   ├── compare_global_incremental.py # Global vs Incremental experiment
│   └── analyze_human_evaluation.py  # Human evaluation analysis
├── data/
│   └── sample_datasets/             # Place your datasets here
├── examples/
│   ├── gsm8k/                       # Example outputs for GSM8K
│   ├── mathvista/                   # Example outputs for MathVista
│   ├── docvqa/                      # Example outputs for DocVQA
│   ├── culturalbench/               # Example outputs for CulturalBench
│   └── translation/                 # Example comic translations
├── figures/                         # Paper figures
├── results/                         # Evaluation results (auto-generated)
├── requirements.txt                 # Python dependencies
└── README.md                        # This file
```

## 🚀 Quick Start

### 1. Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/ThinkWithComics.git
cd ThinkWithComics

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt
```

### 2. Configuration

Edit `code/config.py` to set your API credentials and paths:

```python
# API Configuration
API_HOST = "your-api-host"
API_KEY = "your-api-key"  # Or use environment variable: os.environ.get("COMIC_API_KEY")

# Dataset paths
BASE_DIR = "/path/to/your/project"
```

**Environment Variable (Recommended):**
```bash
export COMIC_API_KEY="your-api-key"
```

### 3. Prepare Datasets

Download and place datasets in the `data/` directory:

| Dataset | Download Link | Path |
|---------|--------------|------|
| GSM8K | [HuggingFace](https://huggingface.co/datasets/gsm8k) | `data/gsm8k/test.jsonl` |
| MATH-500 | [GitHub](https://github.com/hendrycks/math) | `data/MATH-500/test.jsonl` |
| MathVista | [GitHub](https://github.com/lupantech/MathVista) | `data/MathVista/` |
| DocVQA | [HuggingFace](https://huggingface.co/datasets/lmms-lab/DocVQA) | `data/DocVQA/` |
| CulturalBench | [HuggingFace](https://huggingface.co/datasets/CultureBench/CulturalBench) | `data/CulturalBench/` |

### 4. Run Evaluation

```bash
cd code

# Math reasoning evaluation (GSM8K & MATH-500)
python evaluate_math_comics.py

# Visual math reasoning (MathVista)
python evaluate_mathvista_comics.py

# Document QA (DocVQA)
python evaluate_docvqa_comics.py

# Cultural knowledge (CulturalBench)
python evaluate_cultural_comics.py
```

## 📊 Experiments

### Main Evaluation

Run evaluations on different benchmarks:

```bash
# Adjust NUM_SAMPLES in config.py to control sample size
python evaluate_math_comics.py
python evaluate_mathvista_comics.py
```

### Global vs Incremental Comparison

Compare global comic generation with incremental image generation:

```bash
python compare_global_incremental.py
```

This generates:
- Global comic images (single cohesive multi-panel comic)
- Incremental images (step-by-step realistic photos)
- Human evaluation template

### Human Evaluation

After collecting human ratings:

```bash
python analyze_human_evaluation.py
```

This produces:
- Statistical analysis (mean, std, t-test)
- LaTeX table for paper

## 🎭 Comic Styles

The framework supports multiple narrative styles for different tasks:

| Style | Best For | Description |
|-------|----------|-------------|
| **Documentary** | Factual problems | Educational, realistic presentation |
| **Detective** | Logical reasoning | Clue-based, deductive structure |
| **Slice-of-Life** | Word problems | Everyday scenarios, relatable |

Configure style in the evaluation scripts:

```python
# In evaluate_math_comics.py
results = evaluate_dataset(
    dataset_name="gsm8k",
    data_path=GSM8K_PATH,
    question_key="question",
    style="detective"  # Options: default, detective, documentary, slice_of_life
)
```

## 📈 Results

### Reasoning Benchmarks

| Method | MATH-500 | GSM8K | MathVista |
|--------|----------|-------|-----------|
| GPT-5-High | 99.0 | 100.0 | 67.5 |
| TwC (Ours) - Path I | 90.0 | 100.0 | 75.0 |
| TwC (Ours) - Path II | 92.3 | 95.4 | **85.8** |

### Context Understanding

| Method | DocVQA | CulturalBench (E/H) |
|--------|--------|---------------------|
| TwC (Ours) - Path I | 92.8 | 70.0 / 80.5 |
| TwC (Ours) - Path II | **99.4** | **88.3** / **82.2** |

## 🔬 Analysis Experiments

### 1. Role-playing Narrative Alignment

Different comic styles affect performance:

```
Style           | MathVista | GSM8K  | Avg. Δ
----------------|-----------|--------|--------
Documentary     | 60.0      | 68.0   | ---
Slice-of-Life   | 80.0      | 86.3   | +38.3%
Detective       | 85.0      | 100.0  | +18.7%
```

### 2. Panel Scaling

Optimal panel count is 4-6 for best accuracy/cost trade-off.

### 3. Gutter Reasoning

Panel shuffling causes >40% accuracy drop, confirming models use temporal logic between panels.

## 📝 Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{author2026thinking,
  title={Thinking with Comics: Enhancing LLM Generative Capabilities through Structured Visual Storytelling},
  author={Author1, Firstname1 and Author2, Firstname2 and others},
  booktitle={Proceedings of the International Conference on Machine Learning (ICML)},
  year={2026}
}
```

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- Image generation powered by [Nano Banana Pro](https://example.com)
- Datasets: GSM8K, MATH, MathVista, DocVQA, CulturalBench, eBDtheque

## 📧 Contact

For questions or feedback, please open an issue or contact:
- Email: author@university.edu

---

<p align="center">
  <b>🎨 Think with Comics, Reason with Stories! 📖</b>
</p>
