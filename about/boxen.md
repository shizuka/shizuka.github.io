---
title: my boxen
date: 2015-05-09
---

[defboxen]: http://www.catb.org/jargon/html/B/boxen.html
[arsguide]: http://arstechnica.com/gadgets/2012/11/how-to-set-up-a-safe-and-secure-web-server/

# my boxen

> [boxen (n):  plural of box][defboxen]
>
> one ox : two oxen :: one box : two boxen

- [asuka](#asuka)
- [hikari](#hikari)
- [takara](#takara)
- [sayuri](#sayuri)

-----

## asuka {#asuka}

### desktop / windows 7 professional

```
Asuka 2.0:
CPU: Intel Pentium G3258 3.6GHz
RAM: 8GB
GPU: ATI Radeon HD 5750
HDD: Internal C: 0.5TB - OS drive
     Internal L: 1.5TB - previous OS drive
     External N:   2TB - media library
     External R:   2TB - general storage

Asuka 1.0:
MDL: Gateway DX4320
CPU: AMD Phenom IIx6 3.2GHz
```

My window on the world. Everything I do on the internet or with computing
in general passes through Asuka. Gaming, programming, writing, everything.

Asuka started life as a prebuilt box from Gateway, an investment during my
college years, to replace the prebuilt Dell that preceded her, Kimiko. My
desktop experience up to that point was quite underpowered for a 2008
system, and Asuka promised to bring a bit of futureproofing. More memory,
more drive space, a much better processor, and a more up-to-date low tier
GPU to play with. For a long time, she handled everything I could throw at
her, although my desk and house were terrible for ventilation, and she
routinely showed core temperatures in the 60s-70s C under load.

In 2011, I added a second monitor, an old 1280x1024 LCD, and discovered the
freedom that two screens brings. The extra graphical load exacerbated the
heating problem, though, and during the following summers it was rare for
the core to reach below 50C. Still, despite driving two monitors, and with
my new interest in streaming games for Bronystate, she soldiered on.

In 2014 the first signs of trouble started showing. The power supply had
been rattling badly for most of the previous winter, owing to dust buildup
in the fan throwing off bearings. Eventually the fan gave up, and the PSU
promptly died, preventing boot. My first repair went without incident,
though, since power supplies are standardized to a tee, and a replacement
fit everything just perfect. Moving to a new home helped, cleaner air meant
less dust buildup, and she had a little more room to breathe. Finally, the
thermal abuse proved too much, and several capacitors on the motherboard
popped in early 2015, which I only discovered after pulling out the CPU.

Unwilling to risk damaging the core by scraping it off the heatsink, I
opted to build a successor, with a new more cooling-friendly chassis, new
motherboard, and a new CPU not ravaged by years of overheating. Asuka 2.0
kept the RAM, GPU, and hard drives of the original, and my first experience
properly building a desktop went well. Unfortunately, I was misled on the
abilities of the replacement CPU, with only two threads instead of six.
More power per thread, but the raw multitasking ability was cut short. As a
result, streaming became limited to the less CPU-intensive games, to avoid
bottlenecking on the encoders, and occasionally Winamp can get hung up on
programs thinking abnormally hard.

Still, the new motherboard has capacity for a great deal more memory than I
can fathom needing, and the chassis will happily seat an SSD someday. Asuka
2.0 might not have the raw grunt of her previous incarnation, but she's
still the workhorse of the family.

-----

## hikari {#hikari}

### server / debian jessie

```
MDL: Dell Optiplex GX270
CPU: Intel Pentium 4 HT 2.8GHz
RAM: 2GB
HDD: 40GB
```

Hikari is the third server to run out of my bedroom, preceded by herself in
another OS, and a much less powerful server, Haruko.

From the start, my goal was to have a Linux-based server I could run IRC
out of, having been spoiled by a friend's SSH box with irssi for a couple
years at the end of high school. With Haruko, I learned the finer details 
of web serving, and built several exceedingly simple sites to experiment.
Both Haruko and Hikari began life as workstations, donated from my mom's
workplaces, and so were woefully underpowered for much more than irssi and 
Apache. Haruko ran with almost perfect uptime from June 2008 to December
2012, interrupted only when the power would go out during intense storms.

Hikari I set up according to [Ars Technica's guide][arsguide] to setting up
a "safe and secure" web server, using Ubuntu Server, nginx, and MariaDB for
the stack, as opposed to Haruko's Debian, Apache, and MySQL. Making the
mental shift from Apache's `.htaccess`-driven configuration to nginx's
do everything in `/etc/nginx` and restart model was difficult. I was used
to just turning on `mod_php` and going, whereas nginx demanded setting up a
pool of PHP workers standing by in case the population of China all wanted
to see This Vortal Cord at the same time. I felt less able to experiment or
deploy new web applications because it would mean looking up, again, what
the command was to let nginx know this folder needs to route things to this
page, and how do I restart, okay wait did I forget a `try_files` somewhere?
Whereas you could just drop an `.htaccess` in the directory with Apache and
you were done.

Still, I continued experimenting. I added Java to Hikari and hosted a
heavily modded Minecraft server for a while, taking advantage of the memory
Haruko didn't have to work with. In early 2014, Hikari played host to a
private shard of World of Warcraft, populated by a tiny group of Bronystate
regulars. Surprisingly, it held my attention longer than I expected an MMO
to, since my last foray into WoW lasted all of a day out of a ten day
trial. Abusing administrator commands to skip the typical grind made it a
lot more fun, but after we'd all had our fill of the quests, and couldn't
gather ourselves together regularly enough to try bigger dungeons, it fell
off too. In late 2014, I added a Teamspeak 3 server, to replace the Skype
group we generally tried communicating through. And finally, in very late
2014, I started work on HikariBot, an IRC bot for Bronystate intended to
handle our official Twitter account, and eventually manage the crazy number
of user bans we keep track of. The Twitter functions I scraped together in
a couple days; the Banhammer is quite a lot more complex and still under
construction.

But hearing Asuka got new parts for blowing her motherboard, Hikari's
decided she wants in on the fun, and has started randomly segfaulting at
the worst moments. She's a headless server, set up once and then let go,
with most of the configuration done through Webmin or SSH. Because of that,
I don't have the spare cables to give her a head, and can't see what's
going wrong at boot, or run a memtest, or chkdsk, or anything. So Hikari is
sitting silently next to my desk, thinking about what she's done. It's why
I've migrated This Vortal Cord to Github Pages, so that at least I have a
presence, even if she doesn't.

-----

## takara {#takara}

### laptop / windows 7 professional

```
MDL: Dell Inspiron 1525, refurbished
CPU: Intel Pentium Dual Core 1.86GHz
RAM: 2GB
GPU: Intel Integrated GMA965
HDD: 160GB
```

The emergency backup, for when something has gone catastrophically wrong
with Asuka, or if I'm simply out of town and can't be in my chair.

Takara was the first investment I made when entering college, following a
much older laptop barely able to run modern browsers much less a modern
office suite. She's a refurbished laptop from 2008, and is easily the most
abused of the bunch, in terms of what I've tried to get her to do. An Intel
Integrated GPU on two gigabytes of memory doesn't make for a lot of power,
and getting anything more flashy than Bejeweled to run was an exercise in
severely lowered expectations.

I used Takara for document editing, of course, and browsing related to
classes. In my networking courses, I made frequent use of Cisco Packet
Tracer, constructing elaborate virtual networks and following, say, an HTTP
handshake from the first ARP to the pretend page getting served up. I also
ran Linux and Windows installs out of VirtualBox, for experiments I didn't
feel comfortable performing live on my server or desktop.

And then there was the gaming. My schedules left me with a lot of downtime
between classes and getting a ride home, downtime to be filled trying to
get this game or that functional. Team Fortress 2 at 640x480 resolution,
massively downsampled textures, and super low-poly models was outright
adorable to play, invoking memories of the early 3D days of Quake or Half
Life 1. Being able to cram a local server with bots made up for the low
framerate and aim sensitivity.

More recently, Takara saw intense use during the week Asuka 1.0 was out of
commission, forced to hold my IRC terminal, Netbeans IDE, browser, and
any game I could get running, on one screen. Asuka 2.0 coming online was a
welcome break for the poor laptop, who normally only sees use when we take
trips to visit family.

-----

## sayuri {#sayuri}

### phone / android 4.1.1

```
MDL: HTC One S
```

Smartphones are amazing. In 2008, when I graduated high school, the
fanciest phone in sight would have been a much simpler touchscreen with a
slide out keyboard. It was more common to see iPods than phones, and if you
wanted to quickly check the time, you flipped up a Razr or, gasp, still had
a watch. And then somewhere after that, smartphones took off, to the point
that everyone has a pane of glass in their pocket with the sum total of
human knowledge at the swipe of a finger.

That is, if your carrier supports it. Not a year after purchasing one, HTC
announced it was ending support for the One S, in favor of the *helpfully*
named "One" coming out. This of course instantly broke the ability to
search for help on the One S, like the ability to update it, or what would
soon break. Cell reception along the I-94 corridor through Wisconsin is
abysmal, rendering it fairly useless for streaming my music library off
Google. Apps keep cached data in magic black boxes, slowly eating the
limited "phone storage", despite having several free gigabites on "internal
storage", and causing the Play Store updates to fail routinely. Trying to
use Firefox is an exercise in futility; it will run, but at speeds that
make dialup seem appealing.

Still, it's a magical pane of glass with the sum total of human knowledge
in my pocket, and while I might not use it for much more than tracking my
blood glucose or sleep cycle, it's hard to imagine the same life without
it.