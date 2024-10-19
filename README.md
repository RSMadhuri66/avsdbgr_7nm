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

Ngpsice command 

```
plot v(vref) -v(Vctat)

```

#### The PTAT voltage Graph 

<picture>
<img alt = "PTAT Voltage" src="Vptat_bgr.png">
</picture>


#### CTAT Voltage generation - Complementary to Absolute Temperature 

This voltage decreases with an increase in temperature, and it is crucial for creating the temperature compensation effect.

Vctat Node: The voltage at the Vctat node decreases with temperature, exhibiting CTAT behavior. This voltage is generated by the lower half of the circuit, which includes nMOS transistors and their associated current mirrors.

Ngspice command 

```
plot v(Vctat)

```

#### The CTAT Voltage Graph 

<picture>
<img alt = "CTAT Voltage" src="Vctat_bgr .png">
</picture>


#### Temperature Compensation and Voltage Reference (Vref)

The core functionality of the bandgap reference lies in summing the PTAT and CTAT voltages in a specific ratio that cancels out their temperature dependencies. This produces a stable reference voltage (Vref) at the desired node. The circuit is designed to maintain a constant output regardless of temperature variations.

Vref Node: The Vref node in the circuit provides the temperature-stable reference voltage, a sum of the PTAT and CTAT voltages. This reference voltage can be used in various applications, such as ADCs, voltage regulators, and other analog circuits.

Ngspice command 

```
plot v(Vref)

``` 

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
TC = [(Vref(T2) - Vref(T1)) / ((T2 - T1) * Vref(T1))] * 10^6 ppm/°C
```
ngspice command to get temperature coefficient 

```
plot deriv(v(Vref))/1.20 - I have just taken the nominal voltage and plot it across the temp sweep. For accurate value, I have done manual claculation.

