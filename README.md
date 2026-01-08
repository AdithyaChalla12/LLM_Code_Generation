# LLM Fine-Tuning for API Code Generation

This repository contains experiments and training pipelines for **fine-tuning large language models (LLMs) to generate API-correct code from natural-language instructions** using **parameter-efficient fine-tuning (LoRA)**.

The goal of this project is to improve the reliability of API call generation by adapting open-source code models to structured API documentation data.

---

## 📌 Motivation

Although modern LLMs can generate syntactically valid code, they frequently fail at **API-specific correctness**, including:
- Incorrect endpoints or HTTP methods  
- Missing or mismatched parameters  
- Hallucinated API paths  

This project explores whether **LoRA-based fine-tuning** can improve grounding and correctness in API code generation.

---

## 🧠 Task Definition

**Input:**  
A natural-language instruction describing a desired API interaction  
> Example: *"Provide a list of recipes within a specific category"*

**Output:**  
An executable API call snippet that:
- Uses the correct endpoint and HTTP method  
- Includes required parameters, headers, and payload  
- Is syntactically valid and structurally correct  

This is treated as an **instruction-to-code generation task**.

---

## 📂 Dataset

### API Pack Dataset

We use the **API Pack** dataset, which pairs natural-language instructions with grounded API call code.  
Each example includes:
- API name and description  
- Endpoint path and HTTP method  
- Arguments and payload structure  
- Reference API call code snippet  

This structured format enables precise supervision for API-specific code generation.

---
## 🏗️ Methodology

<p align="center">
  <img src="./images/methodology.png" width="800">
  <br>
  <em>End-to-end methodology for LoRA-based fine-tuning of LLMs for API code generation</em>
</p>


### 1. Data Preprocessing
- Normalize whitespace and indentation  
- Remove non-essential formatting artifacts  
- Ensure parsing-friendly code formatting  

**Training format:**
- **Input:** Natural-language instruction  
- **Target:** Ground-truth API call code  

---

### 2. Zero-Shot Baseline
We evaluate unfine-tuned models to establish baseline performance:
- GPT-2  
- StarCoder  
- Qwen-2.5-1.5B  

Result: near-zero exact match and poor API correctness despite occasional syntactic validity.

---

### 3. LoRA Fine-Tuning

We apply **Low-Rank Adaptation (LoRA)** instead of full fine-tuning:
- Base model weights remain frozen  
- Trainable low-rank matrices are added to attention layers  
- Only LoRA parameters are updated during training  

**Benefits:**
- Reduced memory footprint  
- Faster training convergence  
- Enables fine-tuning large models on limited hardware  

---

### 4. Models Fine-Tuned
- StarCoder  
- Qwen-2.5  
- GPT-2  

---

## 📊 Evaluation Metrics

To capture both structural and semantic correctness, we use:

| Metric | Description |
|------|------------|
| Syntax Correctness | Whether generated code parses successfully |
| Exact Match (EM) | Strict string match with reference |
| BLEU / SacreBLEU | N-gram overlap similarity |
| ROUGE-L | Longest common subsequence |
| Perplexity | Model confidence on held-out data |

---

## 📈 Results Summary

Key observations after LoRA fine-tuning:
- Syntax correctness improves from ~20% to **100%**
- SacreBLEU improves from near-zero to ~18–19
- Code-specialized models outperform general LLMs
- Exact endpoint matching remains challenging

**Best performing model:**  
**StarCoder (LoRA fine-tuned)**

---

## 🔍 Key Insights

- High BLEU/ROUGE scores do not guarantee API correctness  
- Models often generate structurally valid but endpoint-incorrect code  
- Performance gains plateau beyond ~100K training samples  
- Exact-match evaluation is extremely brittle for API tasks  

---

## 🚀 Future Work

Potential extensions include:
- Retrieval-Augmented Generation (RAG) with API documentation  
- Constrained decoding using OpenAPI specifications  
- Execution-based evaluation using API sandboxes  
- Scaling to larger models (StarCoder-15B, CodeLlama-34B)  
- Cross-API transfer learning  

---

## 🛠️ Tech Stack

- Python  
- PyTorch  
- Hugging Face Transformers  
- PEFT / LoRA  
- API Pack Dataset  


