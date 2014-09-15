# Tutorial1

Let's go through a short session of using Git.
We will talk about the internals of Git, and its terminology as we go along.

## Basics

Let's clone a remote repo

```
git clone REPO-URL
```

Check the status of your repo

```
git status
```

At this point, we have a clone (i.e. files and full history) of the remote repository on our local machine.    
Let's create a new file (in the *working directory*)

```
echo "Hi" > a.txt
```

Add the file (to the *index*/*staging-area*)

```
git add a.txt
```

Check the status of your repo, and see what's different

```
git status
```

Now, git knows that we are interested in including `a.txt` in our next commit.    
Let's commit the file (to the *history*)

```
git commit -m "Adding a.txt"
```

Check the status of your repo again, and see what's different this time

```
git status
```

Let's stop for a second, and see what happened here:
 * A *repo* is a graph of *commit objects*.
 * A *commit object* is just a snapshot of the codebase, with some metadata (timestamp, who committed it, etc.)
 * When we run `git commit`, a new commit object is gets created and added to the graph.    
   An edge is created from the new commit to "its parent" (i.e. The most recent commit before we ran `git commit`).

Notice that, at this point, `a.txt` was committed to your **local repo** (i.e. "the clone").    
Let's push our changes to the remote repository (the one that we cloned):

```
git push origin master
```

You're probably wondering what the `origin master` arguments are ...
 * `origin` - The name of the repo we're pushing out changes to.    
   Git allows you to define *remotes*, a remote is a name that is associated with a URL of a remote repository.    
   When you clone a repo, Git automatically creates the `origin` remote, and associates it with the URL of the repository that you cloned.
 * `master` - Which branch we're pushing the changes to.    
   By default, a Git repo starts with a single branch, called `master`. More on that later ...

Notice that you can push changes to other repos, not just the one you cloned. Let's try that as follows:
 * *Fork* (i.e. clone on GitHub) the remote repo, using a different GitHub account.
 * Add a remote called `other` and set its URL to the URL of our forked repo.
 * Try to push some changes to `other`.
 * Notice: We will need to grant our user push permissions.
  
## Summary

 * We can clone a repo.
 * We can work with a local clone (i.e. commit changes).
 * When we are ready, we can push the changes to a remote repo.    
   The remote repo doesn't have to be the one we originally cloned - As long as you have push permissions, and the changes that you are pushing "make sense to Git", you're good.    
   If Git cannot safely push the changes, it will ask you to first pull the changes from the remote repo, resolve any conflicts, and only then it will allow you to push the changes back.


## Branching

So far, we've pushed changes between different repositories.    
Branching is used to work on different features/changes in parallel on the same repo.

Let's create a new branch (and switch to this branch)

```
git checkout -b feature1
```

We use the `checkout` command to switch between branches

```
git checkout master
git checkout feature1
```

Let's make some changes in our branch

```
echo "Bye" > b.txt
git add b.txt
git commit -m "Adding b.txt to the feature1 branch"
```

Notice that the changes exist only in the `feature1` branch

```
git checkout master
ls
git checkout feature1
ls
```
## Modifying files

Let's modify `a.txt`

```
echo "Modification 1" >> a.txt
git commit
```

Oops, Git complains that there is nothing to commit (and give us a hint).    
The point is that Git doesn't assume that you want to commit modified files by default. For those of you who are used to SVN, this behaviour may seem a little odd. It will make sense soon.    
We have two options now, we can either run `git add a.txt`, or we can run `git commit` with the `-a` option, which tells Git to add all modified files to this commit.

```
git commit -a -m "Modifying a.txt"
```

## Resolving Conflicts

Next, let's switch to the `feature1` branch, and make other modifications to `a.txt`
