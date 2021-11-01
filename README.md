
[![Arduino CI](https://github.com/RobTillaart/FunctionGenerator/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)

[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/FunctionGenerator/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/FunctionGenerator.svg?maxAge=3600)](https://github.com/RobTillaart/FunctionGenerator/releases)


# FunctionGenerator

Arduino library to generate (numeric) wave forms for a DAC


## Description

TODO.

- hardware function generator https://github.com/RobTillaart/AD985X

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
- sawtooth-reverse(), stair-reverse()
- adjust duty-cycle.
- investigate duty-cycle eg for sinus.
- investigate white noise, pink noise etc.
- mapping to voltage function.
- ESP32 version as separate task...
- check for synergy with https://github.com/RobTillaart/AD985X
- example with DAC. 8 12 16 bit.
- example with potentiometers for 4 parameters
- RC function.
- Trapezium wave.
- Sawtooth reverse?
- record
- play "recording"
- external clock to synchronize two or more function generators.
- 


