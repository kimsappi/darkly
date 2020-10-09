# Insecure HTML tag use
1. `wget -r` will return a list of pages linked to.
2. There is a link to http://192.168.99.100/index.php?page=media&src=nsa, which is an image of an NSA logo. The link is at index.
  * This is odd, because the other images aren't links and the page appears to be pretty useless.
3. Changing the src to anything else returns the 'Sorry Wrong Answer' image from incorrect logins.
  * Results in a 404, as it tries to include `/<src>` in the page in an `<object>` tag
  * Can't be used for XSS as HTML as escaped
  * Can be used for XSS like http://192.168.99.100/index.php?page=media&src=data:text/html,%3Cscript%3Ealert(%27hello%27);%3C/script%3E
  * Flag is returned when base64-encoded JS is used like http://192.168.99.100/index.php?page=media&src=data:text/html;base64,PHNjcmlwdD5hbGVydCgnaGVsbG8nKTs8L3NjcmlwdD4=

# How to fix
Don't blindly trust user input. As the page should probably be used only for finding media on the server, check that the media exists and return that instead of using a potentially executable tag.
