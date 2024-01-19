Dropbox Key Decryption for macOS
================================

This is based off the Linux [implementation](https://github.com/newsoft/dbx-keygen-linux) with some modifications for macOS with the help of [Look inside the box](https://github.com/anvilventures/lookinsidethebox).

## Usage

1. git clone git@github.com:dnicolson/dbx-keygen-macos.git
2. cd dbx-keygen-macos
3. pip2 install pyyaml==5.3.1 crypto pycrypto simplejson pbkdf2
4. python2 dbx-keygen-macos.py


```
KEYSTORE: unique_id = u'/Users/dave/.dropbox/instance1' 1319963
KEYSTORE: got user key (0, f2fcaab781187a3486cebdfc8b5cf6cb)
User key:  f2fcaab781187a3486cebdfc8b5cf6cb
Database key:  02179b253b82f908521b0fae20333232
```

### Legacy Encryption

If the above does not work it could be due to legacy encryption, the Dropbox Python code checks if the `old` argument is true and performs this:

`return ('%d_%s' % (inode, uuid)).encode('ascii')`

Instead of this:

`return ('%d' % inode).encode('ascii')`


The UUID can be retrieved with the following command:
`nvram efi-boot-device | sed -e 's/.*<key>Path<\/key><string>\(.*\)/\1/g' | cut -c2-37`


## Decryption

This is explained in these [slides](https://asfws12.files.wordpress.com/2012/11/dropbox-asfws-version.pdf) and the [repo](https://github.com/newsoft/sqlite3-dbx).

```
$ ~/sqlite3-dbx/sqlite3 -key 02179b253b82f908521b0fae20333232 ~/.dropbox/instance1/config.dbx
SQLite version 3.7.0
Enter ".help" for instructions
Enter SQL statements terminated with a ";"
sqlite> .tables
config
sqlite> select * from config;
```
