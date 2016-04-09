---
title: Bronystate 3.0 Postmortem
date: 2015-12-03
tags: [bronystate]
blurb: |
  I'd grumbled since late 2012 that we needed to drop Wordpress in favor of
  something much simpler that everyone on staff could understand, but the massive
  stress such a project would entail was too much for me to do alone again. I had
  maintained that I would not undertake designing a Bronystate 3.0 unless I were
  paid for the job...
---

# Postmortem: The Story of Bronystate 3.0

with some general state-of-the-world rambling...

[Bronystate](http://www.bronystate.net/) // [Source on Github](https://github.com/Bronystate/brstv3)

* * * * *

I live on the internet.

That's not really hyperbole; I have literally no interaction with anyone outside
the glowy boxes that sit on my desk. My friends from high school all left the
cities after graduation some seven years ago, and while I kept up with the
voyeurism of Facebook for a while, it was clear they'd forgotten me. My family,
well, they deserve a blog post to themselves on the day after I move away. And
other than the occasional cashier or waiter, that's the sum total of meatspace
interaction I get.

And that's why I have Bronystate. Online I'm quite a different person, despite
my best attempts at bringing that side of me into reality. Since stumbling
across their stream in October 2011, I haven't left for more than a day or two,
and even then only because something physically prevented me from getting access
to my IRC client, like being hospitalized. Even *then*, my phone let me check
in. In many ways, I've given myself up to the site, poured dozens of hours and
many a night of lost sleep into projects for them, and generally not done much
of anything creatively that didn't tie back to Bronystate somehow.

It probably isn't healthy. I had bad enough social anxiety exiting high school
without compounding it by becoming distanced from physical people thereafter.
Being trans comprises a monumental level of internal stress in even the most
basic interactions with people, for fear that both they find out and put me
in danger, but also that they don't find out and so walk away with the opposite
impression of me than I want them to have. When the stress of family, the news,
other people, and my own fractured brain get too much, I retreat, and Bronystate
is the one place I can usually assume to be safe. For that reason, and not just
because I'm one of three people with the capability to ensure it, I am heavily
invested in the continued wellbeing of Bronystate.

* * * * *

## Wordpress, an elephant gun for a fly

Our story begins with the decision on the part of 2011 Bronystate staff to take
the site from the loose HTML files it was built on to a Wordpress-based
"real site", with all the fancy amenities a standard CMS would bring. Only
problem was, no one on the staff then knew how to do it, or weren't willing, or
were lost in the labyrinthine arguing that passes for decision-making on staff.

In any event, they came to me in January 2012 to ask if I would take on the
project of creating "Bronystate 2.0", an all-new experience on top of Wordpress.
Saturn, our present hostmaster, was contracted too, although he had a lot more
real world stuff to do than I, and so had much less time to dedicate. Naturally,
I had all the time in the world, and so dove into making things work. Learning
how to write plugins that hooked into Wordpress took a couple days, followed by
hammering out an interface for us to make a pretty record of all the Movie Night
showings, and more than a little back and forth on design decisions the staff
leaders wanted out of the new project. We were going to get a new theater setup,
I was told, where one could drag the boxes for the stream and chatroom around
the page, sensible interface design be damned.

Then came the night of doing the cutover from the old site to the new.
No theater code. The staff member responsible for writing it had been editing it
live on someone else's server, and that someone else had formatted. There were
no backups. Users were expecting a new site in a few hours.

So came the embed switcher, one of my better designs over the years, using an
`iframe` within an `iframe` to hold the stream player, that could withstand not
being cached, and would be written to by a Wordpress plugin to change streams.
The layout of our theaters was finished just barely in time for the scheduled
cutover -- even though staff were already stalling on stream and I could have
had all the time in the world -- and the functional component of the switcher
itself was completed in one night the following week, with some minor tweaks and
patches in the following year. It drove home how bad Wordpress was a choice for
our site, though, as we hardly used *anything* beyond basic news posts and the
switcher's code. At one time, we had three levels of caching between the base
PHP files and what showed up on user browsers, and even that was often barely
enough to survive the spikes of traffic brought on by episode streams.

But survive it did. For three and a half years, Bronystate 2.0 stood up to many
an attack, and the tens of thousands of viewers that have come and gone in the
brony fandom's lifetime. Unfortunately, it was maintained by just three people:
myself, Saturn, and Phase, our master of understanding caching. Of us, over the
course of server migrations, two had and understood the method of managing the
actual files on the server, and neither were me. As for the other twenty staff
members, the less they had to deal with anything resembling code, the happier
they were.

And so Wordpress got out of date.

* * * * *

## Rapid Redesign

Which brings me to our *actual* story, that of the design, construction, and
deployment of Bronystate 3.0 in the wake of Wordpress finally getting exploited.
A frantic call from Saturn late one Saturday night, following an episode stream,
that the server was compromised and would be down until a backup could be blown
out. That it wasn't known how much, if anything, was exposed, despite the attack
likely granting root.

I'd grumbled since late 2012 that we needed to drop Wordpress in favor of
something much simpler that everyone on staff could understand, but the massive
stress such a project would entail was too much for me to do alone again. I had
maintained that I would not undertake designing a Bronystate 3.0 unless I were
paid for the job.

And now the opportunity arose that I would either do it for free, or the site
would be history.

And so I did. Building on the lessons learned from This Vortal Cord, I set out
to design us a new, more streamlined site. Better interface for both the users
and the staff behind the scenes. Gone would be seven different pages all showing
the same stream and, with the sole exception of the hour of a new episode, the
same chatroom -- in its place a single homepage with the stream and chat front
and center. Gone would be a maze of confusing menus standing between a staff
member and the method of setting which stream would be visible -- in its place a
return to the pre-2.0 method of a file with all the necessary details. It would
be simple, cleanly designed, unambiguous.

Most important of all, owing to the circumstances, I had total authority.
I wouldn't have to pass every decision by a committee for approval; I could make
a change, describe in detail the ease of adapting to the change, and trust that
I'm good enough at design that there would be little to no objections.

It took a day to get a basic front page design together, which thankfully also
represented 90% of all the styling the site would need. Implementing the other
major frontend features of the site, like buttons for controlling the embeds,
took up the rest of the first week, including one twelve-hour marathon of waging
war against CSS transitions. The second week was devoted to implementing backend
features, like news posts and the new embed switcher -- no longer writing an
iframe-ception scheme, but rather writing the embedded source directly into the
page. And on the seventh day did Shizuka rest, for all that remained was some
content to fill in about the staff, and the matter of deploying changes smoothly
to the live server...

* * * * *

## Pulling Teeth

And there my miracle working began to crumble. Like the many projects before it,
Bronystate 3.0 benefitted from my undivided attention right up until I needed to
involve someone else in the process. At first, it was getting a hold of Saturn
long enough to establish the deployment chain we would use: site code is held in
a git repo, is pushed to Github as a canonical copy, that gets pulled by Travis
CI, built, and rsync'd down to the live server to replace or update what was
already there. With everything being static HTML, we could eliminate just about
all the caching between the raw pages and the user.

But it took until the end of that first week for Saturn to be available to put
rsync on the server in the first place, leading to another day of me debugging
the configuration files to make Travis build and deploy correctly, without
exposing silly things like the private key it used to do so. My many questions
about little details of the site design seemed to fall on deaf ears, as most
staff simply aren't around as often as I am -- they have lives, and real jobs.
And even after all the major design work was complete, and all that remained was
to get everyone's input on their own spot in the new credits pages, it's a fight
to get it done.

In short, I work alone.

It's not fun, and not remotely as rewarding as my introvert brain tells me it
should be. Having that utter creative authority over the site design meant I got
things done fast, at the expense of preemptively shooting down input from anyone
else. "You need this thing here," someone would say. "I know, it's on my list,"
I would reply, showing the long obsessive task list I had drafted up. I ended up
migrating most of the list to issues on Github, to invite comments and maybe
some assistance, and any ideas anyone had for the site, non-staff included.

None came.

As of this post, the remaining major issues to tackle are the construction of an
"About Us" page with updated staff details, and a "Help" page to guide our less
than technically savvy users on the use of IRC. And it feels like pulling teeth
trying to contact staff members to point them at the file to update, let alone
getting them to actually do it. It's new and different, and so there's a lot of
resistance, and I get that. But there's only so much I can do to make it simple.

* * * * *

## Loneliness

And that brings me to the ultimate reason I decided to write at *length* about
this whole process: that sense that I'm taken for granted, and the sickening,
almost egotistical feeling that I'm the only one able to do work for the site.

I don't do Linux anymore. My server died months ago and I don't have the ability
to fix it, and so I'm without a Linux terminal in my day-to-day. I'm years out
of date on modern web practices -- I only learned about Docker the week after
wrapping up 3.0 -- and even simpler things like navigating properly-hardened
Linux servers completely escape me. I already spent most of my time on Hikari
googling what it was I was trying to do, doing it, and then leaving it to sit
for all time. Having to do that again, but on a system I didn't have physical
control over, just put me off. Doesn't matter we have backups in case I royally
screwed something up; Saturn has too many obligations to respond fast enough to
undo any mistake I might make. So I came up with the push-build-deploy chain,
with suggestions from Phase, so that all the staff, once given write access to
the repo, could easily push *any* changes they wanted right to the site.

But I'm still the only one using it. It leaves me wrongly feeling like everyone
else thinks "oh we got badly hacked? Oh well, Shizuka will fix it."

And generally I do, but Bronystate became what I fear any job in computers would
inevitably lead me to: fighting fires. Start with the small fires no one else
can put out, like a spreadsheet record of the movies shown, or taking over the
failed project to redesign the site, and slowly but surely your entire purpose
is to fight fires all the time. I feel like my attempts at bringing everyone
into the process of keeping the actual site running, not just the chat everyone
works on, are ultimately futile.

* * * * *

## Burnout

It's a little ironic, I suppose. Violet told me once, back in 2012 before she
started going missing for years at a time, that she felt she had nothing left to
contribute to the site. I'd busted my ass multiple times before 2012 was half
over to singlehandedly produce Bronystate 2.0, a new [intermission][v1] video,
and what was meant to be an annual [award show][v2] video (which did see 
[a second year][v3]), but I immensely valued her weekly radio streams, where she
would play or remix or sing along with selections from her vast music library.
But she felt she couldn't add much anymore, and so was starting to drift away.
A lot of the mods we've bled off over the years have expressed the same concern,
nothing to contribute.

And here I am feeling like I contribute too much for my own good.

As of this post, I'm still unemployed, living at home. I'm trans, so my home
life isn't exactly ideal. I've got a bit under six months to get employed, or
else go to war with the American healthcare system to secure a continued supply
of insulin for my type 1 diabetes, and that's without entertaining the agonizing
need for hormone replacement therapy. I'm told again and again that I should go
work in tech, but I can't seem to convey how Bronystate has taught me I want to
stay far away from the field.

Yes I can design, build, and deploy things lightning fast, but it comes at the
cost of sleep, hygeine, nutrition, and my overall health. When I find that flow,
and enough energy to get something done, I can't stop until it *is* done. And 
the moment I *do* stop, whether by completing the project or hitting a wall big
enough to stall my momentum for a day, that's it. I'm done spending energy for
weeks, if not months. My candle burns twice as bright, because a dim candle
doesn't get things done.

* * * * *

## Shameful begging

<iframe class="floatright" src="https://www.youcaring.com/fundraiser-widget.aspx?frid=473310" width="190" height="326" frameborder="0"></iframe>

I stubbornly maintained for three years that I would not do Bronystate 3.0
unless I were paid for it. And now that I have done it, and have a fundraiser
set up to try and help me get things I need that I can't get otherwise, I can't
bring myself to advertise it much. It feels like begging, and trying to take
advantage of the generosity of what friends I have. What I do is irrelevant, the
fact is I don't have a paying job, and so am not entitled to any compensation
for my work... but it would sure be nice to have.

So here I am. If you feel like my contributions to humanity warrant a little
more than a thank you, I invite you to donate, or to share with someone who
wants to, or to ignore it entirely. If I'm going to get out there into meatspace
and contribute *real* things to the world, I need help getting there from where
I am now.

Thanks for reading, many thanks if you donate, and have some happy holidays.  
Stay tuned to Bronystate, where the fun only stops when Shizuka runs out of juice.

[v1]: https://www.youtube.com/watch?v=zmBlxzbLj5s
[v2]: https://www.youtube.com/watch?v=4jNBQ-_4zlo
[v3]: https://www.youtube.com/watch?v=pGUXxjkLo2A
