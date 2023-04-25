# Bash script development with on-save live changes using entr (20230425172311)

- entr is a program that can be installed that can we told to watch a set of files, and run a given command when any of those files changes.
- can't believe I've never heard of this program until a few days ago.

## Usage
- install the `entr` package from your package manager of choice.
- open up a script file you want to work on.
- Assuming your script is called `myscript`
- in a separate terminal pane, run `ls myscript | entr myscript`.
    - This will tell `entr` to watch the myscript file and call that script whenever the file is updated.
- I use this in tmux and have a left-hand pane with the script im working on, and the right-hand pane with `entr` running.

## References
- `man entr` locally after installing it.
- [entr man page on Arch Linux website](https://man.archlinux.org/man/community/entr/entr.1.en)
