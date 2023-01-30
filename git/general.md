# Git

## Installation

## GUI

From Bash run `git-gui`. **Note** Does not work currently?


## Bash

Copy: ctrl + insert
Paste: shift + insert

Check local config:
```bash
pwd # current folder content
~ # home folder
    ls # items in current folder (options -a to display hidden, -l to display last update)
cat ~/.gitconfig # check local config
mkdir # create folder (option -p to create intermediate unexisting folders)
mv old new # rename. This is a delete old and create new in Git's eyes cont.
# run git add -A afterwards to make Git know it was a rename or just use git mv old new 
rm #remove (options -rf to recursively force deletion of child items)
```

## SSH authentication

1. Check for existing ssh keys. Usually stored in home directory. From Bash run:

```console
ls ~/.ssh
```

You are looking for private and public key pairs named `id_rsa` (.pub is the public key). If you do not have a key pair then continue to the next step.

2. Create a new key-pair. From Bash run:

```console
ssh-keygen -o
```

Note: It is recommended to save them in the default location (continue by pressing enter).
Note: password is not neccessary
Note: -o option saves the private key in a format that is more resistant to brute-force password cracking than is the default format.

3. Use/send your public key to create a connection.

You can check your public key by running this from bash:

```console
cat ~/.ssh/id_rsa.pub
```

On Windows you can copy to clipboard by running:

```console
cat ~/.ssh/id_rsa.pub > /dev/clipboard
```

Connect to GitHub:

1.  Click on profile. Go to Settings
2.  Click SSH and GPG keys on the left navigation bar
3.  Click New SSH key
4.  Name the key (title, use something to identify the pc with the key), set type to Authentication key and paste the copied key into the Key box.
5.  Test the connection:

```console
ssh -T git@github.com
ssh -v -T git@github.com
```


## Setting up our configuration file(s)

Once the installation is completed and we have connected to GitHub we should set some settings such as your name and email. We will use the `--global` option to set our information so that it will be used in all of our local repositories.

```
git config --global user.name "FirstName LastName"
git config --global user.email "email address"
git config --global init.defaultbranch main
```

Our configuration is stored in `.gitconfig` files. There are three levels of configuration:

- system (in the Git install folder)
- global (in your home folder)
- local (in a Git repository)

We usually have 1 system and global .gitconfig and multiple local configuration files. We can see the content and location of our files by running:
```Bash
git config --list --show-origin
```

And the specific configuration can be found by specifying the level:

```
git config --list --global
```

Note that there is a hierarchy: if you have different configurations of the same thing, the one in the lowest level will be applied. So if you have specified x as your name in the global config file and y in your local repository, then y will be  used as your name in that repository.

## Ignoring patterns

You can and you should define which files should not be included in your repository. Usually you do not want to include data, outputs and other unnecessary files. This can be done by creating a .gitignore file and specifying the files or patterns to exlude, one per line.

Some examples:
```bash
logs/ # Ignore everything in the logs folder
*.ext # Ignore all .ext files
```

You can also set system wide ignore patterns that will be applied to all local repositories:
```bash
git config core.excludesfile [file]
```

## Creating aliases

We can create aliases for Git commands. This allows us to simplify complex commands into a single word.
We add aliases to the global configuration file:
```
git config --global alias.hist "git command"
```

We can create a new alias to show the commit history of the current branch right away. We will explore it later.
`git config --global alias.hist "log --oneline --graph --decorate --all"`

## Creating a repository

We can either initialize a Git repository in an existing directory by moving into the directory and running 
```bash
git init 
```

After initializing the local repository we can connect it to a remote repository (usually aliased `origin`) with:
```bash
git remote add [alias] [url]
```

We can then check the remote location of the repository:
```
git remote get-url [alias]
```

Or we can retrieve an existing repository from a remote location (e.g. GitHub) by cloning it:

```bash
git clone [url]
```

We can then connect our local repository with the previously setup GitHub repository.

## How the repository works

When a new file is created in a (sub)directory of a git repository, the status of the file is 'untracked', meaning it exists in the foler but it's not tracked by Git.
We can track (and stage) the file by running 
```
git add [file]
```

