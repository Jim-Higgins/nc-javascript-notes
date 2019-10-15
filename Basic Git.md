# Git
​
## Prior Knowledge
​
- Basic bash commands
​
## Learning objectives
​
- Gain an understanding of git's internal model
- Know and understand how to use important git porcelain commands
- Understand and be aware of important git plumbing commands
​
## What is git
​
When working on an application over time we can expect our source code to under go many changes. At a level where the code base is vast, then we can certainly expect lots of revisions to the project over time. It would be useful if we could have some way of tracking the state of our project as it evolves. This gives us the comfort of knowing we can go back to an earlier state of our project when something stops working in the present. Version control or revision control software is software that fulfils this purpose, allowing us to keep track of the different versions or states of our project and giving us the capability of reverting back to a previous version.
​
Git is defined formally as a **distributed version control system** or a **distributed revision control system**. We can analyse the different parts of this definition in more depth but first let's isolate the "revision control system" aspect of this definition. The version control system can be thought of at its heart as a content tracker. This content tracker will keep track of our source code over time.
​
## git commands
​
Git commands are divided broadly speaking into two different categories - **porcelain** and **plumbing** commands. 
_Porcelain commands_ are the high-level commands we use in git -so called because things like kitchen sinks tend to be made out of porcelain- the things that we interact with in our day to day lives. 
These are to be contrasted with the _Plumbing commands_ in git, these are low-level commands so called because plumbing conjures notions of stripping away the kitchen sink in order to see what is going on at a deeper level.
​
## Storing content
​
Suppose we have a string of content that we want stored in our git repository (this is the the place where all our file content will be stored). We can run the command `echo "hello world" | git hash-object --stdin -w`, the standard output from the command `echo "hello world"` is piped (used) as the input for the command. `git hash-object --stdin -w`  calculates something called the **SHA1** for this string. The SHA1 is a unique 40 digit hexadecimal string, which can be thought of as a unique key for identifying this piece of data. Every piece of information that Git stores is referred to as an object, and each object is identified by its unique SHA1 key.
​
When we talk about git as being a content tracker, what we mean is a **persisent map**: keys (in a SHA1 format) that map to the content we are storing inside our git repository. If we inspect the `.git/objects` we can see a directory beginning with the first 2 digits of the SHA1 for this piece of data. Inside this there is a file with a name as the same as the given SHA1. We cannot look inside this file however as its contents are stored as a blob (standarding for **binary large object**). Git essentially compresses our string "hello world" and adds a header to it when it is stored in the repository. Therefore, all the file content we produce that is stored in git is stored in this blob format.
​
## Commits
​
Git is based around the idea of having snapshots of a project (our source code) over time. This becomes very useful in situations when we need to go back to a previous version of the code, perhaps where errors don't exist. These snapshots we refer to as **commits**, which essentially point to a directory which in turn points to other directories and files. In essence, each commit will point to a particular file system. 
​
Suppose we are working in a project, call it `project-a`, then we can use a high level command `git init` in order to create (some would say initialiase) a git repository in your project. The git repository is essentially the place in which Git will store the snapshots of your project over time. After running `git init`, the command `ls -a` can be run and there should be `project-a/.git` - this `.git` is the hidden git repository at the root of the project.
​
In summary, git is a persisent map for our data, the unique SHA1 keys serve as unique keys that map to our data which persists as blobs inside the git repository.
​
## 3 different areas
​
### Working tree / directory - `/project`
​
The working tree are all the files and directories that we currently have access to in the project. We can make changes, updates or add new files or directories to this new project.
​
### Repository `/project/.git/objects`
​
This folder stores the commits of our `project`. Therefore it stores snapshots of our directories and our actual file content, stored in a format called a blob (standing for binary large object). 
​
### Staging area - `/project/.git/index`
​
This is a file stored within the hidden git folder that tracks the difference between the working tree, the latest commit (stored in the repository) and any changes that are regarded as **staged**. A **staged** change is an update in our working tree that we want to be part of our next snapshot (or our next commit).
​
### `git status`
​
Displays the differences between the latest commit and the `./git/index` file and also shows changes in the working tree that are currently not tracked by git. In other words, if we make a change in our working tree then it will be registered in the `.git/index` file but we haven't told git that this change is staged in preparation for a commit. It will also display changes that have been staged in the working tree and that will be added to the git repository if we then commit this change.
​
### `git commit`
​
Any changes that are staged in the `.git/index` file will be stored in the repository.