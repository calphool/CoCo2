# CoCo2

## February 8, 2020

This is my collection area for Tandy Color Computer 2 Projects.  I've only got one at the moment.  It's a composite video mod for the CoCo 2.  The CoCo was designed 
in a time when it was perfectly reasonable to assume that all televisions had an analog RF input.  Of course that's no longer the case, so something has to be done 
to get a CoCo 2 to work on a modern monitor or television.  

My CoCo 2 has a [vertically oriented RF modulator](./motherboard.jpeg).  So, I designed a replacement PCB for the inside of the RF modulator to convert it to a 
composite out and sound out RCA port.  I desoldered the MC1372 chip from inside the old modulator board, since I'm repurposing that chip.

I'm not an expert, but it appears that the video graphics display chip of the CoCo2 (the MC6847) outputs a luminance and two chrominance channels (called PHI-A and PHI-B).
Even though one of these is a luminance channel, and in theory could drive a black and white composite signal, it isn't at the right voltage levels or bias, and so it 
must both be conditioned and mixed with the chrominance channels.  That's where the MC1372 chip comes in.  Originally intended as an RF signal generator, it has a 
useful "test mode" that allows it to output a mixed composite (or CVBS) signal that can then be conditioned to drive a television monitor that accepts CVBS, which most 
TVs still accept (though some very modern ones don't have separate inputs for it -- they accept the CVBS channel on the green RGB input).

Basically the way you accomplish this is to put a diode between pins 13 and 14 of the MC1372, which is where the "tank" circuit is supposed to go, which traditionally 
was used to "tune" the output to match the expected frequency rate for analog channel 3 or 4 on a traditional television.  With the tank circuit bipassed, the chip 
produces a plain composite signal with colors mixed in appropriately.  So, then you simply use some transistors to get the signal into the right level and bias, and tune the 
bias level with a potentiometer.  I also included a potentiometer to tune the output level of the sound signal.  

With some right angle headers, you can simply plug this board in where the original RF modulator can went.  I made the PCB the same size as the original PCB in the 
RF can, so hopefully I'll be able to put it back in the can, drill another hole for the sound RCA jack, drill a clean hole in the back of the CoCo 2 case right where the 
"channel select" hole was, and it should be a nice clean little mod.

As an FYI, there also exists a [VGA mod](http://www.cocovga.com/) that can be purchased for the CoCo 2, which uses a FPGA and plugs right into the socket where the video display generator chip 
(the MC6847) sits.  This would probably be the cleanest signal you can get out of a CoCo 2.

I may decide to build my own VGA or HDMI board, though if I do, I may use the outputs of the original MC6847 rather than "co-opting" it like the the CoCoVGA does.  I've 
wanted to learn how to program FPGAs, and I found this [little reference](http://ocw.abu.edu.ng/courses/electrical-engineering-and-computer-science/6-111-introductory-digital-systems-laboratory-fall-2002/lecture-notes/l18.pdf) to an approach for how to coax a digital signal out of the analog outputs of the MC6847.
Using that approach, I think it would be possible to use the [AM26LS32](http://www.ti.com/lit/ds/symlink/am26ls32ac.pdf) to feed a FPGA (probably along with a few other 
signals from the MC6847 to be able to detect sync signals) to paint an in-memory video buffer, and then figure out some way to push that buffer out an HDMI (TMDS) port.  

I've attached a few pictures below to help someone who was building one of these.

The first picture shows the view from behind.  ![From Behind](view_from_behind.jpg?raw=true)  Notice that I used a Y cable for the RCA jacks, one for 
composite video and the other for audio.  They share a common ground.  You could just wire your cable directly to the PCB if you were okay with having 
cables hanging out of the back of the machine (which wouldn't be a huge issue, since the CoCo actually has a power cable permanently attached).

The second picture shows how the Y cable is connected. ![Showing Y Cable](showing_y_cable.jpg?raw=true).

The third shows a front-on view.  ![Front on view](front_on_view.jpg?raw=true) There are a couple of things to notice.  First, you can see the right angle 
header connecting the Coco's motherboard to the composite board, and also a wire going from ground on the composite board to a hole where the original can was soldered (it's grounded on the 
motherboard).  The next thing to notice is at the top.  R1 and D1 are not installed as the 1.0 PCB shows.  I made a mistake on the original schematic.  So, 
what you can see here is that D1 is inserted in the opposite direction of what the PCB shows (the orientation band is to the RIGHT, not the LEFT).  Also, 
you can see a hacky pair of resistors soldered together.  I didn't have a 360 ohm resistor, so I "made one" by combining a 330 ohm and a 39 ohm, which was 
close enough.  If you have a 360 ohm, you can make a cleaner hack.  Notice that the resistor pair is inserted in the TOP hole for R1, and R1's bottom hole 
is left empty, and the other end of the resistor pair is soldered to the back of D1.

The fourth shows a close up view.  ![close up view](close_up.jpg?raw=true)

The fifth shows another top view ![close up top](view_from_top.jpg?raw=true) where you can clearly see that D1 is in the opposite orientation of what shows 
on the PCB. It's pretty important that if you decide to build this PCB, you follow the PDF instructions carefully. 
