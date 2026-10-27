# Reflection Questions

## 1. Which issues were the easiest and which were the hardest to fix?

The style issues from Flake8 were the easiest because they are purely mechanical: adding blank lines, wrapping long lines and removing an unused import. The hardest was the eval in fine_from_rule. Simply deleting it would break the feature, so I had to decide what a fine rule is allowed to contain and write a small evaluator using the ast module that only accepts numbers, the variable days and basic arithmetic. The mutable default argument was also tricky, because fixing the syntax with None was easy but the output message was still meaningless, so I had to rethink what the number in it should represent.

## 2. Did the tools report any false positives, and did they miss anything?

I did not find real false positives. Pylint's global-statement and missing docstring messages are style opinions, but they were reasonable for this file. The more important point is what the tools missed. None of them noticed that renewing a book for more than 14 days permanently raised the fine rate for everyone, that a negative number of days gave a negative fine, that an integer was accepted as a book id, that an empty student id was ignored without a message, or that typing letters at the prompt crashed the program. These are logic and input validation bugs, and I only found them by running the program and reading its output carefully. Static analysis finds patterns, not intent.

## 3. How could static analysis be integrated into a real development workflow?

The three tools can run automatically so problems are caught before they reach the main branch. Locally, a pre-commit hook can run Flake8, Bandit and Pylint on staged files and block a commit that fails. On the server side, a GitHub Actions workflow can run the same commands on every push and pull request and fail the build if Bandit reports a medium or high issue or the Pylint score drops below a chosen threshold, such as 9.5. Editors such as VS Code can also show Pylint and Flake8 warnings while typing, which is the cheapest time to fix them.

## 4. What tangible improvements did you see in code quality, readability and robustness?

Pylint rose from 6.07 to 10.00, Bandit went from two findings to none and Flake8 went from 14 violations to none. Beyond the numbers, the code is easier to read because every function has a docstring and uses a snake_case name, f-strings replaced long concatenations, and the global state is no longer modified from inside functions. It is more secure because arbitrary code can no longer be run through the fine rule. It is more robust because files are always closed, errors are reported instead of hidden, bad input is rejected with a message and the program no longer runs automatically when it is imported.
