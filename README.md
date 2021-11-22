# after-earth-mushclient-plugins
Miniwindow plugins for After Earth

Bits of this plugin and ideas were borrowed and remixed from the MUSHclient community. http://www.gammon.com.au/forum/?id=9385 and others.
Modifications for Aardwolf and extra awesome sauce added by Fiendish with help from Orogan
Adapted by Nick Gammon for Smaug and similar MUDs

Implementation of:

-Mouse wheel scrolling

-Chat and mention alerts

-Doubleclick to bottom

-Saving of buffer between sessions

-Autosave

-Character name identification

and adapted for After Earth

by Nodens/Akane


How to install:

1)Copy the plugin files either in Mushclient's plugin
directory or like I do in the worlds directory under
a subdirectory dedicated to the MUD's files (eg AfterEarth)

2)Load from Mushclient's plugin interface (ctrl-shift-p)


Command line options for the toggles are available and listed
in each plugin's source, at the header.

Caveats:

-If you get a message about not set name run score in the game.
This is needed for the beep-on-mention functionality.

-On older systems with few resources the default lines to save
may be too much. This is because plugins in Mushclient run in
the main thread which causes a blocking behavior everytime the
plugins serializes and saves the it's scrollback buffer in
plugin settings. There is nothing I can do about that and is
one of the reasons I'm writing my own client heh. What you can
do if you experience freezes is decrease the number of lines
saved by editing this line in each plugin's source:

"MAX_SAVE_LINES = 1000 -- how many lines to save on state file"

To the number that works for you. Do notice that this value must
ALWAYS be LESS than the value set right above it:

"MAX_LINES = 2000 -- how many lines to store in scrollback"

Which sets how many lines are held in the scrollback buffer and
is also the line to edit if you're mudding on a tin-can with 1GB
or something, in order to reduce memory footprint.

Feel free to experiment with those values if you have faster/slower
systems but remember that MAX_LINES must ALWAYS have a higher value
than MAX_SAVE_LINES.

-If you want to change the default beeping sound you have to change
all the trigger definitions with "C:\WINDOWS\Media\Windows Ding.wav"
defined to another file on your system. I plan to eventually expose
sound changing functionality via the right click context menu.

-Currently the Public channels plugin pulls emotes but only those
that display as "emote_text [name of emoting player]". And dispite
my horrific regex this manages to pull a few other lines as well
that are difficult to exclude (mostly from helpfiles). If you don't
like this and want emoted removed entirely, delete these 3 triggers
from the file:

	<trigger
    enabled="y"
	regexp="y"
	expand_variables="y"
	keep_evaluating="y"
	match="^(?! |Syntax:)(.*)(?i)(@name)(?-i)(.*) \[(?!@name|OOC|AFK|up|down|north|south|east|west|northeast|northwest|southeast|southwest|HELP |Enter your|room\||Success|Rummage|App| Air\:|\d| \d)(.*)\]$"
    sequence="100"
	sound="C:\WINDOWS\Media\Windows Ding.wav"
	group="chatmention"
    ></trigger>

	<trigger
    enabled="n"
	regexp="y"
    match="^(?! |Syntax:)(.*) \[(?!@name|OOC|AFK|up|down|north|south|east|west|northeast|northwest|southeast|southwest|HELP |Enter your|room\||Success|Rummage|App| Air\:|\d| \d)(.*)\]$"
    script="chats"
    omit_from_output="y"
    sequence="100"
	sound="C:\WINDOWS\Media\Windows Ding.wav"
	group="chatsound"
    ></trigger>
	
	<trigger
    enabled="y"
	regexp="y"
	keep_evaluating="y"
    match="^(?! |Syntax:)(.*) \[(?!@name|OOC|AFK|up|down|north|south|east|west|northeast|northwest|southeast|southwest|HELP |Enter your|room\||Success|Rummage|App| Air\:|\d| \d)(.*)\]$"
    script="chats"
    omit_from_output="y"
    sequence="100"
	group="chatnosound"
    ></trigger>
	

PS This README is probably going to look like a mess because probably a lot of symbols need to be escaped in github README.md files. :P