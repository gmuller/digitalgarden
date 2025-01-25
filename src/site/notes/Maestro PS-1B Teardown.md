---
{"dg-publish":true,"dg-permalink":"maestrops1teardown","permalink":"/maestrops1teardown/","tags":["electronics","gear"],"created":"2009-01-21T22:15:18+00:00","updated":"2025-01-24T20:18:23.569-05:00"}
---


![Maestro7B.png](/img/user/assets/Maestro7B.png)

A friend of the band recently had a problem with his Maestro PS-1B, a phase shifter from the early 70's designed by the famous Tom Oberheim, and asked me to take a look at it. Based on the problem, a loud humming at the output with no sign of any other signal, I figured it should be an easy fix. It was probably just a ground fault or a bad output or something, so I took it home with me and cracked it open.</p>

After getting the case off (which was bolted together with rarely used square head nuts), I flipped it and took a look at the circuit board. I was a little awed, first by the simplicity, then by the age of some of the components. The board was downright sparse. I thought to myself _"they could have made this a lot smaller if they had just move everything closer together"_. Here is a shot of the top board:

![MaestroBoard.png](/img/user/assets/MaestroBoard.png)

I noticed that the ground pin on the plug was broken. I spliced in a new power cable and turned it on, plugged in a guitar and my Ritz amp and had a go. Seemed fine. Nice and wobbly, quiet and phase-shifty. Ha, Knew it'd be an  easy fix. I ran the new cable into the back, soldered everything into place and turned it on to test one more time. **BUZZZZZ**.

That's odd, I just tested it a second ago and it seemed fine. After checking the solder joints I just made, I turned the unit on again to see if it was still buzzing. Yep. Then I noticed an anomaly, there were two components on the board that didn't look native. At the top left there were two capacitors that looked far newer than the rest of the components:

![MaestroCaps.png](/img/user/assets/MaestroCaps.png)

This has been repaired before.

So I did the naturally stupid thing and poked them. No buzz. _For once, doing something stupid yielded a solution._ The problem was a loose solder joint, one of the caps was literally falling out of the socket. I must have inadvertently been pressing the capacitor back into its slot when I had the cable spliced in. Lucky, lucky catch (though I would have found it anyway, after poring over the other side of the circuit board).

I pulled the board out and turned it over to fix the joint, and was greeted with the strangest circuit board traces I have ever seen in a production machine:

![MaestroCircuitTraces.png](/img/user/assets/MaestroCircuitTraces.png)

It looks like the solder traces were drawn on with a pencil. Like the design was made casually by hand, without a ruler on a cocktail napkin with a crayon. Incredible. I looked closely at the verbiage on the board (hoping to find a production year), and instead found the name Oberheim

![MaestroOberheim.png](/img/user/assets/MaestroOberheim.png)

The _same_ [Tom Oberheim](http://en.wikipedia.org/wiki/Thomas_E._Oberheim) of [Oberheim](http://en.wikipedia.org/wiki/Oberheim) synthesizers, who went on to create the first polyphonic synthesizer, the OB-X, and the DMX drum machine (not the rapper). Neat. I wonder if all circuit designs during this era we as freeform and organic.

After sitting in awe for a moment, I bundled everything back up, tested it a couple of times and called it fixed. I did a little research on this unit and came up with [this site](http://www.wingspreadrecords.com/maestro_ps1_page.html), which has plenty of history on the PS-1, and other effects, not to mention Maestro and Tom Oberheim.

Lessons learned

  * Don't assume there is only one problem
  * Do something stupid every so often, it might work.

And here is an unnecessary shot of my test setup, with my super sweet Ritz Cracker Box Amp:

![RitzCrackerBoxAmp.png](/img/user/assets/RitzCrackerBoxAmp.png)

