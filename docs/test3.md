# Test 3: Intermodulation Distortion (Recording during Stimulation)

Recording instrumentation (EEG or DAQ; with or without filters) is required for performing this test; and any dedicated filters that are to be used at the interface to the instrumentation.

This document will guide the reader how to use the Hardware-Testing PCB to test their TI stimulator and recording instrumentation for intermodulation-distortion (IMD) contributions. An understanding of Non-Linearity and [Intermodulation Distortion](https://www.everythingrf.com/community/what-is-intermodulation-distortion) are recommended.

The tester should perform this test with both TI channels turned on and oscillating at different frequencies.
J1A, J1R, J2A and J2R should be short-circuited. J10, J11, J12 and J13 should contain impedance magnitudes relevant to the tester's use case - further details in [the choosing impedance document](./choosingimpedance.md) - using appropriate resistance values for their study's electrode configuration on the scalp.

This document will make reference to the following variables:
|Label|Type|Unit|Detail|
|-|-|-|-|
|f|Frequency|Hz|The carrier frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|df|Frequency|Hz|The delta frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|I1|Current|mA|The current amplitude for Temporal Interference channel 1, to be defined by the tester for their respective experiment objectives|
|I2|Current|mA|The current amplitude for Temporal Interference channel 2, to be defined by the tester for their respective experiment objectives|

## Procedure - Assessing Intermodulation Distortion of Stimulator and Recording Instrumentation

This test procedure covers how to test for presence of intermodulation distortion in the stimulator or the recording filter/amplifier.

1. Connect the active and return of TI channel 1 to the left side of the PCB (TI_1_ACTIVE and TI_1_RETURN).
2. Connect the active and return of TI channel 2 to the right side of the PCB (TI_2_ACTIVE and TI_2_RETURN).
3. Program the stimulator channel 1 to output with frequency *f*, amplitude *I1*; and stimulator channel 2 to output with amplitude frequency *f+df*, amplitude *I2*.
4. Connect one end of a BNC cable to the EEG_SIG_GND socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
5. Connect one end of a BNC cable to the EEG_REF_GND socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential).
6. Manually ensure that the maximum voltage amplitude observed on either BNC probe is within the safe input range for the recording instrumentation. Record maximum voltage amplitudes for EEG_REF_GND and EEG_SIG_GND.
7. If the previous step is satisfied (and if using an EEG amplifier for recording instrumentation), connect the 1.5mm Touchproof EEG connectors on the PCB (EEG_SIG, EEG_REF and EEG_GND) to the corresponding inputs of the EEG recording instrumentation: 1x Signal, 1x Reference and 1x GND. Disconnect the BNC cables from EEG_SIG_GND and EEG_REF_GND. If not using an EEG amplifier, then use the BNC EEG_REF_GND and EEG_SIG_GND sockets and ensure no touchproof cables are in the 1.5mm EEG sockets.
8. Connect one end of a BNC cable to the VOUT_MON1 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the observed maximum voltage amplitude.
9. Connect one end of a BNC cable to the VOUT_MON2 socket and the other end to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the observed maximum voltage amplitude. Disconnect all BNC cables from the PCB, leaving only the 1.5mm Touchproof EEG connectors connecting the PCB to the EEG recording instrumentation (if using EEG amplifier as recording instrumentation; otherwise leave BNCs for EEG_REF_GND and EEG_SIG_GND connected).
10. While the stimulation is still active, record a timeseries for ~4 minutes. Analyse the recorded data in the frequency domain, using the Fourier Transform. [MATLAB function documentation here](https://uk.mathworks.com/help/matlab/ref/fft.html).
11. Analyse the frequency spectral content at *df* Hz (which is the difference in Hz between each carriers; *f* Hz and *f+df* Hz).

If there is no spectral content at *df* Hz (at least no peak above the noise floor), then all hardware (stimulator, filters, amplifiers) are in linear regions of operation and there is no intermodulation distortion **for these voltage parameters**. It is critical to remember that different voltage parameters (as recorded in steps 6, 8 and 9) will move hardware to different regions of linearity and intermodulation distortion contribution will vary. It is therefore important to choose impedance magnitudes on the PCB that correspond to voltage parameters that are observed on the scalp for a given electrode configuration.

If there is spectral content at *df* Hz (a peak above the noise floor), then at least one piece of hardware is a non-linear region of operation and is contaminating the recording with intermodulation distortion. The tester must ensure that the magnitude of the distortion is below relevant noise floors, i.e. of the recording instrumentation or biological. Alternatively, ensure that the contaminated frequency bins are of no use in the analysis of the recorded signals.

Determining which hardware component is contributing the intermodulation distortion can be done by repeating the procedure with varying impedance values. J10, J11, J12 and J13 can all be varied such that the voltage amplitudes at stimulator output (VOUT_MON1 and VOUT_MON2) can be varied while keeping the same voltage input to recording instrumentation (EEG_SIG, EEG_REF and EEG_GND). The opposite is also true - voltage amplitudes at stimulator output can be kept stationary while varying voltage input to recording instrumentation. For example, increasing the impedance in J10 and J13 will increase the stimulator output voltage, without changing the voltage input to the EEG recording instrumentation. In contrast, increasing the impedance in either J11 or J12 will vary the voltage input to the EEG recording instrumentation, and stimulator output voltage can be kept the same by appropriately attenuating the impedances in J10 and J13 such that the sum of impedances is overall the same.
