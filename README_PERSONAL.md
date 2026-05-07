Today I completed the full Git and GitHub setup required for the AI Engineering From Scratch course. First, I checked whether Git was installed correctly on my Windows system using the command:

git --version

The output showed:

git version 2.49.0.windows.1

which confirmed that Git was already installed and working properly, so no update was needed.

Next, I checked whether Git was configured with my GitHub identity using:

git config --global --list

The output showed:

user.name=SAJID BASHA SHAIK
user.email=tony.sajid.8669@gmail.com

This confirmed that Git already knew my name and email, meaning every future commit I make will be attached to my GitHub identity.

After that, I tested the connection between my computer and GitHub using SSH by running:

ssh -T git@github.com

The first time, GitHub returned:

Permission denied (publickey)

This meant my computer had no SSH authentication key yet, so GitHub did not trust the machine.

To confirm this, I checked the .ssh folder using:

dir %USERPROFILE%\.ssh

The output only showed:

known_hosts

which meant there was no SSH key pair created yet.

I then generated a brand new SSH key pair using:

ssh-keygen -t ed25519 -C "tony.sajid.8669@gmail.com"

During this process I simply pressed Enter for:
- file save location
- passphrase
- passphrase confirmation

Git automatically created two files:

id_ed25519
id_ed25519.pub

The private key (id_ed25519) stays only on my computer and must never be shared. The public key (id_ed25519.pub) is safe to upload to GitHub because it only identifies my machine.

Next, I displayed the public key using:

type %USERPROFILE%\.ssh\id_ed25519.pub

I copied the entire key starting with:

ssh-ed25519

Then I opened GitHub SSH settings at:

https://github.com/settings/keys

and added the public key there under a new SSH key entry. This step connected my computer securely to my GitHub account.

After adding the key, I tested the connection again using:

ssh -T git@github.com

This time the output was:

Hi Sajidbasha-shaik! You've successfully authenticated, but GitHub does not provide shell access.

This confirmed that SSH authentication was fully working and GitHub now trusted my computer. From this point onward, commands like git push, git pull, and git clone can work securely without repeated password login.

After finishing authentication setup, I moved to the Desktop folder using:

cd Desktop

I chose Desktop because I wanted the course repository stored there.

Next, I downloaded the AI Engineering From Scratch repository from GitHub using:

git clone https://github.com/rohitg00/ai-engineering-from-scratch.git

Git downloaded the entire repository including all folders, lessons, commits, and history into a local folder called:

ai-engineering-from-scratch

I then entered the repository using:

cd ai-engineering-from-scratch

and confirmed the project files existed correctly using:

dir

The output showed folders like:
- phases
- projects
- scripts
- outputs
- web

along with README.md and other project files, confirming the repository cloned successfully.

After entering the repository, I created my own personal working branch using:

git checkout -b my-progress

This command did two things at once:
1. created a new branch named my-progress
2. switched me into that branch immediately

This is important because:
- main branch contains the original course repository
- my-progress branch is where I will safely do my own work

Using a separate branch prevents accidental modification of the original codebase.

I verified the active branch using:

git branch

The output showed:

main
* my-progress

The star (*) confirmed that my active branch was my-progress.

Finally, I checked the repository state using:

git status

The output was:

On branch my-progress
nothing to commit, working tree clean

This confirmed:
- the repository is healthy
- there are no modified files
- no pending commits exist
- Git is tracking everything correctly

At this point, my Git environment was fully configured and ready for development work.

After setup was complete, I reviewed the lesson workflow commands:

git status
git add file.py
git commit -m "Add perceptron implementation"
git push origin main

At first I thought these commands should be executed immediately, but that would not make sense yet because the repository was still completely clean. Earlier, git status had already shown:

nothing to commit, working tree clean

This means:
- no files were changed
- no new files existed
- there was nothing for Git to save

Commands like git add and git commit only work after files are created or modified. Since no work had been done yet, there was nothing to stage or commit.

Then I checked the exercise section of the lesson more carefully. The exercise order was:

1. Clone this repo
2. Create a branch called my-progress
3. Make a file
4. Commit it
5. Push it

I had already completed:
- cloning the repository
- creating the my-progress branch

So the next correct step was not git add or git commit yet. The correct next step was to create a file first so Git would have an actual change to track.

This clarified the normal Git workflow sequence:

1. create or modify files
2. check changes with git status
3. stage files using git add
4. save snapshot using git commit
5. upload commits using git push

Without creating or modifying files first, the workflow commands do nothing meaningful.

To begin practicing the real workflow, I created my first file inside the repository using:

echo Hello Git > test.txt

This created a new file named:

test.txt

inside the ai-engineering-from-scratch repository.

After creating the file, I checked repository status again using:

git status

This time Git displayed:

Untracked files:
    test.txt

This was the first important Git state change. It meant Git detected the new file but was not tracking it yet. The file physically existed in the folder, but Git had not included it in version control.

Next, I added the file to Git’s staging area using:

git add test.txt

This command told Git to begin tracking the file and prepare it for the next commit.

After staging the file, I checked status again using:

git status

The output changed to:

Changes to be committed:
    new file: test.txt

This meant the file had moved from the working directory into the staging area. Git was now ready to permanently save the file inside repository history.

At this point I understood the internal Git flow more clearly:

Working Directory -> Staging Area -> Commit History

