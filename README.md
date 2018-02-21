# OLEDMagicEye

Arduino sketch to drive an SSD1306 OLED display at 128x64 pixel resolution.
The display will emulate a
[magic eye](https://en.wikipedia.org/wiki/Magic_eye_tube "Wikipedia: Magic Eye Tube")
valve (vacuum tube).

Magic eyes were commonly used as tuning indicators on valve radios and as recording-level
indicators on valve tape recorders.
Some were linear and showed a ribbon of light from the side of the valve.
Others were end-viewed and showed a glowing circle of light.
This program emulates the circular type.

The display looks best when used with a blue/green OLED. White OLEDs also work, but don't
look as good as one that more closely resembles the original green phosphor of the
magic eye.

## The Arduino Connections to the OLED

The OLED display needs a 3.3V power supply and 3.3V signal levels.
I use a
[CD4050](http://www.ti.com/product/CD4050B "TI: CD4050")
hex non-inverting buffer chip (one of the
[CMOS 4000 series](https://en.wikipedia.org/wiki/4000_series "Wikipedia: 4000 series")
chips.

The OLED's Reset (RST) pin is connected to the Arduino's Pin 9 via the CD4050.
OLED Chip Select (CS) goes to Arduino Pin 10 via the CD4050.
OLED Serial Data (SDA) goes to Arduino Pin 11 via the CD4050.
OLED Data/Control (D/C) goes to Arduino Pin 12 via the CD4050.
OLED Serial Clock (SCLK) goes to Arduino Pin 13 via the CD4050.

## The Sketch

To draw the magic eye graphics on the OLED, I use a rudimentary graphics library that writes
into a 128x64 pixel frame buffer (unsigned char Frame\[8]\[128];).
Each bit in the frame buffer corresponds to a pixel on the OLED.
Bits set to '1' represent pixels that are lit on the OLED.

The graphics library includes line-drawing, ellipse and circle drawing, and text drawing
functions.
It's not fully debugged yet.

Having drawn the magic eye graphics in the frame buffer, I copy the entire contents of the
frame buffer over to the OLED every 40 milliSeconds.
This transfer goes via the Arduino's hardware SPI interface and the CD4050 level shifter.
The entire background drawing, magic eye drawing, and transfer process takes between 11ms
and 35ms (slowest when drawing a wide image with the eye mostly closed).

## The Controls

To control the magic eye, I use three analog input pins on the Arduino. Analog pin 0 sets the
'opening' of the eye, i.e. how much of the green phosphor is illuminated. This would
have been the recording-level signal or the tuning signal in the original magic eye circuit.

Two more analog inputs (A1 and A2) are connected to the X and Y outputs of an analog joystick
(mine is actually desoldered from an old Playstation controller). The A1 input controls the
width of the magic eye drawing, and A2 controls its position (left/right) on the OLED.

## Video on YouTube

[OLED Magic Eye Display](https://www.youtube.com/watch?v=JbrQHo7OYMs "YouTube: OLED Magic Eye Display")

