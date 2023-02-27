# notes

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

In the following usage examples, you might need to install xclip on your system.

```Usage:
  $ notes something you want to jot down (appends that text to the file)
  $ xclip -o | notes                     (appends your clipboard to the file)
  $ notes                                (opens the file in your editor)
  $ notes | grep ...                     (pipes note to stdout for input to a filter)
  $ notes -a | grep ...                  (pipes all notes files concatenated together)
  $ notes -1 | grep ...                  (pipes the previous file only, -2 would be the 2nd previous file etc.)
```

Produces:

  YYYY-MM.txt, or similar named files (see NOTES_FMT_TYPE) in your $NOTES_DIRECTORY (this is set below).

```Notes format type:
  yearly  -> %Y
  monthly -> %Y-%m
  weekly -> %Y-w%V
  daily   -> %Y-%m-%d
```

It is highly desireable to set your environment variables NOTES_DIRECTORY, and EDITOR, before you run this script
for the first time.

Tips:

I find it very handy to set an alias to do the grep of a notes -a. That is: $ alias gn='notes -a | grep'
Then you can use the handy gn SomeSearchText instead of notes -a | grep SomeSearchText

I find it handy, if you have several lines to go together to put a keyword before each line of text, so
that it can be grepped out all together:

for instance:

```$ notes tilix: ^-Alt-D - create terminal Down
$ notes tilix: ^-Alt-R - create terminal Right
$ notes tilix: Alt-(<-|->) - goto terminal left or right
$ notes tilix: Alt-(up|down) - go up or down
```

then you can do a notes -a | grep tilix:
