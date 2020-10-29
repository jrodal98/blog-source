---
title: "Fix Gray Overlay When Video Sharing on Zoom"
date: "2020-10-29"
description: "Fix Gray Overlay When Video Sharing on Zoom"
tags: [
"Linux",
"i3",
"qtile",
"picom",
"compton",
]
type: "post"
---

Are you a Compton/Picom user that has a gray overlay that appears when sharing or recording your screen when using zoom? Here's a possible fix.
<!--more-->

## tl;dr

1) Run `pgrep -l "compton|picom"` to verify that you are running Picom
2) Locate your Picom configuration file. It is either at `~/.config/picom/picom.conf` or `~/.config/picom.conf`. If you don't have this file, run `cp /etc/xdg/picom.conf.example ~/.config/picom.conf`
3) Add `"name = 'cpt_frame_window'"` to the `shadow-exclude = []` list
4) Restart compositor with `pkill compton && compton -b` or `pkill picom && picom -b`, depending on the name of the compositor process

## Verifying that you are using comptom/Picom

If you're a Linux user running a window manager such as i3 or qtile, you probably use [Picom/Compton](https://wiki.archlinux.org/index.php/Picom) as your compositor. To check this, run `pgrep -l "compton|picom"` in your shell. If you are running Picom/Compton, the process id and the name of the compositor will show up in the results.

Picom is a fork of Compton, but now Picom is used in favor of Compton. In the case of my system, the Compton process is merely a symbolic link for Picom, so the Compton process is actually just a Picom process but with a different name. For simplicity, I am going to provide instructions for only Picom. I think they should work for both Picom and Compton, but I can't guarantee it.

## Locate Picom configuration file

Next, we will modify the configuration file for Picom in order to resolve the gray overlay issue.

Per the [Picom arch wiki page](https://wiki.archlinux.org/index.php/Picom), the default configuration is available in `/etc/xdg/picom.conf.example`. For modifications, it can be copied to `~/.config/picom/picom.conf` or `~/.config/picom.conf`. For example, run `cp /etc/xdg/picom.conf.example ~/.config/picom.conf` if you don't have a configuration file yet.

## Modify the configuration file

Add `"name = 'cpt_frame_window'"` to the `shadow-exclude = []` list. For example, this is what mine looks like:

```
shadow-exclude = [
  "name = 'Notification'",
  "class_g = 'Conky'",
  "class_g ?= 'Notify-osd'",
  "class_g = 'Cairo-clock'",
  "_GTK_FRAME_EXTENTS@:c",
  "name = 'cpt_frame_window'"
];
```

## Restart compositor

Run either `pkill compton && compton -b` or `pkill picom && picom -b` depending on name of the process discovered earlier. If this doesn't work, try restarting your computer.
