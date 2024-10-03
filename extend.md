# Extending and Refactoring

This exercise is about extending functionality.
Work on the repository from your previous assignment to extend it (see extensions below). 
Often, code becomes more complex while extending it.
The 'cleanliness' of the code goes down.

The skill of refactoring keeps the code clean and fresh.

This assignment is a continuation in the [Paradigm Shift](paradigm-shift.md).

## Extensions

Try at least **One** of these extensions on your code.
Mention the extensions you select in your `README.md` file.

### Extension 1: Early Warning
Customers need _early warnings_ to take action,
in addition to the alarm that you print after the limit is breached.
Introduce a 'warning' level with a tolerance of 5% of the upper-limit.

Example: If the SoC needs to be between 20 and 80, the warning-tolerance is `5% of 80` = `4`.
Warnings need to be displayed in these ranges:
- `20` to `20+4` Warning: Approaching discharge
- `80-4` to `80` Warning: Approaching charge-peak

Same for Temperature and Charge-rate.

Keep in mind: Though we are starting with warning levels for all parameters, customers may give feedback to have warnings only for _some_ parameters and not others. Minimize the change needed for such 'tuning'.

