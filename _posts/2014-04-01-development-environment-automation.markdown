---
layout: post
title:  "Development environment automation"
date:   2014-04-05 15:30:52
categories: automation
---

My setup is one 11" laptop for maximum mobility and two
desktop-computers (at home and work) to get the best of both worlds.

When we had a burglary in our office, a lot of hardware was stolen,
unfortunately I went out for a beer and left my laptop in the office, too.
Setting up a development environment costs time, so I started with
automation, especially as I knew the delivery time of my 2nd machine
was 3 weeks.

## boxen: development environment automation based on puppet

I've been using [boxen](https://boxen.github.com/) to automate the last
year, which meant I learned a bit more about
[puppet](http://docs.puppetlabs.com).

Once you're setup, it's great. I evangelized it a lot. A big plus is,
all the versions across all machines are the same to avoid _it works
for me_-effects. Have a look at my
[boxen manifest](https://github.com/riethmayer/our-boxen/blob/master/modules/people/manifests/riethmayer.pp)
to get an idea how a setup could look like and to which detail you can automate.

I was super happy with this setup until I tried to share my setup with
coworkers. Most of the tools have been installed already on their
machine and boxen requires a fresh box, which poses big switching
costs so it wasn't adapted.

One <abbr title="Operating System">OS</abbr>-update later my
boxen-setup was running into issues. Fixing the setup
cost me a whole day, so I decided to try something else.

## sub and bash as development environment automation

We introduced [sub](https://github.com/basecamp/sub) at bonusbox.
So I wrote a simple shell-script to install the development
environment required for our projects via `bb devenv setup`.

Not only this was super easy to setup, all of my coworkers were able
to use it right away with zero switching costs and no learning curve,
as our development environment is pretty homogen and it's pure bash.

The only drawback is, that we eventually have different versions of
particular tools running, which didn't bring up any issues yet.

Cheers,
Jan

Have a look at how simple the setup-script is:

<script src="https://gist.github.com/riethmayer/10004470.js"></script>


