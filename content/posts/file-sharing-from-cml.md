---
title: "File sharing from the command line"
date: "2020-07-11"
description: "File sharing from the command line"
tags: [
"bash",
"transfer.sh",
"0x0.st",
]
type: "post"
---

An overview of [0x0.st](https://0x0.st/) and [transfer.sh](https://transfer.sh/), two popular file sharing services that you can use for free from the command line.
<!--more-->

I've run into a few scenarios, mostly when using Reddit, where I want to share a file with someone but don't want it to share any identifying information. For example, one could easily add a file to their Google Drive or Dropbox account, but that could reveal your name or email address. [0x0.st](https://0x0.st/) and [transfer.sh](https://transfer.sh/) both offer solutions to this problem. Both of the aforementioned websites allow you to post files with curl from the command line, which is an added bonus for me, since I almost always have a terminal open on my laptop.

I'll discuss the two websites a bit and then discuss a script I wrote that allows for simpler syntax and easy encryption of files.

## [0x0.st](https://0x0.st/)

* [0x0.st](https://0x0.st/) allows you to post files stored locally or on remote servers. It also offers url shortening, which is pretty nifty.
* File URLs are valid for at least 30 days and up to a year, depending on the file size.
* There are some blocked file types (e.g. android apks) and some warning to not use the platform for things such as piracy or crypto currencies. Refer to the website if you are unsure if what you're posting is acceptable
* Tends to be a bit more reliable than [transfer.sh](https://transfer.sh/), which has experienced occasional outages in the two months that I've been using it.

### Examples

From the [0x0.st](https://0x0.st/) website:

```bash
# upload files stored locally:
curl -F 'file=@yourfile.png' https://0x0.st
# upload files stored remotely:
curl -F 'url=http://example.com/image.jpg' https://0x0.st
# shorten url:
curl -F 'shorten=http://example.com/some/long/url' https://0x0.st
```

This is what a real usage looks like:

```bash
echo "hello" > test.txt
curl -F 'file=@test.txt' https://0x0.st
https://0x0.st/qVM.txt
```

Depending on when you are reading this, you may still be able to access the file: <https://0x0.st/qVM.txt>

You can either download the file by navigating to the url, or you could use another curl command:

```bash
curl https://0x0.st/qVM.txt -o downloaded_test.txt
cat downloaded_test.txt
hello
```

As a bonus, I discovered a way to post from stdin as well:

```bash
echo "hello world" | curl -F 'file=@-' https://0x0.st
https://0x0.st/ATj.txt
curl -O https://0x0.st/ATj.txt
cat ATj.txt
hello world
```

## [transfer.sh](https://transfer.sh/)

* [transfer.sh](https://transfer.sh/) is an aesthetically pleasing alternative, great for sharing with people who won't be using the command line
* You can upload files of up to 10 GB
* You can set a limit on the maximum number of downloads
* You can set a limit on how long the file is stored, which is a maximum of 14 days

### Examples

I copied some of the examples from the [transfer.sh](https://transfer.sh/) website:

Standard upload and download:

```
# Uploading is easy using curl
$ curl --upload-file ./hello.txt https://transfer.sh/hello.txt
https://transfer.sh/66nb8/hello.txt

$ curl -H "Max-Downloads: 1" -H "Max-Days: 5" --upload-file ./hello.txt https://transfer.sh/hello.txt
https://transfer.sh/66nb8/hello.txt
# Download the file
$ curl https://transfer.sh/66nb8/hello.txt -o hello.txt
```

Encrypt your files before transfer:

```
# Encrypt files with password using gpg
$ cat /tmp/hello.txt|gpg -ac -o-|curl -X PUT --upload-file "-" https://transfer.sh/test.txt

# Download and decrypt
$ curl https://transfer.sh/1lDau/test.txt|gpg -o- > /tmp/hello.txt
```

There are a ton more examples on the site, which I recommend looking at.

## My transfer script

I wrote a bash script that hides the usage of curl for uploading/download and gpg for encryption/decryption to make for a more user friendly experience. It currently supports [0x0.st](https://0x0.st/) and [transfer.sh](https://transfer.sh/). The features are as follows:

* Upload files
* Upload directories (archives them as they are being uploaded)
* Download files
* Encrypt files as they are being uploaded
* Decrypt files as they are being downloaded
* Decrypt files after they are downloaded
* Log urls to `~/.transfer_history`
* Provide default password with `pass`

It defaults to uploading to [transfer.sh](https://transfer.sh/). The script is available on my Github: <https://github.com/jrodal98/transfer>

### Examples

* Upload a file or directory: `transfer <FILE|DIRECTORY>`
* Automatically encrypt a file or directory using a default password while uploading: `transfer -e <FILE|DIRECTORY>`
* Automatically encrypt a file or directory using a provided password while uploading: `transfer -e -p <PASSWORD> <FILE|DIRECTORY>`
* Download a file: `transfer <URL>`
* Automatically decrypt a file using a default password while downloading: `transfer -d <URL>`
* Automatically decrypt a file using a provided password while downloading: `transfer -d -p <password> <URL>`
* Decrypt file with default password: `transfer -d <FILE>`
* Decrypt file with a provided password `transfer -d -p <password> <FILE>`
* Do all of the above but upload to <https://0x0.st> instead: `transfer -s 0x0.st <everything else>`

