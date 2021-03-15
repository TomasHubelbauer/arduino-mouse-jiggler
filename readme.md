# Arduino Mouse Jiggler

This repository hosts Arduino code for turning an Arduino Leonardo into a mouse
jiggler.

## What? Why?

A mouse jiggler is a either a hardware appliance of a software program for mouse
cursor movement automation for the purpose of preventing a computer from going
to sleep. Sometimes software-based solutions are locked down by IT departments
at which point hardware-based solutions save the day.

## Inspiration

There are several projects that implement other peripherials and translate them
to mouse inputs, for example:

[![](https://img.youtube.com/vi/t8mE1ayw5bo/default.jpg)](https://www.youtube.com/watch?v=t8mE1ayw5bo)

They all use the [`Mouse.h`](https://www.arduino.cc/en/Reference.MouseKeyboard)
library that seems to come with the Arduino IDE. The
[`move`](https://www.arduino.cc/reference/en/language/functions/usb/mouse/mousemove/)
method in this library moves the mouse cursor relatively to its current position
making it ideal for a jiggler implementation.

## Code

```ino
#include <Mouse.h>

void setup() {
  Mouse.begin();
}

byte shift = 2;
byte wait = 250;

void loop() {
  Mouse.move(shift, -shift, 0);
  delay(wait);
  
  Mouse.move(shift, shift, 0);
  delay(wait);
  
  Mouse.move(-shift, shift, 0);
  delay(wait);
  
  Mouse.move(-shift, -shift, 0);
  delay(wait);
}
```

This code will move the mouse in a diagonal shape, staying around the origin and
not sliding off. The on-board RX/TX LEDs will indicate USB communication, so you
know the board runs and works.

![](recording.gif) ![](screencast.gif)

## Arduino

According to the [`Mouse.h`](https://www.arduino.cc/en/Reference.MouseKeyboard)
documentation, [32u4 based boards](https://learn.adafruit.com/how-to-choose-a-microcontroller/next-step-32u4-boards)
and Due and Zero boards are supported:

> These core libraries allow a 32u4 based boards or Due and Zero board to appear
> as a native Mouse and/or Keyboard to a connected computer.

This means Micro and Micro Pro should work. It didn't for me. I tried:

- [Micro](https://store.arduino.cc/arduino-micro) - Windows USB driver did not
  find it
- [Pro Micro](https://www.sparkfun.com/products/12640) - Windows USB driver did
  not find it
- [Uno](https://store.arduino.cc/arduino-uno-rev3) - No support for USB HID
- [Leonardo](https://store.arduino.cc/arduino-leonardo-with-headers) - Worked!
  I actually had a Freeduino Leonardo, but to the Arduino IDE it's all the same

After flashing the program using the Arduino IDE, the board restarts and starts
acting as a mouse immediately. From this point on merely connecting it to the
computer will power it and make it act as a mouse until disconnected.

While having issues with the Windows USB driver not finding Micro and Micro Pro,
I also tried on macOS, but I was unable to flash it on macOS either. The bare
Arduino IDE install was not enough and I tried installing the driver recommended
by SparkFun: https://learn.sparkfun.com/tutorials/usb-serial-driver-quick-install-/all

But, this article points to an installer they host on their CDN and it is an old
version which is not signed by Apple and won't work on Catalina.

I found the up to date version on https://ftdichip.com/drivers/vcp-drivers. I
installed the signed-by-Apple latest version for macOS, but the Arduino IDE did
not show any USB serial ports anyway. I gave up trying to make it work on macOS,
but it probably would work for the Uno and the Leonardo, mimicking the issue on
Windows.

## Raspberry Pi

I've implemented a Raspberry Pi based mouse jiggler in a complementary repo:
https://github.com/TomasHubelbauer/raspi-mouse-jiggler
