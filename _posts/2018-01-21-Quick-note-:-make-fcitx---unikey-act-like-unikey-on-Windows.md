---
layout: post
title: Quick note - make fcitx-unikey act like unikey on Windows
tags: [unikey]
---



This post is created just for the purpose of not forgetting things.

1. install fcitx-unikey, which also installs fcitx and dependencies.

2. im-config -n fcitx

3. Set 2 hotkey to trigger input method : Ctrl + Lshift & Shift + Lctrl so that switching input method won't be a hit and miss action.

![fcitx config](http://i.imgur.com/4I4Jhsg.png)

Why 2 instead of 1? Because the great fcitx won't recognize your hotkey combination because you-press-them-in-the-wrong-order.

Why Ctrl + Shift? Because my ring and pinky fingers have been familiar with them from the old windows days.

Why [fcitx-unikey](https://github.com/fcitx/fcitx-unikey) over [ibus-unikey](https://github.com/mrlequoctuan/ibus-unikey), [ibus-bogo](https://github.com/BoGoEngine/ibus-bogo), [scim-unikey](https://github.com/scim-im/scim-unikey), etc? Simple, it's still being [actively maintained](https://github.com/fcitx/fcitx-unikey/commits/master) and it's the only one that offer Ctrl + Shift as a hotkey.

