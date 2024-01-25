# Build custom vidardb/postgresql:rocksdb-6.2.4_demoXX images 


## Adding customer layers to `vidardb/postgresql:rocksdb-6.2.4`

The very first layer `vidardb/postgresql:rocksdb-6.2.4_demo02` was more a personal refresher for adding a customer docker layer.

The only different is that I started a `notes.txt` in the root directory

```
root@290796457c09:/# ls -ltr notes.txt
-rw-r--r-- 1 root root 29 Jan 24 21:23 notes.txt
root@290796457c09:/# cat notes.txt
psql -d postgres -U postgres
```
