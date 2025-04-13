# Automated Writing Evaluation with LLM (Decoder-based)

![cover](https://raw.githubusercontent.com/yYorky/Automated-Writing-Evaluation/refs/heads/main/static/AWE%20Cover.JPG)

This repository showcases the decoder-based component of our final group project for **CS614: Generative AI with Large Language Models** at SMU MITB. The focus of this repo is on the decoder model development and deployment that I personally worked on, which includes:

- Fine-tuning a LLaMA 3.1 8B Instruct model using **Unsloth** and **LoRA**.
- Deploying the fine-tuned model in a **Streamlit web application** for real-time essay evaluation.

> The complete project involved other components such as encoder-based models and prompt-based inference; however, this repository focuses only on the decoder-based contributions.

## 📁 Repository Structure

```
.
├── automated-essay-scoring-unsloth-finetune.ipynb   # Notebook for fine-tuning the LLaMA model
├── automated-essay-scoring-streamlit-unsloth.ipynb  # Notebook for deploying the Streamlit demo
├── README.md                                        # This file
```

---

## 🎯 Project Overview

The project aims to create an **open-source Automated Writing Evaluation (AWE)** system that provides consistent, explainable, and scalable essay scoring using Large Language Models.

Key characteristics of our approach:
- Built on **open-source** models (LLaMA 3.1 Instruct 8B).
- Prioritized **parameter-efficient fine-tuning** via LoRA and **Unsloth** for resource-friendly adaptation.
- Emphasized **real-time usability** through a minimal Streamlit demo.

---

## 🔧 Fine-Tuning the Decoder Model

We fine-tuned the LLaMA 3.1 8B model using **Unsloth**, applying **Low-Rank Adaptation (LoRA)** with different ranks and alpha configurations.

### 🧪 Experiments
| Method               | LoRA Config        | Training Time | QWK   | Accuracy | Soft Accuracy |
|----------------------|--------------------|---------------|--------|----------|----------------|
| Prompt-based (Few-Shot) | N/A            | N/A       | 0.666 | 0.286    | 0.806          |
| Fine-tune (Unsloth) | r=64, α=64, 1 epoch | 4h 7m        | **0.925** | 0.592    | **0.990**      |

- **QWK**: Quadratic Weighted Kappa (our primary metric)
- Fine-tuned models showed a **~39% improvement** over the best prompt-based approach.
- Optimal balance achieved with **r=64**, suggesting the need for more expressive LoRA adapters for essay quality assessment.

### 📉 Training Observations
- Training loss showed temporary spikes but converged well.
- Validation loss remained stable with **no signs of overfitting** after 1 epoch.

### 📚 LoRA Configuration Highlights
- Targeted **all attention modules**, not just `q_proj` and `v_proj`.
- Used learning rate of `2e-4` and applied dropout for regularization.

---

## 🖥️ Streamlit Demo App

![cover](https://raw.githubusercontent.com/yYorky/Automated-Writing-Evaluation/refs/heads/main/static/Demo%20AWE.png)

A minimal **Streamlit web application** was built to deploy and test the fine-tuned model.

### 🔍 Features
- Input your essay in a text area.
- Click "Evaluate Essay" to generate:
  - A holistic score from 1 to 6.
  - A justification explaining the score.

### 🚀 Deployment Details
- Hosted on Kaggle Notebooks with **Tesla P100 GPU (16GB VRAM)**.
- Model inference takes around3 to 5 seconds per essay.

---

## 📊 Evaluation Metrics Used

| Metric         | Description |
|----------------|-------------|
| **QWK**        | Penalizes large misclassifications more heavily. |
| **Spearman's correlation** | Measures ranking consistency between predicted and actual scores. |
| **Accuracy**   | Exact score matches. |
| **Soft Accuracy** | Accepts predictions within ±1 of the true score. |

---

## 📚 Dataset
- **Learning Agency Lab - Automated Essay Scoring 2.0** from [Kaggle](https://www.kaggle.com/competitions/learning-agency-lab-automated-essay-scoring-2)
- Final fine-tuning used a **balanced sample of ~1,000 essays** across all score levels (1-6).

---

## 📌 References
- [LoRA: Low-Rank Adaptation of Large Language Models](https://arxiv.org/abs/2106.09685)
- [Unsloth: Efficient LLaMA Fine-tuning](https://github.com/unslothai/unsloth)
- [Dataset: Learning Agency Lab - AES 2.0](https://www.kaggle.com/competitions/learning-agency-lab-automated-essay-scoring-2)



