#pianobar

This is a Fork of the popular unofficial pandora client, pianobar.

pianobar is a console client for the personalized web radio [Pandora]
([http://www.pandora.com](http://www.pandora.com)).

This fork is intended to better satisfy the requirements for interfacing with pianobar on a web server to play pandora.
Currently this means a buch of new events, along with prompts options being asdded to event output. 

Note that I do not plan on maintaining this long term. Just untill I find a pandora client better suited to my needs and implement it in my audio server. However, if you are still interested in this, and run into any problems that do not occure upstream, please tell me.

### New Features

* Experimental Play/Pause events.
* Experimental prompt events.

###Motivation

I forked this because parsing neverending lines with \r and clearline was a bit harder than I hopped, and interacting with multiple clients required saveing stdout to a file which, due to the statusbar, grew extreamly fast. These events eliminate the need to parse stdout completly (or so I hope), while staying close to the original code.

###How To Use Events

Here is some basic information one would need to use the events
properly:

 - All events begining with 'prompt' are requesting user input.
 - If the last event sent is a prompt event, one must respond
   appropriatly to the prompt or send ctrl+c/INT to close the prompt.
   (will generate a showstatus event IF no other events occure
   beforehand. Note all non-prompt events couunt as a status events)
 - Prompts which generate options will have those options listed as
   option1=string, option2=anotherstring, option3=... etc. A optionCount
   is also returned for all events. This is just like stationCount and
   station0,station1, etc.
 - All other prompts (requesting static input like [a]rtist/[t]rack, or
   string input) do not generate options. Their options should be understood
   by the event name.
 - When a prompt is opened, it appears that pianobar will not play
   anymore songs. However, it will finish the song it is currently
   playing. You can calculate the current position in the track by the
   last given songDurration and songPlayed. If songPlayed +
   timePassedSinceEventSent > songDuration then the song has probably
   finished and pianobar is waiting for you to close the prompt before
   continueing. The songfinish event will be sent as soon as the prompt
   is closed. While this may not seem  completly ideal, it is calculable
   so not realy a problem, and it is thanks to this fact that we can
   determin if prompts are open/closed so easily(if it is open, the last
   event will still be the prompt)!
 - You need to ensure that the control fifo is not written to by another applicaion while you recieve the prompt event, and respond to it. ie. a pause/play 'p' could be sent to the prompt before you respond, or another process could close the prompt before you respond. While the events make us easier to use as a server, we still are still intended to be interacted with by a single user/client. Not by multiple like mpd.

Prompt events

Ledgend |   |
-------|------------------------------------------- |
LISTED | Listed in option0 through option(optionCount). |
STRING | Any string value, usualy for searches. |

Event Name          | Prompt Key | Event Description                                                | Options |
------------------- | -------- | ------------------------------------------------------------------| ----------- |
promptselectstation | s | Prompting user to select an existing station to play | LISTED
promptcategory      | g | Prompting for user to select a genre category | LISTED
promptgenre         | g | Prompting for user to select a genre (after haveing selected a genre category) | LISTED
promptsearchstring  | c | Prompting for user to enter search string for adding a station  | STRING
promptsearchstring  | a | Prompting for user to enter search string for adding to a station  |STRING
promptartisttrack   | c | Prompt to select if search was for an artist or a track | 'a' 't'
promptselectartist  | c | Prompt to select artist from search result | LISTED
promptselecttrack  | c | Prompt to select track from search result | LISTED
promptsongartist   | v | Prompt to create station from song or artist | 's' 'a'
promptbookmark     | b | Prompt to bookmark song or artist | 's' 'a'
promptchangesetting     | ! | Prompt to select setting to change: 1) Username 2) Password 3) Explicit content filter | '1' '2' '3'
promptstationid    | j | Prompt for shared station Id to add | STRING

###Source Code

The source code can be downloaded at [github.com](http://github.com/PromyLOPh/pianobar/)
or [6xq.net](http://6xq.net/projects/pianobar/).

###Download/Installation

There are community provided packages available for most Linux distributions (see your distribution’s package manager), Mac OS X ([MacPorts](http://trac.macports.org/browser/trunk/dports/audio/pianobar/Portfile) or [homebrew](http://brew.sh/)) and *BSD as well as a [native Windows port](https://github.com/thedmd/pianobar-windows).
