% Debugging Your Code
% DA 101, Dr. Ladd
% Week 7

# Errors in Your Code Can Be Frustrating! 

## But here are some steps to try:

![](img/ladybug.jpg)

## Step 1: Define the Problem

Think about what you were *trying* to do vs. what happened instead. Create a hypothesis for what went wrong.

## Step 2: Read the Error Message.

Look for a line number where the error is occurring.

Sometimes the bug is in the line *before* the one that threw the error!

## Step 3: Re-run code from the beginning.

Sometimes you ran something out of order. Go back to the beginning of your code and re-run to see if that will fix it.

## Step 4: Talk it out!

Try your best to explain the problem *out loud*, preferably to a friend or teammate.

Practice [rubber duck debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging).

![](img/rubberduck.png)

## Step 5: Check instructions, documentation, and Google.

If you're lost, refer to all the resources you have: cheatsheets, lab guides, online documentation.

And when in doubt: Google the error message and see if someone else had the same problem!

## Step 6: Ask for help!

Don't let a single bug frustrate you for too long. If none of the above strategies worked, ask a classmate, TA, or instructor for help with the problem.

# Avoid Bugs before they happen!

## Save and/or Knit Often.

Remember, your RMarkdown won't knit if there are bugs. This is a great way to find them early.

## Use good names.

Name your variables and dataframes with care. Rename things to make them more clear. Good names can help you find a problem quickly.

## Start simple, and build up little by little.

Don't try to write a whole program all in one go.

## Run your code line-by-line.

Check that it works *as you go*.

## Leave yourself good annotations and comments!

Use `#` to leave comments: remind yourself what certain lines of code are doing.

## Resources for Avoiding Errors

Practice [Defensive Programming](http://adv-r.had.co.nz/Exceptions-Debugging.html)

Follow Style Guides:

[Wickham's Guide](http://adv-r.had.co.nz/Style.html)

[Google's Guide](https://google.github.io/styleguide/Rguide.html)

# Let's Try It

## Example 1

```r
# Get just three columns of mpg.

small_mpg <- mpg %>%
  filter(cyll, trans, cty)
```

## Example 2

```r
# Make a summary table of highway fuel efficiency
# in different manufacturers.

group_by(mpg) %>%
  summarize(hwy)
```

## Example 3

```r
# Make boxplot of city fuel efficiency
# in different drive trains.

ggplot(aes(cty)) +
  geom_boxplot(aes(fill=blue))
```
