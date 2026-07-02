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

- `journalctl -u server_name -o cat -f`: is used to stream and view live logs for a systemd service.

- `systemctl list-units --type=service --state=running`: list all currently running services.

---

Miscellaneous

Unix systems are case-sensitive.

Good naming practice: keep file names all lower case, with only letters, numbers, underscores and hyphens.

---

> [The Linux command line for beginners](https://ubuntu.com/desktop/docs/en/latest/tutorial/the-linux-command-line-for-beginners)
>
> [The Linux Command Line by William Shotts](https://billie66.github.io/TLCL/book/index.html)
>
> [Filesystem hierarchy standard](https://ubuntu.com/project/docs/how-ubuntu-is-made/concepts/filesystem-hierarchy-standard)
