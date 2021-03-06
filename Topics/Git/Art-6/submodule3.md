[Home](https://debbiswal.github.io/Tech-BITE/) \| [Back](https://debbiswal.github.io/Tech-BITE/#git)  

## Git Submodules - III {draft version}
In my previous article [Git Submodule-II](../Art-4/submodule2.md) , we have discussed the below points :
* Getting an update from Submodule repo
* Whats the difference with *diff*
* Pulling a submodule-using repo

Lets discuss below points in this article :
* Updating a submodule in-place in the container
* Removing a submodule

### Updating a submodule in-place in the container

<pre class="highlight"><code>[Customer/TextFileLogger]$ git log --oneline origin/master
<span style="color:#a58702">7125a5</span> (<span style="color:#ef2929">origin/master, origin/HEAD</span>) added Logger_V2.txt
<span style="color:#a58702">8c29b0f</span> (<span style="color:#04aeae">HEAD</span>) added Logger_V1.txt
<span style="color:#a58702">cf93a5d</span> (<span style="color:#55a705">master</span>) Create Logger_V0.txt</code></pre>

### Removing a submodule

Pending .... =>
Adding submodule from a specific branch or commit
show output of git status , gif diff --cache , cat .gitmodule , cat .git/configure  
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
