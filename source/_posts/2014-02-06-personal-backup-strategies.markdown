---
layout: post
title: "Personal Backup Strategies"
date: 2014-02-07 06:24:00
comments: true
published: true
categories: [howto, backup, lifehack]

---
Whenever I tell people that I still have all my email from 1992,
and source code going back to 1989, they look at me like I am some
sort of wizard. One of those old wizard, too, because the first thing
I hear is usually "I was three years old in 1989". I guess I've just
always been good at backups, even though I have never used a
commercial backup software in my life. Or maybe because of that, and
because my strategy is really simple and easy to follow. I figure I
should explain my reasoning, and maybe it'll help somebody else.

<!-- more -->
## What is your backup threat scenario?

Backups give you peace of mind. Depending on your fears, a different
solution might be best for you. 

Are you trying to prevent yourself from a virus, hardware failure, or
from burglars? Then a commercial backup software is probably the
easiest thing to set up, and cloud backup solutions like
[Mozy](https://mozy.com/free?ref=KHLLU7 "Easy Cloud Backup") are very
easy to use. Do you have a lot of data, or don't trust the cloud? Then
you probably want an external disk to store everything on. Are you
afraid that thieves might break into your house? Store that hard drive
off-site, maybe at your office. Afraid the government may come after
your data? Store the drive in a foreign country, preferably one that
is not a NATO member.

Me, I am protecting myself against an asteroid strike that wipes out
North America, or an Earthquake that makes California slide into the
sea, but YMMV.

## How big is your personal data footprint?

When it comes to backup, there are basically two types of data:
Personal files that are exclusively on my own computers, and widely
available ones that I can recover from the internet or elsewhere. 

Your resume and tax documents are a good example that falls in the
first category. Family pictures and video (see my note about Photos,
below) are also hard to recover once they are gone, and for the
average person, these make up the bulk of their personal data.

Files that are easy to recover are Movies and Music you downloaded or
ripped. Even if the earthquake swallows your CD collection along with
the Justin Beiber MP3s you ripped from it, you will be able to buy
that music again, if the Asteroid hasn't wiped out civilization, and
if that happens, let's rebuild without letting the Beiber back into
our lives, shall we? We'll have bigger problems. 

Source code that is stored on github is probably mirrored widely
across the CDNs of this world, although it's in the cloud, so I am
counting it towards the personal data category.  

Adding up everything in the personal data category, I clock in at less
than 300 GiB. In this day and age of [terrabyte SSD
drives](http://www.amazon.com/dp/B00E3W16OU/ref=twister_B00EHFJJHY),
that is a surprisingly tiny number for 30 years of personal computing
history.

## Collect everything in one spot

The reason I never lost anything is that for the longest time, every
time I bought a new disk, I would copy the old disk into a backup
folder on the new one before I replaced it. Eventually, I ended up
with a recursive tree of everything, with a lot of duplication. At
some point, I had multiple computers, and that backup folder kept
growing and growing. At some point, I sat down and cleaned it all up,
removed all the duplicates. You can use
[software](http://www.tucows.com/preview/511502/Duplicate-File-Remover)
to help you with this, and once the duplicates are gone, organize
everything the way you would lay it out in your "My Documents" folder,
and store it into a folder on your main PC or file server. I've named
mine "The Vault", and it sits on a mirrored RAID array of 300 GiB
spinning disks. A Windows SMB share lets all of my other computers
read and write it when I need to get something from there.

## Off-site Backup

Now that I have a single folder, my backup strategy is simple: Keep
everything in there forever, and do frequent off-site backups of this
one folder. I have a 300 GiB external drive, and I use Microsoft's
[SyncToy](https://en.wikipedia.org/wiki/SyncToy) software to mirror my
data from the Vault to the external drive. Since most of my data
doesn't change, this is a relatively quick process, and I can automate
it, which protects me against a failure in the server. Every time I
travel to Europe, I bring the drive with me and exchange it for an
identical one that's in my parents' basement. This is my asteroid
bunker, and it means that my worst-case loss will be a year's worth of
recent data.

## Updating the vault

I treat my Vault share the way one treats the stacks in a library: It
contains everything, but things that are in frequent use have local
copies on other machines, too. The quality of my backup depends on me
frequently synchronizing these local copies back to the Vault. As long
as I keep all my folders in the same hierarchy as they are in the
Vault (remember when I said to lay it out like the My Documents
folder?), I can again use SyncToy to mirror any local changes back to
the authoritative copy in the Vault.

## Potential improvements

* I don't do the synchronization back to the Vault often enough now, because that machine is turned off most of the time. It's also not automated yet. If I didn't have strong reservations against the cloud, I could pair this with Mozy, which lets me configure it to only backup files that are less than a year old.
* I do not have an automated, scheduled synchronization to the external hard drive set up, because the Vault is offline most of the time.
* I could put the Vault on a [NAS](http://www.synology.com/events/2013_us_photographer.php?lang=us), and improve both of the points above, but so far I've gotten away without paying any money and just using recycled hardware.
* Cloud Backup is mighty convenient. I'm planning to set up a separate server at my parents' house and possible other locations, and have the Vault synchronized to it with [Bittorrent Sync](http://www.bittorrent.com/sync), which gives me all the benefits of the cloud along with full control over my own data.

### A note about Photos

Putting all your pictures into the cloud, whether it's on Facebook or
Flickr, is no backup strategy. Aside from the obvious dangers of
putting anything into the cloud, Internet startups come and go on a
daily basis, their privacy and copyright terms can be [extremely
questionable](http://tosdr.org/ "Terms of Service, Didn't Read"), and
they often make downloading difficult. Ask me about the 200 pictures
limit on my Flickr account sometime, and the 7 clicks per photo to
download the original size...

I am not advocating against putting your pictures online. You make
that call. But I am telling you that this is by no means a valid form
of backup. Consider any photo lost that you do not have a personal
copy of.
