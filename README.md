About
===

WARNING -- WIP -- Work in Progress -- GITHUB doesn't allow me to mark libraries private without paying, so this
is public even though it isn't anywhere near functional yet. Ordinarily, I would mark private until ready then
make public.

DO NOT ATTEMPT TO USE THIS LIBRARY AT THIS TIME.

If you want to contribute code (or funding), please let me know.

This warning will be removed once there's an actual library here.

This library provides basic SYSLOG client functionality as an alternative to Serial logging on Particle.

Intent is to make SYSLOG almost as easy to use as Serial.print().

## Table of Contents

This README describes the general use of the library.

	- About -- Describes the library
	- Table of Contents -- This list
	- The bad news -- Talks about the limitations of the library
	- Library Details -- Primary man-page style documentation of the library
	- Example Usage -- An example for using the library

## The bad news

I'd love to have written a general library that would work across all Arduino-like microcontrollers with network connectivity.
Unfortunately, since the underlying network functions are so radically different across each and every different one, there's
really no generic networking library to base such a thing on.

I've written the library for the particle platform initially as that's where I need it the most. I will likely also port it
myself to the ESP8266 boards (specifically the Adafruit ESP8266 Huzzah breakout).

Porting to wired ethernet boards may require more effort. I've tried to stay as close to the Arduino generic WiFi library as possible.

## Library Details

```

#include "Syslog.h" /* If using Particle DEV or most other environments */
#include "Syslog/Syslog.h" /* If using build.particle.io Cloud IDE */

void openlog(const char *ident, int option, int facility);

```

## Example Usage


void setup {

}


void loop {

}



