# RSS→Email Digest POC with n8n

> A simple proof-of-concept that uses n8n to fetch the TechCrunch RSS feed on a schedule, build an HTML digest of the top 5 stories, and email it to you via Gmail SMTP.

---

## Table of Contents

1. [Overview](#overview)  
2. [Features](#features)  
3. [Prerequisites](#prerequisites)  
4. [Installation & Setup](#installation--setup)  
5. [Workflow Import & Configuration](#workflow-import--configuration)  
6. [Usage](#usage)  
7. [Continuous Integration (CI)](#continuous-integration-ci)  
8. [Project Structure](#project-structure)  
9. [Contributing](#contributing)  
10. [License](#license)  

---

## Overview

This repository contains:

- A pre-built n8n workflow (`smtp-test.json`) that:
  1. Triggers on a schedule (e.g. every day at 9 AM).  
  2. Reads the TechCrunch RSS feed.  
  3. Filters and builds an HTML snippet of the top 5 items.  
  4. Sends an email via a Gmail SMTP credential.  
- A GitHub Actions CI workflow to validate that your exported JSON is well-formed.
- A `docs/` folder for any deeper walkthroughs or architecture diagrams.

Use this as a starting point for any RSS-to-email automation.

---

## Features

- **Schedule Trigger**: Cron-style scheduling inside n8n.  
- **RSS Feed Trigger**: Built-in RSS node that detects new items.  
- **Function Node**: Aggregates and formats the feed into HTML.  
- **SMTP Send**: Sends the HTML digest via your Gmail account (using an App Password).  
- **CI**: Validates that all `*.json` workflow files are valid JSON on every PR.

---

## Prerequisites

- **n8n** (v1.101+ recommended)  
- **Node.js** & **npm** (for local n8n installs)  
- A **Gmail** account with 2-Step Verification **enabled**  
- A **Gmail App Password** (create under Google Account → Security → App passwords)  
- **Git** & **GitHub CLI** (optional, for repo management)


# Workflow Import & Configuration

Import the JSON

In n8n’s top-right menu → Import from File…

Select `workflows/smtp-test.json.`

Configure Credentials

Schedule Trigger: No credentials. Set your desired interval (e.g. daily at 9:00).

RSS Read: No credentials. Paste TechCrunch’s RSS URL: https://techcrunch.com/feed/

#### SMTP account:

* Host: smtp.gmail.com
* Port: 465 (SSL)
* User: your full Gmail address
* Password: your Gmail App Password

#### Activate the workflow

Toggle the Active switch in the top bar.

Click **Save.**

## Usage

### Manual Test:

In the editor, click Execute workflow → From Schedule Trigger to force a run.

### Check Logs:

In the n8n UI, go to Executions to inspect inputs/outputs and any errors.

Locally, n8n’s console logs will show HTTP requests and SMTP transactions.

# Continuous Integration (CI)

Use a simple GitHub Actions workflow to ensure every JSON export stays valid:

.github/workflows/validate-json.yml

`name: Validate JSON
on:
  pull_request:
    paths:
      - 'workflows/**/*.json'
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate JSON
        run: |
          for f in workflows/*.json; do
            jq empty "$f"
          done`

# Project Structure

.
├── .github
│   └── workflows
│       └── validate-json.yml   # CI job for JSON validation
├── docs/                       # (optional) deeper architecture or POC docs
├── workflows/
│   └── smtp-test.json          # Exported n8n workflow
├── .gitignore
└── README.md

# Contributing

### Create a feature branch

git checkout -b feature/my-improvement
Make changes, commit, and push to your fork.

Open a Pull Request against main.

Ensure CI passes, add reviewers, and once approved, merge!

# License

This project is licensed under the MIT License. See LICENSE for details.


Paste the above into your **`README.md`**, then:

```bash
# stage, commit & push
git add README.md
git commit -m "docs: add project README"
git push