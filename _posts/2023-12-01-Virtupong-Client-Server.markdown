---
layout: post
title:  "Virtupong: Server authoritative multiplayer SwiftUI app"
date:   2023-12-01 14:02:19 -0800
categories: jekyll update
---
In a two week sprint I recently finished my new app named Virtupong (available [here](https://apps.apple.com/eg/app/virtupong/id6468917351)).

It was interesting to make a multiplayer Swift app, coming from doing Unreal Engine multiplayer work. I needed to roll my own UDP/TCP handshake mechanism. It was a bit more complicated than doing `Execute Console Command -> open IP_ADDR` that is so easy to do in Unreal.

It turned out to be fairly straightforward, and I'm happy I did it because I can use that system for future projects.

Right now the game is iOS only, but I am planning on supporting visionOS. There will be some interesting learnings about supporting the visionOS platform that I plan to write another blog about.
