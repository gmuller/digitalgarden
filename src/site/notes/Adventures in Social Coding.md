---
{"dg-publish":true,"dg-permalink":"adventuresinsocialcoding","permalink":"/adventuresinsocialcoding/","tags":["code","java","processing"],"created":"2011-02-18T16:42:56+00:00","updated":"2025-01-25T18:15:45.099-05:00"}
---

Many moons ago, I created a little tool called the [GOL Sequencer Bank][1]. In order to create the tool, I used [RWMidi](https://github.com/wesen/rwmidi), a Java/Processing library created by Manuel Odendahl of [Ruin&Wesen][5]. While creating the sequencer bank, I discovered that the RWMidi library had no support for MIDI Sync messages, preventing me from syncing the sequencer with a master, like Ableton Live. This simply would not do.

In the past, I would have looked for another library, but given that I had the source readily available, and had already written a _ton_ of code interfacing to RWMidi, I decided it would be a better use of my time to modify the RWMidi library to support sync messages. The changes were minimal, and I learned a lot in the process about MIDI, Java, and the art of modifying open source.

Not too long after that word got out that I had made this change to the RWMidi library, and I started getting one-off requests to send my modified library to folks for their own use. For instance, [John Keston](https://johnkeston.com) over at [AudioCookbook](http://audiocookbook.org/) built his [GMS synth](http://audiocookbook.org/gms/) using my modified library.

A short time after that, Mr. Keston approached me with what I thought at the time was a strange requirement: modify the library to support greater than 24PPQ for recognizing 64th and 128th note resolutions. In plain English, John wanted the GMS to support 64 and 128 notes using plain-jane MIDI clock. I thought it couldn’t be done, but loved the challenge, and modified the RWMidi library accordingly. [[Converting 24PPQ Midi Sync in Java\|It was hard.]]

These modifications to the RWMidi library have only been available as a custom change to the GMS and the GOL Sequencer Bank, but through the power of social coding, I can make these changes available to anyone who wants them. I forked the RWMidi library on Github, incorporated the changes there, and issued a pull request to Manuel to include them in the main source.

You can check out the source [here](https://github.com/gmuller/rwmidi).

I discovered the beauty of social coding, and I plan on eventually uploading the source of both the HarmonicTable and the GOL Seqeuencer. I’ve had requests in the past to make changes to the synth that I just don’t have time for, this way people in the know can simply fork the source, make their own changes, and ask me to pull them into the main body of work.

As more and more regular Janes and Joes become savvy programmers (i.e. our children), I expect we’ll see the power of social coding change the way we think about how software is made in general…

## Part II

Proving that [Jung](http://en.wikipedia.org/wiki/Synchronicity) and [Sting](http://en.wikipedia.org/wiki/Synchronicity_(album)) were on to something, while making the changes above I received an e-mail from a user struggling with RWMidi:

> _I'm trying to send a simple Pitch Bend message and it's proving impossible!_

I was already in the code, so I poked around. The problem was simple.

**You can’t.**

There are no exposed methods to send pitch bend in RWMidi. So, newly empowered with forked code from GitHub, I created a method in the MidiOutput class that would allow a user to do this. I did a little research on the format for pitch bend method and came up with [this](http://www.srm.com/qtma/davidsmidispec.html):

> **_Pitch Bend_**  
> The pitch bend wheel is also a continuous controller on many MIDI keyboards. Possibly, the Pitch Bend message is in its own category because it is something likely to be done frequently. The two bytes of the pitch bend message form a 14 bit number, 0 to 16383. The value 8192 (sent, LSB first, as 0x00 0x40), is centered, or no pitch bend. The value 0 (0x00 0x00) means, bend as low as possible, and, similarly, 16383 (0x7F 0x7F) is to bend as high as possible. The exact range of the pitch bend is specific to the synthesizer.

Crudely translated this simply means that pitch bends are in the range of 0 to 16383, with 8192 meaning “don’t bend”. Bending up means sending a value higher than 8192, bending lower means sending a value lower.

The format is a little funky and deserves some explanation. Essentially rather than sending two 8-bit bytes to represent the 16-bit value, I have to send two 8-bit bytes representing a 14-bit value, which will be interpreted as a number between 0 and 16383. So, in order to send the correct bytes I have to take my original value and rescale the size of each byte from 8 to 7 bits.

For the plain-jane 16-bit number 4000, the two 8-bit bytes would look like:

|     | 8   | 7   | 6   | 5   | 4   | 3   | 2   | 1   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LSB | 1   |     | 1   |     |     |     |     |     |
| MSB |     |     |     |     | 1   | 1   | 1   | 1   |

The 14-bit value that I need to send would be:

|     | 8   | 7   | 6   | 5   | 4   | 3   | 2   | 1   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| LSB |     |     | 1   |     |     |     |     |     |
| MSB |     |     |     | 1   | 1   | 1   | 1   |     |

When visualized like this, it becomes clear that what we need to do is shift the last bit in the least significant byte into the first bit of the most significant byte, thus shifting the entire MSB over a place to the left. The way I have to think about this is to consider the two 8-bit bytes as zero-padded 7-bit bytes, and understand that when the receiving device gets the message, it will trim the last bit of each byte and put the registers together to form one 14-bit value.

Doing this is somewhat straightforward. I take the modulo of the pitch bend value and the greatest possible value I can represent with a 7-bit byte, which is 128, to get the LSB. From there I can simply divide the pitch bend value by 128 to get the binary value of the MSB. To send a pitch bend of 4000, I would send the numerical value 32 for the LSB, and 31 for the MSB. When you strip the first zero of each of these bytes, then string the registers together into one 14-bit number, you get 111110100000. This isn’t the most efficient way of doing this of course, I could shift some bits around, masking off values, etc. But I didn’t. The method I created is pretty straightforward:

``` java
public int sendPitchBend(int channel, int value) {
		if (value &gt; MAXPITCHBEND) value = MAXPITCHBEND;
		if (value &lt; MINPITCHBEND) value = MINPITCHBEND;
		byte lsb = (byte) (value % 128);
		byte msb = (byte) (value/128);

		ShortMessage msg = new ShortMessage();
		try {
			msg.setMessage(MidiEvent.PITCH_BEND, channel, lsb, msb);
			receiver.send(msg, -1);
			return 1;
		} catch (InvalidMidiDataException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
			return 0;
		}
	}
```

There is some error checking here to make sure we don’t try to send a stupid value. Obviously if the max is 16383, there is no reason to allow 16384, not to mention a negative number. To test this I created a little sketch that looks a heck of a lot like a pitch bend slider, using the mouse’s X-position to control the amount of bend. I mapped the value of the 800 pixel width box to match our minimum and maximum pitch bend values, 0 and 16383 respectively:

``` java
public void setup ()
	{
	    size(800, 200);
	    midiOutput = RWMidi.getOutputDevices()[2].createOutput();
	}

	public void draw ()
	{
	    background(0);
	    stroke(255);
	    line(mouseX, 0, mouseX, height);
	}

	public void mouseMoved ()
	{
	    midiOutput.sendPitchBend(1, (int) map(mouseX, 0, 800, 0, 16382));	    
	}
```

And there you have it. The code is checked in to my [fork](https://github.com/gmuller/rwmidi) of the RWMidi library and a pull request has been issued. 

Creating a pitch bend slider on the screen is small potatoes with functionality like this. My sketch should only serve to get you started. You should think about mapping other more exotic pitch bend controls to this method. Perhaps a sine wave generator, allowing you to change the pitch somewhat like an LFO would. Perhaps you can make the pitch “wobble” when the mouse tracks over some position on the screen? That old MIDI keyboard on your desk can already bend the pitch with a slider, how can you take this functionality and make something unique?
