---
title: "Dunst Action"
date: "2020-01-20"
description: "My Experiences with Dunst"
tags: [
"Linux",
"Dunst",
"bash",
]
type: "post"
---

A discussion on my Dunst screenshot actions, which include OCR, uploading to 0x0.st, deletion, renaming, and movement to/from clipboard
<!--more-->

<div align="center"
<figure>
  <img src="https://user-images.githubusercontent.com/35352333/82405961-50555280-9a33-11ea-8240-b690555afc85.gif"  />
  <figcaption>
      <h4>A demo of most of my dunst screenshot actions</h4>
  </figcaption>
</figure>
</div>

## Dunst background

* Arch Linux and similarly minimal Linux distributions don't come with a notification daemon
* Dunst is one of the most popular notification daemons for Arch Linux
* Dunst installation and configuration instructions here: <https://wiki.archlinux.org/index.php/Dunst>
* Dunst actions are essentially commands that are sent with a notification and can be invoked directly through the Dunst context menu.
* The binding that launches the dunst context menu is in your dunstrc. Mine defaulted to `ctrl + shift + .`. Your results may vary.

## Screenshot actions

I regularly take screenshots, so I came up with a few useful dunst actions for screenshots. They are as follows:

1) **clipboard mode**: works with screenshots that were saved to the clipboard
    * Extract text from screenshot using **ocr**
    * **Save** screenshot stored in clipboard to an image file
    * **Upload** screenshot to 0x0.st
2) **file mode**: works with screenshots that were saved to a file
    * **Delete** screenshot
    * **Rename** screenshot
    * **Copy** screenshot to clipboard
    * **Move** screenshot to clipboard (deletes file)
    * **Upload** screenshot to 0x0.st

An installation guide for my Dunst actions are on my github: <https://github.com/jrodal98/screenshot-actions>
