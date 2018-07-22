---
layout: post
title:  "Hand wiring a let's split"
date:   2018-07-21 21:39:21 +0200
categories: jekyll update
---
![My hand wired let's split]( https://i.imgur.com/jCqzdiB.jpg "My hand wired let's split")

[I recently hand wired my first mechanical
keyboard.](https://imgur.com/gallery/yP0R3wh) I had a lot of fun with it and
would definitely recommend the project to others. Me and some friends at the
[Swedish mechanical keyboard Discord server](http://mekaniskatangentbord.se/)
all followed Geekhack user cribbit's [wonderful hand wiring
guide](https://geekhack.org/index.php?topic=87689.0 "A modern hand wiring
guide") with great results, but there was one thing that could be improved.
Another useful guide is [the BrownFox](https://deskthority.net/workshop-f7/brownfox-step-by-step-t6050.html
"BrownFox step by step"). The instructions on how to connect you beautiful hand
wiring matrix to your microcontroller, and how to set up the software config
to use the correct pins. So I will try to expand on that here, standing on the
shoulders of giants. The text should be usable for anyone hand wiring a
keyboard with a layout that already
exists in qmk, but all examples will be for the pro micro microcontroller and
the lets split keyboard files.

## What pins can I use?
> Basically any pin you want, as long as it isn't assigned to anything else!

This was the only answer I could find, but without further explanation that's
not very helpful! So I will try to give some further explanation from what I've
learned so far.

A microprocessor has loads of different input and output pins. Some are analog
and some are digital. Mechanical keyboard switches only need digital pins
(switches are either in or off, never 42 %) but since analog pins can be used
for reading digital signals that's nothing we need to worry about.

There are some hard exceptions that I've found:

- **VCC** and **RAW** are +5 volt sources
- **GND** is ground
- **TX** and **RX** are used for serial communication
- **RESET** is hardwired to reset the microcontroller

These are (to my knowledge) the only pins that can never be used to read
keystrokes. *But wait, there is more...* The let's split keyboard uses I2C or
serial communication to communicate between the halves. I2C is only available
on two specific pins on the pro micro, and serial only on two others. So since I
wanted to use I2C I had to stay clear of those two pins, marked as SDA and SCL
on the pinout below.

![Pinout for the pro micro](https://cdn.sparkfun.com/r/600-600/assets/9/c/3/c/4/523a1765757b7f5c6e8b4567.png "Pinout for the pro micro")

[Picture courtesy of Adafruit](https://learn.sparkfun.com/tutorials/pro-micro--fio-v3-hookup-guide/hardware-overview-pro-micro "Adafruits website")

So as long as you stay clear of those you are free to connect every row and
every column of the matrix to any pin you'd like! Just make sure you take notes
of where you connect each row and col of the matrix, you're gonna need it
later.

## Where in the matrix do I actually attach the wires?
> Just wire the rows and cols of the matrix to the pins

This is something that seems to confuse some people who read the existing hand
wiring guides, so I'm going to try to explain it in a clear way.

What you need to do is making sure that there is connection between each row and
a pin on the microcontroller, and  each col and a pin on the microcontroller.
If you followed "A modern handwiring guide" you can attach the row cable
anywhere on the long wire running on the
black side of the diodes. This can be the end of the wire, one of the
connections between the wire and a diode or any other part of the wire. For the
cols it's even simpler. Literally anywhere on the wire or on any switch pin is
going to yield the exact same result. To make it super clear I've made some
highly scientifical sketches below:

![Illustration of rows](https://i.imgur.com/g2JAmPR.jpg)

&nbsp;

![Illustration of cols](https://i.imgur.com/0GESkrW.jpg?1)

Nothing difficult at all, but it had me worried for a while and I've received
questions about it so I figured I might as well try to explain it.

Deciding on where to attach the wires (to the grid and to the microcontroller)
is simply a matter of considering what will be the most convenient position,
and what will be the most aesthetically pleasing position. This was my first
hand wiring and I'm not super proud but this is how my wiring ended up looking:

![my hand wiring](https://i.imgur.com/Q3G246v.jpg)

## Do I have to wire the halves the same way?
Technically, no. But for the let's split firmware file it's very convenient to
wire them the same way, or mirrored. So that's what I did and what I recommend.

## How do I wire I2C?
[Like this:](https://qmk.fm/keyboards/lets_split/)
![wiring diagram for I2C](https://qmk.fm/keyboards/lets_split/i2c_wiring.png)

## I'm done with the soldering. Now how do I program this thing?
This is probably what is the scariest part for anyone doing their first
keyboard, but it turned out to be super easy. [Except for what you usually need
to do to flash a PCB based let's
split](https://github.com/nicinabox/lets-split-guide/blob/master/flashing.md)
there was only one extra step. You need to let the microcomputer know which
pins to use to communicate with the switch matrix.

All of that information resides in `keyboards/lets_split/rev2/config.h`. Look
for lines that begin like this:

{% highlight c %}
// wiring of each half
#define MATRIX_ROW_PINS { B6, C6, E6, B4 }
#define MATRIX_COL_PINS { D3, D2, D4, D7, B2, B3 }
{% endhighlight %}

This is what they looked like when I was done. Those are from top to bottom and
from left to right the pins I connected to the rows and cols.

One thing that is important to note is that qmk uses the names of the pins on
the *atmega32u4* microchip, not necessarily the labels on your microcontroller.
For the pro micro I looked at this pinout:

![pinout for pro
micro](https://i.redditmedia.com/KyKu8OGG3h3GNG6bKOMu9kmkoYGFlo5JMTArc8mCMq8.png?w=320&s=7d4921d16d6a3ddd187d39905268ea90)

So I wrote down that the first row was wired to pin 10 on the pro micro, I
looked up that this pin is marked PB6 in the picture above, so I typed `B6` in
the code. Simple as that.

That's it! Good luck with your hand wiring project, and hit me up on Reddit
(/u/johananasen) or
discord (janzon#1925) if you need any support!