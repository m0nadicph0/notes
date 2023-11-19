# Git Internals

Create an empty directory structure

```
$ mkdir experiment
$ cd experiment
$ mkdir -p .git/objects/{info, pack}
$ mkdir -p .git/refs/{heads, tags}
$ echo “ref: refs/heads/main” > .git/HEAD
```

Now create an object

```
$ echo "Ticking away the moments that make up a dull day"| git h
ash-object --stdin -w
35f98bd91f3a86e5e51d866b32b7213ee4b7a98b

$ tree .git
.git
├── HEAD
├── objects
│   ├── 35
│   │   └── f98bd91f3a86e5e51d866b32b7213ee4b7a98b
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

8 directories, 2 files
```

Pretty-print the contents of the blob

```
$ git cat-file -p 35f9
Ticking away the moments that make up a dull day
```

Print the type of the object 
```
$ git cat-file -t 35f9
Blob
```

Print the size of the object

```
$ git cat-file -s 35f9
49
```

Add the object into index (staging area)

```
$ git update-index --add --cacheinfo 100644 35f98bd91f3a86e5e51d866b32b7213ee4b7a98b  time.txt
```

Check the status of the repo

```
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   time.txt

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    time.txt
```

Create a tree object from the current index

```
$ git write-tree
812d80a7610ad7d433cb608b7625af507cc2f18e
```

Now look at the current directory tree

```
$ tree .git/
.git
├── HEAD
├── index
├── objects
│   ├── 35
│   │   └── f98bd91f3a86e5e51d866b32b7213ee4b7a98b
│   ├── 81
│   │   └── 2d80a7610ad7d433cb608b7625af507cc2f18e
│   ├── info
│   └── pack
└── refs
    ├── heads
    └── tags

9 directories, 4 files
```

View the properties of the tree object

```
$ git cat-file -t 812d
tree
```

```
$ git cat-file -s 812d
36
```

```
$ git cat-file -p 812d
100644 blob 35f98bd91f3a86e5e51d866b32b7213ee4b7a98b	time.txt
```
Now create a new commit object

```
$ git commit-tree 812d80a7610ad7d433cb608b7625af507cc2f18e -m "Initial Commit"
a2d71ea821e8b6dd37365302ae85747e12c0a38a
```

View the properties of the commit object.

```
$ git cat-file -p a2d7
tree 812d80a7610ad7d433cb608b7625af507cc2f18e
author Tony Stark <tony@stark.io.local> 1700400542 +0530
committer Tony Stark <tony@stark.io.local> 1700400542 +0530

Initial Commit

$ git cat-file -t a2d7
commit

$ git cat-file -s a2d7
215
```


Check status of the repository at this point of time

```
$ git status
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   time.txt

Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    time.txt
```

Now update the head

```
$ cat .git/HEAD
ref: refs/heads/main

$ echo a2d71ea821e8b6dd37365302ae85747e12c0a38a > .git/refs/heads/main
```

Check status of the repository again, at this point of time

```
$ git status
On branch main
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	deleted:    time.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

Git log now shows the commit

```
$ git --no-pager log
commit a2d71ea821e8b6dd37365302ae85747e12c0a38a (HEAD -> main)
Author: Tony Stark <tony@stark.io.local>
Date:   Sun Nov 19 18:59:02 2023 +0530

    Initial Commit

```


Now checkout the file

```
$ git checkout HEAD -- time.txt
```
