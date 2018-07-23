---
layout: post
title:  "Building a great keyboard from scratch"
tags: diy keyboard
---

> This post describes the design and build process for my very first hand built
> mechanical keyboard.


## Why would anyone want a keyboard that ...

### ... is so tiny?

### ... has it's keys in such a strange pattern?

### ... is split into two parts?

### ... has no symbols on it?

## The design process
There are a lot of great tools available for anyone who wants to design their
own keyboard. I started out with the software [Keyboard Layout
Editor](http://www.keyboard-layout-editor.com/#/gists/663d8980d4f937ea0359820a42585a96)
to create a key layout that I was satisfied with. I settled for two halves with
4 by 6 keys, for reasons mentioned above. 

picture 1 kle

The layout above was then pasted into another great tool, the [Plate & Case
Builder](http://builder.swillkb.com/) which exported the layout in SVG or DXF
format. This file can be sent directly to a CNC company if you want them to cut
the plates for you, but I wanted to make some further modifications to the
design. So I fired up Autocad Fusion 360, a great and quite user friendly cad
software, and started sketching. 

picture 2 swilkb

The only part I was interested in from the DXF file was the actual switch
holes. I added a new, tighter plate outline. My goal was to make a plate big
enough to securely hold the switches, but small enough to not be visible from
above. Because of this, there was no room for spacers (standoffs that separate
the top plate, where the switches are mounted, from the bottom plate). These
had to be placed in the middle instead, which also hid them from plain sight. 

Doing it yourself is seldom the path of least resistance, and that was true for
this project as well. I wasn't confident our CNC router would be able to cut
the plate safely with a bit thinner than 1/8 inches. I have  a bunch of broken
1/16 inch bits to thank for that realization... This meant I had to make some
modifications to the switch holes to get the corners tight enough. In the
picture below my dxf looked like the hole on the right, but I applied a fillet
operation to make the corner match the diameter of my tool. The switch would
still fit snuggly since all four sides were still touching it. 

picture 3 holes

tk rim design

The last step of designing is called CAM, and that is where the 3D design is
translated into instructions for the machine, called G-code. I won't go into
details on that step but I will say that adjusting CAM settings and trying to
route parts of the design is definitely where I spent a lions share of my time
on this project. 

## Manufacturing
