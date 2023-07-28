# GIT

### What is Git?

- Git is a Version control management system, or it is a software that we used to manage the multiple version of the software.
- Git is open-source, secure and flexibility and easy to maintain the code

- In git, If you have **to make a new version**, then **we have to crate a new commit**

### What is GitHub?

Github is a online tool that used for contribution or collobration. It just provide the GUI for git.

- `git --version` -> to check the version
- `git config --list` -> to check the config in your local system.

---

### GIT Commands

- `git config --global user.name "Priyanshu"` - Setting your Git username for every repository on your computer. It is used to config your git in your local host.

- `git config user.name "Priyanshu"` -Setting your Git username for a single repository

- `git init` - first cmd you should use to Power your folder to be managed by the git, initializes a new empty repositry(a folder that is managed by the git where we track all the changes we are making in our project ). It is also create a `.git` folder that has all the revelant logic to manage your version your project.

- `git status` - to help to check what is the status of our file or project code. Basically it track the status whether it is modified or commited .

- `git add <fileName>` - it start the tracking the changes in your file .

- `git commit  -m '<message>' ` - to add the new version.

- `git cat-file <flag> <hash>` - to see the flag(-t> type of object & -p> print the object) and the content of the file

- `git rm --cached <filename>` - if you pushed into staging area and revert back into untracked area.

- `git restore --staged <filename>` - if you modified the file and moved into staging area and wwant to revert back into unstage file or (to discard changes in working directory)

- `git branch` - to see other branches in the repo.

- `git checkout -b <nameOfBranch>` - to create the branch

- `git checkout <branchName>` - to swtich btw the branches

- `git merge <branchName>` - to merge your branch into master branch

- `git branch -d  local_branch_name` - to delete the branch from local

- `git push remote_name -d remote_branch_name` - to delete the branch from remote

- `git clone gitURL` - to clone the git repo

- `git add --a` - (means add all change/add will be stage. or to track)

- `git log -p` - (kya cheej remove or add hue h, sub kuch show kr dega)

-`git log -p -n` - (type number place of 'n' to check n number of log)

- `git log --stat` - (in sort show details)

-`git rm <file name> ` - (to remove file and it will stage it also, so relax)

- `git  mv <current file name> <changed name type>` - (it will change file name be using git command, and it will stage it also, so relax)

-`git commit -a -m "NAME"` - (direct commit tracked file no need to use git add command, but not for untracked file for this you have to add yourself in tracking area...)

- `git restore <file>` - (to discard changes in working directory)

- `git checkout -- <filename>` - (to restore last change in that file)

- `git checkout -f` - (to restore last working directory)

- `rm -rf .git` - if you dont want to manage by the git so just delete `.git` .

---

## How git internally works?

git --> hashing --> graph/tree data strucuture

- git is like key, value stores.
  - key - hash of the data.
  - value - data.
- git uses cryptographic hashing function - SHA-1 hashing algorithm for a given data it outputs 40digit hexadecimal no and the hash value is always same for the same data.
- once hash prepared, git compress your data in a blob and stores some metadata about data.

  > Blob Object

|        |        |
| ------ | ------ |
| blob   | size   |
| ------ | ------ |

blob- is the binary object.

> These how `.git` folder looks like from inside:

```
.git
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags



```

### IF we make any changes inside the repo, How it works?

example:
`git hash-object --stdin` - basically convert the data into the 40 digit hash
`git hash-object -w stdin` - to write inside the `.git` folder
When we run a query below, we get a hash data

`git cat-file -p <fileName> ` - read the file inside the `.git/objects/`.

```
echo 'Hello Git' | git hash-object --stdin
9f4d96d5b00d98959ea9960f069585ce42b1349a - this is the hash of `hello git` data


SHA-1 hash - 9f4d96d5b00d98959ea9960f069585ce42b1349

binary object inside the `.git/objects/
xK??OR04`?H???Wp?,?4?c%
```

> After the changes in repo, how `.git` folder looks like :

```


.git
├── HEAD
├── config
├── description
├── hooks
│   ├── applypatch-msg.sample
│   ├── commit-msg.sample
│   ├── fsmonitor-watchman.sample
│   ├── post-update.sample
│   ├── pre-applypatch.sample
│   ├── pre-commit.sample
│   ├── pre-merge-commit.sample
│   ├── pre-push.sample
│   ├── pre-rebase.sample
│   ├── pre-receive.sample
│   ├── prepare-commit-msg.sample
│   ├── push-to-checkout.sample
│   └── update.sample
├── info
│   └── exclude
├── objects
│   ├── 9f
│   │   └── 4d96d5b00d98959ea9960f069585ce42b1349a
│   ├── info
│   └── pack
└── refs
├── heads
└── tags


```

- Whenever we make a changes in our git repo , inside a `.git` folder we have an object folder, inside object folder , we stored a directory(dir name - first two words of the hash) inside which store our hash data.

- Whatever we changes with our data , like adding file, creating new file, deleting etc whatever you do , everything is stores as KEY VALUE Pair

        - Key is actually a SHA-1 hash.
        - Value is a binary object.

  - To store the Key sha-1 hash, What git do is, it takes first two digts of the hash and make a directory and it takes the remaining 38digit create a large binary object name file name.

- To see what inside the `.git/objects/` we cant use directly the `cat <fileHashName`.
  instead we need to use the `git cat-file -p <fileHashName>`

  - _Imp_: whenever two file have identical data , git is not gonna make new key hash .

  ```
  eg- let say, we have two files name as : test.js and test1.js and both file has same data. so what git will do it will not gonna key a new key hash .


  ```

  - `.git` internally does a lots of optimization , to store the object in compressed form.

- `.git` mainly store the data about the change & algorithemically shows us the file content with that change.

### What is Branch?

![](./01%20Git%20branch.svg)

Before dicussing about the branching, let say why we have branching concept in git.
SO
Let's say we have a project and we are building very smoothly and we are workin in a team. One of the team member has a idea like let's build a blah blah features that do blah blah task.
and now we are building the blah blah features in the same repo or project. now we committed our file or project that we are building. (note that : our project is still in building phase only )

After someday, we are checking our blah blah features that we were building and want to add some more coding lines. But the issue here is that we have to check the whole commit history or our whole code base where we build our blah blah feature.

Here, Branching concept introduce
What branching do is that it helps to create a seperate branch or we can say that it make a repo that we can work parallely. It is not dependent from main repo. Now , we can build our blah blah feature here indendently without dependent on main repo or file.

**OR**
Branching means you can maintain multiple state of our project.

### How we can create branching in git?

`git branch` - to see other branches in the repo.
`git checkout -b <nameOfBranch>` - to create the branch
`git checkout <branchName>` - to swtich btw the branches

```
NOTE: whenever we create a new branch all changes or commited we did before creating the new branch those will be avilable in new Branch.
```

`git merge <branchName>` - to merge your branch into master branch

- STANDARD WAY OF ADDING THE BRANCH INTO THE MASTER BRANCH

  - create branch > made some changes then > push our branch into github > then raise the pull request so that we can get the approval from the other team to add in our master branch.
