Step 1:
Install Git:
````bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git
````


Step 2:
Setup Git:

```bash
git config --global user.name "Your Name"
```

````bash
git config --global user.email "yourname@example.com"
````

https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup
https://www.theodinproject.com/lessons/foundations-setting-up-git

-------------------------------------------------------------------
set your local default git branch to `main`
`git config --global init.defaultBranch main`

Create the Repository:
`git remote -v 
`git status  

`git add hello.txt`  = This command adds your hello.txt file to the staging area in Git. The staging area is part of the two-step process for making a commit in Git. Think of the staging area as a “waiting room” for your changes until you commit them. Now, type `git status` again. In the output, notice that your file is now shown in green, which means that this file is now in the staging area.

`git commit -m` "added hello.text" = indicating your changes have been committed


Don’t worry if you get a message that says “_upstream is gone_”. This is normal and only shows when your cloned repository currently has no branches. It will be resolved once you have followed the rest of the steps in this project.

The message, “_Your branch is ahead of ‘origin/main’ by 1 commit_” just means that you now have newer snapshots than what is on your remote repository. You will be uploading your snapshots further down in this lesson.

Basic commands:
1. `git pull origin main` (Pull the latest changes from the remote "main" branch)
2. `git add <your-changes>` (Stage your local changes)
3. `git commit -m "Your commit message"` (Commit your changes locally)
4. `git push origin main` (Push your local commits to the remote "main" branch)

To set ssh-remote push:`git remote set-url origin git@github.com:bkash775/ODIN_PROJECT.git

