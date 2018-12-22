[Home](https://debbiswal.github.io/Tech-BITE/) \| [Back](https://debbiswal.github.io/Tech-BITE/#git)  

## Submodule {DRAFT VERSION}  

What is a Submodule?  
A submodule is an external git repository , which we refer and use in our repository as a sub-repository.  
In simple terms , git submodules as shared libraries or plugins , which we use in our project , by refering them  to reuse the existing code.  

These sub modules exists as an independent repo within our main(parent) repo.  
As submodules are independent repo on its own ,we also can modify and commit changes to these submodules.  

Lets start with an example :  
Say , I have a repository **Customer** and **Product**.  
Also , I have a repo **TextFileLogger** and **DBLogger**.  

Now , I want to reuse the **TextFileLogger** in my **Customer** and  **Product** repo.  
For this , I can copy the contents of **TextFileLogger** and paste it inside my **Customer** and  **Product** repo.  
But , with this approach I will not be able to track the changes made to **TextFileLogger**.  
Every time , I need to check for updates happning on **TextFileLogger** and copy paste them in my **Customer** and  **Product** repo , which is cumbersome.  

So in summary , the problems with this approach are:  

* The original reference is lost. When we copy and paste code, there’s no reference back to the original spot where the code was found, and it’s easily forgotten about.  

* Updates aren’t easily integrated. When changes are made to the original code we copied, it becomes very hard to track what’s changed so we can apply those changes back to our cut and pasted code. Some third party libraries can have thousands of lines of code, spread across hundreds of files, and it’s impossible to keep things synchronized manually.  

* Version information isn’t maintained. Proper software development practices call for versioning releases of our code. We will find this consistent in third party libraries we use in our projects. When we copy and paste code, there’s no easy way to know we are using version 1.0.0 of library **TextFileLogger**  and how will we remember to update our code when version 1.0.1 is released?

The simplest solution , is to refer **TextFileLogger** repo from our repo. And pull/fetch the **TextFileLogger** repo when-ever required.

Now , say there are some changes happend to **TextFileLogger** :  
* change-1 : a new file LOGGING_V1.txt is added with commit id - COMMIT-ID-01
* change-2 : a new file LOGGING_V2.txt is added with commit id - COMMIT-ID-02

We will upgrade the **TextFileLogger** in **Product** repo , to COMMIT-ID-02.  
But **Customer** repo will only be able to use the COMMIT-ID-01 , due to some backward compatibility of other modules inside it.  

This kind of facility makes use of submodules simpler , as we can refer to different commit state in our different projects/repositories.  

![repo](images/img1.png)  

Lets start with creating the required repositories :  

Create TextFileLogger repo with some dummy content :  
```shell
# Create a Github remote repository from CLI
curl -u 'debbiswal' https://api.github.com/user/repos -d "{\"name\":\"TextFileLogger\"}"

#Create the local repo TextFileLogger
mkdir TextFileLogger
cd TextFileLogger
git init
echo "TextLogger_V0" > TextLogger_V0.txt
git add TextLogger_V0.txt
git commit -m "added TextLogger_V0.txt"

#Now link the local repo with remote
git remote add origin https://github.com/debbiswal/TextFileLogger.git

# Check the remote links
git remote -v
Output :
origin	https://github.com/debbiswal/TextFileLogger.git (fetch)
origin	https://github.com/debbiswal/TextFileLogger.git (push)


#Now push the local repo data to remote
git push -u origin master

# For subsequent push , we dont have to use the' -u origin master' argument
echo "TextLogger Updated-1" > TextLogger_V0.txt

git add TextLogger_V0.txt
git commit -m "added TextLogger_V0.txt"
OR
git commit -am "added TextLogger_V0.txt"

git push
```  
In the similar way lets create the below repositories :  
* DBogger  with a fie DBLogger_V0.txt
* Customer with a file Customer_V0.txt
* Product with a file Product_V0.txt

So Now we have all our repositories ready.
Lets use the TextFileLogger repo in Customer and Product repos and submodule.  

### Adding Submodules
Lets add the *TextFileLogger* as submodule to *Customer* repo
```bash
# Get into Customer repo folder
$ cd Customer

# Add the TextFileLogger as a submodule
[Customer]$ git submodule add https://github.com/debbiswal/TextFileLogger.git
```  

Now , if we check the Customer folder .. we can see that TextFileLogger folder is created with all the contents.  
```bash
[Customer]$ ls
Customer_V0.txt  TextFileLogger

[Customer]$ ls TextFileLogger
Logger_V0.txt

# If tree command is not installed , then you have to install it.
# As my ststem is CENTOS , I have used the command 'sudo yum -y install tree'
[Customer]$ tree
.
├── Customer_V0.txt
└── TextFileLogger
    └── Logger_V0.txt

```

Adding submodule , added some settings in our local configuration of *Customer* repo:  
Lets check the *config* file under *.git* folder.  
```bash
[Customer]$ cat .git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = https://github.com/debbiswal/Customer.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[submodule "TextFileLogger"]
	url = https://github.com/debbiswal/TextFileLogger.git
	active = true
```
We can see that a **'[submodule "TextFileLogger"]'** section has been added to .git/config file.  

Now , lets check the status of our *Customer* repo.
```
[Customer]$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   .gitmodules
	new file:   TextFileLogger
```  
We can see that , it also staged two files (.gitmodules , TextFileLogger)  

But what is this **.gitmodules** file ?  
Lets print the contents of *.gitmodules* file from *Customer* repo :  
```
[Customer]$ cat .gitmodules 
[submodule "TextFileLogger"]
	path = TextFileLogger
	url = https://github.com/debbiswal/TextFileLogger.git
```

This is similar to **'[submodule "TextFileLogger"]'** section in .git/config file .  

**So why the duplication ?**  
Its because our local config is local .  

Other contributors won’t see it , so they need a mechanism to get the definitions of all submodules they need to set up in their own repos.  

This is what .gitmodules is for; it will be read later by the **git submodule init** command, as we’ll see in a moment.  



  show output of git status , gif diff --cache , cat .gitmodule , cat .git/configure  
How to clone a repo , issues with submodules while cloning , commands to be used , recursive  
Modifying submodule , commiting mainmodule without commiting submodule  
How to derefer a submodule  
How to refere to different version  
How to switch to a different submodule  
How to remove a submodule 
Parallelized fetching,shallow-submodules(https://dzone.com/articles/the-2016-git-retrospective-submodules)  



### Disadvantages of Submodule  
* We’ve been talking about how susceptible Git Submodules are towards inconsistencies. That seems to be almost the only problem that threatens Git submodules. Listed here are some of the scenarios this issue occur :
  * When one of the collaborators have made changes to the submodule but doesn’t push the submodule repo, other collaborators will not be in sync.
  * When one of the collaborators have made commit changes to the submodules if others don’t use the ‘git submodule update’ command. The repo will have inconsistent commits.
  * If two different branches are being merged, the submodules remain unchanged and submodule update command must be used without fail.
* It’s almost impossible to track the entire project at a time when using submodules. Changes in the submodules aren’t tracked by the main module and must change the current working directory to look at the changes.  
* It’s hard to use IDEs when using Git Submodules and developers are forced to use the shell to maintain consistency.  



Happy Learning :)  

[Home](https://debbiswal.github.io/Tech-BITE/) \| [Back](https://debbiswal.github.io/Tech-BITE/#git)  
