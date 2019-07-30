---
title: "Fighting Ransomware"
description: "Ransomware is a scary type of malware that restricts computer access by demanding a ransom be payed before function is, or isn't, granted agian. I ran into one of these the other day and here is how i got rid of it."
date: 2012-08-08
---

![Randsomware example](/img/ransomware.jpg)

A couple of days ago I was summoned as "the computer guy" to look at a potential virus problem. It seemed that when logging on to the computer, a big message showed up claiming the police has blocked the computer. It also showed the computer's ip and isp (all of which can easily be picked up by any program running on the computer). The message above says **Detta operativsystem är blockerad på grund av brott mot svensk lag. Fastställt följade brott:.** The message itself looks quite legitimate aside from some spelling problems, so i can see how some might get scared by this.

The nasty part of the message is that after it showed up the computer wouldn't respond to most commands like <kbd>Alt</kbd>+<kbd>Tab</kbd>, <kbd>Alt</kbd>+<kbd>F4</kbd> etc. <kbd>Ctrl</kbd>+<kbd>Delete</kbd> worked however, but the task manager could not be started. Only option was to log out again (through <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Delete</kbd> prompt). Other accounts on the same computer however did not show the same signs of infection, so I could do some research (using google) on another account. I tried running a full scan on the computer from another account, but nothing interesting showed up. After a few other dead ends I found a solution that worked, so maybe this can help someone else too.

## Prerequisites:

### Alternative 1:
- **Usb stick** (about 1Gb should be enough)
- **UNetbootin** : This is a small application that can burn images onto a usb memory stick, which can then be used to boot the computer from the memory stick. [Download application here](http://unetbootin.sourceforge.net/). Remember where you save it (an exe file), it does not come with an installer.

### Alternative 2:
- **Empty CDROM**
- **CD Burner application** (that can burn iso files)
- **Kaspersky Rescue Disc** : This is a linux distro that comes with kapersky applications already installed and that can be booted from CD or usb memory. [Download iso file here](http://rescuedisk.kaspersky-labs.com/rescuedisk/updatable/kav_rescue_10.iso)

## Removal
1. Alt 1: Install iso onto usb drive, Alt 2: Burn iso onto an empty CDROM
2. Boot with usb drive into Kaspersky Rescue OS
3. Run Kapersky windows unlocker, for more detailed instructions follow the link under resources.
    1. Start a terminal
    2. Run the command windowsunlocker
    3. Run command 1 to clean the registry (note any file paths to suspicious files).
    4. Run command 2 to save boot sector copies
4. Optional: Run a full scan
5. Startup the computer, now the message should not show up anymore
6. Remove the suspicious files manually, i found two files located at:
`C:\Users\<Username>\AppData\Roaming\deo0_sar.exe` and   
`C:\Users\<Username>\AppData\Loacal\Temp\deo0_sar.exe`

## Resources
- [Article from swedish police (in swedish)](http://www.polisen.se/Aktuellt/Nyheter/Gemensam/jan-mars/Ny-form-av-datorbedrageri/)
- [Blog post about the problem (in swedish)](http://blog.perhellqvist.se/blog/2012/03/14/ny-elaking-laser-datorn-och-kraver-losensumma/)
- [...and follow up (in swedish)](http://blog.perhellqvist.se/blog/2012/03/22/lura-polis-trojanen/)
- [UNetbootin](http://unetbootin.sourceforge.net/)
- [Kaspersky Rescue Disk 10](http://rescuedisk.kaspersky-labs.com/rescuedisk/updatable/kav_rescue_10.iso)
- [Kapersky windows unlocker](http://support.kaspersky.com/faq/?qid=208285998)