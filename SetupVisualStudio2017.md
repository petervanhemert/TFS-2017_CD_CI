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
  <img src="/MD_Images/VS2017/Capture%201.PNG" width="600"/> 
</p>

Once you have a local repo, select items in the status bar to quickly navigate between Git tasks in Team Explorer.

<p align="center">
  <img src="/MD_Images/VS2017/VSaddToSourceControl1.PNG" width="600"/> 
</p>
<p>

- <img src="/MD_Images/unpublished_changes.png"> shows the number of unpublished commits in your local branch. Selecting this will open the Sync view in Team Explorer.

  
- <img src="/MD_Images/pending_changes.png"/> shows the number of uncommitted file changes. Selecting this will open the Changes view in Team Explorer.
    
- <img src="/MD_Images/current_repo.png"/> shows the current Git repo. Selecting this will open the Connect view in Team Explorer.
    
- <img src="/MD_Images/branch_picker.png"/> shows your current Git branch. Selecting this displays a branch picker to quickly switch between Git branches or create new branches.
</p>  
