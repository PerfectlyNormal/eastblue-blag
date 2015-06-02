---
layout: post
title: "New Remote and LIRC"
date: 2009-06-17
category: Linux
tags:
  - Linux
  - LIRC
  - Note to Self
---

(This is mostly a set of instructions for myself if I ever need to format my media PC, so I know how to fix it, but it turned into a short story that might help others as well)

I have a machine connected to my TV for playing movies, both from DVDs and from my file server, TV shows and everything else I might think of. Some time ago I acquired a remote for this machine, so I could control it all from my couch. And for some time, it was good. Until I rebooted the machine, and found that my remote was no longer working. Some experimenting, and I figured out that disconnecting the cable, and reconnecting it worked. And especially if I pressed buttons on the remote at the same time as reconnecting it.

I have no idea whether this was because of the hardware (either the serial receiver, or the PC), or something software-related. There were some issues with the serial port being grabbed before LIRC had a chance to start up. After [I got my Mac mini]({% post_url /blag/2009-03-03-moving-away-from-linux %}) and that took over as my desktop, I started shutting down this machine before going to bed, so having to do the reconnect-dance every day grew tiring really quick.

So I went out and bought a replacement.

#### The new device

I found a "THERMALTAKE Media LAB with remote, Black (A2331)", which identified itself as "SoundGraph IMON VFD Device" when connected.
It has, in addition to a IR receiver, a two line VFD-display with room for 16 characters on each line.

![The remote and display]({{ site.image_url }}/medialab.jpg)

The USB ID is 15c2:0036

#### Getting the IR to work
I use Gentoo, and at the time of writing, LIRC 0.8.4 was the newest version in my tree, which did not work properly. Or I chose the wrong driver. Not really sure. I downloaded the latest 0.8.5 from [lirc.org](http://www.lirc.org) and compiled it. Selected "Soundgraph iMON PAD IR/VFD" from the USB devices `setup.sh` listed and it worked instantly. Modprobing the driver (`lirc_imon`) created both `/dev/lirc0`, `/dev/lirc1`, `/dev/lcd0` and `/dev/lcd1`.

I have no idea what the difference between `/dev/lcd0` and `/dev/lcd1` is, since they both can be used to change what is displayed.

`/dev/lirc0` and `/dev/lirc1` however, have huge differences. Most of the interesting buttons come through `lirc1`, including Play, Eject, Fast-Forward, Rewind, Next Chapter and so on.

Then the mouse-related buttons, except the Mouse/Keyboard toggle button, and the number buttons come through `lirc0`. So to get everything working, we need *two* LIRC daemons running.

To use `irrecord` properly, we need a `mode2` instance watching the other device, since the device maintains a buffer, and nothing new comes out of `lirc0` if `lirc1` has events waiting to be processed, and the other way around. My remote ended up with the following configurations

##### lircd.conf

``` bash
    begin remote

      name  newmote
      bits           24
      eps            30
      aeps          100

      one             0     0
      zero            0     0
      post_data_bits 40
      post_data      0xB700000101
      gap            95996
      toggle_bit_mask 0x0

          begin codes
              AppExit                  0x288195
              Power                    0x289115
              Record                   0x298115
              Play                     0x2A8115
              OpenClose                0x29B195
              Rewind                   0x2A8195
              Pause                    0x2A9115
              FastForward              0x2B8115
              SkipBack                 0x2B9115
              Stop                     0x2B9715
              SkipForward              0x298195
              Videos                   0x2B8515
              Music                    0x299195
              Pictures                 0x2BA115
              TV                       0x28A515
              Bookmark                 0x288515
              Thumbnail                0x2AB715
              Zoom                     0x29A595
              FullScreen               0x2AA395
              DVD                      0x29A395
              Menu                     0x2BA395
              Subtitle                 0x298595
              Audio                    0x2B8595
              AppLauncher              0x29B715
              MiddleThingy             0x2AB195
              TaskSwitcher             0x2A9395
              Eject                    0x299395
              Mute                     0x2B9595
              VolUp                    0x28A395
              VolDown                  0x28A595
              ChUp                     0x289395
              ChDown                   0x288795
              Timer                    0x2B8395
          end codes

    end remote
```

##### lircd.mouse.conf

``` bash
    begin remote

      name  mouse
      bits           32
      eps            30
      aeps          100

      one             0     0
      zero            0     0
      post_data_bits  32
      post_data      0x0
      gap          95990
      toggle_bit_mask 0x0

          begin codes
              1                        0x0200001E
              2                        0x0200001F
              3                        0x02000020
              4                        0x02000021
              5                        0x02000022
              6                        0x02000023
              7                        0x02000024
              8                        0x02000025
              9                        0x02000026
              Star                     0x02200025
              0                        0x02000027
              Hash                     0x02200020
              Backspace                0x0200002A
              SelectSpace              0x0200002C
              ListBottom               0x02800000
              ListTop                  0x02000065
              LClick                   0x0101FF02
              Enter                    0x02000028
              RClick                   0x01020000
              Escape                   0x02000029
          end codes

    end remote
```

##### Double events
The `toggle_bit_mask` lines were originally something else, but I had to modify them a bit to prevent one event when pressing the button, and one when releasing it. Don't remember where I found it, but it worked. Just randomly changing until only one event showed.

##### Starting LIRC
To get lircd running properly, I modified the init script from Gentoo, and had it start two of them, each with their own configuration:

``` bash
    #!/sbin/runscript

    PIDFILE=/var/run/${SVCNAME}

    depend() {
    	provide lirc
    }

    start() {
    	ebegin "Starting lircd"
    	start-stop-daemon --start --quiet --pidfile "${PIDFILE}-0.pid" \
    	  --exec /usr/local/sbin/lircd -- -P "${PIDFILE}-0.pid" \
    	  --device=/dev/lirc0 --listen=8765 /etc/lircd.mouse.conf
    	start-stop-daemon --start --quiet --pidfile "${PIDFILE}-1.pid" \
    	  --exec /usr/local/sbin/lircd -- -P "${PIDFILE}-1.pid" \
    	  --device=/dev/lirc1 --connect=localhost:8765 /etc/lircd.conf
    	eend $?
    }

    stop() {
    	ebegin "Stopping lircd"
    	start-stop-daemon --stop --quiet --pidfile "${PIDFILE}-0.pid" \
    	  --exec /usr/local/sbin/lircd
    	start-stop-daemon --stop --quiet --pidfile "${PIDFILE}-1.pid" \
    	  --exec /usr/local/sbin/lircd
    	echo -n " " > /dev/lcd0
    	eend $?
    }
```

After this, the remote part worked like a charm. Then, onto the display!

#### VFD

I tried running [LCDproc](http://lcdproc.omnipotent.net/) first, which worked fine, but it seemed a bit overkill for my use, since I can just as easily write to `/dev/lcd0` and have it displayed.

`/dev/lcd0` gets created 0660, and owned by root:root, which means my user can't write to it that easily. Instead of looking for a proper solution, I just stuck `chown root:video /dev/lcd0` into my `/etc/conf.d/local.start`, which took care of it.

Writing more than 32 bytes to `/dev/lcd0` gives the unhelpful error message `-bash: echo: write error: Invalid argument`. Just remember to keep it under 32 bytes, and it works fine. And line breaks count, so `echo -n` is recommended.

When powering down the machine, the display still keeps what was written to it last, so if you sleep in the same room as the machine, and want it dark, like I do, you need to clear the display before shutting down the machine. I fixed that with `echo -n " " > /dev/lcd0` in my `/etc/conf.d/local.stop`.

