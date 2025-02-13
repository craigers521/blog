---
title: Creating Scheduled Jobs on an Immutable OS
header:
  excerpt: using systemd timers on bazzite
description: creating scheduled jobs on immutable os like bazzite or silverblue without cron using systemd timers
tags: [linux, bazzite]
---

#### The Problem

So I've been daily driving bazzite for about a year now as my main gaming rig and quasi-HTPC.  It's great I love it. I could talk at length about the joys and tribulations about gaming on Linux but theres enough other people out there doing that, I want to focus on the HTPC side of the house.  

The PC is in my bedroom and I have some nice large monitors that make it a great media machine (coupled nicely with my k8s cluster running my *arr app stack, another topic worth blogging on).  The problem I have run into though, is if I fall asleep watching youtube in fullscreen it will stay fullscreen at video end and won't ever allow my PC to sleep.  To boot the jellyfin flatpak also has the same issue, when a video finishes it stays fullscreen and won't return to the jellyfin selection menu.  

I decided the best course of action was to just write a shell script to kill the apps and set a cron job to run it in the wee hours of the morning when I should be asleep.  It turns out this was much more challenging than I expected on an immutable OS like bazzite (if you are not familiar with bazzite it is an immutable OS based on fedora silverblue, immutable meaning the main system filespace is read-only and only user space is writeable).  I did eventually find a way using systemd timers, see below

#### The Stumbling Blocks

So problem one with bazzite is there is no cron, and its a real pain trying to layer in things to the system that arent containerized.  We do however have systemd, and systemd timers are a really cool mechanism that gives you even more flexibility than cron ever did.  This isn't meant to be a systemd timers tutorial though so do some research on that topic.  

The next problem is that we end up with some permissions issues trying to place services in /etc/systemd, since those services need to be owned by root... but my scripts and app processes are owned by my user. During this journey I also learned there is a per-user instance of systemd, which is really nice for dealing with these kind of permissions schisms between root and your user.  

The final problem I ran into was some strange behavior with the flatpak for firefox.  If I ran a `pkill firefox` or a `killall firefox` from terminal, it worked every time.  If I tried to wrap this in a script it would never work.  So I needed to go after the flatpak directly.   

#### The Solution

**STEP 1**: The Shell Script

here is my bash script: 

```bash
#!usr/bin/bash

apps=( "jellyfin")
flatpaks=("org.mozilla.firefox")

for app in "${apps[@]}"; do
  killall "$app"
done

for pak in "${flatpaks[@]}"; do
  flatpak kill "$pak"
done
```

If you are asking yourself why I used for loops for lists of length 1.  Well originally I had a loop for just apps with jellyfin and firefox, but FF wasnt working until I moved it to the flatpak kill command.  I decided to just keep the loop structure in case I needed to add more apps to the list later.  Jellyfin is also a flatpak but it was working via killall so I left it as is. 

I have this file living in a scripts folder in my home directory

**STEP 2**: The timer and service files

Here is the timer file.  This file is named `mysleepy.timer`

```ini
[Unit]
Description=My Timer

[Timer]
OnCalendar=*-*-* 06:00:00
Unit=mysleepy.service

[Install]
WantedBy=timers.target
```
The OnCalendar key is highly configurable to all kinds of options much like cron. Only a bit more human readable without a secret decoder ring needed.  The format is: 

```
*                     *-*-*                       *:*:*
#Day Of the week      Year-Month-Date             Hour:Minute:Second
```

Here I've skipped day of the week because I want it to run everyday and not on a particular day or date.  It runs at 6AM local time every day. 

Next comes the service file. This file is named `mysleepy.service`.  I think the filenames need to match for the service and timer files, but I do specify the service name directly in the timer unit... so not sure if the filename is arbitrary or not. 

```ini
[Unit]
Description=My Sleepy

[Service]
Type=simple
ExecStart=/usr/bin/bash /home/craig/scripts/kill_apps.sh
```

The path for both of these files is `/home/craig/.config/systemd/user`.  I did have a good shebang at the top of my file but systemd wasn't happy about it and I still needed to infer bash for the ExecStart

**STEP 3**:  Start and verify the timer

Use `systemctl --user enable mysleepy.timer` followed by `systemctl --user start mysleepy.timer` to enable and start the timer.  

You can check the status of the timer like any normal systemd service using `systemctl --user status mysleepy.timer`.  You can also check all timers that are running and how long they have before triggered using the `systemctl --user list-timers` command


**Happy Scheduling!!**