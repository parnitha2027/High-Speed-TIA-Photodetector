# PCB Fabrication

After completing the schematic design, simulation, and PCB layout in KiCad, the finalized PCB design was prepared for fabrication. PCB fabrication is the process of converting the completed PCB layout into a physical printed circuit board that can be assembled and tested.

The fabrication stage comes after the PCB layout and design verification stages:

Schematic Design → Simulation → PCB Layout → Design Verification → PCB Fabrication → Component Assembly → Testing

## 1. Purpose of PCB Fabrication

The purpose of fabrication was to convert the finalized high-speed TIA PCB layout into a physical board.

The PCB contains the electrical connections and physical arrangement required for the TIA circuit, including:

OPA657U amplifier
Photodiode input connection
Feedback resistor and capacitor
Input biasing and filtering components
Power-supply decoupling capacitors
50 Ω output interface
Power connector
Ground and power planes
High-speed signal traces

The fabricated board provides the physical platform on which the components can be mounted and the TIA can be electrically tested.

## 2. Preparing the PCB for Fabrication

Before sending the PCB for fabrication, the final PCB layout was reviewed in KiCad.

The layout was checked to ensure that:

All required components were present.
Components had the correct footprints.
Electrical connections matched the schematic.
Traces were properly routed.
Power connections were correctly implemented.
Ground connections were properly established.
The high-speed feedback path was kept short.
Input and output connections were correctly positioned.
The board dimensions were finalized.
The PCB design satisfied the required design rules.

This stage was important because any error in the PCB layout would be transferred to the physical board during fabrication.

## 3. Design Rule Check Before Fabrication

A Design Rule Check (DRC) was performed in KiCad before fabrication.

The DRC was used to identify possible PCB layout problems such as:

Incorrect clearances
Unconnected nets
Trace-spacing violations
Incorrect routing
Via-related issues
Other manufacturing or layout-rule violations

The purpose of performing DRC was to ensure that the PCB layout was suitable for fabrication and that obvious design errors were identified before manufacturing.

## 4. Fabrication Data Generation

Once the PCB layout was finalized and verified, the required fabrication files were generated from KiCad.

These files contain the information required by the PCB manufacturer to manufacture the physical board.

The fabrication data represents details such as:

Copper layers
PCB outlines
Drilled holes
Vias
Solder-mask openings
Component markings
Other manufacturing information

For a multi-layer PCB, the fabrication files also define the individual copper layers and their corresponding structures.

## 5. Four-Layer PCB Fabrication

The TIA PCB was designed as a 4-layer PCB.

Using multiple layers allows the high-speed circuit to have controlled routing and dedicated areas for power and ground.

The four-layer structure provides additional flexibility for:

High-speed signal routing
Ground distribution
Power distribution
Reducing unwanted parasitic effects
Maintaining a compact PCB layout

This is particularly useful for the TIA because the circuit is designed for high-frequency operation.

## 6. Importance of Fabrication Accuracy

Fabrication accuracy is particularly important for a high-speed TIA.

At high frequencies, the PCB itself becomes part of the electrical system.

Therefore, physical characteristics such as:

Trace length
Trace geometry
Layer structure
Ground-plane continuity
Via placement
Component placement
Parasitic capacitance
Parasitic inductance

can influence circuit performance.

For this reason, the fabricated PCB needs to closely follow the finalized KiCad PCB layout.

## 7. Fabricated PCB

After the fabrication process was completed, the designed PCB was obtained as a physical board.

The fabricated board provides the physical implementation of the TIA circuit that was previously represented through:

Schematic → Simulation → PCB Layout

The physical PCB can then proceed to the next stage:

Component Assembly → Electrical Testing → Performance Verification

## 8. Transition from Fabrication to Testing

PCB fabrication does not itself verify that the TIA is functioning correctly.

It only produces the physical implementation of the designed circuit.

After fabrication, the next stages involve:

Inspecting the fabricated PCB
Checking component placement
Soldering/assembling the components
Checking power connections
Verifying supply rails
Applying the required input signal
Measuring the output
Testing the frequency response
Comparing the measured behavior with the simulation results

The final objective is to determine whether the physical TIA PCB performs as expected from the theoretical and simulated design.

Overall PCB Development Flow

The complete process followed for the TIA can therefore be represented as:

Optical System Requirements
↓
Electrical Requirements
↓
Component Selection
↓
TIA Circuit Design
↓
LTspice / PSpice Simulation
↓
Schematic Design in KiCad
↓
PCB Layout in KiCad
↓
Design Rule Check
↓
Fabrication File Generation
↓
PCB Fabrication
↓
Component Assembly
↓
Electrical Testing
↓
Performance Verification
