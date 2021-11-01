
[![Arduino CI](https://github.com/RobTillaart/FunctionGenerator/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/jsoncheck.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/FunctionGenerator/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/FunctionGenerator.svg?maxAge=3600)](https://github.com/RobTillaart/FunctionGenerator/releases)


# FunctionGenerator

Arduino library to generate (numeric) wave forms for a DAC


## Description

This library presents a class for a function generator in software. 
It is typical used to control one or more DAC's.
To maximize signal quality one has to apply all (or most) processor power 
to calculate new values over and over again to get enough resolution. 
In practice the generator is useful for (very) low frequencies, 
think 0.01 - 25 Hz, depending on waveform and processor (see indication below).

Note: this class generates float values, performance wise this can be optimized,
to achieve higher speeds at cost of accuracy / precision.


## Performance

Indication of what can be done, note that the values need to be transported 
to a DAC or serial port too. Numbers based on performance example, for one 
single signal.

| Processor    | Clock    | Waveform | usec/calls | max freq |
|:-------------|---------:|:---------|-----------:|---------:|
| Arduino UNO  | 16 MHz   | sawtooth |  62        |  60 Hz   |
| Arduino UNO  | 16 MHz   | triangle |  74        |  50 Hz   |
| Arduino UNO  | 16 MHz   | square   |  53        |  1000 Hz |
| Arduino UNO  | 16 MHz   | sinus    |  164       |  25 Hz   |
| Arduino UNO  | 16 MHz   | stair    |  81        |  50 Hz   |
| Arduino UNO  | 16 MHz   | random   |  37        |  1000 Hz |
| ESP32        | 240 MHz  | sawtooth |  3.8       |  1000 Hz |
| ESP32        | 240 MHz  | triangle |  3.9       |  1000 Hz |
| ESP32        | 240 MHz  | square   |  2.8       |  1000 Hz |
| ESP32        | 240 MHz  | sinus    |  13.6      |  250 Hz  |
| ESP32        | 240 MHz  | stair    |  4.8       |  800 Hz  |
| ESP32        | 240 MHz  | random   |  1.3       |  1000 Hz |


- assumption minimal 250 samples per period to get a bit smooth signal.
- ESP32 can do more calculations but 1000 Hz seems to be a nice upper limit
  for a software based function generator.
- if one wants to control multiple DAC's one need to divide the numbers. 


Note: hardware function generator https://github.com/RobTillaart/AD985X


## Interface

### Constructor

- **funcgen(float period = 1.0, float amplitude = 1.0, float phase = 0.0, float yShift = 0.0)**
All parameters can be set in the constructor but also later in configuration.


### Configuration

- **void  setPeriod(float period = 1.0)** set the period of the wave in seconds. 
- **float getPeriod()** returns the set period.
- **void  setFrequency(float frequency = 1.0)** set the frequency of the wave in Hertz (1/s).
- **float getFrequency()** returns the set frequency.
- **void  setAmplitude(float amplitude = 1.0)** sets the amplitude of the wave. TODO point to point?
Setting the amplitude to 0 gives ?what? 
- **float getAmplitude()** returns the set amplitude.
- **void  setPhase(float phase = 0.0)** shifts the phase of the wave. Will only be noticeable when 
compared with other waves.
- **float getPhase()** returns the set phase.
- **void  setYShift(float yShift = 0.0)** sets an Y-shift in amplitude, allows to set some zero point.
- **float getYShift()** returns the set Y-shift.


### Wave forms

t is time in seconds

- **float sawtooth(float t)** slowly rising, steep decay.
- **float triangle(float t)** triangle form, duty cycle 50%.
- **float square(float t)** square wave with duty cycle 50%.
- **float sinus(float t)** sinus wave, has no duty cycle parameter. 
- **float stair(float t, uint16_t steps = 8)** steps up signal
- **float random()** noise generation.
- **float line()** constant voltage line
- **float zero()** constant zero.

Line() and zero() are functions that can be used to drive a constant voltage from a DAC 
and can be used to calibrate the generator / DAC combination.


## Operational

See examples.


## Future

- test a lot more...
- test performance and workable range.
- sawtooth-reverse(), stair-reverse()
- adjust duty-cycle.
- investigate duty-cycle eg for sinus.
- investigate white noise, pink noise etc.
- investigate performance gains.
- stand-alone functions in separate .h??
- mapping to voltage function.
- ESP32 version as separate task...
- check for synergy with https://github.com/RobTillaart/AD985X
- example with DAC. 8 12 16 bit.
- example with potentiometers for 4 parameters
- RC function.
- Trapezium wave.
- record
- play "recording"
- external clock to synchronize two or more function generators.
- fix this list of ideas into something like a plan :)


