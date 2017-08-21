Quick Git Reference
===================

Configuring
-----------

    git config --global user.name "<First Last>"
    git config --global user.email <johndoe@example.com>
    git config --global core.editor <vim>
    git config --global color.ui true


Starting
--------

    git init [<directory>]  # Start! Creates the .git directory inside <directory> or in the current directory if not specified

These are the files created on a newly init'ed git repository:

<pre>
.git
├── HEAD
├── branches/
├── config
├── description
├── hooks/
│   └── ... ( Sample hooks, see http://git-scm.com/book/ch7-3.html )
├── info/
│   └── exclude
├── objects/
│   ├── info
│   └── pack
└── refs/
    ├── heads/
    └── tags/
</pre>


Cloning
-------

    git clone <url> [<directory>]  # Clone a repo and optionally give it an alternative <directory> name


Adding Files
------------

    git add <file>  # Stage a single file
    git add .  # Stage everything
    git add -u  # Stage updated tracked files
    git add -p [<file>]  # Stage hunks of file(s) interactively


Committing
----------

50/72-Rule: 50 or fewer characters for summary and wrap description at 72
characters. Separate the summary and description with an empty line. The
description should explain the motivation behind the change and how it is
different from the previous implementation.

	git commit -a "commit all unstaged changes"
	git commit -m "commit message here" # Commit's currently staged changes with message
    git commit -v  # Commit by opening an editor which displays a unified diff
    git commit --amend  # Modify the last commit with the current index changes

  - http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  - http://ablogaboutcode.com/2011/03/23/proper-git-commit-messages-and-an-elegant-git-history/
  - http://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message


Branching
---------

    git branch # Show list of branches and current branch
    git checkout <branch>  # Switch between branches
    git checkout -b <branch>  # Create and switch to a branch
    git branch -d <branch>  # Remove local branch
    git branch -avv  # Show extra verbose information about branches and their remotes
    
Tagging
---------
    git tag -a <tag_name> # Create a tag
    git tag # List all tags
    git push --tags # Push all the tags to remote repo
    git tag -d <tag_name> # Delete tag
    git push origin :refs/tags/<tag_name> # Update the remote repo after deleting tag on local

Displaying Logs
---------------

Use the `--all` option to show all branches, not only the current one.

    git log ..<other_branch>  # List changes that will be merged into current branch.
    git log --graph --all  # Show log graph for all branches
    git log --graph --abbrev-commit  # Show log graph
    git log --graph --decorate --oneline  # Show condensed log graph
    git log --graph --pretty=format':%C(yellow)%h%Cblue%d%Creset %s %C(white) %an, %ar%Creset'  # Show log graph including commiter information


Undoing
-------

`[<commit>]` defaults to HEAD.
More Examples: http://git-scm.com/docs/git-reset#_examples

    git reset [<commit>]  # Unstage changes
    git reset --hard [<commit>]  # Undo commits permanently
    git reset --soft 'HEAD^'  # http://stackoverflow.com/questions/927358/how-to-undo-the-last-git-commit/927386#927386
    git reset HEAD@{1}  # Undo last pull


Diff'ing
--------

Use `--stat` to show summary of changes instead of full diff. `[<commit>]`
defaults to HEAD if omitted. `<commit>` can be a branch name.

    git diff  # Show unstaged changes
    git diff --staged  # Show staged changes
    git diff HEAD  # Show staged and unstaged changes
    git diff <commit>  # Show changes in your working tree relative to <commit>
    git diff [<commit>]..[<commit>]  # Show diffs between two commits. Whitespace can be used instead of '..' when using both <commit>'s.
    git diff [<commit>]...[<commit>]  # Show diff in second <commit> from common ancestor (the point the two branches split). Note the three dots.


Working with Remotes
--------------------

    git remote add <remote_name> <url>  # Add remote repo to your local
    git remote -v  # Show remote urls after their names
    
    git push # Push local changes to remote repo's corresponding branch or create new branch on remote


Tracking
--------

    git branch -u origin/<remote_branch>  # Start tracking remote branch from current branch. http://stackoverflow.com/questions/520650/how-do-you-make-an-existing-git-branch-track-a-remote-branch/2286030#2286030
    git branch -u origin/<remote_branch> [<branch>]  # Start tracking remote branch from <branch> or current branch if not specified.
    git branch -d -r origin/<remote_branch>  # Stop tracking remote branch. NOTE: Remote branch is not removed.
    git push -u <remote> <branch>  # Create a remote branch from local branch and track it


Stashing
--------

Stash references have the form stash@{0}. WIP means Work In Progress.
An empty `[<stash>]` defaults to the latest stash.

    git stash list  # Show stash list
    git stash [save] [<message>] # Stash current changes with optional message
    git stash show [-p] [<stash>]  # Show stash diffstat or '-p' to show patch
    git stash apply [<stash>]  # Apply stash
    git stash pop  [<stash>]  # Apply stash and drop it
    git stash drop [<stash>]  # Drop stash
    git stash branch <branchname> [<stash>]  # Create and switch to a new branch with the <stash> applied to it
    git stash clear  # Remove all stashes

Rebasing
--------

Rule of Rebasing: "Only rebase local branches"

    git checkout <branch>; git rebase master  # Apply changes on top of master

  - http://git-scm.com/book/en/Git-Branching-Rebasing
  - http://blogs.atlassian.com/2013/10/git-team-workflows-merge-or-rebase/
  - http://stackoverflow.com/questions/16336014/git-merge-vs-rebase
  - http://blog.sourcetreeapp.com/2012/08/21/merge-or-rebase/
  - http://www.derekgourlay.com/archives/428


Using Additional Packages
-------------------------

    gem install git-smart  # https://github.com/geelen/git-smart
    gem install git-up  # https://github.com/aanand/git-up
    gem install omglog  # https://github.com/benhoskings/omglog

    while true; do clear; git --no-pager log --decorate --oneline --graph --all -n 10; sleep 3; done  # Quickly display git repository's commit graph with live update. See https://github.com/benhoskings/omglog for a more permanent solution.


Using Shell Aliases
-------------------

# alias {{{
[alias]
    # Workflow
    # Before starting work on a particular branch
    up = !git pull --rebase --prune $@ && git submodule update --init --recursive
    co = checkout
    cob = checkout -b
    # Commit regularly
    cm = !git add -A && git commit -m
    #The first one adds all changes including untracked files and creates a commit. The second one only commits tracked changes
    #When I return to work, I’ll just use git undo which resets the previous commit, but keeps all the changes from that commit in the working directory
    #Or, if I merely need to modify the previous commit, I’ll use git amend
    save = !git add -A && git commit -m 'SAVEPOINT'
    wip = !git add -u && git commit -m "WIP" 
    undo = reset HEAD~1 --mixed
    amend = commit -a --amend
    #commits everything in my working directory and then does a hard reset to remove that commit
    # Only makes the commit unreachable. run the git reflog command and find the SHA of the commit if reset has been done by mistake.
    wipe = !git add -A && git commit -qm 'WIPE SAVEPOINT' && git reset HEAD~1 --hard
    
    
    # basic {{{
    st = status -s
    cl = clone
    br = branch
    r = reset
    cp = cherry-pick
    gr = grep -Ii
    
    # git list
    #List oneline commits showing dates	
    lds = log --pretty=format:"%C(yellow)%h\\ %ad%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=short
    ld = log --pretty=format:"%C(yellow)%h\\ %ad%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=relative
    le = log --oneline --decorate
   
    # log commands {{{
    ls = log --pretty=format:"%C(green)%h\\ %C(yellow)[%ad]%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --date=relative
    ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]" --decorate --numstat
    lc  = "!f() { git ll "$1"^.."$1"; }; f"
    lnc = log --pretty=format:"%h\\ %s\\ [%cn]"
    fl = log -u
    filelog = log -u
    
    graph = log --graph --oneline --decorate --all
    
    # log commands to inspect (recent) history
    # show modified files in last commit
    dl = "!git ll -1"
    # Show diff last commit
    dlc = diff --cached HEAD^
    
    #Finding files and content inside files (grep)
    #Find a file path in codebase
    f = "!git ls-files | grep -i"
    #Search/grep your entire codebase for a string
    grep = grep -Ii
    gr = grep -Ii			

    # diff {{{
    d = diff --word-diff
    dc = diff --cached
    # diff last commit
    dlc = diff --cached HEAD^
    dr  = "!f() { git diff -w "$1"^.."$1"; }; f"
    diffr  = "!f() { git diff "$1"^.."$1"; }; f"
    # }}}
    # reset commands {{{
    r1 = reset HEAD^
    r2 = reset HEAD^^
    rh = reset --hard
    rh1 = reset HEAD^ --hard
    rh2 = reset HEAD^^ --hard
    # }}}
    
    # stash {{{
    sl = stash list
    sa = stash apply
    ss = stash save
    # }}}
    
    # conflict/merges
    ours = "!f() { git co --ours $@ && git add $@; }; f"
    theirs = "!f() { git co --theirs $@ && git add $@; }; f"
   
    #list remotes
    rem="!git config -l | grep remote.*url | tail -n +2"	

    # list all aliases
    la = "!git config -l | grep alias | cut -c 7-"
    # }}}
    
    # worktree list {{{
    wl = worktree list	


Ref: 
http://haacked.com/archive/2014/07/28/github-flow-aliases/
http://durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples/
	


References
----------

  - http://git-scm.com/book
  - http://git-scm.com/docs
  - http://www.ndpsoftware.com/git-cheatsheet.html
  - http://gitref.org/
