# SSH into the VPS

.ssh/config

```
Host vmiss-hk
HostName xxx.xxx.xx.xxx
User root
IdentityFile ~/.ssh/id_vmiss_hk
ProxyCommand "C:\Program Files\Git\mingw64\bin\connect.exe" -S 127.0.0.1:7897 %h %p
```

```shell
ssh vmiss-hk
```

# The Linux command line

- `pwd` (print working directory)

- `cd` (change directory)

  `cd /` switches to the **root directory**. Using `/` at the start of a path means "starting from the root directory".

  `cd ~` / `cd` switches to the **home directory**. Using `~` at the start of a path similarly means "starting from my home directory".

- `man` (manual): check the man page for more details.

- `reset`: reset command.

# Creating folders and files

`mkdir` (make directory)

```shell
# -p: also creates the parent directories
mkdir -p dir4/dir5/dir6
```

`ls` (list)

## Creating files using redirection

```shell
# >: captures the command's output into a text file
ls > output.txt
```

## Copy-pasting

`Ctrl Shift C`: copy

`Ctrl Shift V`: paste

## Adding text to a file

`echo` (just prints its arguments back out again)

```shell
echo "This is a test" > test_1.txt
```

## Linking text together

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

# Moving and manipulating files

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

## Deleting files and folders

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

# Plumbing 

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
# The command line and the superuser

Don’t use the root account.

Don’t use `su`.

`sudo`: is used to prefix a command that has to be run with superuser privileges.

# Miscellaneous

Unix systems are case-sensitive.

Good naming practice: keep file names all lower case, with only letters, numbers, underscores and hyphens.


