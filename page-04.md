[Prev](/page-03.md) | [Next](/page-05.md)

## How to Rebase a Feature Branch onto Staging

Rebasing is a great way to incorporate the latest changes from a parent branch (like `staging`) into your feature branch. It helps maintain a clean, linear project history.

### What is Rebasing?

Instead of creating a "merge commit," a rebase replays your branch's commits on top of the latest commits from the target branch. This results in a straight-line history that is often easier to follow.

### Step-by-Step Guide

Here are the commands to safely rebase your feature branch onto `staging`.

#### 1\. Sync Your Local `staging` Branch

First, ensure your local `staging` branch is up-to-date with the remote repository.

```bash
# Switch to the staging branch
git checkout staging

# Pull the latest changes from the remote
git pull origin staging
```

#### 2\. Return to Your Feature Branch

Switch back to the branch you are working on.

```bash
# Replace 'your-feature-branch' with the actual name
git checkout your-feature-branch
```

#### 3\. Perform the Rebase

This command reapplies your commits on top of the `staging` branch.

```bash
git rebase staging
```

If you encounter conflicts, Git will pause the process for you to resolve them. If there are no conflicts, you can skip to Step 5.

#### 4\. Handling Conflicts (If Necessary)

Conflicts are a normal part of rebasing and occur when your changes and the `staging` branch's changes affect the same lines in a file.

1.  **Identify Conflicts**: Git will list the conflicted files. You can also run `git status` to see them at any time.

2.  **Resolve Conflicts**: Open each file in your editor. You will see conflict markers that need to be removed. Choose which code to keep, or combine the changes as needed.

    ```plaintext
    <<<<<<< HEAD
    // Your changes are here
    =======
    // Incoming changes from staging are here
    >>>>>>> commit-hash-from-staging
    ```

3.  **Stage Resolved Files**: After fixing a file, stage it to mark it as resolved.

    ```bash
    git add <path/to/resolved/file.js>
    ```

4.  **Continue the Rebase**: Once all conflicts for the current commit are staged, continue the rebase.

    ```bash
    git rebase --continue
    ```

    Git will proceed to the next commit and repeat the process if more conflicts exist.

> **Tip**: If you feel overwhelmed, you can safely abort the entire rebase and return your branch to its original state with `git rebase --abort`.

#### 5\. Push Your Rebased Branch

Because rebasing rewrites your branch's history, a standard `git push` will be rejected. You must perform a "force push."

Using `--force-with-lease` is highly recommended as a safer alternative to `--force`. It prevents you from overwriting changes if someone else has pushed to the remote branch since you last pulled.

```bash
# Safely force push your rebased branch
git push origin your-feature-branch --force-with-lease
```

That's it\! Your feature branch is now up-to-date with a clean, linear history.