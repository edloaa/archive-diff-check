# archive-diff-check

*archdiff* is a very simple tool that can be used to determine if files in an *archive 1* have a set or subset of copies in an *archive 2*. The locations (folder paths) of the individual files are irrelevant when the archives are being compared, since the comparison is based on hash values.

### Scenario

Consider having two archives (`archive_1` and `archive_2`). Their folder structures and files are indicated below. The hashes of the files are given in parenthesis.

```
archive_1
|
|___ dir1
|    |___ a.jpg (hash: 1111)
|    |___ b.jpg (hash: 2222)
|    |___ c.jpg (hash: 3333)
|
|___ dir2
     |___ d.jpg (hash: 4444)
```

```
archive_2
|
|___ dir1
     |___ a.jpg (hash: 1111)
     |___ c.jpg (hash: 1729)
     |___ d.jpg (hash: 4444)
     |
     |___ dir3
          |___ d_copy.jpg (hash: 4444)
          |___ e.jpg      (hash: 5555)
          |___ f.txt      (hash: 6666)
```

When comparing the hashes from `archive_1` to those from `archive_2`, the following statements can be made:

- `a.jpg`: found in archive 2
- `b.jpg`: *NOT* found in archive 2
- `c.jpg`: *NOT* found in archive 2 (there is a file with same name but hash is different)
- `d.jpg`: found in archive 2 (at *two* different locations)

*archdiff* can help to compare these archives and generate output similar to the output indicated below.

```
FOUND (1): 1111 archive_1/dir1/a.jpg --> archive_2/dir1/a.jpg
NOT FOUND: 2222 archive_1/dir1/b.jpg
NOT FOUND: 3333 archive_1/dir1/c.jpg
FOUND (2): 4444 archive_1/dir2/d.jpg --> archive_2/dir1/d.jpg|archive_2/dir3/d_copy.jpg
```

### Usage of archdiff

To determine which files from `archive_1` exist in `archive_2`, all files from both archives have to be indexed with hashes (md5sum). Subsequently, the indices are being compared (by trying to `grep` for the hashes).

**Step 1:** Generate hash index for each archive (\*.md5)

```sh
$ archdiff hash "archive_1"
$ archdiff hash "archive_2"
```

**Step 2:** Compare the indices of the archives

```sh
archdiff compare "archive_1.md5" "archive_2.md5" >diff.txt
```

It is convenient to redirect the output to another file (e.g. *diff.txt*).

Run `archdiff help` for more information.

### License

**Disclaimer:** This script was written for my personal use when I had to compare several large archives containing lots of pictures and other data. Use or reuse at you own risk.

**License:** GPL-3.0-or-later

