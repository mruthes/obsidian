


These steps cover the basic Git workflow from creating a repository to pushing changes to a remote repository.

![](https://blogger.googleusercontent.com/img/b/R29vZ2xl/AVvXsEh2Bi2SZzdQhOU1nK4frXPuj1hdfYKT0_QkXy2qgGhxEcR3vidCf5mNexv2daRbrBZlFod_LVOfnNyvhfSy29gKZzt0b4BOEqF6qqg1L73GKLDGRFADCN-fIAQxWqc7TiUl0UgR_49yfsughg0gjTwo_aoY6qPSVBSCPHGqn2GC6Jrrc-0Nf_waYIToaBoA/s16000/Picture1.jpg)

  

### 1) Create a New Local Git Repository

To create a new local Git repository, navigate to the desired directory and run `**git init**`:

`git init` 

### 2) Provide User Info to Git Repo (One Time)

You need to configure your username and email for Git, which is usually done once. Replace `**"your name"**` and `**"your email"**` with your actual name and email:

`git config --global user.name` "your name" 

`git config --global user.email` "your email" 

### 3) Adding Files or Folders to Staging

To add files or folders to the staging area, use the `**git add**` command. You can add specific files, all files, or files matching a certain pattern:

`git add -A`                                  # Add all files and folders to staging 

`git add filename`                     # Add a specific file to staging 

`git add *.java`                           # Add all Java files to staging 

`git add foldername`                # Add all files within a folder to staging 

### 4) Commit the Code into Local (Git) Repository

After adding files to the staging area, you commit them to the local repository along with a commit message using `**git commit -m "commit message"**`:

`git commit -m` "commit message" 

### 5) Connect Local Repository with Remote Repository (One Time)

If you haven't already, you need to connect your local repository with a remote repository. This is typically done using the `**git remote add**` command followed by the remote repository URL:

`git remote add origin` "https://github.com/pavanoltraining/opencart.git" 

### 6) Push the Code into Remote Repository

Finally, to push your committed changes from the local repository to the remote repository, use the `**git push**` command followed by the name of the remote repository (often `**origin**`) and the branch you want to push (often `**master**` for the main branch):

`git push origin master` 
  

**Note:** These steps cover the basic Git workflow from creating a repository to pushing changes to a remote repository. Remember to replace `**"your name"**`, `**"your email"**`, and the repository URL with your actual information.



Source: [[SDET-QA]]
Related: [[Git commands]]
Tags: #git 

