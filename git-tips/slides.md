# Git Tips
Askeing Yen &lt;fyen@mozilla.com&gt;

Notes: Oh hey, these are some notes. They'll be hidden in your presentation, but you can see them if you open the speaker notes window (hit 's' on your keyboard).

--

## Goal

* Speed
* Simple
* Non-linear development
* Distributed

Notes: Four Goal

----

## States

modified, staged, and committed  
![git states](http://git-scm.com/figures/18333fig0106-tn.png)  
(from http://git-scm.com/)

Notes: picture from git-scm

----

# Config

--

## Basic

* `git config --list`
* `git config --global user.name "foo"`
* `git config --global user.email "foo@mozilla.com"`

Notes: The --list will list all config.  
First, we should setup the username and email for git.

--

## Customizing

* `git config --global core.editor vim`
* `git config --global merge.tool vimdiff`
* `git config --global core.pager ''`
* `git config --global core.whitespace cr-at-eol`
	* stop viewing those <span style="color: Red;">^M</span> symbols at the end of lines created by Windows

Notes: The default pager is "less", and "" means no pager.  
Latest one will stop viewing the ^M which created by Windows.

--

## Config files

* /etc/gitconfig
	* --system
* ~/.gitconfig
	* --global
* {REPO}/.git/config
	* --local

Notes: Read config: system &gt; global &gt; local.  
The priority: system &lt; global &lt; local.

----

# Alias

--

## Commands

* `git config --global alias.co checkout`
* `git config --global alias.br branch`
* `git config --global alias.ci commit`
* `git config --global alias.st status`

Notes: We can setup our own alias for each git command.

--

## Creating commands

* `git config --global alias.unstage 'reset HEAD --'`
* `git config --global alias.last 'log -1 HEAD'`
* `git config --global alias.root 'rev-parse --show-toplevel'` <!-- .element: style="font-size:90%" -->

Notes: And we can create our own commands.  
By the way, the combination of "cd" and "git root" will very useful.  
ex: "cd `git root`"

----

# History

--

## Commit logs

* `git log -p`
* `git log -p --word-diff`
* `git log -S {string}`
* `git log --graph`
* `git log -U{n}`
* `git log --format={format}`
	* oneline
	* format:{format}
		* format:"The author of %h was %an, %ar%nTitle was >>%s<<%n"
* `git log --oneline`
* `git log --grep={pattern}`

Notes: The -p, --patch will show patches of each commit.  
The word-diff default is plain.  
And -S will look for differences that introduce or remove an instance of string.  
The --graph will draw the commit history.  
The -U will show diffs with n lines of context. Default is 3.

--

## Blame

* `git blame -L {n},{m} {file}`
* `git blame -C {file}`

Notes: The -L will only annotate the given line range. (number, /regex/, +offset or -offset for {m})  
The -C detect lines moved or copied from other files.

--

## Binary search

* `git bisect start`
* `git bisect bad`
* `git bisect good {commit}`
* `git bisect reset`

Notes: After doing "start, bad, good with good_commit", bisect will checked out the middle one for you.  
Run test and tell bisect this commit is good or bad.  
If bisect has enough info, it will tell you which commit is first bad commit.  
Then you should run "reset" to reset the HEAD.

----

# Branch

--

## Create

* `git branch {name} {start-point}`
* `git branch -t {name} {start-point}`
* `git checkout -b {name} {start-point}`

Notes: The -t, --track will set up config to mark the start-point branch as upstream.  
The checkout -b means creating branch and start it at start-point.

--

## Delete

* `git branch -d {name}`
* `git branch -D {name}`
* `git push origin :{name}`
* `git push origin --delete {name}`
* `git remote prune origin`

Notes: The -d can delete the branch which is fully merged in its upstream.  
(no upstream will check the HEAD.)  
The -D will delete branch irrespective of its merged status.  
To delete the remote branch, we have to using push command.  
The ":{name}" means push a None to name branch.  
Or we can use --delete option.  
The remote prune can prune the local ref of remote branch.

--

## Other

* `git branch --set-upstream {branch} {upstream}`
* `git push -u {upstream}`
* `git branch -v`
* `git branch -vv`
* `git branch --merged`
* `git branch --no-merged`
* `git branch --contains {commit}`

Notes: If we cannot find upstream, first two commands can help you.  
The -v can list the latest commit of each branch.  
The -vv will add the upstream info.  
The --merged and --no-merged will show you the branch by merged status.  
The --contains can help you find the branchs which contains one commit.

----

# Commit

--

## Amend last

* message
	* `git commit --amend -m {message}`
* content
	1. modify
	2. `git add` or `git rm`
	3. `git commit --amend`

Notes: First command will help you to modify the latest commit message.  
And the following commands will help you to modify the latest commit.

--

## Partially commit

1. `git add --patch`
	* or `git add -i` then `patch`
2. edit the current hunk, save
3. commit
* check the partially staged part
	* `git diff --cached`
* check the unstaged part
	* `git diff`

Notes: Selecting the "edit" after the "add --patch", then remove what you don't want to commit in the editor and save.

--

## Squash

1. `git rebase -i {commit}`
2. edit the list of commits in the editor
	* `pick`
	* or `squash`
3. edit the commit message in the editor

Notes: Squash can merge many commits into one commit.

--

## Amend

1. `git rebase -i {commit}`
2. edit the list of commits in the editor
	* `edit`
	* or reorder the commits
3. `git commit --amend`
4. `git rebase --continue`

Notes: Rebase also can help you to modify the commits.

--

## Rebase

* `git rebase --onto {newbase} {upstream} {branch}`
* for more detail
	* `git help rebase`
	* [rebase docs](http://git-scm.com/docs/git-rebase)

--

## Note

<br/>
Do <span style="color: Red;">NOT</span> rebase commits that you have pushed to a public repository.  
<br/>
<span>Or people will <span style="color: Red;">HATE</span> you!</span> <!-- .element: class="fragment" -->

----

# Stash

--

## Stashing WIP work

* `git stash`
* `git stash list`
* `git stash apply --index {stash}`
* `git stash pop {stash}`
* `git stash branch {branchname} {stash}`
* `git stash drop {stash}`

Notes: Stashing takes the dirty state of your working directory.  
After stashing, you can easily switch branches and do work elsewhere; your changes are stored on your stack.  
The "apply" command with a --index option to tell the command to try to reapply the staged changes.  
The "pop" will apply stash and drop it from stack.  
The "branch" will create a new branch, checkout the commit you were on when you stashed your work, reapplies your work there, and then drops the stash.

--

## Unapply

* `git stash show -p {stash} | git apply -R`
* `git config --global alias.stash-unapply '!git stash show -p | git apply -R'` <!-- .element: style="font-size:90%" -->

----

# Patch

--

## Create

* `git diff > foo.patch`
* `git format-patch -M {start-point}`
* `git format-patch -M -{n} {start-point}`

Notes: We will suggest you creating patches by format-patch command.  
The -M will detect renames.  
And -{n} will prepare patches from the topmost {n} commits.

--

## Apply

* `git apply --check foo.patch`
* `git apply foo.patch`
* `git am 001.foo.patch`

Notes: The "apply --check" will see if the patch is applicable to the current working tree.  
For the patches which creating by "format-patch", please apply them by "am" command.

----

# Ignore

--

## gitignore

* {REPO}/.gitignore
* `git config --global core.excludesfile '~/.gitignore'`

----

# Github

--

## SSH Key

* [Generating SSH keys for Github](https://help.github.com/articles/generating-ssh-keys)

--

## Syncing The Fork

* `git fetch --all`
* `git checkout {branch-name}`
* `git pull`
* `git merge {upstream-name}/{branch-name}`
* `git push {fork-name}/{branch-name}`

Notes: Fetch the branches and commits. Checkout to local-branch which you want to sync, then git pull to get it up-to-date. Merge from upstream, if local-branch didn't have any unique commits, it will fast-forward. Then push to your fork. 

--

## Delete the Remote Branch

* `git push {remote-name} :{branch-name}`
* `git remote prune {remote-name}`

Notes: The `git push {remote-name} {local-branch}:{branch-name}` will push the {local-branch} to {remote-name}, but it is renamed to {branch-name}. The `git remote prune` will deletes all stale remote-tracking branches under {remote-name}.

--

## Checkout PR Locally

* `git fetch {remote-name} pull/{ID}/head:{branch-name} && git checkout {branch-name}`

* External Alias
  * `git config --global alias.pr '! git fetch ${1} pull/${2}/head:PR_${2} && git checkout PR_${2} #'`
  * Then run `git pr mozilla 99` for fetching PR 99 from mozilla repo.

Notes: It will fetch the HEAD of pull request base on ID from remote repository, and create the new branch. Then we can switch to this branch.

----

# Prompt

--

## For Bash

1. Download this [script](https://gist.github.com/tung/409780)
2. Modify your `~/.bashrc` to source this script

<br/>
<div class="fragment"><span style="color: Blue;">[</span><span style="color: Orange;">askeing@askeing-ubuntu</span> <span style="color: Red;">~/workspace/B2G-flash-tool</span> <span style="color: Green;">(master)</span><span style="color: Orange;">↑</span><span style="color: Blue;">]</span><span style="color: Green;">$</span>_ </div>

----

## Reference

* Git Project [http://git-scm.com/](http://git-scm.com/)
* Try Github [https://try.github.io/](https://try.github.io/)
* [https://gist.github.com/tung/409780](https://gist.github.com/tung/409780)

