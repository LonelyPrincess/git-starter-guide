# Workflow

## Basic workflow

The two most basic operations in Git are `add` and `commit`, which you'll need to use all the time when working with this tool. The following image illustrates this process:

![Basic Git workflow](images/basic_workflow.png "Basic Git workflow")


With `add` you can choose the files which changes will be included in the next commit.

```
git add [file name | folder name | regular expression]
```

You can check the files you have modified and those which you've added to the next commit by using the `status` command.

```
git status
```

The output of this command will look like the following:

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   src/App.js

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   src/App.test.js
```

The files listed as not staged for commit won't be included in the next commit you make, while the others will indeed be part in the set of changes you'll be creating when you use the `commit` command.

In case you've added a file by mistake, you can remove it from the staging area by using this command:

```
git reset [file name | folder name | regular expression]
```

Once you have successfully selected all of the files you'll be adding to your next commit, it's time to actually create a commit. In order to do so, you'll need to use the `commit` command:

```
git commit -m "[message]"
```

This will create a new commit with the message you passed as a parameter. This `-m` option will prove to be useful if you want to write one-line short commit messages. If you want your commit message to include more details, then you'll be better by using this instead:

```
git commit
```

Once you do this, the editor you set as default editor while installing Git will open up and you'll be able to write a commit message with as much paragraphs and details as you'd like.

Please, **note that the first line of your commit message will be used as its title** in most Git graphic interfaces, so make sure it's short and summarizes what the commit is about. You can include as many details as you want in the following lines.

## Synchronize with remote repository

If your project is linked to a remote repository, **it is important to always keep it up-to-date**, specially if you're working with other people.

Whenever you want to send your changes to the remote repository, you should use `push`:

```
git push origin [branch name]
```

This will upload the contents of a single branch (`master`, for example), but if you're working with multiple branches in your project, you can upload them all at once by using the following command:

```
git push --all
```

If you're working with other developers, downloading their changes every now and then is a must. Indeed, you should ensure you get their updates often, so you avoid getting into conflicts in the future.

In order to bring other people changes into your repository, you must use `pull`:

```
git pull origin [branch name]
```

Before running this, make sure you're located in the same branch you will be downloading, as the remote changes will be merged into yours.

## Multiple branches flow

Working with a single branch can seem like the easiest choice at first, but in doing so you won't be exploiting one of the greatest features Git has to offer. Mainly when there are multiple developers working in the same project, using a single branch can easily become hard to handle.

Here it is when branches enter into action to save the day. Branches allow you to have different versions of the code and easily switch between them, as if you had multiple copies of the code in different folders and you could jump from one to another anytime.

This is particularly useful when you're developing a new functionality and testing it, so whatever change you do, you do it in your own "folder" and whichever experiment you make will not affect the other people working on the same project.

### Branch management

You can create new branches anytime by using the following command:

```
git branch [branch name]
```

The branch you're in at the time you run this command is important, as **the new one will contain all of the commits made so far in their parent branch**. This is pretty much as if you had duplicated the folder in which you had the original code and start working on the new one. Any change you make on the newly created branch will not change anything in the original one.

After running the `branch` command you would have created a new branch, but you would still be working on the parent branch. If you want to switch to a different branch, you should run the following command:

```
git checkout [branch name]
```

When you have done that, you'll be now located into the branch you chose. This is no different than having opened one of the duplicate folders you had created with your application in different states, and working on it as if the other didn't exist.

In this case, after creating various commits in your new branch, if you switch back to the parent branch you'll notice the code is exactly as it was before you used the `branch` command.

### Branch merging

The biggest difference between Git branches and having multiple folders in your computer with different versions of your code is that, in the last case, mixing them into one, final version, can become a real nightmare. First, you must remember exactly what you did in each one to know exactly which files you touched and what changes you really do intend to move to the final version. Second, it takes a lot of manual work to, not only remember, but perform that mix yourself.

With Git, merging two versions of the same code together is much easier. When you mix two branches together, Git will automatically find out the differences between them and change the files which were modified in each of the branches. It will also keep the list of commits performed in both, so you can still have a full history of the changes that happened in the project, regardless of which branch they happened into.

The code to mix two branches together is `merge`, which can be used as follows:

```
git merge [branch name]
```

What this command does is **bringing the changes that have happened in the branch passed as parameter to the branch you're currently working on**. If you're located, for example, in the `develop` branch, doing `git merge database` will take all the changes into the `database` branch and add them into the current `develop` branch. This operation will **not** alter the contents of the `database` branch in any way.

After performing a merge, two outcomes are possible:

* Git is able to merge the two branches automatically.

* A conflict arise, so human interaction is required to merge the two branches together.

The second output is highly likely to happen if the team is not well organized and there are multiple people working on the same files and even lines. Given the case, Git won't be able to tell which of the changes is right, and so the developer calling `merge` should review the conflicting files and manually fix the problem.

When opening the files in which a conflict occurred you should see something like this.

```
<<<<<<< HEAD
changes in current branch
=======
changes in the branch you're trying to merge
>>>>>>> BRANCH-TO-MERGE
```

The lines located after the `HEAD` marker are the code that was originally in the branch you're currently in. After the line `=======` you'll see the code that was added in the branch you're trying to merge.

You have three choices here:

* Discard the current changes and keep the ones in the branch you want to merge.

* Discard the incoming changes and keep the content that was originally in the branch you're working in.

* Combine the changes in both branches, so you can keep the functionality defined in both.

Remember to always try out the application after the merge to ensure you did so properly and that nothing was broken during the operation.

Once the conflict has been resolved, you must let Git know by running `add` and `commit` commands. After that's been done, the merge would now be officialy completed.

It's important you **do not forget to remove the merge markers** (the lines Git adds to the files to let you know which piece of the code is provoking the conflict) to the files you modified. If you haven't, Git will throw an error saying you still have conflicts that you haven't resolved and will not allow you to finish the merge.
