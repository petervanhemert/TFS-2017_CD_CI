# Continuous Integration & Continuous Delivery with Visual Studio TeamS ervices 2017, Visual Studio & Azure. 

## Setup Visual Studio 2017.

- Setup VS 2017(add extension).
- Add VS2017 solution "Add to source control" to GIT project in VSTS 2017(select project in Advanced).

### Add extensions.
Tools > Extensions and Updates > (search for "Continuous Delivery Tools for Visual Studio")

<p align="center">
  <img src="/MD_Images/Capture%201.PNG" width="600"/> 
</p>

### Add project to source control.

Open your existing project.

#### Create a local Git repo for your project from a folder containing existing code on your computer.

Create a new local Git repo for your project, after you opend or created your project, by selecting <img src="/MD_Images/addsrccontrol.png" width="150"/> on the status bar in the lower right hand corner of Visual Studio. This will create a new repo in the folder the solution is in and commit your code into that repo.
<p align="center">
  <img src="/MD_Images/VS2017/VSaddToSourceControl1.PNG" width="600"/> 
</p>
Once you have a local repo, select items in the status bar to quickly navigate between Git tasks in Team Explorer.


<p>

- <img src="/MD_Images/unpublished_changes.png"> shows the number of unpublished commits in your local branch. Selecting this will open the Sync view in Team Explorer.

  
- <img src="/MD_Images/pending_changes.png"/> shows the number of uncommitted file changes. Selecting this will open the Changes view in Team Explorer.
    
- <img src="/MD_Images/VS2017/git_repo.PNG"/> shows the current Git repo. Selecting this will open the Connect view in Team Explorer.
    
- <img src="/MD_Images/branch_picker.png"/> shows your current Git branch. Selecting this displays a branch picker to quickly switch between Git branches or create new branches.
</p>  

### Publish your code to Team Services

In the Push view in Team Explorer, select the Publish Git Repo button under Push to Visual Studio Team Services.
<p align="center">
  <img src="/MD_Images/Demo2_2.PNG" width="600"/> 
</p>

Verify your email and select your account in the Team Services Domain drop down.

Enter your repository name and select Publish Repository. 

<p align="center">
  <img src="/MD_Images/VS2017/Demo2_3.PNG" width="600"/> 
</p>

This creates a new Team Project in your account with the same name as the repository. To create the repo in an existing Team Project, click Advanced next to Repository name and select a team project.

<p align="center">
  <img src="/MD_Images/Demo2_4.PNG" width="600"/> 
</p>
Your code is now in a Team Services repo. You can view your code on the web by selecting See it on the web.

### Commit and push updates.

1. As you write your code, your changes are automatically tracked by Visual Studio. You can commit changes to your local Git repository by selecting the pending changes icon ( <img src="/MD_Images/pending_changes.png"/>  ) from the status bar.

2. On the Changes view in Team Explorer, add a message describing your update and commit your changes.

<p align="center">
  <img src="/MD_Images/Demo2_5.png" width="600"/> 
</p>

3. Select the unpublished changes status bar icon ( <img src="/MD_Images/unpublished_changes.png"> ) or the Sync view in Team Explorer. Select Push to update your code in Team Services/TFS.

<p align="center">
  <img src="/MD_Images/vspush.gif" width="600"/> 
</p>
