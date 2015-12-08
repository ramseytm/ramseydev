---
layout: post
title: "Tool of the Month - Atom!"
description: "The ongoing monthly series begins and this month we're covering Atom!"
author: Tyler Ramsey
date: 2015-12-20 21:00
twitter_card: summary
header_image: http://tylerramsey.net/assets/201512/atom.jpg
include_math: false
tags:
- development
- atom
- editors
---

![Atom Text Editor]({{ site.url }}/assets/201512/atom.jpg "Atom Text Editor")
{: style="text-align: center"}

As an excuse to play around with some new development frameworks and tools I'm starting a new ongoing monthly series, Tool of the Month! Each month I'm going to provide a basic overview of a development tool or framework that I've been playing around with including my intial impressions and recommendations. So, with that introduction complete let's get started! This month's tool is [Atom - a hackable text editor](https://atom.io/)!


## What is it?

Atom is a text editor that bosts multi-language support, autocompletion, a package manager, cross-platform support and deep extensibility. Beyond basic syntax highlighting and formatting users can add intellisense, compilation and autocompletion by extending the editor with core and community developed packages that integrate third-party utilities. It runs on [Electron](http://electron.atom.io/) which allows cross-platform applications to be developed with html5, javascript and css. This makes it easy to extend and stylize Atom using the web technologies that many people use Atom to work with. It is also completely [open source](https://github.com/atom/atom).

<!-- excerpt -->

## What's good about it?

Full disclosure, I haven't used Atom on any large projects yet so my impressions only go skin deep. That said, I've been using it exclusively while working through some simple react/redux apps along with some existing typescript code and I think there is a lot to like in Atom. For web development specifically I can see it becoming a main contender as my editor of choice for forseeable future. For a relatively new product Atom provides a nice set of features.

### The package manager

As a [Sublime](http://www.sublimetext.com/) and [Visual Studio](https://www.visualstudio.com) user I've been spoiled by development tools sporting a well integrated package manager. A package manager coupled with a large, mature repository can allow seamless extensibility to a product and Atom's package manager [apm](https://github.com/atom/apm) delivers well on this account. So far it's been one of the highlights of my experience with Atom so far.

#### APM in Atom
![Atom Package Manger GUI]({{ site.url }}/assets/201512/apm.jpg "Atom Package Manager in Atom")
{: style="text-align: center"}

Atom supports it's package manager with a nice GUI provided by the settings tab in the editor. As shown above, a user can search for new packages, update existing ones or removed unneeded ones all from this tab. I found the available updates pane to be very helpful and the interface as a whole to be very user friendly.

#### APM in Console

![Atom Package Manger Console]({{ site.url }}/assets/201512/apm_console.jpg "Atom Package Manager in Console")
{: style="text-align: center"}

For those more comfortable with the command-line apm has you covered here as well. As shown above it provides a nice set commands that will be familiar to anyone who has experience with [npm](https://www.npmjs.com/), the package manager for [node.js](https://nodejs.org/en/). There's a very good reason for this familiarity in fact, apm is built on top of npm and even spawns npm processes in order to manage packages. This means users comfortable with npm should be equally comfortable using apm.

Whether you're more comfortable with GUI or a command line I'm finding both a pleasure to use. The package manager along with the outstanding packages I've had the opportunity to use are one of the major selling points of Atom as a whole.

### TypeScript Support

Speaking of outstanding packages in the apm repository, [atom-typescript](https://atom.io/packages/atom-typescript) is one that boasts as being "the only TypeScript plugin you will ever need" and I'm inclined to agree with them! This plugin provides Atom with first class [TypeScript](http://www.typescriptlang.org/) support including autocompletion, compile on save, live error reporting, react support, refactoring and whole host of more features. 

# What could be better?