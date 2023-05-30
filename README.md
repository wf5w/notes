# Notes

## Summary

This is a note taking script. It creates dated text file/files with any notes you want to stow away.
You can have dated notes that are stored daily, monthly, weekly, or yearly. The recommendation is monthly
(see the Notes format type for more information).

This script has been created on Linux specifically for bash
it is unknown if this would work in other unices or other than bash (maybe zsh)

Create a dated text file at a specific location and append text to it.

I got the initial file and idea from https://github.com/nickjj/notes : kudos and thanks.

It was good as far as it went, but I needed more. I wanted to use notes in a pipeline,
and I wanted to determine if it was being run in a tty or pipe, or no arguments or if there are 
special options -a and -n.

So this is significantly different from nickjj's version, and I thought it warrented a
completely different place to be stored.

## Usage

In the following usage examples, you might need to install xclip on your system.

```Usage:
  $ notes -h                             (print usage)
  $ notes something you want to jot down (appends that text to the file)
  $ xclip -o | notes                     (appends your clipboard to the file)
  $ notes                                (opens the file in your editor)
  $ notes | grep ...                     (pipes note to stdout for input to a filter)
  $ notes -a | grep ...                  (pipes all notes files concatenated together)
  $ notes -1 | grep ...                  (pipes the previous file only, -2 would be the 2nd previous file etc.)
```

The notes script produces a file in the following format:

  YYYY-MM.txt, or similar named files (see NOTES_FMT_TYPE) in your $NOTES_DIRECTORY (this is set below).

```Notes format type:
  yearly  -> %Y
  monthly -> %Y-%m
  weekly -> %Y-w%V
  daily   -> %Y-%m-%d
```

It is highly desireable to set your environment variables NOTES_DIRECTORY, and EDITOR, before you run this script
for the first time.

## Installation:

```
1. copy the notes and ns files to somewhere in your path
2. optionally edit your .bashrc to define the NOTES_DIRECTORY environment variable
3. optionally edit your .bashrc to define the EDITOR environment variable
4. if you edited your .bashrc file, then do a . .bashrc
5. The notes script, defaults to monthly files, if you wish something else then please edit the notes file
   and change the NOTES_FMT_TYPE to one of the one of either daily, weekly, or yearly instead.
   
```

## Tips:

* I find it very handy to set an alias to do the grep of a notes -a. Then you can use the handy gn SomeSearchText. That is: 

```
$ alias gn='notes -a | grep -i'
```


* If you need to put in escape characters, you need to quote the argument, like this:

```
$ notes this is a "line\nand the next line" and more text
```

* I also find it handy, if you have several lines to go together to put a keyword before each line of text, so
that it can be grepped out all together. Then you can do a notes -a | grep tilix: like so:

```
for instance:

$ notes tilix: ^-Alt-D - create terminal Down
$ notes tilix: ^-Alt-R - create terminal Right
$ notes tilix: Alt-(<-|->) - goto terminal left or right
$ notes tilix: Alt-(up|down) - go up or down
```

* Alternatively, do notes without any arguments to edit the notes file directly, and add keywords before and after.  Then use the ns program below. See below example

## Use the same notes directory on multiple linux clients

let say you have a notes directory (in ~/Documents/notes) on your server machine
that you want to be able to access on your client.

here is how to do that:
* create a passwordless ssh from your client to your server, and test it
* install sshfs if it is not already installed
* edit /etc/fuse.conf and uncomment user_allow_other
* mount your server notes directory to your local client mount point

remote: ~/Documents/notes 

local: create a mount point (~/Documents/notes)

$ sshfs user@remote_machine:/home/user/Documents/notes/ ~/Documents/notes

once you have this working, then look at this to set it up on fstab:
see https://ubuntuforums.org/showthread.php?t=430312 for explanation

# NS

## Summary

ns is a companion script to the notes command

ns stands for notes-sed, and is used to print out text in the notes files between two patterns

so for instance, if I edited the current notes file by just doing 
```
$ notes (without any arguments)
```

I could add a bunch of text between two labels, like for instance:

```
BASH_TIPS
some amount of text on as many lines as you want
more text
and even more text

END_BASH_TIPS
```

I could then do the following command:

```
$ ns bash_tips end_bash_tips

```

(and all the text in between would then print out)
(sort of like grep on steroids!)

## Implementation:

1. the arguments (patterns) are case insensitive 
2. to account for the very real possibility that there could be more than one of either of the patterns, this script first finds the very first line of the first argument and the very last line of the second argument

# FN

Find in files. This script is similar to doing $ notes -a | grep, but shows in which notes files, and what line number in those files, what you are trying to find.

for instance:

```
fn lvim
2023-04.txt: 00050 : lunarvim_config : define custom keys in .config/lvim/config.lua
2023-04.txt: 00051 : lvim.keys.normal_mode["<A-h>"] = "<C-w>h"
2023-04.txt: 00052 : lvim.keys.normal_mode["<A-j>"] = "<C-w>j"
2023-04.txt: 00053 : lvim.keys.normal_mode["<A-k>"] = "<C-w>k"
2023-04.txt: 00054 : lvim.keys.normal_mode["<A-l>"] = "<C-w>l"
2023-04.txt: 00056 : lvim.keys.normal_mode["h1"] = "i====== ======<esc>7hi"
2023-04.txt: 00057 : lvim.keys.normal_mode["h2"] = "i===== =====<esc>6hi"
2023-04.txt: 00058 : lvim.keys.normal_mode["h3"] = "i==== ====<esc>5hi"
2023-04.txt: 00059 : lvim.keys.normal_mode["h4"] = "i=== ===<esc>4hi"
2023-04.txt: 00060 : lvim.keys.normal_mode["h5"] = "i== ==<esc>3hi"
2023-04.txt: 00061 : lvim.keys.normal_mode["<C-A-b>"] = "gg^i#/usr/bin/env bash<cr><cr><esc ...
2023-04.txt: 00068 : better lualine style: lvim.builtin.lualine.style = "default"
```


