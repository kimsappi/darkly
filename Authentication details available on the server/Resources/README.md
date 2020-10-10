# Authentication details available on the server
`robots.txt` contains 2 entries:
* `/whatever/`
* `/.hidden/`

## /whatever/
* Contains `htpasswd`: `root:8621ffdbc5698829397d97767ac13db3`
* `reverseMD5(8621ffdbc5698829397d97767ac13db3)` == dragon
  * https://md5.gromweb.com/?md5=8621ffdbc5698829397d97767ac13db3
* These credentials don't work on the main login page
* There is an admin panel login at http://192.168.99.100/admin/
* Log in with root:dragon and you get the flag

# How to fix
The file `htpasswd` should not be in the document root and customarily it would be named `.htpasswd`, which some web servers wouldn't serve by default.
