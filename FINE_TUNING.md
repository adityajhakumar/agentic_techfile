
# 📘 `README_FINE_TUNING.md` — How to Fine-Tune an AI Model

---

## 🔹 1. Purpose of This Guide

This guide explains in **step-by-step, simple terms** how an electronics engineer (with little computer science background) can fine-tune an open-source AI model like **Mistral**, **Qwen**, or **DeepSeek** to better handle **your company’s techfiles and Ruby definitions**.

---

## 🔹 2. What Is Fine-Tuning?

* Fine-tuning means **teaching the AI model your specific rules**.
* Example: If your company always defines **Metal\_M3** in a certain way, you can train the AI so it always remembers that rule — no need to re-explain every time.

Think of it like:

* **Pretrained AI** = A general student who knows math.
* **Fine-tuned AI** = That same student trained in **your factory’s math style**.

---

## 🔹 3. When to Fine-Tune

✅ Fine-tuning is helpful if:

* You have **hundreds of similar techfiles**.
* You want **consistent output** (same format, no surprises).
* You want AI to be **faster** and need fewer instructions.

❌ Not required if:

* You only need occasional corrections.
* Your repo is small.
* You’re okay with giving rules to the AI each time.

---

## 🔹 4. Steps for Fine-Tuning

### **Step 1 — Collect Data**

* Gather at least **50–200 example techfiles**.
* Each example should contain:

  * Input (Ruby definitions).
  * Output (final correct techfile or section).

👉 Store them in `/opt/ai-agent/datasets/`.

Example:

```
datasets/
  example1_input.rb
  example1_output.tech
  example2_input.rb
  example2_output.tech
```

---

### **Step 2 — Convert to Training Format**

Most AI models need JSONL (JSON Lines) format:

Example `dataset.jsonl`:

```json
{"input": "Ruby code with missing Metal_M3", "output": "Corrected Ruby + Metal_M3 definition"}
{"input": "Ruby code with wrong syntax", "output": "Corrected Ruby syntax"}
```

👉 Tools like Python scripts can help generate this automatically.

---

### **Step 3 — Choose Model for Fine-Tuning**

* If **offline** → Use Mistral/Mixtral (smaller, can run locally).
* If **big company setup** → Use Qwen or DeepSeek (better for large context).

---

### **Step 4 — Start Fine-Tuning**

If using Hugging Face or OpenAI-style interface, the command looks like:

```bash
python3 fine_tune.py \
  --model mistral-7b \
  --dataset datasets/dataset.jsonl \
  --output_dir fine_tuned_model/
```

This process may take **hours to days** depending on data size and hardware (GPUs).

---

### **Step 5 — Test the Fine-Tuned Model**

After training:

```bash
python3 run_agent.py \
  --model fine_tuned_model/ \
  --input ../repo/ \
  --log ../logs/fine_tuned_run.log
```

* Run on a few files first.
* Compare with **backup + validation system**.

---

### **Step 6 — Deploy for Daily Use**

Once you’re satisfied:

* Replace the base model with your fine-tuned version in the agent config.
* Engineers can now run `./run_agent.sh` as usual, but the agent will **already know your company’s rules**.

---

## 🔹 5. Hardware & Resource Needs

* Fine-tuning requires **GPUs** (NVIDIA A100, H100, or RTX 4090).
* If hardware is not available → use **cloud fine-tuning services**.
* Once fine-tuned, the model can often be run **on smaller hardware** for inference.

---

## 🔹 6. Maintenance of Fine-Tuned Model

* Re-train once in a while when **new rules or metals** are added.
* Keep datasets versioned:

  ```
  datasets/v1/
  datasets/v2/
  ```
* Always validate before deploying new fine-tuned versions.

---

## 🔹 7. Quick Checklist

* [ ] Collect enough examples (50–200 minimum).
* [ ] Convert into JSONL format.
* [ ] Select proper model (Mistral/Qwen/DeepSeek).
* [ ] Fine-tune with GPUs or cloud.
* [ ] Test with backup + validation.
* [ ] Deploy safely into agent workflow.

---

✅ With this README, an electronics engineer can now **understand the entire lifecycle of fine-tuning**: dataset preparation → training → testing → safe deployment.

---
