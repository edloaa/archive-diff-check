# Example output

First, the hash indices have to be generated as seen below (output omitted here).

```
$ archdiff hash archive_1
$ archdiff hash archive_2
```

The two archives can now be compared using `archdiff compare`.

Checking for file copies from archive 1 in archive 2:

```
$ archdiff compare archive_1.md5 archive_2.md5
number of entries in source index:      4 archive_1.md5
number of entries in destination index: 6 archive_2.md5

NOT FOUND: 6d7fce9fee471194aa8b5b6e47267f03  archive_1/dir1/c.jpg
NOT FOUND: 26ab0db90d72e28ad0ba1e22ee510510  archive_1/dir1/b.jpg
FOUND (1): b026324c6904b2a9cb4b88d6d61c81d1  archive_1/dir1/a.jpg --> archive_2/dir1/a.jpg
FOUND (2): 48a24b70a0b376535542b996af517398  archive_1/dir2/d.jpg --> archive_2/dir1/dir3/d_copy.jpg|archive_2/dir1/d.jpg
```

Checking for file copies from archive 2 in archive 1:

```
$ archdiff compare archive_2.md5 archive_1.md5
number of entries in source index:      6 archive_2.md5
number of entries in destination index: 4 archive_1.md5

NOT FOUND: 0133314d0a03cbd9fb3beb51617eb8a0  archive_2/dir1/c.jpg
NOT FOUND: df78237bdca54fe43dae7d3eb2a81b41  archive_2/dir1/dir3/f.txt
NOT FOUND: 1dcca23355272056f04fe8bf20edfce0  archive_2/dir1/dir3/e.jpg
FOUND (1): 48a24b70a0b376535542b996af517398  archive_2/dir1/dir3/d_copy.jpg --> archive_1/dir2/d.jpg
FOUND (1): b026324c6904b2a9cb4b88d6d61c81d1  archive_2/dir1/a.jpg --> archive_1/dir1/a.jpg
FOUND (1): 48a24b70a0b376535542b996af517398  archive_2/dir1/d.jpg --> archive_1/dir2/d.jpg
```

Checking for duplicates in archive 2:

```
$ archdiff compare archive_2.md5 archive_2.md5
number of entries in source index:      6 archive_2.md5
number of entries in destination index: 6 archive_2.md5

FOUND (1): 0133314d0a03cbd9fb3beb51617eb8a0  archive_2/dir1/c.jpg --> archive_2/dir1/c.jpg
FOUND (1): df78237bdca54fe43dae7d3eb2a81b41  archive_2/dir1/dir3/f.txt --> archive_2/dir1/dir3/f.txt
FOUND (1): 1dcca23355272056f04fe8bf20edfce0  archive_2/dir1/dir3/e.jpg --> archive_2/dir1/dir3/e.jpg
FOUND (2): 48a24b70a0b376535542b996af517398  archive_2/dir1/dir3/d_copy.jpg --> archive_2/dir1/dir3/d_copy.jpg|archive_2/dir1/d.jpg
FOUND (1): b026324c6904b2a9cb4b88d6d61c81d1  archive_2/dir1/a.jpg --> archive_2/dir1/a.jpg
FOUND (2): 48a24b70a0b376535542b996af517398  archive_2/dir1/d.jpg --> archive_2/dir1/dir3/d_copy.jpg|archive_2/dir1/d.jpg
```

*(Note: It would be more useful if only files that have duplicates are printed, i.e. there should not be any lines with `FOUND (1)`. And the `FOUND (2)` only needs to be printed once. ==> TODO)*

