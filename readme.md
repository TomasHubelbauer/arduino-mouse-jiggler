# Arduino Mouse Jiggler

This repository is my attempt to build a mouse cursor jiggling appliance for
preventing a Windows computer from locking.

## Why/when to choose this route? What are the alternatives?

- Turn off lock screen in Windows settings - unless group policy forbids it
- Use [Caffeine](https://www.zhornsoftware.co.uk/caffeine/) or
  [Mouse Jiggler](https://github.com/cerebrate/mousejiggler) to make Windows
  think keyboard/mouse is being used - unless Windows ignores virtual inputs
  (probably as a result of the same group policy) and locks itself anyway

## Software

There are several projects that implement other peripherials and translate them
to mouse inputs, for example:

https://www.youtube.com/watch?v=t8mE1ayw5bo

They all use the [`Mouse.h`](https://www.arduino.cc/en/Reference.MouseKeyboard)
library that seems to come with the Arduino IDE. The
[`move`](https://www.arduino.cc/reference/en/language/functions/usb/mouse/mousemove/)
method in this library moves the mouse cursor relatively to its current position
making it ideal for a jiggler implementation.

To use the Mouse library:

```ino
#include <Mouse.h>
```

To move the cursor with the library:

```ino
##include <Mouse.h>

void setup() {
  pinMode(LED_BUILTIN, OUTPUT);
  Mouse.begin();
}

int state;

void loop() {
  // Blink the LED to indicate activity
  digitalWrite(LED_BUILTIN, state % 2 == 0 ? HIGH : LOW);
  state++;

  // Move the mouse in cyclic diagonal
  Mouse.move(1, 0, 0);

  delay(3000);
}
```

## Hardware

Once the code is completed, that should be it and we can use the Arduino IDE to
build and upload the code to the Arduino board. The compatible boards seem to
include
[32u4 based boards](https://learn.adafruit.com/how-to-choose-a-microcontroller/next-step-32u4-boards)
and the Due and Zero as per the `Reference.MouseKeyboard` documentation:

> These core libraries allow a 32u4 based boards or Due and Zero board to appear
> as a native Mouse and/or Keyboard to a connected computer.

I tried a [Micro](https://store.arduino.cc/arduino-micro), but the Windows USB
driver wouldn't recognize it. I also tried a Uno, but that board does not
support USB HID.

I will try a Micro Pro that I have.

I think the USB connection between the board and the computer should be able to
power the board. Once `Mouse.begin` is called, the computer will hopefully be
able to immediately recognize and use the mouse without need for further setup.

The experience then should be just the bare Micro with a short USB cable, once
plugged to the computer taking over the cursor and jiggling in around the point
it was at at the moment of plugging in. This indefinite (or rather, interrupted
only by disconnecting the jiggler board) movement should keep the computer
awake as it should be indistinguishible from human operating the device by the
computer.

## Arduino

I was unable to flash the board at all on Windows even when using the latest
Arduino IDE and the USB driver that comes with it.

I was unable to flash it on macOS either. The bare Arduino IDE install was not
enough and I tried installing the driver recommended by SparkFun:
https://learn.sparkfun.com/tutorials/usb-serial-driver-quick-install-/all

But, this article points to an installer they host on their CDN and it is an
older version which is not signed by Apple and won't work on Catalina.

I found the up to date version on:
https://ftdichip.com/drivers/vcp-drivers/

I installed the signed-by-Apple latest version, but the Arduino IDE still did
not show any USB serial ports.

## Raspberry Pi

This same thing is possible to do on a Raspberry Pi, but it has no advantage
over using an Arduino. Maybe it would make sense with a Pi Pico? But that
board is fairly new so there are probably not many libraries for such stuff
for it.

https://github.com/stjeong/rasp_vusb

For the Pico specifically:

- https://github.com/iot-crazy/pi-pico-usb-joystick
- https://github.com/speedypotato/Pico-Game-Controller

## To-Do

### Find or buy a Micro

I should have some.

### Find USB cables to use

If I'm going to program the board using the Mac I'll need a USB C to a micro
USB cable. I'll also need a micro USB to a USB A cable for the Windows computer
to control.

### Develop the code, flash the Micro and test out whether it works or not

Essentially, "do this stuff" once I have the equipment and time ready.