Now the repository started tracking the file and also staged the file. The file is still not included in the repository. We first have to commit the file:
```
git commit -m "message with information about commit"
```

The file is now also stored in the local git repository. This means that there are essentially two copies of the file. If we make some changes to the file then the file in the git repository remains unchanged until we again stage the file with `git add [file]` and commit `git commit -m "second commit"`. This means that the file can be in different states. For tracked files we can shorten this process by running a single command:
```
git commit -am "all in one"
```

We could have skipped the `-m` option in both of the two cases. In that case the default Git editor would open for you to write a message about changes.


We usually have a third, remote repository, which can hold yet another state of the directory.
We send a copy of the local repository (aliased as main) to the remote repository (aliased as origin) by running

```
git push origin main
```

We can check the status of the repository with
```
git status
```

## Adding files to the repository

I will show how to make changes by example. Let's first create a README file, which is a standard piece of a Git repository. You can create it manually or running 

```
echo "# Testing Git" >> README.md
```

If we then check the status with `git status`, we see that the file is shown in the Untracked files section. 
It is part of the working directory but not part of the Git repository (or even tracked by Git).

We can start tracking the file with the following command:
```
git add README.md 
```

Checking the status shows that there are new ready to commit changed in the working directory. The README.md file is now tracked and staged.
We can see the list of the tracked files with
```
git ls-files
```

If we wanted to unstage the file prior to commiting we can do so with 

```
git rm --cached README.md 
```

We will continue and commit the changes to the git repository:

```
git commit -m "Message about commit"
```

The README.md file is now part of our local git repository.

If we wish to remove a file only from the git repository we can do so with
```
git rm --cached README.md 
```

If we instead want to remove the file altogether we replace the --cached option with -f.


### HEAD - TO DO
https://stackoverflow.com/questions/2304087/what-is-head-in-git

A head is simply a reference to a commit object. Each head has a name (branch name or tag name, etc). By default, there is a head in every repository called master. A repository can contain any number of heads. At any given time, one head is selected as the “current head.” This head is aliased to HEAD, always in capitals".

Note this difference: a “head” (lowercase) refers to any one of the named heads in the repository; “HEAD” (uppercase) refers exclusively to the currently active head. This distinction is used frequently in Git documentation.

The usual case is that the HEAD follows you and depends on the current branch. You can see the current HEAD with `cat .git/HEAD`.
If the result of that shows a SHA-1 checksum it is considered that the HEAD is detached and does not follow you (the current branch).




## Moving forward - Making, inspecting, applying changes

It is important to note that the "same" file can be saved in different states at the same time. In fact there can be up to 4 "copies" or states of the same file:

- in the working directory
- in the staged area
- in the local repository
- in the remote repository

Changes done in the working directory are not automatically applied to the other versions of the file. We have to follow the same procedure as we have for adding a new file to the repository.

![Git stages and commands](git_stages.png) 



### Inspecting and comparing changes 


We have already created an alias (a shortcut) to see the history of the repository. We have used the `log` command, which in it's basic form returns the history of the repository in reverse chronological order. For each commit we see the SHA-1 checksum, the author, date and message. 
To navigate down the output press ENTER. The end of the output will show END in the command line. You can press `q` to exit the log interface at any point.

The history usually is not enough. We also want to see the changes that were introduced with each commit. This is usually done with `git diff`. There are multiple uses of the diff command which we will mention briefly in the following subsections.

History can be also seen with the log command, if the add the -p option, which will show the history and the changes for all commits or just for a selected number of commits by adding -[number], as shown below:

```
git log -p -1 # (-p or --patch)
```

Another useful use case is to see the history of a specified file. The history of commits pertaining (that introduced changes) to a certain file can be seen with:

```
git log [file]  # add -p before [file] to also show the differences 
```

Note that if we have at any point changed the name of the file, we will not see the commits for changes with the previous name. We can however add the `--follow` option, which will show the commits that changed the file even across renames.



There are many other options that can be passed to the command, for example:
- we can limit the timeframe of the history by adding --since=2days ago, --before=[date], --after=[date],...
- we can show just the statistics of the changes to the file with --stat (how many additions, deletions,...)

