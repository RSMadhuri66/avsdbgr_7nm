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


## Graphs

### The Schematic Diagram 

<picture>
<img alt = "Schematic" src="Xschem_bgr.png">
</picture>

### The Reference Voltage 

<picture>
<img alt = "BGR voltage" src="Vref_bgr.png">
</picture>

### The PTAT voltage 

<picture>
<img alt = "PTAT Voltage" src="Vptat_bgr.png">
</picture>

### The CTAT Voltage 

<picture>
<img alt = "CTAT Voltage" src="Vctat_bgr .png">
</picture>

### Temperature Coefficient

<picture>
<img alt = "Temp_coeff" src="temp_coeff.png">
</picture>







