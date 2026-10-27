# Issue Table: library_system.py

Line numbers refer to the original file.

| Issue | Type | Line(s) | Description | Fix Approach |
|---|---|---|---|---|
| Use of eval (Bandit B307, Pylint W0123) | Security, Medium | 45 | fine_from_rule passes a user supplied string to eval, so a rule such as __import__('os').system(...) runs arbitrary code. | Replaced eval with an ast based evaluator that allows only numbers, the name days and the operators + - * /. Anything else raises ValueError. |
| Try, except, pass (Bandit B110) | Security, Low | 21 to 22 | return_book swallows every error silently. | Catch only KeyError and ValueError and print a clear message. |
| Bare except (Pylint W0702, Flake8 E722) | Bug | 21 | Catches every exception, including KeyboardInterrupt, and hides real bugs. | Catch only KeyError and ValueError. |
| Mutable default argument (Pylint W0102) | Bug | 9 | dates=[] is created once and shared by every call, so the 'loan dates on record' count was shared between unrelated students. | Default changed to None and a new list is created inside the function. The message now shows how many books that student has on loan. |
| File opened without with and without encoding (Pylint R1732, W1514) | Code quality | 34, 40 | Files may stay open if an error occurs, and the encoding depends on the platform. | Use with open(..., encoding='utf-8') with json.load and json.dump. |
| Global statement (Pylint W0603) | Code quality | 28, 35 | global FINE_PER_DAY and global loans make state changes hard to follow. | load_records now uses loans.clear() and loans.update(). renew_book no longer touches the global fine rate. |
| Renewal changes the fine rate | Logic bug (not flagged by any tool) | 28 to 30 | Renewing for more than 14 days permanently raised FINE_PER_DAY for every student (Fine for 5 days printed 15 instead of 10). | Removed the change to the global fine rate. |
| Negative fine | Logic bug (not flagged by any tool) | 24 to 25 | calculate_fine(-3) returned -9. | Return max(days_overdue, 0) * FINE_PER_DAY. |
| Integer book id accepted | Logic bug (not flagged by any tool) | 56 | A loan was recorded for book id 4521 although ids must be strings. | Reject non-string book ids and print the reason. |
| Empty student id fails silently | Logic bug (not flagged by any tool) | 10 to 11 | add_book_loan returned without any message when the student id was empty. | Print 'Loan rejected: student id is empty'. |
| Unvalidated int(input) | Logic bug (not flagged by any tool) | 62 to 63 | Typing a non-number at the days prompt crashed the program with ValueError. | Wrap in try and except ValueError, then use 0 days with a message. |
| Unused import os (Pylint W0611, Flake8 F401) | Code quality | 2 | Imported but never used. | Removed. |
| Function name renewBook (Pylint C0103) | Style | 27 | Not snake_case. | Renamed to renew_book. |
| String formatting (Pylint C0209) | Style | 14 | % formatting where an f-string is clearer. | Converted to f-strings, including the string concatenation on line 31. |
| Missing docstrings (Pylint C0114, C0116) | Style | 1 and every function | Module and every function lack docstrings. | Added a module docstring and a docstring for every function. |
| Code runs on import | Design | 68 | main() is called at module level. | Wrapped in if __name__ == "__main__":. |
| Blank lines (Flake8 E302, E305) | Style | 9, 16, 24, 27, 33, 39, 44, 47, 52, 68 | Two blank lines are required around top level definitions. | Added the missing blank lines. |
| Line too long (Flake8 E501) | Style | 14, 31 | Lines are 90 and 91 characters, the limit is 79. | Stored the count in a variable and used an f-string, so no line is over 79. |

## Results

| Tool | Before | After |
|---|---|---|
| Pylint | 6.07/10 (22 messages) | 10.00/10 (0 messages) |
| Bandit | 2 issues (1 Medium, 1 Low) | No issues identified |
| Flake8 | 14 violations | 0 violations |
