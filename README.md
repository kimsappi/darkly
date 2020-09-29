# Gaining access to an account
## "Resetting" a password
1. Try to reset a password.
2. There is no way to give an email, but there is a hidden input with an email that can be changed.
3. Changing the email allows the Submit button to work.

Flag: 1d4855f7337c0c14b6f44946872c4eb33853f40b2d54393fbe94f49f1e19bbb0 (font-variant uppercase)

## Member search
* Allows SQL injection.
* There is a hidden input field that specifies the table to search.
* ID 5: `First name: Flag \n Surname: GetThe`. ID 4 and 6 don't exist.
* There is a table called `users` with columns `first_name` and `last_name`
