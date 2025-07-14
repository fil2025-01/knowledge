[next](/page-05.md)

Of course! Rebasing your feature branch onto staging is a great way to incorporate the latest changes and maintain a clean, linear project history. Here is a step-by-step guide on how to do it safely.

What is Rebasing?
Think of it like this: instead of merging staging into your branch and creating a "merge commit," a rebase takes all the commits you've made on your feature branch and replays them, one by one, on top of the latest commit from staging.

This results in a straight-line history that is often easier to read and follow.

Step-by-Step Guide
Here are the commands to rebase your feature branch onto staging.

1. Update Your Local staging Branch
First, make sure your local staging branch has all the latest changes from the remote repository.

bash
# Switch to the staging branch
git checkout staging

# Pull the latest changes
git pull origin staging
2. Switch Back to Your Feature Branch
Now, go back to the branch you are working on.

bash
# Replace 'your-feature-branch' with your actual branch name
git checkout your-feature-branch
3. Perform the Rebase
This is the core command. It tells Git to take your current branch (your-feature-branch) and re-apply its commits on top of staging.

bash
git rebase staging
If there are no conflicts, Git will complete the process, and you can move to step 5.
If there are conflicts, Git will pause the rebase and ask you to resolve them.
4. Handling Conflicts (If They Occur)
Conflicts happen when changes in staging and your feature branch affect the same lines in the same file. Don't worry, this is a normal part of the process.

Identify Conflicted Files: Git will list the files that have conflicts. You can also run git status to see them.
Resolve Conflicts: Open each conflicted file in your code editor. You will see conflict markers like this:
plaintext
```
    <<<<<<< HEAD
    // Your changes from the feature branch are here
    =======
    // The incoming changes from the staging branch are here
    >>>>>>> commit-hash-from-staging
```
You need to edit the file to remove the markers and decide which code to keep.
Stage the Resolved Files: After you've fixed a file, you must tell Git you're done with it.
bash
git add <path/to/your/resolved-file.js>
Continue the Rebase: Once all conflicts for the current commit are resolved and staged, continue the rebase.
bash
git rebase --continue
Git will then move to the next commit in your branch and repeat the process if necessary.
Tip: If you get overwhelmed or make a mistake, you can always safely stop the rebase and return to your branch's original state with: git rebase --abort

5. Push Your Rebased Branch
Because you have rewritten the commit history of your branch, a standard git push will be rejected. You need to "force push".

It is highly recommended to use --force-with-lease, as it's a safer option that won't overwrite work if someone else has pushed to the same branch in the meantime.

bash
# This is the safe way to force push
git push origin your-feature-branch --force-with-lease
And that's it! Your feature branch is now up-to-date with staging and has a clean history.