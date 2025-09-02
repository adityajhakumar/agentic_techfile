

# 📘 `README_AGENT_WORKFLOW.md` — How the AI Agent Works Step by Step

---

## 🔹 1. Purpose of This Guide

This document explains in **simple language** how the AI Agent actually works behind the scenes.
It is written for **electronics engineers** with little or no computer science background.

Think of this as a **“user manual of the brain of the system.”**

---

## 🔹 2. Big Picture Flow

The agent always follows the same loop:

```
Scan → Chunk → Send to AI → Backup → Modify → Validate → Save or Rollback
```

---

## 🔹 3. Step-by-Step Workflow

### **Step 1 — Scan Repository**

* The agent looks inside `/opt/ai-agent/repo/`.
* Collects all files ending in `.rb` (Ruby files).
* Example:

  ```
  material1.rb
  material2.rb
  layer_definitions.rb
  ```

---

### **Step 2 — Chunking (Divide into Groups)**

* To avoid overloading the AI, files are processed in groups of **5–10 at a time**.
* Example: If there are **200 files**, the agent makes **20 groups**.

---

### **Step 3 — AI Analysis**

* Each group is sent to the AI model (Qwen, Mistral, DeepSeek).
* The AI checks:

  * Are all metal definitions present?
  * Any duplicate or conflicting definitions?
  * Any syntax problems?

👉 If everything looks fine → AI skips modification.
👉 If something is missing → AI prepares the missing definition(s).

---

### **Step 4 — Backup Before Modifying**

* Before touching your files, the agent **saves a copy** into `/opt/ai-agent/backups/`.
* Example:

  ```
  backups/layer_definitions_20250902-1200.rb
  ```

This ensures you can always go back if needed.

---

### **Step 5 — Modify Repository**

* The AI generates the corrected Ruby code.
* The agent places this new code into the right file inside `/opt/ai-agent/repo/`.
* Example: If `metal_M3` was missing, a new `metal_M3.rb` file will appear.

---

### **Step 6 — Validation**

After modifications, the agent **tests** everything:

1. **Ruby Syntax Check**

   ```bash
   ruby -c file.rb
   ```

   → Ensures the Ruby code is valid.

2. **Optional Code Style Check** (Rubocop).

3. **Techfile Build**

   ```bash
   ./generate_techfile.sh
   ```

   → Ensures final 11,000–12,000 line techfile builds successfully.

---

### **Step 7 — If-Else Decision Making**

This is the heart of the agent:

* **If validation passes** → Save the updated file + log success.
* **If validation fails** → Rollback from backup, retry with AI (max 3 times).
* **If still fails after 3 retries** → Mark error in logs + human engineer must check manually.

---

## 🔹 4. Flowchart (ASCII Diagram)

```
 ┌─────────────┐
 │   Start     │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │  Scan repo  │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │  Chunk 5-10 │
 │   files     │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │ Send to AI  │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │  Backup old │
 │   version   │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │   Modify    │
 │   repo file │
 └─────┬───────┘
       │
       ▼
 ┌─────────────┐
 │ Validate?   │
 └─────┬───────┘
   Yes │   │ No
       ▼   ▼
 ┌─────────────┐     ┌─────────────┐
 │ Save result │     │ Rollback +  │
 │   + Log     │     │ Retry (max3)│
 └─────┬───────┘     └─────┬───────┘
       │                   │
       ▼                   ▼
 ┌─────────────┐     ┌─────────────┐
 │   Success   │     │ Human Check │
 └─────────────┘     └─────────────┘
```

---

## 🔹 5. Logs & Reports

Every run is logged inside `/opt/ai-agent/logs/`.

Example log:

```
[2025-09-02 12:00] Scanned 240 files
[2025-09-02 12:01] Chunk 1/24 → Missing metal_M3
[2025-09-02 12:02] Added file: metal_M3.rb
[2025-09-02 12:03] Validation passed
[2025-09-02 12:03] Success
```

This helps you **track exactly what the agent did**.

---

## 🔹 6. Safety Measures

* Nothing is overwritten without backup.
* Rollback happens automatically if things fail.
* Human engineers are always the final authority in case of repeated failures.

---

✅ With this README, an electronics engineer can now understand **how the AI agent “thinks” step by step** and how it protects their data.

---
