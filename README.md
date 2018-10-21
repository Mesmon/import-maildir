# import-maildir

This application allows to import emails directly from a maildir formatted folder to WildDuck email database, skipping any IMAP based syncing. This is mostly useful for speed and also if you do not know user passwords required for IMAP access.

## Install

```bash
npm install --production
```

## Config

Database config can be changed in [config/default.toml](./config/default.toml)

## Run

```bash
./bin/import-maildir user1:/maildri/path1 user2:/maildri/path2 userN:/maildri/pathN
```

Where

*   _userX_ is either WildDuck user ID (24 byte hex), username or an email address. This uset must already exists (should have been created via [WildDuck API](https://api.wildduck.email/#api-Users-PostUser))
*   _/maildir/pathX_ is the maildir folder for that user

## Example

Create users

```
curl -i -XPOST http://localhost:8080/users \
-H 'Content-type: application/json' \
-d '{
  "username": "user1",
  "password": "verysecret",
  "address": "user1@example.com"
}'
curl -i -XPOST http://localhost:8080/users \
-H 'Content-type: application/json' \
-d '{
  "username": "user2",
  "password": "verysecret",
  "address": "user2@example.com"
}'
```

Run the importer

```
./bin/import-maildir user1:./fixtures/user1 user2:./fixtures/user2
```

The output should look like this

```
Processing 2 users
info 1540106306097 Master Starting importer
info 1540106306098 Master Generate folders only: NO (--foldersOnly)
info 1540106306104 Master Forked worker 66829
...
```

Once import has finished there should be some log files in current directory. See _messagelog.txt_ to see which file from maildir was imported into which message in WildDuck message database:

*   _STORENEW_ means that message was stored
*   _STORESKIP_ means that a duplicate was found and message was not stored
*   _STOREFAIL_ means that message was not stored because of some error

## License

**EUPL 1.1 or later**
