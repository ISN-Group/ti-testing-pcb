# Choosing Impedance Values for J10, J11, J12 and J13

An understanding of [Ohm's Law](https://en.wikipedia.org/wiki/Ohm%27s_law) and [Kirchoff's Laws](https://en.wikipedia.org/wiki/Kirchhoff%27s_circuit_laws) are recommended.

The purpose of placing impeding elements in connectors J10, J11, J12 and J13 is to provide voltage waveforms that can represent a superposition of the two stimulation waveforms, with amplitude scaled according to the impedance values. The exact parameters to be controlled are:

|Signal Name|Unit|PCB Label|
|-|-|-|
|TI1 Active-Return Potential Difference|Volts|VOUT_MON1|
|TI2 Active-Return Potential Difference|Volts|VOUT_MON2|
|EEG SIG_GND Potential Difference |Volts|EEG_SIG_GND|
|EEG REF_GND Potential Difference |Volts|EEG_REF_GND|

This document will explain some considerations in choosing impedance values for the board's various header connectors - for the purpose of emulating voltage parameters to match a given configuration of electrodes on the human scalp.

This document will make reference to the following variables:
|Label|Type|Unit|Detail|
|-|-|-|-|
|f|Frequency|Hz|The carrier frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|df|Frequency|Hz|The delta frequency for Temporal Interference to be defined by the tester for their respective experiment objectives|
|I1|Current|mA|The current amplitude for Temporal Interference channel 1, to be defined by the tester for their respective experiment objectives|
|I2|Current|mA|The current amplitude for Temporal Interference channel 2, to be defined by the tester for their respective experiment objectives|
|Rsum|Resistance|Ohms|The sum of the impedance magnitudes for all impedances in J10, J11, J12 and J13.|

## Determining Target Voltage Parameters

The process for determining which physical electrode locations should be used for stimulation/recording is outside the scope of this document. This document presumes the relevant locations have already been determined according to the objectives of the stimulation paradigm as designed by the respective lab.

Stimulation electrodes (1x active and 1x return per waveform, 4 electrodes total) should be placed on the scalp of a human participant. EEG electrodes should be placed on the scalp for EEG reference and ground. One more EEG electrode should be selected for probing. There should be seven total electrodes on the scalp at this point.

Temporal Interference stimulation should be powered on for both channels at the carrier frequencies as defined in a given lab's particular study objective(s). The current amplitude should be set to the target value to be used, again, as defined in a given lab's particular study objective(s). **It is critical that the set current in Amps and set frequency in Hertz are maintained when testing resulting impedance magnitudes on the PCB.**

While stimulation is active, using a DAQ or oscilloscope, the following parameters should be recorded:

* Maximum potential difference (V) between the active and return electrodes for TI waveform 1
* Maximum potential difference (V) between the active and return electrodes for TI waveform 2
* Maximum potential difference (V) between EEG Signal and EEG Ground
* Maximum potential difference (V) between EEG Reference and EEG Ground

These four parameters should be recorded across a variety of human participants (with the same electrode locations) to capture an understanding of variation. These parameters will determine the target voltage parameters when defining impedance values to use in the PCB.

## PCB: Choosing Impedance Values

The maximum potential difference in volts between the active and return electrodes, once inserted to the testing PCB, will depend on the sum of:

* 2kOhms (from both 1kOhm resistors)
* Any impedance in J1A+J1R or J2A+J2R for waveform 1 or 2 respectively
* Sum of impedances in J10+J11+J12+J13

The voltage waveform for a given active-return pair will also be modulated in amplitude by the opposite current source, via the common impedances. The formula for expected voltage between a given active-return pair is therefore:\
**VOUT_MON1 = I1\*(2k + J1A + J1R + J10 + J11 + J12 + J13) + I2\*(J10 + J11 + J12 + J13)**, and;\
**VOUT_MON2 = I2\*(2k + J2A + J2R + J10 + J11 + J12 + J13) + I1\*(J10 + J11 + J12 + J13)** \
Where 'I1' and 'I2' are current amplitudes for TI waveform 1 and 2 respectively. Connectors referred to by their PCB labels (e.g. 'J10') represent the impedance magnitude in Ohms of the component plugged into the connector.

The expected voltage for EEG signal referred to EEG ground (PCB) will simply be the sum of both stimulation currents (since the components are common to both channels) multiplied by the sum of impedances in J11 and J12. The expected voltage for EEG reference referred to EEG ground (PCB labelling) will simply be the sum of both stimulation currents (since the components are common to both channels) multiplied by the impedance in J12.

The tester should choose impedance values such that the PCB will recreate voltage values matching those observed when the electrodes are placed on human particpants for a given electrode configuration.

## Procedure

1. Connect the active and return of TI channel 1 to the corresponding electrode locations on the scalp (TI_1_ACTIVE and TI_1_RETURN).
2. Connect the active and return of TI channel 2 to the corresponding electrode locations on the scalp (TI_2_ACTIVE and TI_2_RETURN).
3. Program the stimulator channel 1 to output with frequency *f*, amplitude *I1*; and stimulator channel 2 to output with amplitude frequency *f+df*, amplitude *I2*.
4. Connect the inner conductor of a BNC cable to an EEG electrode placed at the chosen location for EEG reference and outer conductor of a BNC cable to the chosen location for EEG Ground. Connect this cable to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the maximum voltage amplitude while TI stimulation is active.
    * The maximum voltage amplitude observed in EEG_REF_GND should be divided by the sum of the stimulator current amplitudes (I1+I2). The resulting value is the impedance magnitude that should be placed in J12.
5. Connect the inner conductor of a BNC cable to an EEG electrode placed at the chosen location for an EEG signal and outer conductor of a BNC cable to the chosen location for EEG Ground. Connect this cable to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the maximum voltage amplitude while TI stimulation is active.
    * The maximum voltage amplitude observed in EEG_SIG_GND should be divided by the sum of the stimulator current amplitudes (I1+I2). Then, the impedance magnitude to be placed in J12 (previous step's result) should be subtracted. The resulting value is the impedance magnitude that should be placed in J11.
6. Connect the inner conductor of a BNC cable to the stimulator active output (for channel 1) and outer conductor of a BNC cable to the stimulator return output (for channel 1). Connect this cable to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the maximum voltage amplitude while TI stimulation is active.
    * Subtract I1\*2000 from the maximum voltage amplitude observed in VOUT_MON1. Call the result **V_INNER_A**.
7. Connect the inner conductor of a BNC cable to the stimulator active output (for channel 2) and outer conductor of a BNC cable to the stimulator return output (for channel 2). Connect this cable to an oscilloscope or DAQ (note - make sure the DAQ or scope is taking a differential voltage reading and not referring the BNC inner conductor potential to the instrument's ground potential). Record the maximum voltage amplitude while TI stimulation is active.
    * Subtract I2\*2000 from the maximum voltage amplitude observed in VOUT_MON2. Call the result **V_INNER_B**.
8. Whichever is larger, V_INNER_A or V_INNER_B, divide by **I1+I2**. Call the result '*Rsum*'. This will be the sum of impedance magnitudes in J10, J11, J12 and J13. Since J11 and J12 are determined from steps 5 and 4 respectively; Subtract **J11+J12** from *Rsum*. Divide the result by two, leaving the impedance magnitude value that should be placed in both J10 and J13.
    * The full equation: **J10=J13=(Rsum-J11-J12)/2**

*Note: As step 6, 7 and 8 details, the TI channel with the larger potential difference across active and return (with respect to programmed current) will determine the impedance values to be used in J10 and J13. This means voltage mismatch on the human scalp for each TI channel might not be perfectly reproducable on the PCB, though it is assumed the larger VOUT parameter will dominate hardware testing findings. The tester can experiment with impedances in J1A, J1R, J2A and J2R for finer control over stimulator output voltage parameters.*

### Example

The tester places the relevant electrodes to the relevant locations on the scalp.

* The tester sets both TI waveform channels to a current amplitude of 0.002A each.
* The tester observes a maximum potential difference of 12.2V between active and return electrodes, for both TI channels.
* The tester observes a maximum potential difference of 0.2V between EEG signal and EEG ground.
* The tester observes a maximum potential difference of 0.02V between EEG reference and EEG ground.

For simplicity, J1A, J1R, J2A and J2R are short-circuited.

The impedance that should be placed in J12 (sets EEG reference voltage referred to EEG ground) is simply 0.02/(0.004), i.e. 5 Ohms. \
The impedance that should be placed in J11 (sets EEG signal voltage referred to EEG ground) is 0.2/(0.004) - 5. The impedance in J12 (5) is subtracted since EEG signal voltage is the sum of J11 and J12. The resulting impedance magnitude is 45 Ohms.

The sum of J10, J11, J12 and J13 is (12.2 - 0.002*2000)/0.004; in this case, 2050 Ohms. This is the target voltage between active and return, minus the voltage drop from the 2x 1kOhm resistors that are hardwired, divided by the sum of the two superimposed current amplitudes. Since the tester has already determined J11 and J12 as 45 and 5 Ohms respectively, 1kOhm is selected for J10 and J13 each.

A calculator for determining impedance magnitude values (for when the stimulator currents are symmetrical, and target stimulator output voltages are symmetrical) is included on the sheet named '*Impedance Calculator*', in the spreadsheet '*ImpedanceCalculator.xlsx*'.
