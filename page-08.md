[Prev](/page-07.md) | [Next](/page-09.md)

The most direct way is to add the **`--name-only`** flag. This will list the file paths changed in each commit right below the commit message.

```bash
git log --format="%h, %s" --grep=GEEK-7128 --name-only
```

-----

### Other Useful Options

Here are a couple of other great options you might find even more helpful.

#### A More Informative Option: `--name-status` 📁

This flag shows you *how* each file was changed: `A` for added, `M` for modified, `D` for deleted, etc.

```bash
git log --format="%h, %s" --grep=GEEK-7128 --name-status
```

**Example Output:**

```
a1b2c3d, feat: Implement new user dashboard
M       src/components/Dashboard.js
A       src/components/NewFeature.js
```

-----

#### For a Summary of Changes: `--stat` 📊

This flag provides a concise summary (a "diffstat") showing which files were changed, how many lines were added/deleted, and a visual representation of the changes.

```bash
git log --format="%h, %s" --grep=GEEK-7128 --stat
```

**Example Output:**

```
a1b2c3d, feat: Implement new user dashboard
 src/components/Dashboard.js | 15 ++++++++++-----
 src/components/NewFeature.js| 30 ++++++++++++++++++++++++++++++
 2 files changed, 40 insertions(+), 5 deletions(-)
```