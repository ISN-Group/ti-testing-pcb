# Test 2: Stimulator Channel Isolation

This document will guide the reader how to use the Hardware-Testing PCB to test the channel isolation of the TI stimulation source. An understanding of [Ohm's Law](https://en.wikipedia.org/wiki/Ohm%27s_law) and [Amplitude Modulation](https://en.wikipedia.org/wiki/Amplitude_modulation) are recommended.

The tester should perform this test with both TI channels turned on and oscillating at different frequencies.

J1A, J1R, J2A and J2R should be short-circuited. J10, J11, J12 and J13 should contain impedance magnitudes relevant to the tester's use case - further details in [the choosing impedance document](./choosingimpedance.md).

This document will make reference to the following variables:
|Label|Type|Unit|Detail|
|-|-|-|-|
|f|Frequency|Hz|The carrier frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|df|Frequency|Hz|The delta frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|I1|Current|mA|The current amplitude for Temporal Interference channel 1, to be defined by the tester for their respective experiment objectives|
|I2|Current|mA|The current amplitude for Temporal Interference channel 2, to be defined by the tester for their respective experiment objectives|
|Rsum|Resistance|Ohms|The sum of the impedance magnitudes for all impedances in J10, J11, J12 and J13.|

## Procedure - Assessing Isolation and Modulation of Waveforms

This test procedure covers how to verify the current waveforms being delivered by each channel of the stimulator are isolated from eachother at the source.

1. Connect the active and return of TI channel 1 to the left side of the PCB (TI_1_ACTIVE and TI_1_RETURN).
2. Connect the active and return of TI channel 2 to the right side of the PCB (TI_2_ACTIVE and TI_2_RETURN).
3. Program the stimulator channel 1 to output with frequency *f*, amplitude *I1*; and stimulator channel 2 to output with amplitude frequency *f+df*, amplitude *I2*.
4. Connect one end of a BNC cable to the VOUT_MON1 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). **The tester should observe a waveform with amplitude modulation index: I1\*(Rsum + 2000)/(I2\*Rsum)**.
5. Connect one end of a BNC cable to the VOUT_MON2 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential - nor short-circuiting the outer conductor of the BNC to the outer conductor of any other connected BNC). **The tester should observe a waveform with amplitude modulation index: I2\*(Rsum + 2000)/(I1\*Rsum)**.
6. Connect one end of a BNC cable to the AMP_MON1 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential - nor short-circuiting the outer conductor of the BNC to the outer conductor of any other connected BNC). **The tester should observe a waveform with 0% amplitude modulation**.
7. Connect one end of a BNC cable to the AMP_MON2 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential - nor short-circuiting the outer conductor of the BNC to the outer conductor of any other connected BNC). **The tester should observe a waveform with 0% amplitude modulation**.
8. Connect one end of a BNC cable to the EEG_REF_GND socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential - nor short-circuiting the outer conductor of the BNC to the outer conductor of any other connected BNC). **The tester should observe a waveform with 100% amplitude modulation, with an envelope oscillating at df Hz**.

**Note** that if the output of either step 6 or step 7 **was not** a waveform with 0% amplitude modulation, then the current sources are not adequately isolated, indicating a hardware fault or connection error. The results of steps 4,5 and 8 will indicate which current source channel is problematic.
