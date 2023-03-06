# Night Shift
Harmonic and Dimension Phaser

Provided to the DIY community for non-commercial use by Brian Thornock, copyright 2023


## Overview

I love designing modulation effects. You can do so many weird things
with it. After doing my Aeronometron design last year, and after
waffling around on harmonic tremolo circuits, I had the crazy thought
of a harmonic phaser using the CD4066 chips as the variable
resistors. And, while I was at it, I thought maybe it would be fun to
have the option of a "dimension phaser" mode, where the dry signal is
mixed with two separate phasing paths that are 180 degrees out of
phase. Well, I couldn't stop there, I wanted to be able to change the
relative phase of the two LFO's as well. Put all that into a single
box, and you get the Night Shift.

## How it Works

There are lots of phasers out there, and at its heart, the Night Shift
is two parallel, 4-stage phaser circuits. The thing that makes it
unique is that it can work as a "harmonic phaser", where the input
signal for each path is filtered (one low pass, one high pass) and
then phased with an LFO that is out of phase. Because of the PWM
necessary to drive the CD4066 as a variable resistor array, a
microcontroller was the obvious choice. The great thing about this is
that it makes arbitrary relative phase shifts to the LFO's super
easy. Want to have your LFO's in sync for classic phaser tones? No
problem, set it to minimum. Want them 180 degrees out of phase for
classic harmonic operation? Easy, it's right there. Want 67 degrees of
relative phase? You can do that, too, regardless of LFO speed,
something that would be impossible with analog circuitry.  

Another unique feature is the ability to use it as a "dimension"
phaser, where the dry signal is mixed with the two phaser sections
(input filters bypassed) and out of phase with each other, but with
the ability to do the relative phase shifts as with the harmonic mode.
The digital LFO also allows for any LFO wave shape, though I
programmed in sine, triangle, and rising sawtooth by default. Add in a
sweepable filter frequency control, and you've got all the building
blocks of the Night Shift.  The input stage is a buffer for splitting
the signal off into the various parts needed by our phasing
stages. It's a simple non-inverting, unity-gain buffer as seen below.

TODO: Night Shift Input Buffer

The signal from the input buffer is then filtered by two Sallen-Key
filters. One is a high pass for the high frequency content and the
other is a low pass for the low frequency content. The filter control
is a dual gang pot that automatically adjusts the crossover frequency
with one control. You will note that they are not identical. There is
a small region where the two filters "overlap". This is so that the
low frequency section doesn't get completely lost at some
settings. Notice how the dry signal for each phasing stage is pulled
after the filter and mode switch. This is so that any phase shift due
to the Sallen- Key filters themselves is removed to eliminate unwanted
static comb filtering.

These Sallen-Key filters are bypassable by way of a 3PDT "Mode" toggle
switch. With the filters in place, the Night Shift operates in
harmonic mode. With them bypassed, it's dimension mode. Because of how
the dry signal is pulled, we want one of them (I arbitrarily chose the
high side) needs to be disconnected during harmonic mode to eliminate
a dry signal level change relative to the phased signals.

TODO: Night Shift Sallen-Key Low Pass Filter

TODO: Night Shift Sallen-Key High Pass Filter

After the filtering, the two phase stages are identical. These are
basically what the Phase 90 uses, except that the JFET's are replaced
with the CD4066 switches for the variable resistance elements. I
didn't know until after I had this mostly designed that Parasit
Studios also has a CD4066/PWM phaser (though I knew about the Sentient
Machine, as it was the inspiration/basis for the Aeronometron).

TODO: Night Shift Phase Shifting Stages

Once the phased signals are created, they are mixed into a summing
amplifier with the dry signal to created the whooshy, phasey sound we
love. This is a pretty standard summing amplifier/output buffer.

TODO: Night Shift Output Buffer

As mentioned above, the LFO is created using a microcontroller. It
uses analog inputs to determine the speed and phase relationship
between the two LFO PWM outputs. An analog input also serves to
indicate which of the wave shapes to be used. The circuit itself is
pretty simple, though the code has a little bit going on. I also opted
for an RGB LED so that I can have it blink any color I want (really
great idea given that I'm colorblind....).

TODO: Night Shift Digital Section

The variable resistance elements in the phase stages are implemented
using CD4066 CMOS switches that are opened and closed super fast. This
implementation of switches with a single PWM driving them ensures that
they are precisely matched and no JFET matching is necessary. There
are two banks, one for the high side and the other for the low side.

TODO: Night Shift CMOS Switches

Lastly, the power section produces +9V, VREF, and +5V for all the
various elements. This is my standard mixed signal (digital and
analog) power section.

TODO: Night Shift Power Section

## BOM

The BOM below is the list of parts I used for mine along with
quantities. This is a surface mount project due to the number of IC's,
but most should be easily available from Tayda and similar suppliers.

TODO: BOM

## Schematic

The schematic for this project is provided below as well as a separate image in the project documentation folder.

TODO: Night Shift Full Schematic

## Build Notes

Here are some things I noted from building the Night Shift that might
be helpful to you. Please read this section to make sure you don't go
through excessive frustration.

### Enclosure Size/Drilling

The Night Shift fits nicely into a 125B. The board is a little too wide to fit a 1590B.

### Jacks

Whatever jacks you use for in/out and power in 125B are fair game; no restrictions here.

## In Closing

The Night Shift will give you phasing sounds not readily available in
other DIY pedals and, as far as surface mount goes, is a pretty
straightforward project.