The file had now moved from:
- Working Directory
to:
- Staging Area

Next, I created my first real Git commit using:

git commit -m "Add test file"

The output showed:

[my-progress 53fc672] Add test file

This contained:
- branch name: my-progress
- commit ID: 53fc672
- commit message: Add test file

Git also displayed:

1 file changed
create mode 100644 test.txt

This confirmed that Git permanently saved the file inside local repository history.

At this point, the file had successfully moved through the full local Git cycle:

Working Directory -> Staging Area -> Commit History

Next, I attempted to upload the commit to GitHub using:

git push origin my-progress

The push failed with:

remote: Invalid username or token.
fatal: Authentication failed

This happened because the repository had originally been cloned using HTTPS, while my authentication setup used SSH. Git was trying to use password/token authentication instead of my SSH key.

To fix this, I changed the repository remote URL from HTTPS to SSH using:

git remote set-url origin git@github.com:rohitg00/ai-engineering-from-scratch.git

I verified the change using:

git remote -v

The output confirmed that both fetch and push URLs now used SSH.

I attempted to push again, but another error occurred:

Permission to rohitg00/ai-engineering-from-scratch.git denied

This error was different. SSH authentication itself was working correctly, but I did not have permission to directly push to the original repository owned by rohitg00.

To solve this properly, I created my own fork of the repository on GitHub. A fork is my personal copy of someone else’s repository hosted under my GitHub account.

After creating the fork, I updated the local repository remote again using:

git remote set-url origin git@github.com:Sajidbasha-shaik/ai-engineering-from-scratch.git

This changed the remote repository from the original owner’s repository to my personal fork.

I verified the remote again using:

git remote -v

The output showed:

git@github.com:Sajidbasha-shaik/ai-engineering-from-scratch.git

for both fetch and push operations.

At this point:
- SSH authentication worked
- my GitHub fork existed
- my local repository pointed to my own fork

I then retried the push using:

git push origin my-progress

This time the push succeeded successfully.

Git displayed:

[new branch] my-progress -> my-progress

This confirmed that:
- my local branch my-progress
- was uploaded to GitHub
- and now existed remotely on my fork

Git also displayed a pull request suggestion link, which is used for collaboration workflows. I did not need pull requests yet because the goal was only to learn the normal Git workflow.

At the end of this process, I had completed the full real-world Git workflow successfully:

create file
-> git status
-> git add
-> git commit
-> git push

I now fully understand:
- local repositories
- Git tracking states
- staging
- commits
- SSH authentication
- GitHub forks
- remotes
- branch workflows
- and pushing code to GitHub safely.

After completing the main Git workflow, I wanted to create documentation files to store my learning notes. I decided to create two separate Markdown files:

README_PERSONAL.md
→ personal learning journey including mistakes and debugging process

README_BEGINNER_GUIDE.md
→ clean beginner-friendly Git notes without mistakes

To create these files quickly from CMD without opening an editor first, I used:

type nul > README_PERSONAL.md
type nul > README_BEGINNER_GUIDE.md

At first, I did not understand why "type nul" was used to create files, so I learned what each part means internally.

Understanding the command:

type
→ Windows command normally used to display file contents

nul
→ special empty device in Windows representing “nothing”

>
→ redirect output into another file

So:

type nul > README.md

literally means:

take nothing
and redirect it into README.md

This creates an empty file instantly.

I also learned that this works for ANY file type because Windows identifies file types mainly through extensions.

Examples:

type nul > app.py
→ creates empty Python file

type nul > index.js
→ creates empty JavaScript file

type nul > notes.txt
→ creates empty text file

type nul > index.html
→ creates empty HTML file

type nul > config.json
→ creates empty JSON file

I then learned the difference between:

type nul > file.txt
→ create empty file

and:

echo Hello > file.txt
→ create file containing text immediately

This clarified that file extensions mainly tell:
- editors how to color and interpret the file
- programs how to process the file
- operating system what kind of file it is

Examples:

.md
→ Markdown documentation

.py
→ Python source code

.js
→ JavaScript code

.html
→ webpage structure

.json
→ structured configuration data

After creating the Markdown files, I verified them using:

dir *.md

The output showed all Markdown files inside the repository including:

README_PERSONAL.md
README_BEGINNER_GUIDE.md

Both files initially showed:

0 bytes

which confirmed:
- files existed successfully
- but they were still empty

This helped me understand an important beginner concept:

Creating a file does NOT mean writing content inside it.

The files existed physically inside the repository, but I still needed a text editor to actually write notes into them.

I then learned that the normal workflow after file creation is:

Create File
        ↓
Open In Editor
        ↓
Write Content
        ↓
Save File
        ↓
Git Detects Changes
        ↓
git add
        ↓
git commit
        ↓
git push

I also learned that Git tracks saved file changes, not typing itself.

Until a file is saved:
- Git sees no modification
- repository state does not change

Only after saving the file does Git detect changes through:

git status

At this stage, the newly created README files were located here internally:

[ Working Directory ]
        ↓
Files Exist
        ↓
Currently Empty
        ↓
Ready To Edit

File Positions:

README_PERSONAL.md
→ Working Directory

README_BEGINNER_GUIDE.md
→ Working Directory

This helped me clearly understand the difference between:
- file creation
- file editing
- file tracking
- and Git version control.