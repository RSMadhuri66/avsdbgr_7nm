# SCMB bandgap reference circuit for ASAP 7nm PDK - avsdbgr_7nm 

## Bandgap Reference Circuit characteristics table 

This document summarizes the key parameters of the bandgap reference circuit used for temperature compensation and stable output voltage generation.

## Parameters

| Parameter         | Description                              | Min   | Type       | Max   | Unit | Condition                         |
|-------------------|------------------------------------------|-------|-----------|-------|------|-----------------------------------|
| **Vref**          | Output reference voltage                 | 1.12  | 1.19756   | 1.26  | V    | T = -45°C to 150°C, VDD = 1.4V   |
| **Vctat**         | Complementary to absolute temperature     | 140   | 190       | 210   | mV   | T = -45°C to 150°C, VDD = 1.4V   |
| **Vptat**         | Proportional to absolute temperature      | 0.93  | -         | 1.11  | V    | T = -45°C to 150°C, VDD = 1.4V   |
| **VDD**           | Supply Voltage                            | -     | 1.4       | -     | V    | T = -45°C to 150°C               |
| **IDD (EN=1)**    | Supply Current                           | -     | 35.20     | -     | µA   | VDD = 1.4V, T = 27°C             |
| **RL**            | Load Resistance                          | -     | 33.33     | -     | kΩ   | VDD = 1.4V, T = 27°C             |
| **R1**            | Resistance 1                             | -     | 33        | -     | kΩ   | VDD = 1.4V, T = 27°C             |
| **temp_coeff**    | Temperature Coefficient                  | 360  |    -       | 432   | ppm/°C| T = -45°C to 150°C, VDD = 1.4V | 

## Summary of Key Parameters

- **Output Reference Voltage (\(V_{\text{ref}}\))**: Ranges from 1.12 V to 1.26 V across a wide temperature range.
- **Complementary to Absolute Temperature (\(V_{ctat}\))**: Essential for temperature compensation.
- **Proportional to Absolute Temperature (\(V_{ptat}\))**: Ranges from 0.93 V to 1.11 V.
- **Supply Voltage (\(V_{DD}\))**: Fixed at 1.4 V.
- **Supply Current (\(IDD\))**: The current draw when enabled is **35.20 µA** at 27°C.
- **Load Resistance (\(R_L\))**: 33.33 kΩ, affecting output voltage under load.
- **Resistance \(R1\)**: Set at 33 kΩ which is the resistance near PTAT circuit.
- **Temperature Coefficient** : To find out voltage stability.

## Explanation 

This project demonstrates the design and operation of a Bandgap Reference (BGR) circuit using 7nm ASAP technology. The goal of the circuit is to generate a temperature-independent reference voltage by combining the properties of Proportional to Absolute Temperature (PTAT) and Complementary to Absolute Temperature (CTAT) voltages. Such circuits are crucial in integrated circuit design for creating a stable voltage reference, regardless of temperature, process variations, or supply fluctuations.

```
Key Components:
MOSFETs (pMOS and nMOS transistors)
Resistors
Self-Biasing Current Mirrors
Startup Circuit
Main Output:
Vref: The desired temperature-independent reference voltage.
Vctat: A voltage that is complementary to the absolute temperature, used in temperature compensation.
```

### Circuit Functionality 

#### SCMB - Self Current Mirror Based Confiuration 

The circuit employs self-biasing current mirrors using matched pMOS and nMOS transistors to maintain stable currents in different branches of the circuit. Current mirrors play a vital role in balancing the PTAT and CTAT components that determine the reference voltage.

Key Current Mirror Transistors:
- pfet1, pfet2, pfet3: These pMOS transistors form a current mirror network to maintain stable biasing throughout the circuit.
- nfet1, nfet2: These nMOS transistors mirror the current in the lower portion of the circuit, which contributes to generating the CTAT voltage.

#### Startup Circuit
A startup circuit is required to ensure that the bandgap reference starts up correctly and avoids falling into a non-operational state (zero-current state). Without this circuit, the bandgap could remain in an undesired state where no current flows, rendering the reference non-functional.

Startup Transistors: The bottom part of the circuit includes additional transistors, like nfet8 and pfet6, that help kick-start the current when power is first applied, ensuring the circuit reaches its stable operating point.

#### The Schematic Diagram 

<picture>
<img alt = "Schematic" src="Xschem_bgr.png">
</picture>


#### PTAT Voltage Generation - Proportional to Absolute Temperature 

PTAT voltage is a crucial part of the temperature compensation mechanism in bandgap reference circuits. It is generated by creating a current that increases proportionally with temperature.

Resistor Network: The resistors R1 and R2 in the circuit, along with the associated transistors, are used to generate the PTAT current.
nMOS Transistors: The nfet1, nfet2 transistors create a current that depends on the temperature-dependent voltage drop across the resistors. This forms the PTAT component.

The PTAT current increases as temperature increases, helping to counteract the CTAT component and maintain a stable reference voltage.

#### The PTAT voltage Graph 

<picture>
<img alt = "PTAT Voltage" src="Vptat_bgr.png">
</picture>


#### CTAT Voltage generation - Complementary to Absolute Temperature 

This voltage decreases with an increase in temperature, and it is crucial for creating the temperature compensation effect.

Vctat Node: The voltage at the Vctat node decreases with temperature, exhibiting CTAT behavior. This voltage is generated by the lower half of the circuit, which includes nMOS transistors and their associated current mirrors.

#### The CTAT Voltage Graph 

<picture>
<img alt = "CTAT Voltage" src="Vctat_bgr .png">
</picture>


#### Temperature Compensation and Voltage Reference (Vref)

The core functionality of the bandgap reference lies in summing the PTAT and CTAT voltages in a specific ratio that cancels out their temperature dependencies. This produces a stable reference voltage (Vref) at the desired node. The circuit is designed to maintain a constant output regardless of temperature variations.

Vref Node: The Vref node in the circuit provides the temperature-stable reference voltage, a sum of the PTAT and CTAT voltages. This reference voltage can be used in various applications, such as ADCs, voltage regulators, and other analog circuits.

#### The Reference Voltage 

<picture>
<img alt = "BGR voltage" src="Vref_bgr.png">
</picture>





### Temperature Coefficient

The temperature coefficient (TC) of a reference voltage is a measure of how much the output voltage changes with temperature. In the context of bandgap reference (BGR) circuits, the goal is to create a voltage that remains as stable as possible across a wide temperature range. The TC tells us how sensitive the reference voltage is to temperature variations.

The temperature coefficient is expressed as the change in the reference voltage per degree Celsius (or Kelvin) relative to the nominal voltage at a specific temperature (typically room temperature).

It is usually expressed in ppm/°C (parts per million per degree Celsius) or mV/°C.

Formula 

```
\[
TC = \frac{V_{ref}(T_2) - V_{ref}(T_1)}{(T_2 - T_1) \cdot V_{ref}(T_1)} \cdot 10^6 \, \text{ppm/°C}
\]

Where:
- \( V_{ref}(T_1) \) is the reference voltage at temperature \( T_1 \) (usually at room temperature, e.g., 25°C).
- \( V_{ref}(T_2) \) is the reference voltage at temperature \( T_2 \) (another temperature, e.g., 85°C).
- \( T_2 - T_1 \) is the temperature difference in °C.
- \( 10^6 \) converts the result to parts per million (ppm).

```


<picture>
<img alt = "Temp_coeff" src="temp_coeff.png">
</picture>