```

Where:
- V_ref(T1) is the reference voltage at temperature T1 (usually at room temperature at -45°C).
- V_ref(T2) is the reference voltage at temperature T2 (temperature at 150°C).
- (T2 - T1) is the temperature difference in °C.
- 10^6 converts the result to parts per million (ppm).


<picture>
<img alt = "Temp_coeff" src="temp_coeff.png">
</picture>

### Formulas to calculate the parameters 


I have calculated the parameters from the graphs that were plotted, but to get the parameters we can always refer to the formulas. 

Equations for Bandgap Reference Circuit
1. PTAT Current Calculation
The Proportional to Absolute Temperature (PTAT) current is generated by the voltage difference across two resistors 
𝑅
1
R 
1
​
  and 
𝑅
2
R 
2
​
 . The voltage difference is proportional to the thermal voltage 
𝑉
𝑇
V 
T
​
 .

The PTAT current 
𝐼
𝑃
𝑇
𝐴
𝑇
I 
PTAT
​
  is given by:

𝐼
𝑃
𝑇
𝐴
𝑇
=
Δ
𝑉
𝑇
𝑅
1
=
𝑉
𝑇
⋅
ln
⁡
(
𝑁
)
𝑅
1
I 
PTAT
​
 = 
R 
1
​
 
ΔV 
T
​
 
​
 = 
R 
1
​
 
V 
T
​
 ⋅ln(N)
​
 
Where:

𝑉
𝑇
V 
T
​
  is the thermal voltage 
𝑉
𝑇
=
𝑘
𝑇
𝑞
V 
T
​
 = 
q
kT
​
 , where 
𝑘
k is Boltzmann's constant, 
𝑇
T is the absolute temperature in Kelvin, and 
𝑞
q is the electron charge.
𝑁
N is the emitter area ratio of the matched transistors generating the PTAT voltage.
𝑅
1
R 
1
​
  is the resistor value used in the PTAT generation branch.
2. CTAT Voltage Calculation
The Complementary to Absolute Temperature (CTAT) voltage is generated using the base-emitter voltage 
𝑉
𝐵
𝐸
V 
BE
​
  (or its MOSFET equivalent). The CTAT voltage 
𝑉
𝐶
𝑇
𝐴
𝑇
V 
CTAT
​
  decreases with temperature:

𝑉
𝐶
𝑇
𝐴
𝑇
=
𝑉
𝐵
𝐸
=
𝑉
𝐵
𝐸
0
−
𝛽
𝑇
V 
CTAT
​
 =V 
BE
​
 =V 
BE 
0
​
 
​
 −βT
Where:

𝑉
𝐵
𝐸
0
V 
BE 
0
​
 
​
  is the base-emitter voltage at absolute zero temperature.
𝛽
β is the temperature coefficient of 
𝑉
𝐵
𝐸
V 
BE
​
 , typically around 
−
2
 
mV/K
−2mV/K.
𝑇
T is the temperature in Kelvin.
3. Reference Voltage Calculation (Vref)
The reference voltage 
𝑉
𝑟
𝑒
𝑓
V 
ref
​
  is obtained by combining the PTAT and CTAT voltages:

𝑉
𝑟
𝑒
𝑓
=
𝑉
𝐶
𝑇
𝐴
𝑇
+
𝑘
⋅
𝑉
𝑃
𝑇
𝐴
𝑇
V 
ref
​
 =V 
CTAT
​
 +k⋅V 
PTAT
​
 
Where 
𝑘
k is a scaling factor determined by the ratio of the resistors in the circuit (typically 
𝑅
2
/
𝑅
1
R 
2
​
 /R 
1
​
 ):

𝑘
=
𝑅
2
𝑅
1
k= 
R 
1
​
 
R 
2
​
 
​
 
Thus, the reference voltage is a weighted sum of the CTAT and PTAT voltages, designed to be temperature independent.

4. Resistor Values
The resistor values 
𝑅
1
R 
1
​
  and 
𝑅
2
R 
2
​
  are critical in determining the ratio of PTAT to CTAT components:

𝑅
2
=
𝑘
⋅
𝑅
1
R 
2
​
 =k⋅R 
1
​
 
Where 
𝑘
k is the desired ratio to balance the temperature coefficient of the output reference voltage.

5. Startup Circuit Currents
The startup circuit is responsible for ensuring that the bandgap reference initializes correctly. The current through the startup transistors is small and ensures the main current branches are activated.

If the startup circuit includes a small nMOS transistor, the current 
𝐼
𝑠
𝑡
𝑎
𝑟
𝑡
𝑢
𝑝
I 
startup
​
  can be calculated by:

𝐼
𝑠
𝑡
𝑎
𝑟
𝑡
𝑢
𝑝
≈
𝑉
𝑔
𝑠
−
𝑉
𝑡
ℎ
𝑅
𝑠
𝑡
𝑎
𝑟
𝑡
𝑢
𝑝
I 
startup
​
 ≈ 
R 
startup
​
 
V 
gs
​
 −V 
th
​
 
​
 
Where:

𝑉
𝑔
𝑠
V 
gs
​
  is the gate-source voltage of the startup transistor.
𝑉
𝑡
ℎ
V 
th
​
  is the threshold voltage of the nMOS transistor.
𝑅
𝑠
𝑡
𝑎
𝑟
𝑡
𝑢
𝑝
R 
startup
​
  is the resistance in the startup circuit path.
6. Current Mirror Relationships
In current mirrors, the currents in the branches are proportional to the transistor sizing (i.e., the width-to-length ratio 
𝑊
/
𝐿
W/L):

𝐼
𝑜
𝑢
𝑡
=
(
𝑊
𝐿
)
𝑜
𝑢
𝑡
⋅
(
𝐼
𝑟
𝑒
𝑓
(
𝑊
𝐿
)
𝑟
𝑒
𝑓
)
I 
out
​
 =( 
L
W
​
 ) 
out
​
 ⋅( 
( 
L
W
​
 ) 
ref
​
 
I 
ref
​
 
​
 )
Where:

𝐼
𝑜
𝑢
𝑡
I 
out
​
  is the output current of the current mirror.
𝑊
/
𝐿
W/L represents the transistor sizing ratio.
𝐼
𝑟
𝑒
𝑓
I 
ref
​
  is the reference current set by the current mirror.
7. Thermal Voltage (V_T)
The thermal voltage 
𝑉
𝑇
V 
T
​
  is a key factor in PTAT calculations. It is given by:

𝑉
𝑇
=
𝑘
𝑇
𝑞
V 
T
​
 = 
q
kT
​
 
Where:

𝑘
k is Boltzmann's constant 
(
1.38
×
1
0
−
23
 
J/K
)
(1.38×10 
−23
 J/K),
𝑇
T is the absolute temperature in Kelvin,
𝑞
q is the charge of an electron 
(
1.6
×
1
0
−
19
 
C
)
(1.6×10 
−19
 C).
At room temperature 
(
𝑇
=
300
 
K
)
(T=300K), the thermal voltage 
𝑉
𝑇
≈
26
 
mV
V 
T
​
 ≈26mV.

You can directly copy these equations into your README file to provide a clear mathematical description of your Bandgap Reference Circuit.





