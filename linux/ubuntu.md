# Beginner's Guide: The Linux command line

Creating folders and files:

`mkdir` (make directory)

```shell
# -p: also creates the parent directories
mkdir -p dir4/dir5/dir6
```

`ls` (list)

---

Creating files using redirection:

```shell
# >: captures the command's output into a text file
ls > output.txt
```

---

Adding text to a file:

`echo` (just prints its arguments back out again)

```shell
echo "This is a test" > test_1.txt
```

---

Linking text together:

`cat` (concatenate)

```shell
cat test_1.txt test_2.txt test_3.txt

# ?: matches any single character
cat test_?.txt

# *: matches zero or more characters
cat test_*

# >: overwrites the file
cat t* > combined.txt

# >>: appends to the file
cat t* >> combined.txt
echo "I've appended a line!" >> combined.txt

# less: pages through the whole file
less combined.txt
```

---

Moving and manipulating files:

`mv` (move)

```shell
mv combined.txt dir1

# *: matches any filename in that directory
# .: represents the current working directory
mv dir1/* .

# moving more than one file at a time
# the last one is taken to be the destination directory, and the others are considered to be files (or directories) to move.
mv combined.txt test_* dir3 dir2

# rename
mv backup_combined.txt combined_backup.txt
```

`cp` (copy)

```shell
cp dir4/dir5/dir6/combined.txt .

# give it a new file name
cp combined.txt backup_combined.txt
```

---

Deleting files and folders:

`rm` (remove)

```shell
rm dir4/dir5/dir6/combined.txt combined_backup.txt

rm folder_*

# -r: works recursively (folders & files)
rm -r folder_6
```

`rmdir` (remove directory)

```shell
rmdir folder_*

# -p: also removes the parent directories
rmdir -p dir1/dir2/dir3
```

---

Plumbing

Plumbing takes the output of one command (its standard output, or STDOUT) and feeds it directly in as the input of another command (its standard input, or STDIN).

`|`: the pipe character, which connects two commands

`wc -l`: counts the lines

```shell
ls ~ | wc -l
```

```shell
ls ~ > file_list.txt
wc -l file_list.txt
rm file_list.txt
```

`uniq`: outputs only unique lines, and works only on adjacent **matching** lines.

`sort`: sorts the lines, which puts matching lines next to each other so `uniq` can work.

```shell
sort combined.txt | uniq | wc -l
```

---

The command line and the superuser

Don’t use the root account.

Don’t use `su`.

It is better to disable the root account entirely and, instead of allowing long-lived sessions with dangerous powers, require superuser rights to be requested on a per-command basis. The key to this approach is a command called `sudo` (as in "switch user and do this command").

`sudo` (be careful with it): prefixes a command that has to be run with superuser privileges.

---

Installing software

`apt` or `apt-get`

```shell
sudo apt install tree

tree

# .
# ├── another
# ├── dir1
# ├── dir2
# │   ├── dir3
# │   ├── test_1.txt
# │   ├── test_2.txt
# │   └── test_3.txt
# ├── dir4
# │   └── dir5
# │       └── dir6
# ├── folder
# └── output.txt

# 9 directories, 4 files
```

---

Hidden files

Simply starting a name with a dot (`.`) is enough to make it hidden.

`ls -a` (show all): shows everything in a directory, including hidden files and folders.

---

Copy-pasting

`Ctrl Shift C`: copy

`Ctrl Shift V`: paste

---

Command line

- `pwd` (print working directory)

- `cd` (change directory)

  `cd /` switches to the **root directory**. Using `/` at the start of a path means "starting from the root directory".

  `cd ~` / `cd` switches to the **home directory**. Using `~` at the start of a path similarly means "starting from my home directory".

- `man` (manual): check the man page for more details.

- `reset`: reset command.

- `sudo su`: root shell.

- `uname -m` machine hardware name (eg. x86_64)

- `> filename`: empty the contents of `filename` (or create it if missing).

- `journalctl -u server_name -o cat -f`

---

Miscellaneous

Unix systems are case-sensitive.

Good naming practice: keep file names all lower case, with only letters, numbers, underscores and hyphens.

---

> [The Linux command line for beginners](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners)
>
> [The Linux Command Line by William Shotts](https://billie66.github.io/TLCL/book/index.html)
