# AI PR Reviewer

**AI PR Reviewer** is a GitHub Action that automatically reviews your Pull Requests using OpenAI’s GPT models. It posts a summary and inline comments with suggestions, helping you maintain clean, safe, and idiomatic code.

---

## ✨ Features

- 📝 Posts **AI-powered summaries** of your PRs  
- 💬 Adds **inline comments** with improvement suggestions  
- 🚫 Flags issues like force unwraps, logic in SwiftUI views, naming issues, typos, unused code, and more  
- ⚡ Supports Swift, iOS projects, and other languages like Python, JS, Java, Kotlin  
- 🔒 Comments only on changed lines

---

## 🚀 Setup

### 1. Add `review.py` to your repo

Place the [`review.py`](ai-review/review.py) in your repository (e.g., in an `ai-review` folder).

### 2. Install dependencies

Update your `requirements.txt`:

```
openai==1.97.1
requests
```
### 3. Create GitHub workflow

```
name: AI Code Review Bot

on:
  pull_request:
    types: [opened, synchronize, reopened]
    
permissions:
  pull-requests: write

jobs:
  ai_review:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r ai-review/requirements.txt

      - name: Run AI Code Review
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          PR_NUMBER: ${{ github.event.pull_request.number }}
          REPO: ${{ github.repository }}
        run: |
          python ai-review/review.py
```
### 4. 🔐 Set Secrets

To enable the GitHub Action, set the following secrets in your repository:

- `OPENAI_API_KEY`: Your OpenAI API key from [OpenAI](https://platform.openai.com/account/api-keys)
- `GITHUB_TOKEN`: Automatically provided by GitHub Actions (no need to create this manually)

> 📌 To add secrets:
> - Go to your GitHub repository
> - Click on **Settings** → **Secrets and variables** → **Actions**
> - Click **New repository secret**
> - Add `OPENAI_API_KEY` with your API key



### 🌟 Why use this?

- ⏳ **Save reviewer time** — Automatically reviews your code with AI  
- 🐛 **Catch hidden bugs** — Spot potential issues before they reach production  
- 🧼 **Enforce consistent code style & practices** — Keep your codebase clean and idiomatic  
- 👨‍💻 **Perfect for solo developers & busy teams** — Get fast, helpful feedback in every PR

