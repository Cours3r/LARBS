# LARBS fork

This is a custom repo that is forked from LARBS to contain my personal dotfiles and software. I give full credit to Luke Smith for creating it, it's a wonderful script. For more information, or for the original LARBS visit https://github.com/LukeSmithxyz/LARBS 

Below is parts of the original README, which I kept for reference as I was building this.

# Luke's Auto-Rice Bootstraping Scripts (LARBS)

## Installation:

On an Arch based distribution as root, run the following:

```
curl -LO lukesmith.xyz/larbs.sh
bash larbs.sh
```

That's it.

## What is LARBS?

LARBS is a script that autoinstalls and autoconfigures a fully-functioning
and minimal terminal-and-vim-based Arch Linux environment.

LARBS was originally intended to be run on a fresh install of Arch Linux, and
provides you with a fully configured diving-board for work or more
customization. But LARBS also works on already configured systems *and* other
Arch-based distros such as Manjaro, Antergos and Parabola (although Parabola,
which uses slightly different repositories might miss one or two minor
programs).

### The `progs.csv` list

LARBS will parse the given programs list and install all given programs. Note
that the programs file must be a three column `.csv`.

The first column is a "tag" that determines how the program is installed, ""
(blank) for the main repository, `A` for via the AUR or `G` if the program is a
git repository that is meant to be `make && sudo make install`ed.

The second column is the name of the program in the repository, or the link to
the git repository, and the third comment is a description (should be a verb
phrase) that describes the program. During installation, LARBS will print out
this information in a grammatical sentence. It also doubles as documentation
for people who read the csv or who want to install my dotfiles manually.

Depending on your own build, you may want to tactically order the programs in
your programs file. LARBS will install from the top to the bottom.

As it is now, don't include commas in your program descriptions since this is
a csv. LARBS will not parse it correctly (I think...). It won't crash, but the
dialog display will be truncated. You're welcome to pull a fix for this, but
please make the end result take csvs of consensus format, since I want my progs
file to be a true csv so it will display properly on Github (trust me it
counts!).

### The script itself

The script is broken up extensively into functions for easier readability and
trouble-shooting. Most everything should be self-explanitory.

The main work is done by the `installationloop` function, which iterates
through the programs file and determines based on the tag of each program,
which commands to run to install it. You can easily add new methods of
installations and tags as well.

Note that programs from the AUR can only be built by a non-root user. What
LARBS does to bypass this by default is to temporarily allow the newly created
user to use `sudo` without a password (so the user won't be prompted for a
password multiple times in installation). This is done ad-hocly, but
effectively with the `newperms` function. At the end of installation,
`newperms` removes those settings, giving the user the ability to run only
several basic sudo commands without a password (`shutdown`, `reboot`,
`pacman -Syu`).
