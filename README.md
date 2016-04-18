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


###Source Code

The source code can be downloaded at [github.com](http://github.com/PromyLOPh/pianobar/)
or [6xq.net](http://6xq.net/projects/pianobar/).

###Download/Installation

There are community provided packages available for most Linux distributions (see your distribution’s package manager), Mac OS X ([MacPorts](http://trac.macports.org/browser/trunk/dports/audio/pianobar/Portfile) or [homebrew](http://brew.sh/)) and *BSD as well as a [native Windows port](https://github.com/thedmd/pianobar-windows).
