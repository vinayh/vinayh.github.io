---
title: "Converting Audible books"
date: 2020-08-29T11:09:24+02:00
categories: [ "post" ]
draft: false
summary: "A very succinct guide on converting Audible's proprietary .aax DRM audiobook format into .m4b and other standard formats playable on most any device."
tags: ["tech"]
---

*Disclaimer: I did not create the software referenced here, and I link to the original authors of the projects (to whom I am grateful) throughout. Also, this is intended purely for removing the DRM on books that you have purchased for your own personal use, which I do so that I may access my books on a portable device while running.*


This is a very succinct guide on converting Audible's proprietary .aax DRM audiobook format (for books that you have purchased) into .m4b and other standard formats playable on most any device.

Supposedly, it is possible to use a file from an authorized Audible device (e.g. an Android device with the Audible app) to extract one's Audible authorization code, which appears to be unique to an Audible account. However, I could not easily retrieve this file, so I attempted to extract the code using the [audible-activator](https://github.com/inAudible-NG/audible-activator) tool. This proved problematic on my setup as it appeared to require installing Google Chrome and it was unclear whether it would work with 2FA on my Audible/Amazon account.

Therefore, I decided to go about it another way and used a [RainbowCrack-based plugin](https://github.com/inAudible-NG/tables) designed to crack the authorization code using the file's hash, which was far more seamless than I expected.

---

## Instructions

If you already have the authorization code for your account, you may skip steps 1-3.

#### Step 0: Download the `.aax` file
You can do this for books you have purchased after logging in to your Audible account on a desktop web browser.

#### Step 1: Find the file's hash

`$ ffprobe <your .aax file>`

Example output:
```
[mov,mp4,m4a,3gp,3g2,mj2 @ 0x55a24fe4cf40] [aax] file checksum == 999a6ab80443094b028aaf3882af0a13f177dada
```

#### Step 2: Download or clone the `tables` project from https://github.com/inAudible-NG/tables

#### Step 3: Extract your authorization code
In the project's directory, run `rcrack` from the aforementioned project to crack the authorization code for your account.

`$ ./rcrack . -h <your hash here>`

After a while, you will see a result that lists a code such as `hex:CAFED00D`

#### Step 4: Download AAXtoMP3

It is possible to do this using `ffmpeg` without additional downloads, but I found it easier to use the [AAXtoMP3](https://github.com/KrumpetPirate/AAXtoMP3) tool which facilitates a variety of options such as automatically splitting output files by chapter. Download the Bash script titled `AAXtoMP3` from the project's repository: https://github.com/KrumpetPirate/AAXtoMP3

#### Step 5: Convert the file

Basic use to generate standard `.mp3` audio files involves running, from the directory where you downloaded the script:

`$ ./AAXtoMP3 -A <your hex auth code> <your .aax file>`

Or to generate multiple output files split by chapter in `.m4b` format:

`$ ./AAXtoMP3 -A <your hex auth code> --aac -e:m4a -e:m4b --c <your .aax file>`

You may now play these files on nearly any application or device that supports audio playback, such as VLC (or my outdoor watch). Enjoy!