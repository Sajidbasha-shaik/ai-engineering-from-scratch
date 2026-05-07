# Git + GitHub Beginner Notes (Clean Understanding Version)

Git is a system that tracks changes made to files over time. Instead of manually saving multiple copies like:

project-final
project-final-final
project-final-real

Git keeps structured snapshots called commits.

GitHub is the online platform where Git repositories are stored remotely for:
- backup
- collaboration
- synchronization across devices

--------------------------------------------------

# Step 1 — Verify Git Installation

Command:

git --version

Purpose:
Checks whether Git is installed and accessible from the terminal.

--------------------------------------------------

# Step 2 — Configure Git Identity

Commands:

git config --global user.name "Your Name"
git config --global user.email "you@example.com"

Purpose:
Every commit in Git stores:
- author name
- author email

This identifies who created each change.

To verify configuration:

git config --global --list

--------------------------------------------------

# Step 3 — Setup Secure GitHub Authentication (SSH)

GitHub must trust the computer before allowing pushes.

Generate SSH keys:

ssh-keygen -t ed25519 -C "you@example.com"

Git creates:

id_ed25519
id_ed25519.pub

Understanding:

id_ed25519
→ private key
→ stays only on your computer
→ never shared

id_ed25519.pub
→ public key
→ uploaded to GitHub
→ identifies your machine

--------------------------------------------------

# Step 4 — Connect SSH Key To GitHub

Display public key:

type %USERPROFILE%\.ssh\id_ed25519.pub

Copy the full key and add it in:

GitHub
→ Settings
→ SSH and GPG Keys
→ New SSH Key

Purpose:
This registers the computer with GitHub securely.

--------------------------------------------------

# Step 5 — Verify SSH Authentication

Command:

ssh -T git@github.com

Purpose:
Checks whether:
- GitHub recognizes the machine
- SSH authentication works correctly

--------------------------------------------------

# Step 6 — Download Repository

Command:

git clone https://github.com/username/repository.git

Example:

git clone https://github.com/rohitg00/ai-engineering-from-scratch.git

Meaning:
- clone downloads the repository
- includes files
- folders
- branches
- commit history

--------------------------------------------------

# Step 7 — Enter Repository

Command:

cd repository-name

Example:

cd ai-engineering-from-scratch

Purpose:
Git commands only affect the current repository folder.

--------------------------------------------------

# Step 8 — Create Personal Branch

Command:

git checkout -b my-progress

Meaning:
- create branch
- immediately switch into branch

Why branches exist:

main
→ original stable project

my-progress
→ personal workspace

This protects the original code from accidental modifications.

--------------------------------------------------

# Step 9 — Check Repository State

Command:

git status

This is one of the most important Git commands.

It shows:
- current branch
- changed files
- staged files
- untracked files
- repository health

--------------------------------------------------

# Understanding Git Internally

Git mainly works through 4 stages:

1. Working Directory
2. Staging Area
3. Local Repository
4. Remote Repository (GitHub)

--------------------------------------------------

# Stage 1 — Working Directory

Example:

echo Hello Git > test.txt

This creates:

test.txt

At this moment:

- file exists on computer
- Git notices it
- Git is NOT tracking it yet

--------------------------------------------------

# Flow After File Creation

Flow:

[ Working Directory ]
        ↓
File Exists Locally
        ↓
Git Detects File
        ↓
Untracked State

File Position:
test.txt → Working Directory

--------------------------------------------------

# Check Status

Command:

git status

Git now shows:

Untracked files:
    test.txt

Meaning:
- file exists
- Git sees it
- Git is not tracking it yet

--------------------------------------------------

# Stage 2 — Staging Area

Command:

git add test.txt

Purpose:
Moves file into Git's staging area.

The staging area is like:
- preparation zone
- waiting room before saving

--------------------------------------------------

# Flow After git add

Flow:

[ Working Directory ]
        ↓
[ Staging Area ]
        ↓
Ready For Commit

File Position:
test.txt → Staging Area

--------------------------------------------------

# Verify Staging

Command:

git status

Git now shows:

Changes to be committed:
    new file: test.txt

Meaning:
- Git tracks the file now
- file is staged
- commit can save it permanently

--------------------------------------------------

# Stage 3 — Local Repository (Commit History)

Command:

git commit -m "Add test file"

Meaning:
- commit creates permanent snapshot
- Git records current staged changes forever

Commit message explains:
- what changed
- why it changed

--------------------------------------------------

# Flow After Commit

Flow:

[ Working Directory ]
        ↓
[ Staging Area ]
        ↓
[ Local Git History ]

File Position:
test.txt → Saved Inside Local Git Repository

At this point:
- file is permanently tracked locally
- commit has unique ID
- history is preserved

--------------------------------------------------

# Stage 4 — Remote Repository (GitHub)

Command:

git push origin my-progress

Meaning:
- upload commits to GitHub
- synchronize local work online

Understanding:
origin
→ connected GitHub repository

my-progress
→ branch being uploaded

--------------------------------------------------

# Flow After Push

Flow:

[ Working Directory ]
        ↓
[ Staging Area ]
        ↓
[ Local Git History ]
        ↓
[ GitHub Remote Repository ]

File Position:
test.txt → Uploaded To GitHub

At this stage:
- work exists online
- work is backed up
- work can be shared
- work can be recovered later

--------------------------------------------------

# HTTPS vs SSH Problem

Sometimes push fails because:
- repository uses HTTPS
- authentication uses SSH

Fix:

git remote set-url origin git@github.com:USERNAME/repository.git

Purpose:
Changes repository communication from:
- password-based HTTPS
to:
- SSH authentication

--------------------------------------------------

# Fork Understanding

Original repositories often block direct push access.

Solution:
Create a fork.

Fork meaning:
- personal copy of another repository
- hosted under your own GitHub account

Then reconnect local repository to your fork:

git remote set-url origin git@github.com:YourUsername/repository.git

Now pushes go to:
- your GitHub account
- your repository copy

--------------------------------------------------

# Final Beginner Mental Model

Create File
        ↓
Git Detects Change
        ↓
git add
        ↓
File Moves To Staging Area
        ↓
git commit
        ↓
Snapshot Saved Locally
        ↓
git push
        ↓
Uploaded To GitHub

--------------------------------------------------

# Most Important Beginner Commands

git status
→ inspect repository state

git add .
→ stage changes

git commit -m "message"
→ save snapshot locally

git push origin branch-name
→ upload work to GitHub

--------------------------------------------------

# Simple Real-World Analogy

Working Directory
→ desk where work happens

Staging Area
→ packing table before shipment

Commit
→ sealed package with label

GitHub
→ cloud warehouse storing packages safely

--------------------------------------------------

# Most Important Beginner Insight

Git does NOT automatically save files.

Git only saves files after:

git add
AND
git commit

Until then:
- changes are temporary
- files are not part of Git history
- GitHub knows nothing about them