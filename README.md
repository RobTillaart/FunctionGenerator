
[![Arduino CI](https://github.com/RobTillaart/FunctionGenerator/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/FunctionGenerator/actions/workflows/jsoncheck.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/FunctionGenerator/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/FunctionGenerator.svg?maxAge=3600)](https://github.com/RobTillaart/FunctionGenerator/releases)


# FunctionGenerator

Arduino library to generate (numeric) wave forms for a DAC.


## Description

This library presents a class for a function generator in **software**. 
It is typical used to control one or more DAC's.
To maximize signal quality one has to apply all (or most) processor power 
to calculate new values over and over again to get enough resolution. 
In practice the generator is useful for low frequencies, 
0.01 - 25 Hz, depending on waveform and processor and number of DAC's.
(see indication below).

Note: this class generates float values, performance wise this can be optimized,
to achieve higher speeds at cost of accuracy / precision.


## Performance

Indication of what performance can be expected (based upon 0.2.1 version).  
Note that the values need to be transported to a DAC or serial port too.  
Numbers based on performance example, for one single signal.


|  Processor    |  Clock    |  Waveform  |  usec/call  |  max freq  |
|:--------------|----------:|:-----------|------------:|-----------:|
|  Arduino UNO  |  16 MHz   |  sawtooth  |   62        |    60 Hz   |
|  Arduino UNO  |  16 MHz   |  triangle  |   74        |    50 Hz   |
|  Arduino UNO  |  16 MHz   |  square    |   53        |  1000 Hz   |
|  Arduino UNO  |  16 MHz   |  sinus     |   164       |    25 Hz   |
|  Arduino UNO  |  16 MHz   |  stair     |   81        |    50 Hz   |
|  Arduino UNO  |  16 MHz   |  random    |   37        |  1000 Hz   |
|               |           |            |             |            |
|  ESP32        |  240 MHz  |  sawtooth  |   3.8       |  1000 Hz   |
|  ESP32        |  240 MHz  |  triangle  |   3.9       |  1000 Hz   |
|  ESP32        |  240 MHz  |  square    |   2.8       |  1000 Hz   |
|  ESP32        |  240 MHz  |  sinus     |   13.6      |   250 Hz   |
|  ESP32        |  240 MHz  |  stair     |   4.8       |   800 Hz   |
|  ESP32        |  240 MHz  |  random    |   1.3       |  1000 Hz   |


- assumption minimal 250 samples per period to get a smooth signal.
  if a rougher signal is OK, higher frequencies are possible.
- ESP32 can do more calculations however 1000 Hz seems to be a nice 
  upper limit for a software based function generator.
- if one wants to control multiple DAC's one need to divide the numbers
  and round down.


Note: hardware function generator https://github.com/RobTillaart/AD985X


## Interface

```cpp
#include "functionGenerator.h"
```

#### Constructor

- **funcgen(float period = 1.0, float amplitude = 1.0, float phase = 0.0, float yShift = 0.0)**
All parameters can be set in the constructor but also later in configuration.


#### Configuration

- **void  setPeriod(float period = 1.0)** set the period of the wave in seconds. 
- **float getPeriod()** returns the set period.
- **void  setFrequency(float frequency = 1.0)** set the frequency of the wave in Hertz (1/s).
- **float getFrequency()** returns the set frequency in Hertz.
- **void  setAmplitude(float amplitude = 1.0)** sets the amplitude of the wave. TODO point to point?
Setting the amplitude to 0 gives ?what? 
- **float getAmplitude()** returns the set amplitude.
- **void  setPhase(float phase = 0.0)** shifts the phase of the wave. Will only be noticeable when 
compared with other waves.
- **float getPhase()** returns the set phase.
- **void  setYShift(float yShift = 0.0)** sets an Y-shift in amplitude, allows to set some zero point.
- **float getYShift()** returns the set Y-shift.
- **void  setDutyCycle(float percentage = 100)** sets the duty cycle of the signal.
Experimental, not all waveforms have a duty cycle.
Duty cycle must be between 0 and 100%.
- **float getDutyCycle()** returns the set duty cycle.
- **void  setRandomSeed(uint32_t a, uint32_t b = 314159265)** initial seed for the
(Marsaglia) random number generator. 


#### Wave forms

The variable t == time in seconds.

- **float sawtooth(float t, uint8_t mode = 0)** mode == 0 (default) ==>  sawtooth /|. mode == 1 ==> sawtooth |\\.
- **float triangle(float t)** triangle form, duty cycle 50%.
- **float square(float t)** square wave with duty cycle 50%.
- **float sinus(float t)** sinus wave, has no duty cycle. 
- **float stair(float t, uint16_t steps = 8, uint8_t mode = 0)** mode = 0 ==> steps up, mode = 1 steps down.
- **float random()** random noise generation.
- **float line()** constant voltage line. Depends on the set YShift and amplitude.
- **float zero()** constant zero.

Line() and zero() are functions that can be used to drive a constant voltage from a DAC 
and can be used to calibrate the generator / DAC combination.


#### Experimental

Since 0.2.5 experimental support for duty cycle has been added.

In first iteration only **square** and **triangle** support duty cycle.
The other functions need to be investigated what duty cycle means, and
if it is (easy) implementable.

Feedback is welcome.


## Future


#### Must

- documentation
  - quality of signals, max freq etc


#### Should

- smart reseed needed for random()
- initialize random generator with compile time


#### Could

- trapezium wave (would merge square and triangle and sawtooth)
- white noise, pink noise etc.
- investigate algorithms for performance gains (DAC specific values 10-12-16 bit)
- external clock to synchronize two or more software function generators.
- RC function curve.
- stand-alone functions in separate .h
- check for synergy with https://github.com/RobTillaart/AD985X


#### Examples

  - Amplitude modulation ?
  - heartbeat curve?
  - example ESP32 version as separate task.
  - example with DAC. 8 12 16 bit.
  - example with potentiometers for 4 parameters

#### Wont

- investigate duty cycle for waveforms
  - Derived class for the duty cycle variants? or functions!
  - **float squareDC()** performance (loss)
  - **float triangleDC()**
  - **float sawtoothDC()**
  - **float sinusDC()** duty-cycle for sinus what does it mean. 
    - ==> move peaks, two half sinus with diff frequency
  - **float stairDC()**
- Bezier curve? (too complex)
- record a signal and play back  ==> separate class
