

# 📘 `README_AI_MODELS.md` — Understanding AI Models for Code Automation

---

## 🔹 1. Purpose of This Guide

This file introduces the **AI models** we can use in this system.
It explains in **simple language for electronics engineers**:

* What these models are.
* Which ones are best for our task.
* How they connect to Linux.
* When to use **fine-tuning** vs. **prompting**.

---

## 🔹 2. What Is an AI Model?

Think of the AI model as the **“smart assistant”** that reads your Ruby files, finds mistakes or missing parts, and suggests corrections.

It doesn’t replace you — it **augments your work** by checking thousands of lines automatically.

---

## 🔹 3. Types of Models We Can Use

### **1. Qwen3 Coder (480B A35B)**

* Very strong at **coding tasks** (Ruby, Python, C++, Verilog, etc.).
* Can process **very large repositories** (up to 262,144 tokens).
* Good at **agent workflows** (if–else, retry loops).
* Best choice for **direct automation inside Linux**.

👉 We recommend **Qwen Coder** as the **primary model** for this workflow.

---

### **2. Mistral / Mixtral Models**

* Smaller but faster models.
* Good for lightweight checks.
* Useful when:

  * You want to run AI **locally** without cloud access.
  * The repository is small (<100 files).

👉 Best for **offline/local experiments**.

---

### **3. DeepSeek Models**

* Specially optimized for **long-context reasoning** (handling many files together).
* Good for **pattern recognition across large datasets**.
* Can be helpful when generating the **final techfile**.

---

## 🔹 4. How Models Connect in Linux

Inside your Linux environment:

1. **Files live in `/opt/ai-agent/repo/`.**
2. The **Agent script** (Python or Shell) takes chunks of files.
3. These chunks are **sent to the AI model** via:

   * Local API (if model runs on your machine).
   * Remote API (if model runs in cloud).
4. AI responds with corrections.
5. Agent applies corrections → validates → logs.

👉 Engineers don’t have to know networking — just run `./run_agent.sh`.

---

## 🔹 5. Prompting vs. Fine-Tuning

### **Prompting (Default Mode)**

* You give instructions **on the fly** (e.g., “Check if all metals are defined”).
* Model does the work based on the instructions.
* No training needed.
* Best for **daily use**.

---

### **Fine-Tuning (Advanced Mode)**

* You prepare a dataset of **examples of correct techfiles**.
* Train the AI to learn your **style and domain rules**.
* After fine-tuning, the model becomes **specialized for your company’s format**.

👉 Useful if:

* Many custom metal definitions exist.
* Industry standards must be strictly followed.
* You want **faster and more accurate results** without re-explaining rules every time.

---

## 🔹 6. Which Model to Choose?

| Scenario                         | Recommended Model               |
| -------------------------------- | ------------------------------- |
| Large repository (200+ files)    | **Qwen Coder 480B**             |
| Local lightweight use            | **Mistral/Mixtral**             |
| Very long context, deep analysis | **DeepSeek**                    |
| Specialized company rules        | Fine-tuned version of any model |

---

## 🔹 7. Example: Using Qwen Coder

Here’s a **simplified flow** when using Qwen:

```bash
cd /opt/ai-agent/agent
python3 run_agent.py \
  --model qwen/qwen3-coder \
  --input ../repo/ \
  --log ../logs/run-20250902.log
```

The agent script takes care of chunking, backups, retries, etc.
You just see logs and results.

---

## 🔹 8. Safety Reminder

Even with the most powerful AI:

* Always keep **backups**.
* Always validate with `ruby -c` and techfile build.
* AI is a **co-pilot**, not a replacement for engineering checks.

---

✅ With this README, an electronics engineer now understands **what AI model is being used, why, and how**.

---
