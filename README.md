# org.Roehmapton.Algorithim.thompsonA
/
#!/bin/bash

REPO_NAME="technical-journal"
DESCRIPTION="Personal technical learning journal"
PRIVATE=false   # set to true if you want private repo

# --- Create Repo on GitHub ---
if [ "$PRIVATE" = true ]; then
  gh repo create "$REPO_NAME" --private -y --description "$DESCRIPTION"
else
  gh repo create "$REPO_NAME" --public -y --description "$DESCRIPTION"
fi

# --- Clone Repo ---
git clone "https://github.com/$(gh api user --jq '.login')/$REPO_NAME.git"
cd "$REPO_NAME" || exit

# --- Journal Structure ---
mkdir -p entries templates assets

# --- README ---
cat <<EOF > README.md
# Technical Journal

A living record of what I learn, build, break, fix, and explore.

## Structure
- entries/ — journal entries
- templates/ — reusable writing templates
- assets/ — images, diagrams, notes

## How to Use
Create entries with:
\`\`\`
cp templates/entry-template.md entries/YYYY-MM-DD-topic.md
\`\`\`
EOF

# --- Journal Entry Template ---
cat <<EOF > templates/entry-template.md
# Title

**Date:**  
**Category:**  
**Tags:**  

---

## Problem / Topic

## Notes

## What I Learned

## References
EOF

# --- Example Entry ---
TODAY=$(date +"%Y-%m-%d")
cat <<EOF > "entries/$TODAY-getting-started.md"
# Getting Started Journal Entry

**Date:** $TODAY  
**Category:** Setup  
**Tags:** github, journal

---

## What I did
- Created my technical journal on GitHub

## Thoughts
Excited to document everything I learn.

EOF

# --- Optional: GitHub Pages Setup (Jekyll Minimal Theme) ---
cat <<EOF > _config.yml
title: Technical Journal
theme: minima
EOF

mkdir -p docs
echo "index.html" > docs/.gitkeep

# --- Commit ---
git add .
git commit -m "Initial technical journal setup"
git push -u origin main

echo "🎉 Technical journal created at:"
gh repo view "$REPO_NAME" --web

mkdir technical-journal
cd technical-journal
git init
mkdir entries
touch entries/$(date +"%Y-%m-%d").md
git add .
git commit -m "Initial journal"

├── index.md        (Home)
├── week1.md 
Planning, system setup, architecture
├── week2.md
Security plan + threat model
├── week3.md
Application selection + performance plan
├── week4.md
SSH + firewall + initial security
├── week5.md
Advanced security + scripts
├── week6.md
Performance testing + optimisation
└── week7.md
Security audit + conclusion
