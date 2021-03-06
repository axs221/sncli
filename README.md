sncli
=====

Simplenote Command Line Interface

sncli is a Python application that gives you access to your Simplenote account
via the command line. You can access your notes via a customizable console GUI
that implements vi-like keybinds or via a simple command line interface that
you can script.

Notes can be viewed/created/edited in *both an* **online** *and* **offline**
*mode*. All changes are saved to a local cache on disk and automatically
sync'ed when sncli is brought online.

**Pull requests are welcome!**

Check your OS distribution for installation packages.

### Requirements

* [Python 2](http://python.org)
* [Urwid](http://urwid.org) Python 2 module
* A love for the command line!

### Features

* Console GUI
  - full two-way sync with Simplenote performed dynamically in the background
  - all actions logged and easily reviewed
  - list note titles (configurable format w/ title, date, flags, tags, keys, etc)
  - sort notes by date, alpha by title, pinned on top
  - search for notes using a Google style search pattern or Regular Expression
  - view note contents and meta data
  - view and restore previous versions of notes
  - pipe note contents to external command
  - create and edit notes (using your editor)
  - edit note tags
  - trash/untrash notes
  - pin/unpin notes
  - flag notes as markdown or not
  - vi-like keybinds (fully configurable)
  - Colors! (fully configurable)
* Command Line (scripting)
  - force a full two-way sync with Simplenote
  - all actions logged and easily reviewed
  - list note titles and keys
  - search for notes using a Google style search pattern or Regular Expression
  - dump note contents
  - create a new note (via stdin or editor)
  - edit a note (via editor)
  - trash/untrash a note
  - pin/unpin a note
  - flag note as markdown or not

### Screenshots

![sncli](https://github.com/insanum/sncli/raw/master/screenshots/screenshot1.png)
![sncli](https://github.com/insanum/sncli/raw/master/screenshots/screenshot2.png)
![sncli](https://github.com/insanum/sncli/raw/master/screenshots/screenshot3.png)
![sncli](https://github.com/insanum/sncli/raw/master/screenshots/screenshot4.png)

### HowTo

```
Usage:
 sncli [OPTIONS] [COMMAND] [COMMAND_ARGS]
 
 OPTIONS:
  -h, --help                  - usage help
  -v, --verbose               - verbose output (cli mode)
  -n, --nosync                - don't perform a server sync
  -r, --regex                 - search string is a regular expression
  -k <key>, --key=<key>       - note key
  -t <title>, --title=<title> - title of note for create (cli mode)
              
 COMMANDS:
  <none>                      - console gui mode when no command specified
  sync                        - perform a full sync with the server
  list [search_string]        - list notes (refined with search string)
  dump [search_string]        - dump notes (refined with search string)
  create [-]                  - create a note ('-' content from stdin)
  dump                        - dump a note (specified by <key>)
  edit                        - edit a note (specified by <key>)
  < trash | untrash >         - trash/untrash a note (specified by <key>)
  < pin | unpin >             - pin/unpin a note (specified by <key>)
  < markdown | unmarkdown >   - markdown/unmarkdown a note (specified by <key>)
```

#### Configuration

The current Simplenote API does not support oauth authentication so your
Simplenote account information must live in the configuration file. Please be
sure to protect this file.

sncli pulls in configuration from the `.snclirc` file located in your $HOME
directory. At the very least, the following example `.snclirc` will get you
going (using your account information):

```
[sncli]
cfg_sn_username = lebowski@thedude.com
cfg_sn_password = nihilist
```

Start sncli with no arguments which starts the console GUI mode. sncli with
start sync'ing all your existing notes and you'll see log messages at the
bottom of the console. You can view these log messages at any time by pressing
the `l` key.

View the help by pressing `h`. Here you'll see all the keybinds and
configuration items. The middle column shows the config name that can be used
in your `.snclirc` to override the default setting.

#### Note Title Format

The format of each line in the note list is driven by the
`cfg_format_note_title` config item. Various formatting tags are supported for
dynamically building the title string. Each of these formatting tags supports
a width specifier (decimal) and a left justification (-) like that supported
by printf:

```
  %F - flags (fixed 5 char width)
       X - needs sync
       T - trashed
       * - pinned
       S - published/shared
       m - markdown
  %T - tags  
  %D - date
  %N - title
```

The default note title format pushes the note tags to the far right of the
terminal and left justifies the note title after the date and flags:

```
cfg_format_note_title = '[%D] %F %-N %T'
```

Note that the `%D` date format is further defined by the strftime format
specified in `cfg_format_strftime`.

#### Colors

sncli utilizes the Python [Uwrid](http://urwid.org) module to implement the
console user interface.

At this time, sncli does not yet support 256-color terminals and is limited to
just 16-colors. Color names that can be specified in the `.snclirc` file are
listed [here](http://urwid.org/manual/displayattributes.html#standard-foreground-colors).

### Searching

sncli supports two styles of search strings. First is a Google style search
string and second is a Regular Expression.

A Google style search string is a group of tokens (separated by spaces) with
an implied *AND* between each token. This style search is case insensitive. For
example:

```
/tag:tag1 tag:tag2 word1 "word2 word3" tag:tag3
```

### Tricks

I personally store a lot of my notes in
[Votl/VimOutliner](https://github.com/insanum/votl) format. Specific to Vim, I
put a modeline at the end of these notes (note that Emacs also supports
modelines):

```
; vim:ft=votl
```

Now when I edit this note Vim will automatically load the votl plugin. Lots of
possibilities here...


### Thanks

This application pulls in and uses the
[simplenote.py](https://github.com/mrtazz/simplenote.py) module by
[mrtazz](https://github.com/mrtazz) and the
[notes_db.py](https://github.com/cpbotha/nvpy/blob/master/nvpy/notes_db.py)
module from [nvpy](https://github.com/cpbotha/nvpy) by
[cpbotha](https://github.com/cpbotha).