Further options can be explored in [commit history](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).


#### Differences between two commits
```
git diff commit1 commit2
```

where commit1 and commit2 are SHA-1 checksums or abbreviated SHA (as printed with our alias). If there are differnces between these two commits, the command returns what's new in commit2 (+, in green) and what is removed with commit2 (-, in red).

#### Differences between local workspace and local repository

We can see the differences by runing `git diff` without passing any other arguments. For each file with changes, this command will output the differences between the local file and the file stored in the local repository. We can focus on a single file by adding the file name: `git diff [file]`.
Note that this will not show differences between staged files and files in the local repository.

#### Differences between branches

The `diff` command is also used to compare branches, which we will explore in the branching section.





### Postopning changes - TO DO 

Check [git stash](https://git-scm.com/docs/git-stash) 

The idea of this section will again be presented by example:
Suppose you get an urgent task in a project whilst already doing some work in the same project. 
You have already made changes (possibly also staged/indexed some of them) to the files but are not yet finished and have not done any testing. 
You can easily clean the dirty working directory and save/record the current state of the working directory and the index with
```
git stash # same as git stash push
```

Notes:
- You can also stash only certain files by listing them after the command. Note that in this case you will have to add the push option.
- untracked files can be stashed away by adding the `--include-untracked` option.

To reiterate: this will save your modifications and revert the working directory to the HEAD. 

You can see the stashed modifications with `git stash list` and inspect the changes with `git stash show`.
The changes are restored to the (possibly modified) working directory with `git stash apply`. Doing this will not remove the stashed changes (check `git stash list`).
This is instead done with `git stash drop`. Note that there is a shortcut with `git stash pop`. 
This brings us to the next thing we have to note. We can do multiple stashes and they are stacked on top of each other. 
You can see that by listing the stashes. The last stash will be indexed as 0. You can inspect specific stashes by adding the numbered refernce as an option.
To inspect the previous stash you can run `git stash show 1`. Replace with 0 to see the last stash, which is also the one shown by default.

This means the last stashed changes will be the first to be restored with the `pop` option.
You can however pop a stash of your choosing by adding the refernce to the stash. `git stash pop 1` to apply and drop the previous stash whilst preserving the last stash in the stashed area.





### Making changes

Example:
Suppose we make some changes to the README.md file, a copy of which is now in the working directory, the local repository and the remote repository.

If we then run `git status` we can see that Git noticed the changes and marks the file as modified. It is now in the unstaged area.
We can then stage the file with the `git add README.md` command (changes are now ready to be committed). 
And then we commit the changes to the local repository. We are now ahead of origin/main. 


### Collaborating

If there are other persons working with the repository you should first check if there are any changes commited by other members of the team:

```bash
git pull # automatically merges and automatically merges the commits of the remote branch
git fetch origin # fetches the branches from the remote repo in the local repo. Does not affect prior stages.
```

Example git fetch:
Go to the remote repository, make some changes in a file and then commit the changes directly to the main branch.
If we straight away check the status of the main branch we won't notice any changes.  
In fact we have to fetch the remote repository first. Checking the status reveals that we are actually behind the remote branch by one commit.
Depending on the changes we might be able to do a fast-forward merge/pull or we might have to resolve conflicting changes (this will be discussed later).
What is important is that the changes are not yet applied to your local file. If we wanted them to be applied we can then merge the fetched changes with the local workspace:
```
git merge
```
We have now updated our local workspace with the remote changes.

Example git pull:
Do some other non-conflicting changes in the remote repository. 
By running `git pull` we will immediately merge the remote repository with our workspace.


After adjusting for the differences (merging the remote and local workspace) we can push our changes to the remote with `git push origin main`.

## merge or rebase?



[Article](https://www.simplilearn.com/git-rebase-vs-merge-article#:~:text=Merge%20lets%20you%20merge%20different,Rebase%20logs%20are%20linear.)

### Larger changes





## Reverting the status Moving Backwards

In the previous section we went forward with new changes. We can also revert changes and restore the file to a previous version.

https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things
