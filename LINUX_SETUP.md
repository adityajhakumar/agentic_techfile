

# 📘 `README_LINUX_SETUP.md` — Linux Setup & Environment Guide

---

## 🔹 1. Purpose of This Guide

This document explains how to prepare your **Linux environment** so the AI Agent can scan, fix, and validate Ruby files.

It is written for **electronics engineers**, so no prior computer science knowledge is required.

By the end of this guide, you will know:

* Where files should be stored.
* What folders the agent uses (repo, backups, logs).
* The basic Linux commands you need.
* How to run the agent step by step.

---

## 🔹 2. Folder Structure

Inside your Linux VM, we create a clear workspace under `/opt/ai-agent/`.

```
/opt/ai-agent/
   ├── repo/        → Your project Ruby + text files go here
   ├── backups/     → Automatic safe copies of files before AI changes them
   ├── logs/        → Run history, errors, validation reports
   └── agent/       → Agent scripts (Python or shell scripts)
```

---

## 🔹 3. Preparing the Environment

### Step 1 — Create the folders

Run these commands in your Linux terminal:

```bash
sudo mkdir -p /opt/ai-agent/{repo,backups,logs,agent}
sudo chown -R $USER:$USER /opt/ai-agent/
```

This ensures you have permission to read/write in these folders.

---

### Step 2 — Place your files

Copy your **Ruby (.rb)** and **text files** into the `repo/` folder:

```bash
cp ~/Downloads/material_files/*.rb /opt/ai-agent/repo/
```

Now the agent knows where to look.

---

### Step 3 — Check Ruby is installed

The AI agent works on **Ruby files**, so we need Ruby in your VM:

```bash
ruby -v
```

* If you see something like `ruby 3.0.2`, you’re good.
* If not installed:

```bash
sudo apt update
sudo apt install ruby-full -y
```

---

### Step 4 — (Optional) Install Linter

Linters are tools that check Ruby code quality:

```bash
gem install rubocop
```

---

## 🔹 4. Basic Linux Commands (Essential Only)

Here are the **few commands you need to know**:

| Command                    | Meaning (in simple words)                         |
| -------------------------- | ------------------------------------------------- |
| `ls`                       | Show files in current folder                      |
| `cd folder/`               | Go into a folder                                  |
| `pwd`                      | Show your current location                        |
| `find . -name "*.rb"`      | List all Ruby files in this folder and subfolders |
| `cp file1.rb file1.rb.bak` | Copy (used for backups)                           |
| `ruby -c file1.rb`         | Check if a Ruby file is valid                     |
| `./generate_techfile.sh`   | Run your techfile builder script                  |

👉 **Tip:** You don’t need to memorize them. This README will always show them when needed.

---

## 🔹 5. How the Agent Uses These Folders

1. **Input** → Takes files from `/opt/ai-agent/repo/`.
2. **Backup** → Before modifying, saves a copy in `/opt/ai-agent/backups/`.
3. **Logs** → Every run creates a log in `/opt/ai-agent/logs/`. Example:

   ```
   run-20250902-1130.log
   ```

   This helps engineers see what happened.
4. **Output** → A valid techfile will be saved in `repo/` or `logs/`, depending on the script.

---

## 🔹 6. Running the Agent

Once everything is set up:

```bash
cd /opt/ai-agent/agent
./run_agent.sh
```

What happens step by step:

1. Agent scans `/opt/ai-agent/repo/` for `.rb` files.
2. Splits them into groups of 5–10.
3. Sends to AI model (Qwen/Mistral/DeepSeek).
4. If fixes are needed → AI generates new Ruby file(s).
5. Creates backups → applies changes → validates.
6. Logs everything in `/opt/ai-agent/logs/`.

---

## 🔹 7. Rollback (If Something Goes Wrong)

If a generated file breaks things, you can restore the backup:

```bash
cp /opt/ai-agent/backups/file1.rb.bak /opt/ai-agent/repo/file1.rb
```

---

## 🔹 8. Checklist for Engineers

Before starting, confirm:

* [ ] Ruby is installed (`ruby -v`).
* [ ] Project files are in `/opt/ai-agent/repo/`.
* [ ] You can run `ruby -c somefile.rb` without error.
* [ ] Backups and logs folders exist.

If all are ✅, you’re ready.

---

