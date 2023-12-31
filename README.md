# Git Tutorial

Git is a distributed version control system, in which complete codebase with its full history is stored. This tutorial aims to provide you with fundamental knowledge and provide solutions to common problems.

## Configuration

* Setup your Git username:

```sh
git config --global user.name "Mona Lisa"
```

* Verify your username (change with previous command):

```sh
git config --global user.name
```

* Setup your email (WARNING: If you have public Git repositories on ex. Github, everyone will be able to view this address, it is recommended to view [this turorial](https://docs.github.com/en/account-and-profile/setting-up-and-managing-your-personal-account-on-github/managing-email-preferences/setting-your-commit-email-address) and setup users.noreply.github.com address):

```sh
git config --global user.email "YOUR_EMAIL"
```

* Verify your email:

```sh
git config --global user.email
```

* [Install Github CLI](https://github.com/cli/cli#installation) (search for MSI installer for Windows for the easiest way to install) and connect your Github account with Git with:

```sh
gh auth login
```

* Configure your default branch to have a name *main*:

```sh
git config --global init.defaultBranch main
```

## Basics

**A commit** is a building block of your Git history. It contains changes you made and other information like id, title, description, etc.

You can initialize a Git repository with:

```sh
git init
```

This command creates *.git* folder, which depending on your system configuration may not be instantly visible. In Windows select file explorer option to show hidden files and on Linux, type the *ls* with ex. *-a* parameter. When you want to remove a Git repository, simply delete *.git* directory. Important: *.git* should not be modified manually.

Let's say you add a file called *file* to the same place in which *.git* folder resides. It is not yet added to Git history. Type the following command to view what files are not yet commited:

```sh
git status
```

It can give you output like this:

    On branch main

    No commits yet

    Untracked files:
    (use "git add <file>..." to include in what will be committed)
        file

    nothing added to commit but untracked files present (use "git add" to track)

Let's go over each line individually. First there is information about some *main* branch. You can think of **branches** as separate histories, some may contain common commits in their history. We'll go over them in detail in the next chapter.
Next line informs us that there are no commits in our repository. But there is an untracked file. Such files can be **staged**, which means telling Git, when you prepare your commit, that you want this file to be added to the commit. Tell Git to stage your file with:

```sh
git add file
```

Now check:

```sh
git status
```

Output looks like this:

    On branch main

    No commits yet

    Changes to be committed:
    (use "git rm --cached <file>..." to unstage)
        new file:   file

Great! Git knows what files to add to our first commit. In this output there is also a hint, how to unstage our file. If you change your mind and don't want to commit this file run:

```sh
git rm --cached file
```

You can add it again with *git add ...*

With *git add* you can add entire directories. So to add all files from your Git project, make sure you are in the **root** (highest directory in the hierarchy, beginning of a particular folder structure) of the project:

```sh
git add .
```

Okay, when have our file staged. To commit:

```sh
git commit -m "first commit"
```

The *-m* argument tells git that the title of our commit will be *first commit*.
If you run just *git commit*, a text editor would open for you to write the title and optionally a description. It is recommended to do *git commit*, when you have to describe your changes in  detail. After commiting we get information, which tells us about changes in the project:

    1 file changed, 0 insertions(+), 0 deletions(-)
    create mode 100644 file

When we run *git status*:

    On branch main
    nothing to commit, working tree clean

Marvelous! Our commit has been created. How do we view it? With:

```sh
git log
```

This command displays a scrollable list of commits with their ids, authors, etc. To exit press **q** for quit.

## Remote repository

First go the Github website, log in and create a new repository on the website.

Copy the link to the new repository on Github and run:

```sh
git remote add origin YOUR_LINK
```

Then to **push** changes to the website:

```sh
git push -u origin main
```

*-u* signals to create branch *main* in origin (origin is set to Github repository, which you entered in the previous command). You have to run such command only when you create a branch that does not exist in remote repository. From now when you commit to *main*, you can push your changes with:

```sh
git push
```

Now there is an option to edit files on the Github website. If you do so, you can then **pull** your new changes, that you made on the website with:

```sh
git pull
```

## Branches

### Why branching?

When working in a team, where each person works on a different feature, everyone can create separate branches for each feature, make commits on each branch and then these branches can be merged into *main* branch. But why not just commit to the main branch all the time? Let's see an example.

Lucas creates a repository and sends changes to Github. Another person working on the same repository adds a new commit. Now, Lucas commits new changes and wants to send them to Github too. He gets this error, which tells him that he has to download new changes, which the other person made:

    ! [rejected]        main -> main (fetch first)
    error: failed to push some refs to 'https://github.com/LucasHazardous/test.git'
    hint: Updates were rejected because the remote contains work that you do not
    hint: have locally. This is usually caused by another repository pushing to
    hint: the same ref. If you want to integrate the remote changes, use
    hint: 'git pull' before pushing again.
    hint: See the 'Note about fast-forwards' in 'git push --help' for details.

Now he has to undo his commit, download new commits and add his changes again to the newest commit!

When working alone, having a single branch is acceptable, but with multiple people its better to have multiple branches and then when one person finishes their feature, they can **merge** them with the *main* branch. If a file from your branch was edited before merge, you would get a **merge confict**, which you could resolve (meaning choosing appropriate changes) upon merging. That way you won't have to constantly download changes and edit changing files to make a new commit, but rather you will be able to focus on making your feature and deal with merge conflicts later, when you make a **pull request**. Continue to learn what's a **pull request**. 

### Managing branches

To list branches in your current repository use:

```sh
git branch
```

This will list all your branches, with your current branch highlighted and annotated with \*. Exit with *q*.

How to create a new branch?

```sh
git checkout -b feature1
```

**Checkout** can create branches and it allows you to switch between them. If you check you current branch with *git branch*, you will see that it changed to feature1. To go back to main use:

```sh
git checkout main
```

Let's use *feature1* for something! Switch with *git checkout feature1*. Now create a file called *feature_file*, stage it and commit.

Check output of *git log*, yours may be slighly different:

    commit e3d54cbfe6d9b0a56e6f29453ab7485c793556af (HEAD -> feature1)
    Author: LucasHazardous <70599201+LucasHazardous@users.noreply.github.com>
    Date:   Fri Sep 1 15:08:20 2023 +0200

        feature_file

    commit c86992f5e97b40864cf3e5b815c5da6c921a082b (origin/main, main)
    Author: LucasHazardous <70599201+LucasHazardous@users.noreply.github.com>
    Date:   Fri Sep 1 14:34:38 2023 +0200

        first commit

    (END)

**HEAD** indicates which branch we are currently on, which is feature1. You see that main branch is one commit behind feature1. There is also origin/main, which is a remote version of main, that is in the Github repository.

So let's do *git checkout main*. Now the feature_file disappeared. Actually, it didn't - it is stored on the other branch. Go back to feature1 and push it to the Github repository:

```sh
git push -u origin feature1
```

Now, if you go to the Github repository and switch branch to feature1, you will be able to create a **pull request** to **merge** new changes into main branch. When you create a pull request in Github repository, others can review your code, approve it, comment on it, etc. When a pull request gets merged, all commits from the branch that is merged are added to ex. main and also new commit is created with the title of the pull request, in which changes chosen in merge conflicts resolution are located. Besides pull request on Github, for the sake of this tutorial we can merge to main locally:

```sh
git checkout main
git merge feature1
```

Now changes from feature1 got merged into main. The commit with feature_file got copied to main branch. We don't need feature1 branch anymore, so let's remove it.

```sh
git branch -d feature1
```

The command above removed it locally. To remove it in Github, use the web interface or the command below.

```sh
git push origin --delete feature1
```

When you remove a branch with web interface or someone else removes it with the command above, your local git repository will still think that this branch exists in the remote repository. To update this information use *git fetch* with *-p* option to prune (delete) branches deleted remotely:

```sh
git fetch -p
```

This deletes gone branches that look like this **origin/feature1**, which means that **feature1** will still be present. Delete it locally with *git branch -d feature1*.

## Tagging

Sometimes you will want to mark specific points in a repository’s history. Tags are a perfect way to do this.

Let's create a new tag named first_tag:

```sh
git tag first_tag
```

Now you can check both *git log* and *git tag* to see that the tag has been added. You can switch to your tag with:

```sh
git checkout first_tag
```

After switching we get the following output.

    Note: switching to 'first_tag'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by switching back to a branch.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -c with the switch command. Example:

    git switch -c <new-branch-name>

    Or undo this operation with:

    git switch -

    Turn off this advice by setting config variable advice.detachedHead to false

    HEAD is now at 59ab498 added feature_file

It tells us that we can create a branch starting with our current commit using *git switch -c new-branch-name* or we can go back to the latest commit with:

```sh
git switch -
```

If we want our tag to appear in remote repository as well, we have to push it there:

```sh
git push --tags
```

We can delete the tag locally:

```sh
git tag -d first_tag
```

or remotely with Github web interface, in which case we have to sync your local repository with:

```sh
git fetch -p -P
```

That command will delete both remote branches and tags that are gone in the remote repository.

## Common problems

* **Go to a particular commit**

```sh
git checkout COMMIT_HASH
```

* **Edit previous commit title**

```sh
git commit --amend
```

* **Remove previous commit, keep changes present and staged**

```sh
git reset --soft HEAD^1
```

* **Remove previous commit with changes**

```sh
git reset --hard HEAD^1
```

* **You made some uncommited changes, there is a commit on the same branch that you need to download**

```sh
git stash
git pull
git stash pop
```

## End

This is the end of this tutorial. It covers a lot of Git functionality, so feel free to go over it again as many times as you want, testing different commands. Practice is the best teacher, the best way to learn Git is to use it and test different stuff. Good luck!