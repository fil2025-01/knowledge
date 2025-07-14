1. Using Relative Dates (e.g., "2 days ago")
This is likely what you're looking for. You can add the %ar placeholder to your format string, which stands for "author date, relative".

I've added it in yellow C(yellow) for better visibility.

bash
git log --format="%C(red)[%h]%C(green)%an %C(yellow)(%ar)%C(reset) --&gt; %s" -n 20
What changed:

%C(yellow)(%ar) was added to display the relative author date in yellow right after the author's name.
2. Using a Specific Date Format (e.g., "2023-10-27")
If you prefer a fixed date format, you can use the --date=short flag combined with the %ad placeholder.

bash
git log --date=short --format="%C(red)[%h]%C(green)%an %C(yellow)(%ad)%C(reset) --&gt; %s" -n 20
What changed:

--date=short: This tells Git to format dates as YYYY-MM-DD.
%C(yellow)(%ad): This uses the specified date format (%ad) and colors it yellow.
Both commands will give you a nicely formatted log with the date included. You can choose whichever style you prefer!