---
title: "Dropdown/Scratchpad Terminals with i3"
date: "2020-02-14"
description: "Dropdown/Scratchpad Terminals with i3"
tags: [
"Linux",
"i3",
"terminal",
]
type: "post"
---

Tutorial on implementing dropdown terminals in i3 using the scratchpad feature
<!--more-->

## Examples

<div align="center"
<figure>
    <img src="/img/dropdown_term.png"/>
    <figcaption>
        <h4>Neofetch</h4>
    </figcaption>
</figure>
</div>

<div align="center"
<figure>
    <img src="/img/spotifytui.png"/>
    <figcaption>
        <h4>Spotifytui</h4>
    </figcaption>
</figure>
</div>

## Implementation

### i3 configuration

```
# #---Dropdown Windows---# #
# # General dropdown window traits. The order can matter.
for_window [instance="dropdown_*"] floating enable
for_window [instance="dropdown_*"] move scratchpad
for_window [instance="dropdown_*"] sticky enable
for_window [instance="dropdown_*"] scratchpad show
for_window [instance="dropdown_*"] move position center
for_window [instance="dropdown_*"] resize set 800 600
for_window [instance="dropdown_*"] border pixel 1

for_window [instance="dropdown_spt"] resize set 1200 600
for_window [instance="dropdown_taskell"] resize set 1200 600

bindsym $mod+t exec --no-startup-id $HOME/.config/i3/toggle-dropdowns.sh terminal
bindsym $mod+Shift+t exec --no-startup-id $HOME/.config/i3/toggle-dropdowns.sh taskell
bindsym $mod+n exec --no-startup-id $HOME/.config/i3/toggle-dropdowns.sh notepad

# Spotify controls
bindsym $mod+Up exec --no-startup-id $HOME/.config/i3/toggle-dropdowns.sh spt
```


The for_window portion of the configuration essentially sets a series of rules for any windows with the class `"dropdown_*"`, where `"*"` is the wildcard that means "anything". I have an additional resize rule for my spotify-tui dropdown and my taskell dropdown.

I binded my drop down terminals below the window rules. Each of the bindings is associated with the `toggle-dropdowns.sh` script, which essentially launches the terminals if they haven't already been launched and toggles whether or not they are showing. The code is rather inelegant and duplicate code could certainly be reduced, but I got a bit lazy when I was writing it :). The code is in the section below.

### `toggle-dropdowns.sh`

```
#!/usr/bin/env zsh
source ~/.zshrc
case $1 in
    spt)
        if  [ "_$(xdotool search --classname "dropdown_spt" | head -1)"  = "_" ]; then
            systemctl --user restart spotifyd.service
            $TERMINAL --class dropdown_spt -e spt &
            sleep .1
            i3-msg "[instance="dropdown_spt"] move position center"
        else
            i3-msg "[instance="dropdown_spt"] scratchpad show; [instance="dropdown_spt"] move position center"
        fi
        ;;
    terminal)
        if  [ "_$(xdotool search --classname "dropdown_term" | head -1)"  = "_" ]; then
            $TERMINAL --class dropdown_term &
            sleep .1
            i3-msg "[instance="dropdown_term"] move position center"
        else
            i3-msg "[instance="dropdown_term"] scratchpad show; [instance="dropdown_term"] move position center"
        fi
        ;;
    taskell)
        if  [ "_$(xdotool search --classname "dropdown_taskell" | head -1)"  = "_" ]; then
            $TERMINAL --class dropdown_taskell -e taskell $HOME/todo.md &
            sleep .1
            i3-msg "[instance="dropdown_taskell"] move position center"
        else
            i3-msg "[instance="dropdown_taskell"] scratchpad show; [instance="dropdown_taskell"] move position center"
        fi
        ;;
    notepad)
        if  [ "_$(xdotool search --classname "dropdown_notepad" | head -1)"  = "_" ]; then
            $TERMINAL --class dropdown_notepad -e ~/.config/i3/run-notepad &
            sleep .1
            i3-msg "[instance="dropdown_notepad"] move position center"
        else
            i3-msg "[instance="dropdown_notepad"] scratchpad show; [instance="dropdown_notepad"] move position center"
        fi
        ;;
    *)
        notify-send -i $HOME/.local/share/icons/dunst_icons/icons8-high-importance-48.png "Error" "Invalid option passed to toggle-dropdowns script."
        ;;
esac
```
