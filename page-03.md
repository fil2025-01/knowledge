[Prev](/page-02.md) | [Next](/page-04.md)

-----

## Add Dates to Your Custom `git log` Format

You can easily add author dates to your custom `git log` alias in a couple of different styles. Here are two common ways to do it.

### 1\. Using Relative Dates (e.g., "2 days ago")

To display a human-readable, relative timestamp, use the `%ar` placeholder. This is often preferred for quickly seeing how recent the work is.

The placeholder `%C(yellow)(%ar)` will display the **a**uthor date, **r**elative, in yellow.

**Command:**

```bash
git log --format="%C(red)[%h]%C(green)%an %C(yellow)(%ar)%C(reset) --&gt; %s" -n 20
```

### 2\. Using a Specific Date Format (e.g., "2023-10-27")

If you prefer a consistent `YYYY-MM-DD` format, use the `--date=short` flag combined with the `%ad` placeholder for the **a**uthor **d**ate.

**Command:**

```bash
git log --date=short --format="%C(red)[%h]%C(green)%an %C(yellow)(%ad)%C(reset) --&gt; %s" -n 20
```

Choose whichever style you prefer for your workflow\!