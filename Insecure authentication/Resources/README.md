# Broken authentication
* Loading the site sets the following cookie: I_am_admin=68934a3e9455fa72420237eb05902327
* `68934a3e9455fa72420237eb05902327` is the md5 hash of `false`
* Setting the value of I_am_admin to md5(`true`) (`b326b5062b2f0e69046810717534cb09`) and reloading the site brings up an alert containing:
`Good job! Flag : df2eb4ba34ed059a1e3e89ff4dfc13445f104a1a52295214def1c4fb1693a5c3`
