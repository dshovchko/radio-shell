# radio-shell
Listen to an Internet Radio Stream.

Support for Icecast metatags and sending notifications about the track being played through libnotify, logging of streaming channels, low memory consumption, working from the terminal.

## What is the program for?

There is absolutely no problem with listening to online streaming radio. Any completely adequate station has a site on which you can listen to and see what track is playing at the moment, as well as a list of previous songs. All you have to do is to open the browser and select a station from the bookmarks. It is a suitable option.

But not for real geeks! What's the point of launching a browser with a radio tab, when it takes up to 60-80 megabytes of RAM? Instead, you can open the radio player in your terminal without any loss of memory. In addition, you can combine radio stations into collections and manage these groups!

All you need to do is install the radio-shell

In the home directory run
```
git clone https://github.com/dshovchko/radio-shell
```

Add the directory with the binary of our program to the PATH variable in `~/.profile` file.
```
#set PATH so it includes user's private bin directories
PATH="$HOME/radio-shell/bin:$HOME/.local/bin:$PATH"
```

To reload `.profile` and take changes effects without logout/login, run:
```
source ~/.profile
```

Check that all works, run:
```
$ radio
```
Will output help on success.

## File Structure Description

The radio-shell contains three directories: `/bin`, `/etc` and `/share`.

 - `/bin` is for keeping executable files
 - `/etc` - for config file
 - `/share` - for data files. Here is placed the directory `/icon` that stores the logos of the radio stations.

## Config Description

This is located in `radio-shell/etс/radio.cfg`.
Its structure is simple, six fields separated by symbol `|`. Like that:

```
# group | short name | stream | player | full name | logo picture
```

- `group` - name of group. May contain an arbitrary string without spaces or the keyword `{root}`. `{root}` means that this row not included in a group. Any other value means that row included in a group named as value.
- `short name` - short name of radio station. Used to choose radio stations.
- `stream` - URL of stream. May contain URL of stream or special config string for soma.fm radio station, like this `{somafm!defcon!256}`. URL of soma.fm streams looks like this `http://ice3.somafm.com/defcon-256-mp3`. It is built according to the rules in which we describe the name of the radio station and bitrate. `{}` - is a container that contains three values separated by a symbol `!`:
  - `somafm` - constant
  - `defcon` - name of soma.fm radio
  - `256` - bitrate. Some of soma.fm's radio have a several streams 
  
- `player` - name of player. The program supports four players:
  - mpg123
  - mplayer
  - ogg123
  - vlc (as console interface - cvlc)
- `full name` - full name of radio station or very short description
- `logo picture` - path to logo picture inside `radio-shell/share/icon` directory

## Default Config

`radio-shell/etc/radio.cfg`

My usual config. This is what I am listening to. A few multi-genre radio stations and four groups from the radio:
 - [soma.fm](https://somafm.com/)
 - [Radio ROKS](https://www.radioroks.ua/)
 - [Hi On Line Radio](https://www.hionline.eu/)
 - [Chill Out Zone](https://chillout.zone/)
  
Radio station settings in the default configuration file as of May 3, 2020.

## Test config

`radio-shell/etc/radio.test-players.cfg`

This file contains settings for radio stations that cover all supported players and stream codecs. For these purposes, the [AI Radio](http://ai-radio.org/) streams were used, because this radio broadcasts in 11 different stream formats, which is very convenient for testing.

## Players and Streams

### Which player and stream should you choose?

Choose from your preferences for sound quality, memory consumption and bandwidth of your Internet channel.
| Player        | Codec         | Memory  |
| :-------------: |-------------| :-----:|
| mpg123      | mp3 | < 1mb |
| mplayer      | mp3, ogg vorbis, aac, aac+, flac | ~ 12mb |
| ogg123      | ogg vorbis | ~ 1.7mb |
| vlc      | mp3, ogg vorbis, aac, aac+, flac, opus | 3-3.3mb |

### About soma.fm streams

A special format for soma.fm radio stations implies the use of streams using the mp3 codec. You can specify which bitrate to choose. Randomly one of the three streaming servers will be selected.

## Launch Description

Let me briefly describe how you can use this program. Run
```
radio help
```
and you will see a help page that starts something like this
```
usage:  radio [cmd]
	    radio [channel]
	    radio [group] [channel]
	    radio ~
	    radio [group] ~

list of commands:

	config check	check config file
	config show	    show detailed config
	help		    show this help
	log del [days]	remove log files older than [days] days
	                (script auto removes log files older than 60 days)
```

### Commands for help and maintenance 

To check config file, run
```
radio config check
```

To display a detailed description of the configuration file as understood by the program, run
```
radio config show
```

To display help, run
```
radio help
```

To clear logs older than the specified number of days, run
```
radio log del [days]
```
`[days]` - is required number of days. By default, the program removes logs older than 60 days at each start.

### Commands for listening to the radio

For all examples, commands using the radio from the default configuration file are described.

To listen to radio not included in any group, run
```
radio [channel]
```
Here and below, `[group]` - is a `group` name, from the config file, `[channel]` - is a `short name` from the config file.

To listen to radio included in some group, run
```
radio [group] [channel]
```

The commands above are for launching a specific radio. Below are the commands for randomly selecting a radio.

For random selection within the same group, run
```
radio [group] ~
```

For a random selection among all radio stations, run
```
radio ~
```

---

## P.S.

This program is written solely for my pleasure of programming and warm tube sound, which cannot be obtained in GUI audio players.

Have a nice day!
