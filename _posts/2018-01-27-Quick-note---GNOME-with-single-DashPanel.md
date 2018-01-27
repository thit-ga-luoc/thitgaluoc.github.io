---
layout: post
title: Quick note - GNOME with a single Dash/Panel
tags: [gnome]
---

This post is created just for the purpose of not forgetting things.

The most annoying thing about GNOME is that its desktop is too bloated and include many cosmetic features. Combining the "Panel" at the top(which shows nothing but a few app indicators, the clock and Activities button - which can be called by pressing Super) and the "Dash" (which shows a bunch of icons of your choice) is a crucial step towards a cleaner and more confortable interface.

There are a super cool extension for GNOME Tweak Tool called "Dash to Panel" (don't be confused with "Dash to Dock" - another cool extension incorporated into Ubuntu Aardvark and up) does exactly what I want. However, there has been some glitches in the process that I need to remember.

![panel and dash](https://i.redd.it/2pdjgmdm8uoy.png)
Panel at the top and Dash at the bottom

1. Install `gnome-tweak-tool` using `apt-get`.

2. Head over to [extensions.gnome.org](https://extensions.gnome.org/), install Firefox/Chrome extension then `Dash to Panel` extension.

3. If GNOME extensions page says that connector is not running, run `sudo apt-get install chrome-gnome-shell`, both Firefox and Chrome should work after installing this.

4. Enabling Dash to Panel automatically disable Ubuntu dock extension, so no conflict, but you'll see 2 different dock until you reboot.
