# Test 1: Stimulator Current Output

This document will guide the reader how to use the Hardware-Testing PCB to test the actual current amplitude that is being output from the stimulator. An understanding of [Ohm's Law](https://en.wikipedia.org/wiki/Ohm%27s_law) is recommended.

The tester should test each TI channel independently for this test, i.e. if channel 1 is outputting current, then channel 2 should be switched off - and vice versa.

J1A, J1R, J2A and J2R should be short-circuited. J10, J11, J12 and J13 should contain impedance magnitudes relevant to the tester's use case - further details in [the choosing impedance document](./choosingimpedance.md).

This document will make reference to the following variables:
|Label|Type|Unit|Detail|
|-|-|-|-|
|f|Frequency|Hz|The carrier frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|I1|Current|mA|The current amplitude for Temporal Interference channel 1, to be defined by the tester for their respective experiment objectives|
|I2|Current|mA|The current amplitude for Temporal Interference channel 2, to be defined by the tester for their respective experiment objectives|

## Procedure - Comparing Actual Current Amplitude to Programmed Current Amplitude

This test procedure covers to how to verify the actual current amplitude matches the programmed current amplitude of the stimulator.

1. Connect the active and return of TI channel 1 to the left side of the PCB (TI_1_ACTIVE and TI_1_RETURN).
2. Connect the active and return of TI channel 2 to the right side of the PCB (TI_2_ACTIVE and TI_2_RETURN).
3. Program the stimulator channel 1 to output with frequency *f*, amplitude *I1*; and stimulator channel 2 to output with amplitude 0mA.
4. Connect one end of a BNC cable to the AMP_MON1 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
5. Record the amplitude (0 to peak) voltage value, and divide this by 1000 (because the voltage is across a 1kOhm resistor). Compare the resulting value to the programmed current amplitude.
6. Program the stimulator channel 1 to output with amplitude 0mA; and stimulator channel 2 to output with frequency *f* amplitude *I2*.
7. Connect one end of a BNC cable to the AMP_MON2 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
8. Record the amplitude (0 to peak) voltage value, and divide this by 1000 (because the voltage is across a 1kOhm resistor). Compare the resulting value to the programmed current amplitude.

Analysing the output of steps 5 and 8, ensure that the programmed current amplitudes is acceptably close to the actual current amplitudes. A non-negligble mismatch indicates a hardware fault, or connection issue.

## Procedure - Assessing Frequency Response of Stimulator Output

This test procudure covers how to verify the stimulation frequency is within the capability of the stimulator used. Example starting frequency of 1kHz is used here and the range of testing frequencies is not defined - it is up to the tester to determine which frequencies are of interest to their use cases.

1. Connect the active and return of TI channel 1 to the left side of the PCB (TI_1_ACTIVE and TI_1_RETURN).
2. Connect the active and return of TI channel 2 to the right side of the PCB (TI_2_ACTIVE and TI_2_RETURN).
3. Program the stimulator to output at amplitude *I1* on channel 1 and 0mA on channel 2. Program the frequency of the waveform on channel 1 to 1kHz (just an example starting point).
4. Connect one end of a BNC cable to the AMP_MON1 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
5. Record the amplitude (0 to peak) voltage value, and divide this by 1000 (Ohm's Law; potential difference across a 1kOhm resistor).
6. Repeat steps 3 to 5, varying the frequency of channel oscillation. Record the output of step 5 for each frequency value, i.e. the voltage amplitude at AMP_MON1 divided by 1000 each time.
7. For each recording, do a Decibel transformation *10\*log10(V)*. The decibel transformation will allow better visualisation of the frequency response, where 0dB means there is no loss in amplitude for the corresponding frequency.
8. Program the stimulator to output 0mA on channel 1 and *I2* on channel 2. Program the frequency of the waveform on channel 2 to 1kHz.
9. Connect one end of a BNC cable to the AMP_MON2 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
10. Record the amplitude (0 to peak) voltage value, and divide this by 1000 (Ohm's Law; potential difference across a 1kOhm resistor).
11. Repeat steps 3 to 5, varying the frequency of channel oscillation. Record the output of step 5 for each frequency value, i.e. the voltage amplitude at AMP_MON2 divided by 1000 each time.
12. For each recording, do a Decibel transformation *10\*log10(V)*. The decibel transformation will allow better visualisation of the frequency response, where 0dB means there is no loss in amplitude for the corresponding frequency.

Analysing the output of steps 7 and 12, ensure that the attenuation (if any) at the Temporal Interference carrier frequencies to be used in human-experiments is acceptable. If the attenuation is non-negligble, consult the datasheet of the stimulator in use to ensure the frequency is within the advertised output bandwidth. If it is within the advertised bandwidth, non-neglible attenuation indicates a hardware-fault or connection issue.
