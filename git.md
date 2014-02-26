# GIT

small private abstract for git. sources are:
[the git book](http://git-scm.com/book)
[successfull git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->

## VERSION CONTROL SYSTEM

<!-- ----------------------------------------------------------------------- -->

### tasks of version control systems

version control systems are used to manage changes on files:

* revert files back to previous state
* revert the entire project back to previous state
* compare changes over time
* see last modifications
* see contributors of last modifications
* ...


<!-- ----------------------------------------------------------------------- -->

### categories of version control systems

Version control systems can be devided in three categories:

* local version control systems
  simple local database that keeps changes to files under revision
  control.
* centralized version control systems
  a single server that contains all the versioned files, and a number
  of clients that check out the files form that central place.
* distribiuted version control systems
  DVCS clients don't just check out the latest snapshot of the files -
  they fully mirror the repository. every checkout is a real full
  backup of the data.



<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->
\newpage

## GIT BASICS

<!-- ----------------------------------------------------------------------- -->

### git data architecure

<!-- ----------------------------------------------------------------------- -->

### git, checksumming and hash values

A checksum of hash sum is a (small sized) datum computed from an arbitrary
block of digital data for the purpose of detecting errors that may have
been introduced during its transmission or storage.

~~~
        Input                            Output
    -------------    ------------   -----------------
    | blue and   |   | checksum |   |               |
    | green fox  |-->|          |-->| 1561234234123 |
    | jumps over |   | function |   |               |
    --------------   ------------   -----------------
        Input                            Output
    -------------    ------------   -----------------
    | #lue and   |   | checksum |   |               | minor change -
    | green fox  |-->|          |-->| 4356341287781 | significantly different
    | jumps over |   | function |   |               | checksum
    --------------   ------------   -----------------
~~~

__Git__ checksums data before storing and refers to that data via the checksum.
The checksum function used is the SHA-1 hash:
a 40 character string composed of hexadecimal characters (0-9 and a-f)
calculated based on the file contents and the directory structure.

Example:

~~~
24b9da6552252987aa493b52f8696cd6d3b00373
~~~

__Git__ stores data not by its filename but in the __Git__-database
addressable by its hashvalue.


<!-- ----------------------------------------------------------------------- -->
\newpage


### __Git__ stages IMPORTANT!

__Git__ has three main stages for files:

* comitted
  the data is stored in the local database.
  if a particular version of the file is in the __Git__ directory it's
  considered committed.

* modified
  the file has been changed but not yet been committed to the
  database. If a file has been changed since it was checked out but
  has not been staged it's modified.

* staged
  staged means that a modified file has been marked in its current
  version to go into the next commit snapshot. if a particular file
  has been modified and added to the staging area it is staged.


~~~
	working				staging				git directory
	directory			area				(repository)
		|					|					|
		|<======================================|
		|        checkout the project			|
		|					|					|
		|==================>|					|
		| stage files		|					|
		|					|==================>|
		|					| commit files		|
		|					|					|
~~~

* __Git__ directory: 		metadata, object database for project
* working directory:	single checkout of one version of the project.
						files get pulled out the compressed database in
						the __Git__ directory and placed on disk to be modified.
* staging area:			file that stores information what goes in the next commit.


### __Git__ workflow
simple:

1. modify files in the working directory
2. stage files, adding snapshots of them to the staging area.
3. commit: snapshot of files in the staging area are stored in the database.

advanced:

### __Git__ help

The __Git__ help functions can be accessed in three ways:

~~~
git help <verb>
git <verb> --help
man git-<verb>
~~~



<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->
\newpage

## GIT CONFIGURATION

### Tool: gitconfig
Get and set all configuration variables that determine how git looks and operates.

These varialbles can be stored in three different places:

* /etc/gitconfig
  information for every user on the system (gitconfig --system writes here).
* ~./gitconfig
  specific to a user (gitconfig --global writes here).
* .git/config
  config file in the git directory
  each level override values of the previous level.


<!-- ----------------------------------------------------------------------- -->
\newpage

## RUNNING GIT

###Initializing a repository in an existing directory###
Go to the projects directory and type:

~~~
git init
~~~

The command creates a subdirectoy named `.git` that contains a skeleton
repository file.

### Cloning an existing repository

~~~
git clone git://github.com/julmue/foo.git

# different name for folder:
git clone git://github.com/julmue/foo.git myfoo
~~~

* creates a directory named grit
* initializes a `.git` directory inside it
* pulls down all the data for that repository
* checks out a working copy of the latest version

__Git__ has a number of different transfer protocols.


### Repository: Recording changes

Each file in a working repository can be in one of two stages:

* _tracked_: files that where in the last snapshot. They can be:
	* unmodified
	* modified
	* staged

* _untracked_: everything else.


### Repository: Checking the status of the files

Tool: `git status` command

~~~
# Example output:
git status
On branch master
nothing to commit, working directory clean
~~~

* lists current branch
* lists untracked files
* lists modified files
* lists staged files


### Repository: Tracking files (adding untracked files)

Tool: `git add` command

~~~
# Examples:
git add foo
git add *.bar
git commit -m 'initial project version'
~~~


### Repository: Staging modified files

Tool: `git add` (`add` is a multi-purpose command)

~~~
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   foo.bar

git add foo.bar

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   foo.bar
~~~

__Git__ stages files exactly as they are when `git add` is run. It is possible
to have `staged` and `unstaged` files at the same moment. If a file is
changed after being staged `git add` has to be called again.

~~~
git add .   // current directory with subfolders
git add -A  // all files
git add *   // all files
~~~


### Repository: Viewing staged and unstaged changed

tool: `git diff`. `diff` shows the exact lines to be added or removed.

~~~
# To see what has been changed but not yet staged:
git diff
~~~

~~~
# To see what staged changes will go in the next commit:
git diff --staged
~~~


### Repository: Commiting changes

tool: `git commit`.

~~~
# Commit and launch editor of choice:
git commit

# Putting the diff output in the editor to make changes explicit:
git commit -v

# Pass commit message from command line:
git commit -m "message body"

# Skip the staging area and commit every tracked file
git commit -a
~~~

Every commit is a snapshot of the project file system that can be reverted
or compared at a later time.


### Repository: Removing Files

Tool: `git rm`. `rm` removes a file from being tracked.
Remove a file from the staging area and then commit.

~~~
rm foo.bar

$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    foo.bar

no changes added to commit (use "git add" and/or "git commit -a")

# Stages the files removal
git rm foo.bar

# File will be gone and no longer tracked
git commit
~~~

If a file is already staged it has to be removed with `git rm -f`.

If a file is already tracked and should be made untracked but kept on the
harddrive (maybe accidently not mentioned in the `.gitignore` file):

~~
git rm --cached foo.bar
~~


### Repository: Moving and renaming files

__Git__ doesn't explicitly track file movement.
If a file is renamed no metadata is stored that tells __Git__ that the file was renamed.

~~~
git mv foo1.bar foo2.bar

# equivalent
mv foo1.bar foo2.bar
git rm foo1.bar
git foo2.bar
~~~

__Git__ can figure out that this is an implicit rename.


<!-- ----------------------------------------------------------------------- -->
\newpage

### Viewing the commit:

tool: `git log`

Option 				Description
------				-----------
`-p` 				Show the patch introduced with each commit.
`--word-diff` 		Show the patch in a word diff format.
`--stat` 			Show statistics for files modified in each commit.
`--shortstat` 		Display only the changed/insertions/deletions line from the --stat command.
`--name-only` 		Show the list of files modified after the commit information.
`--name-status` 	Show the list of files affected with added/modified/deleted information as well.
`--abbrev-commit` 	Show only the first few characters of the SHA-1 checksum instead of all 40.
`--relative-date` 	Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.
`--graph` 			Display an ASCII graph of the branch and merge history beside the log output.
`--pretty` 			Show commits in an alternate format. Options include oneline, short, full, fuller, and format (where you specify your own format).
`--oneline` 		A convenience option short for --pretty=oneline --abbrev-commit.




<!-- ----------------------------------------------------------------------- -->
\newpage


### Undoing things: Changing last commit

tool: `git commit --amend`




### Undoing things: Unstaging a staged file

tool: `git reset HEAD <file>`.



### Undoing things: Unmodifying a modified file.

tool: `git checkout <file>`

~~~
git checkout foo.bar // resets file foo.bar
git checkout .		 // resets working directory to state of last commit
~~~



<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->
\newpage

## WORKING WITH REMOTES

Remote repositories are repositories that are hosted on the internet or network.
Remote repositories can be:

* read only

* read/write

Data can be pulled from or pushed to a repository.


### Showing the remotes

tool: `git remote`. Command lists the shortnames of each remote handle specified.
If the repository is cloned `origin` is already in the listed --
the default name __Git__ gives the server that the repo is cloned from.

~~~
git remote
origin

# list repos with urls. example:
git remote -v
origin  https://github.com/julmue/dotfilesM1.git (fetch)
origin  https://github.com/julmue/dotfilesM1.git (push)
~~~

if there is more than one remote the command lists them all.



### Adding remote repositories

tool: `git remote add [shortname] [url]`

~~~
git remote add origin https://github.com/username/Hello-World.git
~~~

Now `shortname` can be used to reference the whole `url`.

~~~
# example: fetch all the information from a remote repository

git fetch foorepo
~~~
The masterbranch of this repository is accessible locally as `foorepo/master`


### fetching from remotes

tool: `git fetch [remote-name]`

The command pulls down all data from the remote repository that is not already
in the local repository.



### pulling from remotes

tool: `git pull`.

Important: pulling = fetching + merging. If a branch is set up to track a
remote branch `git pull` fetches and merges the remote branch into the current
branch. `git clone` automatically sets up the local master branch to track
the remote master branch on the server the repository is cloned from.


### Pushing to remotes

Changs that are ready have to be pushed upstream.

tool: `git push [remote-name] [branch-name]`.

~~~
git push origin master
~~~

This command works only if the repository is cloned from a server to which
the user has write access, and if nobody has pushed to the master repo in the meantime.
If any changes in the master repository have occured since the local repository
has been changed the changes have to be fetched an merged first.



### Inscpecting a remote

tool: `git remote show [remote-name]`


### Removing and renaming remotes

Changing the name of a remote repository:

tool: `git remote rename [oldname] [newname]`

This changes the remote branch names, too.

Removing a remote repository:

tool: `git remote rm [remote-name]`



<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->
\newpage

## Branching

Branching means diverging form the main line of development an continue to
do work without interfering with that main line.
The leightweight branching model of __Git__ is its "killer" feature.
A workflow with branching and merging on a daily basis is common.

### What is a branch?

__Git__ stores data as a series of snapshots.

A commit object in __Git__ stores:

* a pointer to the snapshot of the content of the commit
* author and message metadata
* zero or more pointers to the commits that where direct parents to this commit:
    * 0 parents for the initial commit
    * 1 parent for a regular commit
    * multiple parents for a commit that is the result of a merge of two or more branches

~~~
            commit                                 snapshot (tree + blobs)
                              ........................................................
                              :
            98c63...          :                                         55e37...
    COMMIT    | size          :                                 |==> blob |   size
    ----------------------    :             9e5f7...            |    -----------------
    tree      | 9e5f7...      :      TREE | size                |         DATA
    ----------------------    :      -----------------------    |
    author    | fooman      ======>  blob | 55e37 | foo1.bar    |
    ----------------------    :      -----------------------    |==>  blob |   size
    committer | fooman        :      blob | 923ab | foo2.bar    |     ----------------
    ----------------------    :      -----------------------    |         ....
    this is a first commit    :       ... |  ...  |  ...        |
                              :
                              :
                              ........................................................

~~~

After a some commits:


~~~
          98c63...                   e3f56d...                  ab88c6...

    COMMIT    | size            COMMIT    | size            COMMIT    | size
    ------------------          ------------------          ------------------
    tree      | 9e5f7           tree      | 673aa           tree      | bdf35d
    ------------------          ------------------          ------------------
    author    | fooman  ------> author    | fooman  ------> author    | fooman
    ------------------          ------------------          ------------------
    committer | fooman          committer | fooman          committer | fooman
    ------------------          ------------------          ------------------
    this is a first c.          this is a sec. c.           this is a third c.
           _                            _                           _
           |                            |                           |
           |                            |                           |
           V                            V                           V
    -----------------           -----------------           -----------------
    |               |           |               |           |               |
    |   snapshot A  |           |   snapshot B  |           |   snapshot C  |
    |               |           |               |           |               |
    -----------------           -----------------           -----------------

~~~

A branch is a movable pointer to one of the commits. It moves foreward with
every commit. Creating a new branch creates a new pointer to move around.
HEAD is a special pointer to the current local branch.


~~~
                                                            -----------------
                                                            |               |
                                                            |  HEAD         |
                                                            |               |
                                                            -----------------
                                                                   |
                                                                   |
                                                                   v
                                                            -----------------
                                                            |               |
                                                            |   MASTER      |
                                                            |               |
                                                            -----------------
                                                                   |
                                                                   |
                                                                   V
    -----------------           -----------------           -----------------
    |               |           |               |           |               |
    |   98c63...    | --------> |   e3f56d...   | --------> |   ab88c6...   |
    |               |           |               |           |               |
    -----------------           -----------------           -----------------
            |                           |                          |
       snapshot A                  snapshot B                 snapshot C
                                                                   ^
                                                                   |
                                                                   |
                                                            -----------------
                                                            |               |
                                                            |   testing     |
                                                            |               |
                                                            -----------------

~~~


### Creating a branch:

~~~
git branch <branchname>
~~~

### Checking out a branch

Checking out a branch moves the HEAD pointer.

~~~
git checkout <branchname>

# Create a branch and check it out

git checkout -b <branchname>
~~~


### Deleting a branch

~~~
git branch -d <branchname>
~~~


If the working area or staging area has uncommitted changes that conflict with the branch
that is to be checked out __Git__ refuses to switch branches.
It's best to have clean work state when switching branches.
Alternatives: stashing, commit amending

When checking out a branch __Git__ resets the working directory to look like the
snapshot of the commit that the checked-out branch points to.
It adds, removes and modifies files automatically.


### Merging a branch

~~~
git merge <branchname>

# 1. checkout branch you wish to merge into
# 2. pull in changes from another branch
git checkout <branch1>  " e.g. master
git merge <branch2>     " e.g. testing
~~~

Merging:

* Fast-forward: When merging a branch that is directly upstream (can be reached through the commits history) __Git__ moves the pointer forward.
* Three-Way-Merge: two branches have a common ancestor as a merge-base; __Git__ creates a new commit that has more than one ancestor as
  a result of the merging operatrion.


### Branch management

~~~
# show all branches
git branch

# show the last commit on each branch
git branch -v

# show branches that are not merged into the current branch
git branch --no-merged

# show branches that have been merged into the current branch
git branch --merged
~~~

### Merging conflicts

* Changes in the same part of a file on different branches -- __Git__ is unable to resolve the conflict.



## Branching Workflows

### Modell 1:


One central repo (origin). Every developer pulls and pushes to that repo.
Every developer may also pull from members of sub-teams.

#### The main branches:

* origin/master: production ready state
* origin/develop: latest delivered development features for the next release.
  Integration branch (nightly builds form here). Each time it is merged into master: new release.

#### Support branches:

Variety of supporting branches. These branches have a limited lifetime.

* Feature branch
* Release branch
* Hotfix branch

Each of this branches have a specific purpose and are bound to strict rules
for origin branch and merge target.

Branch: Feature
parent: develop
target: develop
naming: anything except master, develop, release-*, hotfix-*

Branches used to develop new features for future releases.
Exist as long as the feature is in development. Will eventually
be merged back into develop or discarded.

Feature branches typically exist in developer repos not in origin.

Creating a feature branch:

~~~
git checkout -b myfeature develop
~~~

Incooperating a finished feature into development:

~~~
git checkout development
git merge --no-ff myfeature
git branch -d myfeature
git push origin develop
~~~

`--no-ff` flag causes merge to a alway create a new commit object, even if the merge could be performed with
fast-forward. Avoids loosing information of the history of a feature.


Branch: Hotfix
parent: master
target: master and develop
naming: hotfix-

Hotfix branches are to fix bugs in a living release.

creating the hotfix branch:

~~~
git checkout -b hotfix-1.1.1 master
# now bump version number (shellscript?)
git commit -a -m "bumped version number to 1.1.1"
~~~

Fix bug and commit fix in one or more commits

~~~
git commit -m "fixed production problem"
~~~

Finishing a hotfix branch

~~~
## merge in master
git checkout master
git merge --no-ff hotfix-1.1.1
git tag -a 1.1.1
# merge in develop
git checkout develop
git merge --no-ff hotfix-1.1.1
git branch -d hotfix-1.1.1
~~~















<!-- ----------------------------------------------------------------------- -->
<!-- ----------------------------------------------------------------------- -->
\newpage

## GitHub

### Create a repository

1. on Github page
    1. Click `New Repository`
    2. Fill out information on the page
    3. Click `Create Repository`
2. local
	1. make new directory
	2. initialize git: `git init`
	3. add and commit Readme (optional)
	4. Push:
~~~
# Creates a remote named "origin" pointing at your GitHub repository
git remote add origin https://github.com/username/Hello-World.git
~~~

~~~
# Sends your commits in the "master" branch to GitHub
git push origin master
~~~















