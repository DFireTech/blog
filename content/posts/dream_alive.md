---
title: "A Dream, An Awakening"
date: 2023-08-26T09:50:10-05:00
draft: false
---
*In a world where support is everything, value for value is there to help*

# Gruezi, one and all!
Welcome back! Not much in the way of sats support this week, *which is a-okay to me*, just being transparent!
Before we begin, I have some corrections to make to a previous article, [The Golden Tech Stack](https://www.goldenblogathon.com/posts/goldentechstack/):

## Corrections A
Initially, I stated that (some liberties here):

> I will be running a start9 node for the blog

Well, this was great in theory as its all FOSS. *For my usecase though, having to hack in a VPN to access from anywhere that isnt TOR is a non-starter.*
Thus, I now have a second umbrel node that I now need to redirect the boosts to! This is still on the same ol' Kamrui mini PC that I got it for though.

## Some shout-outs!
Guys, we really should send some sats over to [BeardedTek](https://beardedtek.org)
He has reached out on X (i am off the mainstream platforms at this time) and got some good info from Alby. [His article is here](https://beardedtek.org/boosts-for-blogs). He highlights the best points in the V4V model *just so greatly* and is a call-to-action on what we are after!

I would also like to shout out [Kyrin](https://fountain.fm/show/bPuh1aiEXWELOqf6q) from Mere Mortals podcast and the Value 4 Value pod (linked here). Episodes 43 and 44 target boostagrams and streaming payments, as well as RSS blocks that can be implemented in blogging, podcasting, videos, etc. His latest is on V4V Music, which brings me to a couple things:

I will be sending 20% of the support that I get with this post (as you all know, I track this pretty well so far) over to BeardedTek and Kyrin. If we get none, I will be pledging 1500 sats a piece at least to both of these guys. BeardedTek is the Dave Jones of the blogging side of this movement and I cannot stress enough how much his help and enthusiasm has energized me to get off my butt and continue writing this blog. 
As for Kyrin, while I haven't listened to MM (this will change this week for sure, he needs them ducks as well!), his info is *invaluable* to V4V in general as well as helping us understand what we can implement and where. 

# The Call-to-Action

We are currently looking for help from the community at large!

What we are after is this:

We need to know what you all know about LN payment processors as well as Fiat payment processors so we can rank them based on some key points!

Elements we are after are:

- Who is the processor
- Does it cost?
- How secure is it?
- Ease of access
- Ease of use
- Do they track you? (This could be in the form of ads/KYC/etc)
- Bait and switch?

You can find a very well written article on BeardedTek's blog on this. 

I would like to thank each and everyone of you that reads our musings and gives feedback. Blogs thrive on community, just like podcasts and music, so thank you. 

# In Distro News

This week, I had a tech issue that I had to sort out with a friend of mine. 
This issue should have been simple, but, me and tech don't always get along.

He is a Windows user and wanted to get Stable Diffusion rocking on his system and was having issues with that due to space, so he buys a 1TB WD Blue HDD. Great! I install it and get it going in five. 
Stable Diffusion didn't like that he was trying to install to a seperate drive. Easy enough, I say. Lets just reformat and set it as a folder on your C drive. Per usual, Windows gives me a fit. Easy enough, *Linux to the rescue!*
I was in a pinch and needed a tiny desktop OS to run GParted and delete the partition from that drive and redo the MS process. 
*I can already hear the taps of your keyboards*
- That system **needs** Linux!
- Aww s***, here we go again.
- What did you use?
To all of those, I say, *touche* and *I'm gettng there. It's the journey.*

My buddy is **not** a techincal person and relies on stuff to 'Just Work (tm)', so, Linux isn't a fit for him yet. 

After neglecting the issue at hand for a few days, I go over after work as I was helping with a different project over there and get a flashdrive from him (2GB!!).
I take a dive into [DistroWatch](https://distrowatch.com) and find on their mid-page posts [miniOS review](distrowatch.com/weekly.php?issue20230821#minios)

## miniOS. Fast. Simple. Reliable.
*The ISO for the file was under 400MB*

I flash the ISO onto this drive and it all fit wonderfully. Plug it in, boot it up. I am greeted by what I can only describe as one of the most beautiful desktops I have seen since I used endeavousOS
{{<center src="/d_a/miniOS_SS.jpg">}}

I mean, wow. Clean, **blazing** fast, and all under 400MB for just a minimal iso. They have larger ones and ones you can build yourself as well. 

Here is what they offer:

|Name|Size|Arch|Desc|
|----|---|------|------|
|Flux|355MB|32 and 64|Visually similar to Slax, most compact|
|Minimum|375MB|32 and 64|Based on Debian 11, compressed w xz. Minimal software|
|Standard|560MB|32 and 64|Most balancd;zstd compression|
|Maximum|695MB|64|Complete system, swiss army knife|
|Ultra|1370MB|64|AiO system w extras|
|Puzzle|530+|64 base, 64 full|Like standard, but, can contruct system modularly|
* Actual size may vary slightly according to their site

*Editing TGD here. I hope this table looks nice.*

I haven't even talked [docs](https://minios.dev/docs/) yet. 


### The Documentation

Tl;Dr *They have fantastic documentation*

The docs page is basically just a minimalist wiki that goes over whatever you need.
Anything from "How do I actually make this portable?" to "What modularity do you speak of?" is on here. 

### Portability

To make it a super portable (fits on a flash drive) system, its very easy.

- Download whatever ISO you wish and open that file. I used XArchiver, which came on my Debian 12 system. 
- Format a flash drive to either ext4 or FAT32 (MBR)
- Inside of the .ISO is a folder `minios`. Take that folder and copy it to the root of that drive
- In the `minios` folder, find `minios.conf` and open it up. Change the settings in here to meet your demands. SSH is enabled by default for other init systems. 
- Finally, find `boot` inside of `minios`. Open a terminal here and run `bootinst.sh` or `bootinst.bat`. This will make the drive bootable
- Reboot and boot from the drive you just used and voila, you now have miniOS on a thumb drive!

If you select `Persistent` everything should go like any other linux instance you run. 

### Air Gap

The way I can see this being cool is that now you have your data on the go. Is this near as private as Tails? Probably not. With some technical knowledge, however, I can see this being really cool. Get Tailscale set up with this or whatever VPN you prefer and you have an OTG disk to meet your demands. I considered possibly using this as a software BTC cold storage, but, that needs some researching before I go and put the coin on the drive. 

Speaking of the 'Coin...

# Site Updates

You now see the "Donate" button on the page! BeardedTek was able to get a modal up and running! In [this post](https://beardedtek.org/get-a-boost-with-alby/) he goes over the modal in Ghost and this can be placed into Hugo in the site where you want it to appear. *However* this didnt allow the scrolling. I am very grateful for this!


# Impressions

miniOS looks great and is fast. I suggest you try it out if you are into persistent and OTG desktops that you can impress your peers with. 
You can find them at [https://minios.dev/](https://minios.dev)

That's it for this edition! Have any comments? Boost em in or just leave a comment! Share this about if you found it valuable to you!

Have a great day, hope to chat soon.
