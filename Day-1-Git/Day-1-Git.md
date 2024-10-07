### **Day 1: Git Mastery Guide for DevOps Engineers — From Zero to Hero**

---

**Welcome to Day 1 of our comprehensive DevOps/SRE guide!** Today, we’re going to cover Git in-depth, starting from the basics and progressing to advanced scenarios. Each section is structured into three levels—**Beginner**, **Intermediate**, and **Advanced**—to ensure that you gain a solid understanding of every core concept and workflow in Git.

By the end of this session, you’ll have practiced over **15 real-world Git scenarios** and will be confident in using Git for day-to-day tasks as well as complex workflows in a professional setting. Let’s dive into the complete Git mastery experience!

### **Environment Setup for Git Scenarios**

All scenarios will be executed using the **[DevOps-Zero-to-Hero](https://github.com/ruBhutan/DevOps-Zero-to-Hero)** repository hosted on an **AWS EC2 instance**. If you haven’t set up your environment yet, follow these instructions:

1. **Launch an EC2 Instance**:

- **Instance Type**: `t2.medium`

- **AMI**: Amazon Linux 2 or Ubuntu 20.04

- **Security Group**: Open ports `22` (SSH) and `8080` (Jenkins).

2. **Install Git and Other Dependencies**:

```bash

sudo yum update -y

sudo yum install git docker -y

sudo service docker start

sudo usermod -aG docker ec2-user

```

3. **Install Jenkins**:

```bash

sudo yum install java-11-openjdk-devel -y

sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo

sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

sudo yum install jenkins -y

sudo systemctl start jenkins

```

4. **Clone the DevOps-Zero-to-Hero Repository**:

```bash

git clone https://github.com/ruBhutan/DevOps-Zero-to-Hero.git

cd DevOps-Zero-to-Hero

```

## **Configuring Git for the First Time: Windows, Linux, and Mac**

When setting up Git for the first time on your system, configuring global settings and authenticating with a remote repository (e.g., GitHub, GitLab) is crucial. Let’s break down the step-by-step process for different operating systems.

### **Step 1: Install Git**

1. **Windows**:

- Download and install Git from [Git for Windows](https://gitforwindows.org/).

- Open the **Git Bash** application to start using Git commands.

2. **Linux**:

- Ubuntu/Debian:

```bash

sudo apt update

sudo apt install git

```

- CentOS/RHEL:

```bash

sudo yum install git

```

3. **Mac**:

- Use Homebrew:

```bash

brew install git

```

### **Step 2: Configure Git Username and Email**

After installing Git, the first step is to set up your **username** and **email address**, which will be associated with every commit you make.

1. **Set Global Username and Email**:

```bash

git config --global user.name "Your Name"

git config --global user.email "yourname@example.com"

```

2. **Verify Configuration**:

```bash

git config --global --list

```

- This should show:

```

user.name=Your Name

user.email=yourname@example.com

```

3. **Set Up Local Config (For Specific Projects)**:

- Navigate to your project directory:

```bash

cd /path/to/project

```

- Run local configuration:

```bash

git config user.name "Project Specific Name"

git config user.email "project.email@example.com"

```

### **Step 3: GitHub SSH Key Setup for Authentication**

To securely authenticate with GitHub, set up an SSH key. This eliminates the need to enter your GitHub username and password each time you pull or push changes.

1. **Generate a New SSH Key**:

```bash

ssh-keygen -t ed25519 -C "yourname@example.com"

```

- This command will generate a new SSH key pair in the `~/.ssh` directory.

2. **Start the SSH Agent**:

```bash

eval "$(ssh-agent -s)"

```

3. **Add Your SSH Key to the Agent**:

```bash

ssh-add ~/.ssh/id_ed25519

```

4. **Copy the Public Key**:

```bash

cat ~/.ssh/id_ed25519.pub

```

- Copy the output and add it to your GitHub account under **Settings > SSH and GPG Keys**.

5. **Test Your Connection**:

```bash

ssh -T git@github.com

```

- You should see a message like `Hi <username>! You've successfully authenticated.`

### **Step 4: Using HTTPS Instead of SSH (Windows, Linux, and Mac)**

If you prefer to use HTTPS for authentication:

1. **Store Credentials Using Credential Manager**:

```bash

git config --global credential.helper cache

```

2. **Clone a Repository Using HTTPS**:

```bash

git clone https://github.com/yourusername/repo.git

```

- Git will prompt you to enter your GitHub username and password once, and it will cache them for subsequent operations.

---

## **Common Git Configuration Issues and Their Solutions**

### **Issue 1: Git Asks for Username and Password Every Time**

#### **Cause**:

This usually happens if your SSH key is not added to the agent or if HTTPS is configured incorrectly.

#### **Solution**:

1. **For SSH Users**:

- Make sure the SSH key is added to the agent:

```bash

ssh-add ~/.ssh/id_ed25519

```

2. **For HTTPS Users**:

- Use the credential helper:

```bash

git config --global credential.helper cache

```

### **Issue 2: `Permission Denied (publickey)` Error**

#### **Cause**:

Your SSH key is not recognized by GitHub.

#### **Solution**:

1. **Verify Your SSH Key**:

- Check if the SSH key exists:

```bash

ls -al ~/.ssh

```

- If no key exists, generate a new one:

```bash

ssh-keygen -t ed25519 -C "yourname@example.com"

```

2. **Add the SSH Key to GitHub**:

- Copy and add the public key (`id_ed25519.pub`) to **GitHub > Settings > SSH and GPG Keys**.

3. **Test the Connection**:

```bash

ssh -T git@github.com

```

### **Issue 3: `fatal: Authentication Failed` for HTTPS**

#### **Cause**:

This can occur due to incorrect credentials or when GitHub removes password-based authentication in favor of **personal access tokens (PATs)**.

#### **Solution**:

1. **Create a Personal Access Token**:

- Go to [GitHub Settings > Developer Settings > Personal Access Tokens](https://github.com/settings/tokens).

- Generate a new token with the necessary scopes (e.g., `repo`).

2. **Update the Remote URL**:

```bash

git remote set-url origin https://<username>:<PAT>@github.com/username/repo.git

```

3. **Push the Changes Again**:

```bash

git push

```

### **Issue 4: `git config` Not Saving Changes Globally**

#### **Cause**:

Incorrect usage of the `--global` flag or a corrupted `.gitconfig` file.

#### **Solution**:

1. **Verify the `.gitconfig` Location**:

```bash

git config --global --list

```

2. **Manually Edit the `.gitconfig` File**:

- Open the file:

```bash

nano ~/.gitconfig

```

- Manually add the `[user]` section:

```

[user]

name = Your Name

email = yourname@example.com

```

3. **Save and Retry**:

- Save the file and try running a Git command like `git commit` again.

### Now that our environment is ready, let’s get started with our **Beginner-Level** Git scenarios!

---

## **Level 1: Beginner Git Scenarios**

### **Scenario 1: Setting Up a New Git Repository**

1. **Create a new directory and initialize Git**:

```bash

mkdir beginner-git

cd beginner-git

git init

```

2. **Create and Commit a README File**:

```bash

echo "# Beginner Git Project" > README.md

git add README.md

git commit -m "Initial commit with README"

```

3. **Create a Remote Repository and Push Changes**:

```bash

git remote add origin https://github.com/yourusername/beginner-git.git

git push -u origin main

```

### **Scenario 2: Creating and Managing Branches**

1. **Create a New Branch and Switch to It**:

```bash

git checkout -b feature-branch

```

2. **Make Changes and Commit in the New Branch**:

```bash

echo "New Feature" > feature.txt

git add feature.txt

git commit -m "Added a new feature"

```

3. **Switch Back to `main` and Merge**:

```bash

git checkout main

git merge feature-branch

```

4. **Delete the Feature Branch**:

```bash

git branch -d feature-branch

```

### **Scenario 3: Working with the Staging Area**

1. **Create a File and Stage It**:

```bash

echo "Staged File Content" > stage.txt

git add stage.txt

```

2. **Modify the File Without Staging**:

```bash

echo "Unstaged Changes" >> stage.txt

```

3. **Check the Status and View Differences**:

```bash

git status

git diff

```

### **Scenario 4: Conflict Resolution**

1. **Create a Conflict**:

```bash

git checkout -b conflict-branch

echo "Conflicting Line 1" > conflict.txt

git add conflict.txt

git commit -m "Commit in conflict-branch"

git checkout main

echo "Main Branch Line 1" > conflict.txt

git add conflict.txt

git commit -m "Commit in main"

```

2. **Merge and Resolve the Conflict**:

```bash

git merge conflict-branch

```

3. **Manually Resolve the Conflict in `conflict.txt`**:

- Edit the file, stage, and commit:

```bash

git add conflict.txt

git commit -m "Resolved conflict in conflict.txt"

```

### **Scenario 5: Undoing Changes Using `git revert`**

1. **Create an Unwanted Commit**:

```bash

echo "Sensitive Data" >> secrets.txt

git add secrets.txt

git commit -m "Added sensitive data"

```

2. **Revert the Commit**:

```bash

git revert HEAD

```

---

## **Level 2: Intermediate Git Scenarios**

### **Scenario 1: Using `git stash` for Temporary Work**

1. **Make Changes Without Committing**:

```bash

echo "Temporary changes" >> temp.txt

git add temp.txt

```

2. **Stash the Changes**:

```bash

git stash

```

3. **View and Apply the Stash**:

```bash

git stash list

git stash pop

```

### **Scenario 2: Implementing Git Flow Strategy**

1. **Create `develop` and `release` Branches**:

```bash

git checkout -b develop

git checkout -b release/1.0

```

2. **Create a Feature Branch**:

```bash

git checkout -b feature-branch

echo "Feature Content" > feature.txt

git add feature.txt

git commit -m "Added new feature"

```

3. **Merge into `develop` and `release`**:

```bash

git checkout develop

git merge feature-branch

git checkout release/1.0

git merge develop

```

### **Scenario 3: Cherry-Picking Across Branches**

1. **Create a Branch and Make Commits**:

```bash

git checkout -b experimental-branch

echo "Experimental Feature" > experiment.txt

git add experiment.txt

git commit -m "Added experimental feature"

```

2. **Cherry-Pick the Commit into `main`**:

```bash

git checkout main

git cherry-pick <commit-hash>

```

### **Scenario 4: Understanding `git fetch` and `git pull`**

#### **Concepts:**

- **`git fetch`**: Retrieves the latest changes from the remote repository but **does not merge** them into the current branch.

- **`git pull`**: A combination of `git fetch` and `git merge`. It **fetches** and **merges** the changes into your local branch.

#### **Real-Time Example:**

You’re working on a feature branch while your teammate is making changes to the `main` branch. Use `git fetch` to view the latest changes without merging. Use `git pull` if you want to merge those changes into your branch.

#### **Commands:**

1. **Check the Current Status**:

```bash

git status

```

2. **Fetch Changes from Remote**:

```bash

git fetch origin

```

3. **View the Logs to See Fetched Changes**:

```bash

git log origin/main

```

4. **Merge the Fetched Changes**:

```bash

git merge origin/main

```

5. **Using `git pull` Directly**:

```bash

git pull origin main

```

6. **Common Issue**: Merge Conflicts

- **Problem**: Conflicts occur when the same file is modified differently in the local and remote repositories.

- **Solution**:

- View the conflicting files using `git status`.

- Manually edit the conflicts in the file, choosing which changes to keep.

- Stage and commit the resolved files:

```bash

git add <conflicted-file>

git commit -m "Resolved merge conflicts in <file>"

```

### **Scenario 5: Real-Time Scenario with `git fetch` and `git rebase`**

#### **Concepts:**

- Use `git fetch` to see the latest changes and then use `git rebase` to reapply your commits on top of the latest changes, making your branch history linear and clean.

#### **Commands:**

1. **Fetch Changes from the Remote Repository**:

```bash

git fetch origin

```

2. **Rebase Your Feature Branch onto the Latest Main**:

```bash

git checkout feature-branch

git rebase origin/main

```

3. **Resolve Any Conflicts During Rebase**:

- View the conflicting files and edit manually.

- Use `git rebase --continue` to complete the rebase process.

4. **Real-World Issue**: Rebasing in a Shared Repository

- **Problem**: If you rebase a shared branch, it rewrites the commit history, causing confusion for other team members.

- **Solution**: Use `git pull --rebase` instead of `git pull` to incorporate changes without rewriting history.

### **Scenario 6: Handling Fast-Forward Merges**

#### **Concepts:**

- A **fast-forward merge** occurs when the branch being merged has not diverged from the current branch. Git just moves the pointer forward without creating a new commit.

#### **Commands:**

1. **Create and Switch to a New Branch**:

```bash

git checkout -b fast-forward-branch

echo "Fast-forward merge example" >> fast-forward.txt

git add fast-forward.txt

git commit -m "Added fast-forward.txt"

```

2. **Switch Back to `main` and Merge**:

```bash

git checkout main

git merge fast-forward-branch

```

3. **Verify the Merge with `git log`**:

- A new commit for merging should **not** be created in this scenario.

### **Scenario 7: Using `git merge` vs `git rebase`**

#### **Concepts:**

- **`git merge`**: Combines changes from two branches, preserving their histories.

- **`git rebase`**: Re-applies commits from one branch onto another, creating a **linear history**.

#### **Hands-On Example:**

1. **Create Two Separate Feature Branches**:

```bash

git checkout -b featureA

echo "Feature A Content" > featureA.txt

git add featureA.txt

git commit -m "Added featureA.txt"

```

```bash

git checkout -b featureB

echo "Feature B Content" > featureB.txt

git add featureB.txt

git commit -m "Added featureB.txt"

```

2. **Switch Back to `main` and Merge `featureA`**:

```bash

git checkout main

git merge featureA

```

3. **Rebase `featureB` on `main`**:

```bash

git checkout featureB

git rebase main

```

4. **Common Issue**: Conflicts During Rebase

- **Solution**: Use `git rebase --abort` if you want to cancel the rebase and use `git rebase --continue` after resolving conflicts.

### **Scenario 8: Managing Remote Branches**

#### **Concepts:**

- Learn how to track and manage remote branches, delete branches on remote, and handle `origin` references.

#### **Commands:**

1. **List All Remote Branches**:

```bash

git branch -r

```

2. **Create a Remote Tracking Branch**:

```bash

git checkout -b new-feature origin/new-feature

```

3. **Delete a Remote Branch**:

```bash

git push origin --delete old-feature

```

### **Scenario 9: Using `git reset` vs `git revert` vs `git restore`**

Each of these commands serves a different purpose in managing your repository’s history. Let’s break them down:

1. **`git reset`**:

- **Purpose**: Move the HEAD pointer back to a specific commit, effectively rewriting history.

- **Usage**:

```bash

git reset --soft HEAD~1   # Move HEAD, keep changes staged

git reset --hard HEAD~1   # Move HEAD, discard all changes

```

- **Example**: If you accidentally made a commit, `git reset --soft` will uncommit the changes and keep them in the staging area for you to modify.

2. **`git revert`**:

- **Purpose**: Create a new commit that undoes the changes from a specific commit.

- **Usage**:

```bash

git revert <commit-hash>

```

- **Example**: If you committed a bug to production, use `git revert` to create a new commit that removes those changes, preserving history.

3. **`git restore`**:

- **Purpose**: Restore a file or set of files to a previous version.

- **Usage**:

```bash

git restore --source=HEAD~1 README.md

```

- **Example**: Use `git restore` if you want to discard local changes and return to the last committed state.

### **Scenario 10: Understanding and Handling Fast-Forward and No-Fast-Forward Merges**

1. **Fast-Forward Merge**:

- **Concept**: A fast-forward merge moves the branch pointer forward without creating a new commit. This happens when there are no divergent commits.

- **Example**:

```bash

git checkout main

git merge --ff-only feature-branch

```

- **Use Case**: When you want to integrate a feature branch that’s linearly ahead of the current branch.

2. **No-Fast-Forward Merge**:

- **Concept**: Creates a merge commit, preserving the history of both branches.

- **Example**:

```bash

git merge --no-ff feature-branch

```

- **Use Case**: Useful for preserving commit history in long-lived branches like `main` or `production`.

### **Scenario 11: Handling Detached HEAD State**

#### **Issue:**

The **detached HEAD state** occurs when you check out a specific commit rather than a branch, making any changes non-persistent.

#### **Hands-On:**

1. **Switch to a Detached HEAD**:

```bash

git checkout <commit-hash>

```

2. **Make Changes and Realize You’re Not on a Branch**.

3. **Recover by Creating a New Branch**:

```bash

git checkout -b recover-branch

git push origin recover-branch

```

### **Scenario 12: Managing Remote Branches**

1. **List All Remote Branches**:

```bash

git branch -r

```

2. **Create a Remote Tracking Branch**:

```bash

git checkout -b new-feature origin/new-feature

```

3. **Delete a Remote Branch**:

```bash

git push origin --delete old-feature

```

### **Scenario 13: Git Bisect for Finding Bugs**

1. **Start Bisect**:

```bash

git bisect start

git bisect bad

git bisect good <commit-hash>

```

2. **Mark Commits as Good or Bad Until the Issue is Found**:

```bash

git bisect good/bad

```

3. **End Bisect**:

```bash

git bisect reset

```

### **Scenario 14: Using `git reflog` to Recover Lost Commits**

1. **View All Reflog Entries**:

```bash

git reflog

```

2. **Restore a Lost Commit**:

```bash

git checkout <commit-hash>

```

3. **Create a New Branch from the Lost Commit**:

```bash

git checkout -b recovered-branch

```

---

## **Level 3: Advanced Git Scenarios**

### **Scenario 15: Squashing Commits with Interactive Rebase**

1. **Create a Feature Branch with Multiple Commits**:

```bash

git checkout -b squash-feature

echo "Line 1" > squash.txt

git add squash.txt

git commit -m "Added line 1"

echo "Line 2" >> squash.txt

git add squash.txt

git commit -m "Added line 2"

```

2. **Use Interactive Rebase to Squash Commits**:

```bash

git rebase -i HEAD~2

```

- Change `pick` to `squash` for the second commit.

3. **Verify the Squashed Commit**:

```bash

git log --oneline

```

### **Scenario 16: Managing Submodules in a Large Repository**

1. **Add a Submodule**:

```bash

git submodule add https://github.com/example/example-submodule.git

```

2. **Update Submodules**:

```bash

git submodule update --remote

```

3. **Remove a Submodule**:

```bash

git rm --cached example-submodule

```

### **Scenario 17: Using Git Hooks for Automating Pre-Commit Checks**

1. **Create a Pre-Commit Hook**:

```bash

echo "#!/bin/bash" > .git/hooks/pre-commit

echo "echo 'Running pre-commit checks...'" >> .git/hooks/pre-commit

chmod +x .git/hooks/pre-commit

```

2. **Add a Check for Python Linting**:

```bash

echo "pylint *.py" >> .git/hooks/pre-commit

```

3. **Test the Pre-Commit Hook**:

```bash

git add .

git commit -m "Testing pre-commit hook"

```

### **Scenario 18: Configuring Global Git Aliases for Efficiency**

1. **Create Aliases for Common Commands**:

```bash

git config --global alias.co checkout

git config --global alias.br branch

git config --global alias.ci commit

git config --global alias.st status

```

2. **Verify the Configuration**:

```bash

git config --global --list

```

- You should see the aliases listed:

```

alias.co=checkout

alias.br=branch

alias.ci=commit

alias.st=status

```

### **Scenario 19: Using `.gitignore` Effectively**

1. **Create a `.gitignore` File**:

- Add commonly ignored files:

```

*.log

*.tmp

node_modules/

```

2. **Commit the `.gitignore`**:

```bash

git add .gitignore

git commit -m "Added .gitignore file"

```
