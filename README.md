
# 🗓️ daily-issue-reporter

This GitHub Actions workflow collects **GitHub Issues** from multiple repositories within the last 24 hours and sends a daily summary report to a **Microsoft Teams** channel using a webhook.

---

## 📁 Repository Structure

>daily-issue-reporter/
>
>├── repos.txt
>
>└── .github/workflows/daily-issue-report.yml
---

## 🚀 Features

✅ Collects issues (opened, closed, and still open) from multiple repositories  
✅ Shows issue title, author, and clickable link  
✅ Sends the summary to Microsoft Teams  
✅ Runs daily via cron or manually on-demand

---

## 🔐 Requirements

### 1. GitHub Personal Access Token (PAT)

You need a token so the workflow can access multiple repositories.  
You can use either of the following:

---

### 🔹 **Recommended: Classic PAT Method (Simpler)**

1. Go to [GitHub Settings → Developer Settings → Personal Access Tokens → Tokens (classic)](https://github.com/settings/tokens)
2. Click **"Generate new token (classic)"**
3. Set a **note** and **expiration** (e.g., 90 days or no expiration)
4. Enable these **scopes**:
   - `repo` (full access to public and private repos)
   - *(Optional)* `read:org` if using organization repos with restrictions
5. Click **Generate Token** and **copy it**
6. Store it in your repo secrets:
   - **Name**: `GH_TOKEN`
   - **Value**: *your copied token*

---

### 🔸 Fine-Grained PAT Method (Optional)

1. Go to [GitHub → Settings → Developer Settings → Fine-grained tokens](https://github.com/settings/personal-access-tokens)
2. Click **"Generate new token"**
3. Choose:
   - **Resource Owner** → your GitHub account
   - **Repository Access** → Only select repositories (choose the ones to monitor)
4. Under **Permissions**, enable:
   - `Contents`: Read-only
   - `Metadata`: Read-only
5. Generate and copy the token
6. Store it in your repo secrets as `GH_TOKEN`

---

### 2. Microsoft Teams Webhook

1. Open Microsoft Teams
2. Go to the channel where you want the messages
3. Click `...` → **Connectors**
4. Search for **Incoming Webhook**, click **Configure**
5. Give it a name (e.g., `GitHub Issue Bot`) and upload an icon (optional)
6. Copy the generated **Webhook URL**
7. Store it in your repo secrets as:

- **Name**: `TEAMS_WEBHOOK_URL`

---

## 📄 Configuration Files

### `repos.txt`

This file lists all the GitHub repositories to monitor (one per line):

your-username/repo-1
org-name/repo-2


✅ Can include both **public** and **private** repositories  
✅ Your token must have access to all listed repos

---

## ⚙️ Workflow Details

### `.github/workflows/daily-issue-report.yml`

- Triggers **daily at 9:00 AM IST** (`cron: '30 3 * * *'`)
- Also supports **manual trigger**
- Uses `repos.txt` to read repository list
- Fetches issues opened, closed, and still open
- Sends formatted report to Teams via webhook

---

## ✅ Setup Steps Summary

1. Create your GitHub repo (e.g., `daily-issue-reporter`)
2. Add `repos.txt` with list of repositories
3. Add the workflow at `.github/workflows/daily-issue-report.yml`
4. Generate a PAT (classic or fine-grained)
5. Add secrets:
   - `GH_TOKEN` = your personal access token
   - `TEAMS_WEBHOOK_URL` = your Teams webhook URL
6. Workflow will run **daily** or via **manual trigger**

---

## 📨 Sample Teams Output

📅 GitHub Issue Report — 2025-07-31

📁 Repository: your-username/project-1
🟢 Opened in last 24 hours

#12: Add search bar View (by @user)

🔴 Closed in last 24 hours

#9: Fix footer layout View (by @user)

🟠 Still Open (older than 24h)

#3: Dark mode toggle View (by @user)

📁 Repository: your-username/project-2
🟢 Opened in last 24 hours
None

---

## ❓ Troubleshooting

- **No issues appearing?**
  - Check if your PAT has access to those repos
  - Confirm repo names are correct in `repos.txt`
  - Validate the timestamp is within the UTC last 24 hours

---

## 📦 About

This project automates GitHub issue tracking and reporting to Microsoft Teams using GitHub Actions.
