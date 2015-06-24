---
layout: page
title: Git and GitHub
permalink: /git-and-github/
---

## About this guide

Many guides and books have been written about Git and GitHub. Here are a few of
them:

- [GitHub's Hello World Guide (10 minute read, no code/shell!)](https://guides.github.com/activities/hello-world/)
- [GitHub Bootcamp](https://help.github.com/categories/bootcamp/)
- [*The* Git Book, _Pro Git_, free online](https://git-scm.com/book/en/v2)
- [Git Book Videos](https://git-scm.com/videos)
- [Linus Torvalds, inventor of Git and Linux, highly opinionated Google Tech
  Talk](https://www.youtube.com/watch?v=4XpnKHJAok8)

The goal of this tutorial is to make Git and GitHub relevant for an individual
by means of me explaining how I use Git and why I can't live without it anymore.
Of course other people may have other experiences and this is somewhat
subjective. However, the status of Git as the industry standard and most popular 
version control system today is not a subjective fact. Please see this 

GitHub is also incredibly powerful. For a teaser on why that is, let me say
this: if you find any errors or if anything could be more clear, please 
[open an issue on the github repository for this website](https://github.com/mtpain/me/issues/new).

## Why Version Control?

Version control helps me stay sane and be much more productive than I would be 
otherwise. The fundamental benefit of using version control is to be able to
immediately revert breaking changes back to a known working version of the code.

## Why Git?

I won't try to argue this here, but it's just the best and so is GitHub. See
this article for arguments and links to information on why it's the industry
standard and what that means for you: [The Case for Git in
2015](http://www.netinstructions.com/the-case-for-git/).


## How I use Git/GitHub: Tutorial

This section will focus on how I use Git and GitHub. At this point we haven't
talked about the difference between the two. GitHub does a job that any web
server could do: It holds one or many WWW-available versions of any given 
repository. There are alternatives to GitHub that you can host yourself if you so desire,
but we won't cover them here.

### Install it!

On OS X, install Git like so

{% highlight bash %}
brew install git
{% endhighlight %}

This requires Homebrew. If you don't have homebrew installed, you should install
it by running

{% highlight bash %}
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
{% endhighlight %}

at the command line. For more information, see [http://brew.sh](http://brew.sh).


### Initialize a new repository

First, create a new directory and move into that directory

{% highlight bash %}
mkdir my_new_project && cd my_new_project
{% endhighlight %}

Then initialize that directory as a git repository

{% highlight bash %}
git init
{% endhighlight %}

It will seem as though nothing has changed, but in fact a new directory has
been created and populated. You can see it by listing everything

{% highlight bash %}
ls -a
{% endhighlight %}

It's called `.git` and it contains everything Git needs to track
changes you've made within the directory. To see the changes Git sees, run
the Git command

{% highlight bash %}
git status
{% endhighlight %}

You will see that there is `nothing to commit (create/copy files and use "git add"
to track)`. Let's follow Git's instructions and use the `add` command to 
add files. But we need something to add first, so before we add, we'll 
create a new file with a line of text in it


{% highlight bash %}
echo "Hello, Git..." > hello.txt
{% endhighlight %}

Now when we run `git status`, we see that Git sees an "untracked file" named
`hello.txt`. Great! Now let's add that as a file for Git to track.

{% highlight bash %}
git add hello.txt
{% endhighlight %}

Now when we run `git status` we see that Git is now tracking a "new file", 
`hello.txt`. So far we haven't taken a snapshot, or committed, our changes that
we could return to or reference in the future. To create that snapshot for
future reference, we must `commit` the changes we `git add`ed

{% highlight bash %}
git commit -m"first commit; created hello.txt"
{% endhighlight %}

The `-m` flag is our commit _message_. If we had left it and our message out,
Git would have started up a text editor in the shell and asked you to write one,
save the file, and quit, which would have achieved the same thing.

Now if we run `git status` we see that there is `nothing to commit (create/copy
files and use "git add" to track)`. However, we have one new thing to show, and
that is our worklog, shown by running

{% highlight bash %}
git log
{% endhighlight %}

This should show something like this:

![Git Log](/static/git_log.png)

We can use this log to go back in time if we encounter some badness and need to
get back to the last working commit. How that works would be the subject of a
more advanced guide.


## Collaboration on Git/GitHub

GitHub has an excellent interface for merging and reviewing updates from
contributors. Just last night, I submitted a 
["Pull Request" for Marisa to
review](https://github.com/northwest-knowledge-network/mdedit/pull/34). Pull
Requests are where collaborators review the changes that others have made.
If there are unittests, they should be run on a peer's computer. If the 
pull request claims to have fixed GitHub issues, the validity of that claim
should be verified. This not only serves to, but it also keeps project
maintainers up-to-date on the latest changes.

#### A Pull Request

![Pull Request](/static/github_pullreq_screenshot.png)

Unlike a centralized versioning system, each contributor's commits are kept
locally until they are intentionally "pushed" to the remote server. 

This next part of the tutorial will show you how to "push" your code changes to
a central repository that you're working on with peers.

### Tutorial Part Two: Personalize your repo and push to shared repo

In the first part of the tutorial, we created a local Git repository and added
and committed a file. Now we'll go through the steps required to push your
changes to the central repository that will be accessible and visible by all
collaborators (or the world if it's public). The Git/GitHub term for the central
repository is the _remote_ repository. Essentially this is a Git repository that
sits on a web server and is thus accessible by the outside world. GitHub is
currently making bank on adding value to remote Git repositories by offering
excellent collaborative tools.

We have to tell Git where the remote repository is and what we want to call it.
If there is only one remote repository, the convention is to call it `origin`.
Otherwise, the first remote repository is called `origin` and others are
assigned descriptive names typically based on their purpose or creator.

The remote we will be using can be viewed on GitHub by visiting 
[https://github.com/northwest-knowledge-network/tutorial](https://github.com/northwest-knowledge-network/tutorial).
However when we give the remote address, it will have a `.git` extension:
`https://github.com/northwest-knowledge-network/tutorial.git`

Do this by running

{% highlight bash %}
git remote add origin https://github.com/northwest-knowledge-network/tutorial.git
{% endhighlight %}

Then get any files that are on the origin's master branch but not on your system

{% highlight bash %}
git pull origin master
{% endhighlight %}

This should trigger the following merge prompt

![merging](/static/git_pull_merge_prompt.png)

No need to enter anything here, just save the file and quit. Now when you list
the files in the directory, you'll see a new one that Git "pulled" from the
origin's master branch, `README.txt`. 

Next, we'll create a Git branch for our changes that we'll share with our collaborators.
We do this by using the `git checkout` command with a flag, `-b` for "branch".
After the branch is created, we don't need the `-b` to use the branch.

A branch is just a container for changes that we don't necessarily want in our
master branch. Often we want to keep the master branch pristine and merge 
our changes only when someone else has reviewed our changes. We'll show here
how to create a new branch and ask someone else to review our changes.

First, create a new branch named by your name like so

{% highlight bash %}
git checkout -b matt-turner
{% endhighlight %}

but replace `matt-turner` with your name.

Then change `hello.txt` to `hello_matt-turner.txt` (but with your name). 
Edit your new file with some thoughts you'd like to share with the group.

After you've done this, try `git status` again and you'll see that Git thinks
you've deleted `hello.txt` and created a new file it hasn't heard of yet, 
`hello_matt-turner.txt`. That's fine, tell Git you really meant to remove it
by running 


{% highlight bash %}
git rm hello.txt
{% endhighlight %}

and add `hello_matt-turner.txt` like we did before. Commit the changes with a
message like we did before. Now we are ready to push our changes to the remote
repository. To do this we use the `git push` command and tell Git where exactly
to push these changes. `push` is used exclusively to move locally committed
changes to the remote repository. In our case we want to do

{% highlight bash %}
git push origin matt-turner
{% endhighlight %}

This will create a new branch on the remote repository called `matt-turner` and
put our code there. Navigate to GitHub now and you will see the option to 
open a pull request on this branch. Again, this amounts to asking a peer to
review your changes and if they do something good and don't do anything bad,
then your peer will merge your changes. GitHub can do this automatically in many
cases, this being one of them.

#### GitHub shows your new remote branch you made
![Open a pull request?](/static/github_newbranch.png)

#### The GitHub pull request page
![New Pull Request Interface](/static/github_new_pullreq.png)


## Exercises

1. Work through this tutorial and think about what was either plain wrong or
unclear and open a GitHub issue on this page's [GitHub
repository](https://github.com/mtpain/me/issues/new).

2. Initialize a new git repository to start doing version control on an existing
project. You may want to make a backup before hand.
