---
created: 1361701235
layout: post
redirect_from:
- article/92/
- node/92/
- steam-linux-thoughts/
title: Steam for Linux thoughts
---
I am a long-time [Linux](http://kernel.org/) user and avid gamer who has always been excited about the prospect of gaming on Linux. I setup various games in wine, applied patched, maintained a game on [WineHQ](http://winehq.org), tried out the Linux version of games like [Unreal 2004](http://en.wikipedia.org/wiki/Unreal_Tournament_2004) and humble indie bundle games, etc. When the first rumors about Valve possibly porting [Steam](http://steampowered.com/) to Linux started to spread a couple of years ago I couldn't wait. Fast forward and now [Steam for Linux](http://store.steampowered.com/news/9289/) is a reality. Obviously, I have been playing with Steam on Linux and various games from my collection that are available for Linux. I noticed a couple of nice differences when gaming on Linux and figured it was worth writing up my overall thoughts.

![Steam for Linux](/files/steam-for-linux-celebration-sale.png)

# No more repetitive installation

Unlike Windows where you are greeted with the [installing DirectX](http://www.reddit.com/r/Steam/comments/18ns10/why_does_steam_always_do_this_when_i_first_play_a/) dialog over and over Steam on Linux simply starts the game. It isn't that big of a deal for typical Linux applications since package managers remove the need for silly installation wizards, but it is one of the small things that just feels smoother.

# Full-screen window focus

One of the more annoying aspects of using full-screen applications on Windows with multiple monitors is that although I can see the applications on the other screens while playing the game I am forced to completely minimize the game in order to use any other applications. Most of the time I usually have other persistent applications such as Skype open while playing games and it is somewhat annoying to switch back and forth. World of Warcraft seemed to get around this with a _full-screen windowed mode_ that removed the window decoration and made it the size of the screen (thus looking the same as full-screen) without entering full-screen mode. This meant you could switch much more quickly and without loosing the game window. Being able to keep an eye on the game while doing something outside is handy and this is the norm in my experience with full-screen applications on Linux.

# Valve listening to community

[Valve](http://www.valvesoftware.com/) created a [GitHub](http://github.com/) [repo for the purpose of tracking issues on with Steam on Linux](https://github.com/ValveSoftware/steam-for-linux) and has done an excellent job reading through, managing, responding, and actually fixing the issues presented. Valve even worked on an [agreeable license that allowed for distributions to package Steam](https://github.com/ValveSoftware/steam-for-linux/issues/66). I filed a few issues myself and was impressed with the prompt responses from Valve employees and see things get fixed.

# Drivers

A big pain point in the Linux world has been video drivers. When I originally started using Linux the proprietary drivers were pretty much the only way to go. During my initial days of Linux I used an Nvidia card and the proprietary drivers provided by Nvidia were not bad, but were definitely nothing like the Windows drivers. Various activities would result in unpleasant behavior and slowness. I later purchased an ATI card and with the open source drivers really coming into their own I started using them. Although the OSS drivers (both radeon [ati] and nouveau [nvidia]) worked great for 2D they had little to no support for 3D). I would [argue that both the OSS drivers perform much better then their proprietary counterparts for general desktop use](http://www.reddit.com/r/linux/comments/fg06w/amd_vs_nvidia_drivers_for_linux/).

With the advent of Steam for Linux I figured it was worth trying out the latest ATI drivers with my Radeon HD 7970 and put them through their paces. I was quite hopefully with the [posts from Valve stating they got better performance then Windows](http://blogs.valvesoftware.com/linux/faster-zombies/) with the Nvidia drivers and seemed to be working to get the drivers improved. The [latest driver release from ATI](http://support.amd.com/us/kbarticles/Pages/RN_LN_CAT13-2_Beta.aspx) has a specific note about a fix for Big Picture mode in Steam.

    [370839]: Resolves a sporadic Steam Big Picture mode crashing issue encountered with AMD Radeon™ HD 7000, 6000 Series

I was pleasantly surprised to find that the drivers worked quite well and a number of my prior complaints were no longer an issue. The drivers are still not perfect, but they perform quite well. WebGL demos run beautifully and the games I have tried from Steam work extremely well. I was even able to max the settings for [Trine 2](http://trine2.com/site/) for a [beautiful result](http://steamcommunity.com/sharedfiles/filedetails/?id=127030021).

# Closing thoughts

It will be interesting to see how Valve's move plays out for the future of the Linux desktop and gaming. I can only image what being able to profile things through the Linux kernel source will allow game developers in terms of tuned and determining issues. There will obviously be holdouts and what not, but without gaming holding people back from a purely superficial view Linux costs nothing and can browse the web just fine...what more do 99% of Windows users need? Of course who doesn't love [wobbly Steam](/files/steam-wobble.png)? I am definitely looking forward to see improved driver quality and with the continued rise of HTML5 there being less and less platform specific development.

For those interested I am running on [openSUSE 12.2](https://www.opensuse.org/en/) ([tumbleweed](https://en.opensuse.org/Portal:Tumbleweed)). For easy installation just visit [software.opensuse.org](http://software.opensuse.org/) and search for [_steam_](http://software.opensuse.org/package/steam).
