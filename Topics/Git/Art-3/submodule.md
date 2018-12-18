[:house:Home](https://github.com/debbiswal/Articles) | [Back](https://github.com/debbiswal/Articles/blob/master/README.md#git)

## Submodule {DRAFT VERSION}  

What is a Submodule?  
A submodule is an external git repository , which we refer and use in our repository as a sub-repository.  
In simple terms , git submodules as shared libraries or plugins , which we use in our project , by refering them  to reuse the existing code.  

These sub modules exists as an independent repo within our main(parent) repo.  
As submodules are independent repo on its own ,we also can modify and commit changes to this submodules.  

Lets start with an example :  
Say , I have a repository **Customer** and **Product**.  
Also , I have a repo **TextFileLogger** and **DBLogger**.  

Now , I want to reuse the **TextFileLogger** in my **Customer** and  **Product** repo.  
For this , I can copy the contents of **TextFileLogger** and paste it inside my repo.  
But , with this approach I will not be able to track the changes made to **TextFileLogger**.  
Evry time , I need to check for updates happning on **TextFileLogger** and copy paste them in my repo , which is cumbersome.  

The simplest solution , is to refer **TextFileLogger** repo from our repo. And pull/fetch the **TextFileLogger** repo when-ever required.




Explain with diagram  , how two different repo can refer to different version of submodule  
How to add a submodule to existing repo   
  show output of git status , gif diff --cache , cat .gitmodule , cat .git/configure  
How to clone a repo , issues with submodules while cloning , commands to be used , recursive  
Modifying submodule , commiting mainmodule without commiting submodule  
How to derefer a submodule  
How to refere to different version  
How to switch to a different submodule  
How to remove a submodule  


### Disadvantages of Submodule  
* We’ve been talking about how susceptible Git Submodules are towards inconsistencies. That seems to be almost the only problem that threatens Git submodules. Listed here are some of the scenarios this issue occur :
  * When one of the collaborators have made changes to the submodule but doesn’t push the submodule repo, other collaborators will not be in sync.
  * When one of the collaborators have made commit changes to the submodules if others don’t use the ‘git submodule update’ command. The repo will have inconsistent commits.
  * If two different branches are being merged, the submodules remain unchanged and submodule update command must be used without fail.
* It’s almost impossible to track the entire project at a time when using submodules. Changes in the submodules aren’t tracked by the main module and must change the current working directory to look at the changes.  
* It’s hard to use IDEs when using Git Submodules and developers are forced to use the shell to maintain consistency.  



Happy Learning :smiley:  

[:house:Home](https://github.com/debbiswal/Articles) | [Back](https://github.com/debbiswal/Articles/blob/master/README.md#git)
