

# 📘 `README_VALIDATION_AND_SAFETY.md` — Backups, Validation, and Safety Nets

---

## 🔹 1. Purpose of This Guide

This document explains in **simple, step-by-step language** how the AI Agent keeps your files safe while making changes.

Electronics engineers don’t need to worry about “losing data” — the system is designed with **multiple protection layers**:

1. Backups (before any change).
2. Validation (test after change).
3. Rollback (restore if error).
4. Logging (record everything for review).

---

## 🔹 2. The 4 Safety Layers

### **Layer 1 — Backup**

* Before the AI touches any file, it makes a copy.
* Saved inside `/opt/ai-agent/backups/`.
* Example:

  ```
  layer_definitions_20250902-1200.rb
  ```

  → Original file from Sept 2, 12:00.

**Why important?**
Even if something breaks, you can always restore the original.

---

### **Layer 2 — Validation**

After modification, the agent **checks correctness**.

1. **Ruby Syntax Check**

   ```bash
   ruby -c file.rb
   ```

   Ensures Ruby code is not broken.

2. **Optional Style Check (Rubocop)**

   ```bash
   rubocop file.rb
   ```

   Ensures code formatting & consistency.

3. **Techfile Build Check**

   ```bash
   ./generate_techfile.sh
   ```

   Ensures the final **11,000–12,000 line techfile** builds properly.

---

### **Layer 3 — Rollback (Automatic Recovery)**

* If validation fails, the agent **restores from backup automatically**.
* Retry limit = **3 attempts**.

**If still fails after 3 tries** → The system **stops** and writes in logs:

```
[ERROR] Validation failed for chunk 12/24 after 3 retries. Human review needed.
```

---

### **Layer 4 — Logging**

* Every action is written into `/opt/ai-agent/logs/`.
* Engineers can open the log in any text editor (`nano`, `vim`, or `gedit`).

**Example log:**

```
[2025-09-02 14:10] Backup created: backups/layer_definitions_20250902-1410.rb
[2025-09-02 14:11] AI added metal_M4.rb
[2025-09-02 14:12] ruby -c check passed
[2025-09-02 14:12] Techfile build passed
[2025-09-02 14:13] Final result saved
```

---

## 🔹 3. If–Else Logic for Safety

```
If (Backup created) {
   Modify file
   Run validation
   If (Validation passes) {
       Save + Log success
   }
   Else {
       Rollback file
       Retry up to 3 times
       If (Still fails) {
           Stop process
           Mark for human engineer
       }
   }
}
Else {
   Abort process (safety stop)
}
```

---

## 🔹 4. Manual Safety for Engineers

If something goes wrong and you want to **manually restore**:

```bash
cp /opt/ai-agent/backups/layer_definitions_20250902-1200.rb /opt/ai-agent/repo/layer_definitions.rb
```

This puts the file back exactly as it was.

---

## 🔹 5. Checklist for Safety

* [ ] Backup is always created before modification.
* [ ] Validation is always run after modification.
* [ ] Rollback triggers automatically if validation fails.
* [ ] Logs are available for human review.

If all ✅ → Your system is **industry-grade safe**.

---

✅ With this README, an electronics engineer now fully understands **how the system protects their files** and **what to do if something fails**.

---
