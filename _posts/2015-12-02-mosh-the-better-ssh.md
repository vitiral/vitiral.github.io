---
layout: post
title:  "i3 Arch: the promised land for vimmers"
date:   2015-09-07
categories: linux
---

One of the greatest things about my job at Solidfire is that they are very open to whatever schedule employees want to use. Since I live in Lakewood and work in Boulder (CO), my commute by driving is 50-75 minutes each way. However, I happen to live next to the light rail, and a bus ride is between 75-90 minutes each way. Therefore I do a good amount of work on the bus (using my T-mobile tethering).

Remote work, especially on the bus, has it's own pitfalls -- namely that connections can get dropped and re-established repeatedly throughout a trip. This is why it is essential that I use **mosh** instead of ssh.

**Mosh** is a better ssh for laptops and other situations where the network is faulty. It uses UDP packets instead of TCP and has various tricks (including a running mosh instance on the server) that overcomes many networking issues, and helps you diagnose when you network is faulty.

The first way it is better than ssh is if your connection goes down (or you put your laptop to sleep) it doesn't freeze for 5 minutes before timing out -- instead it gives you a helpful message telling you the last time it talked to the server, and as soon as a connection is established you are back in buisness. I don't have to tell how awesome that is compared to ssh.

It also has "guess typing" that helpfully displays what you are typing even if there is no connection with the server. This is very helpful for intermittent connections, as it allows you to work on even the crapiest of connections.

If you work remotely, especially on a laptop or a crappy connection, try out mosh. I recommend using it with tmux+zsh+vim for maximum productivity :)
