
# 📘 `README_TROUBLESHOOTING.md` — Common Problems and Fixes

---

## 🔹 1. Purpose of This Guide

This document helps electronics engineers quickly **fix common issues** when using the AI Agent for Ruby files and techfiles.

It’s written in **simple, if–else style**, so even non-programmers can follow step by step.

---

## 🔹 2. Common Problems & Solutions

---

### ⚠️ Problem 1: Techfile Build Fails

**Symptoms:**

* The AI generates a techfile, but the build script (`./generate_techfile.sh`) fails.
* Error message may look like:

  ```
  Undefined metal layer: M5
  ```

**Fix:**

1. Check if **AI forgot to add a metal definition**.

   * Look inside `/opt/ai-agent/repo/metal_M5.rb`.
2. If missing → Manually add or ask AI to regenerate.
3. Run validation again:

   ```bash
   ./generate_techfile.sh
   ```

---

### ⚠️ Problem 2: Duplicate Metal Definition

**Symptoms:**

* Two files define the same metal (e.g., `metal_M3.rb` exists twice).
* Build error:

  ```
  Duplicate definition: Metal_M3
  ```

**Fix:**

1. Search duplicates:

   ```bash
   grep -r "Metal_M3" /opt/ai-agent/repo/
   ```
2. Keep the **correct version**, delete the duplicate.
3. Re-run validation.

---

### ⚠️ Problem 3: Ruby Syntax Error

**Symptoms:**

* Validation fails with:

  ```
  syntax error, unexpected end-of-input
  ```

**Fix:**

1. Run:

   ```bash
   ruby -c filename.rb
   ```
2. The output tells the **line number** of the error.
3. Open with editor:

   ```bash
   nano filename.rb
   ```
4. Correct missing `end` or misplaced `do`.
5. Save → Re-run validation.

---

### ⚠️ Problem 4: AI Keeps Failing (3 Retries Used)

**Symptoms:**

* Log shows repeated rollback:

  ```
  [ERROR] Validation failed after 3 retries
  ```

**Fix:**

1. Open the log:

   ```bash
   nano /opt/ai-agent/logs/last_run.log
   ```
2. Check what AI kept failing on.
3. Fix manually (e.g., missing definition).
4. Re-run with agent:

   ```bash
   ./run_agent.sh
   ```

---

### ⚠️ Problem 5: Disk Space Full (Backups Too Many)

**Symptoms:**

* System says:

  ```
  No space left on device
  ```

**Fix:**

1. Clean old backups:

   ```bash
   rm -rf /opt/ai-agent/backups/*
   ```
2. Or archive them to external storage.
3. Re-run the agent.

---

### ⚠️ Problem 6: AI Didn’t Understand Context

**Symptoms:**

* AI produces incomplete definitions.
* Output seems “half correct.”

**Fix:**

1. Ensure **chunking is small (5–10 files max)**.
2. If still wrong, try **fine-tuned model** (see `README_FINE_TUNING.md`).
3. Or manually adjust input files.

---

## 🔹 3. General Debugging Flow (If–Else)

```
If (Techfile build fails) {
   Check for missing or duplicate metals
}
Else if (Ruby syntax error) {
   Run "ruby -c" and fix manually
}
Else if (Validation keeps failing) {
   Read logs and fix manually
}
Else if (Disk full) {
   Clear backups/logs
}
Else {
   Ask AI to regenerate with smaller chunk
}
```

---

## 🔹 4. Emergency Recovery

If all else fails, restore backup:

```bash
cp /opt/ai-agent/backups/filename_timestamp.rb /opt/ai-agent/repo/filename.rb
```

This guarantees system stability.

---

## 🔹 5. Tips for Engineers

* Always **read logs first** → `/opt/ai-agent/logs/`.
* Keep **backups external** if repo is large.
* If AI fails repeatedly → switch to **manual + AI hybrid** (AI suggests, you verify).

---

✅ With this README, engineers now have a **self-service troubleshooting manual**, so they don’t panic when something breaks.

---

Now we’ve completed:

1. `README_MAIN.md`
2. `README_INNER_SETUP.md`
3. `README_AGENT_WORKFLOW.md`
4. `README_VALIDATION_AND_SAFETY.md`
5. `README_AI_MODELS.md`
6. `README_FINE_TUNING.md`
7. `README_TROUBLESHOOTING.md`

---
