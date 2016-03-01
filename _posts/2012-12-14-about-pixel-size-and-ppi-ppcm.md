---
layout: post
title:  About pixel size and ppi/ppcm
author: Zhuyi Xue
tags: pixel,ppi,ppcm
---

Yesterday, I learned the concept of **ppi** (pixel per inch), similarly there
is **ppcm** (pixel per centimeter), and find that the ppi of my monitor is
110. Then I become curious about how many pixel I have for each letter in my
terminal.

I also have the question how big is a pixel in mind for a quite while, now I
realized it's not the right way to ask the question.

ppi is self-explained. Looking at my monitor resolution, it's `1280 x 800`,
i.e. 1280 pixels in width and 800 in height. I am only interested in width for
now, height is all the same. I found that a full line in my terminal contains
about 180 letters. Assuming this 180 letters use up all pixels in width
(actually this is an approximation, though I am aware some pixels are used by
the border of the terminal window), this leads to

    1280 / 180 = 7 pixels

which means each letter occupies 7 pixels in width. Of course this number
depends on the letter (but if it's mono spaced, it's fine), the font, and the
font size (11pt in my case), as well. For a given letter and font, the number
increases as font size gets bigger.

Using ppi, I also found the size of pixel is

    1/110 = 9.1^(-3) inches ≊ 0.02 cm = 0.2mm.

I was a bit surprised at this, a pixel on a regular monitor is actually quite
big. I thought it would be invisible with naked eyes, but now if I look closer,
definitely I can see the pixels, and even count them, I did that for letter `T`
in my terminal, it's around `7 +/- 1`, so my calculation is right. The
fluctuation is due to my burning eyes. I should not look at the screen so
closely, but it's around 7 for sure.

The pixel size gets even bigger when it comes to projectors. That's for
sure. When I was presenting stuff, I totally remember those squared dots when I
come close to the screen.

Therefore, don't ask what is the size of a pixel because it depends on the
display. Ask for ppi instead, all right, it also depends on the display, but it
is more of a standard way of asking such questions, and you can calculate the
size of the pixel if interested easily with ppi.

Above is a little memo for my understanding of ppi.
