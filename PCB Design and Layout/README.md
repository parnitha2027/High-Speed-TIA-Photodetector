# What is the purpose of the PCB design stage?

After completing the circuit design and verifying the circuit through simulation, the next step was to convert the finalized schematic into a physical PCB design.

The schematic describes how the electrical components are connected, but it does not describe the physical arrangement of those components on a circuit board.

Therefore, the PCB design stage converts the verified electrical circuit into a physical board that can be fabricated and tested.

The overall flow was:

Electrical requirements → Component selection → Circuit design → Simulation → Schematic → PCB layout → Fabrication → Testing

For this project, both the schematic design and PCB layout were carried out using KiCad.

## 1. Schematic Design in KiCad

Before creating the physical PCB, the finalized electrical circuit was represented as a schematic in KiCad.

The schematic contains the electrical connections between the components of the TIA.

The main components included:

U1 — OPA657U
R1 — 80 Ω feedback resistor
C1 — 15 pF feedback capacitor
R2 — 100 Ω input-side resistor
C2 — 2.2 nF capacitor
C3 — 0.1 µF capacitor
R3 — 50 Ω output resistor
Supply decoupling capacitors
J1 — photodiode/input connector
J2 — output connector
J3 — power connector

The schematic therefore provided the complete electrical representation of the TIA before physical layout.

## 2. Why was the schematic created first?

The schematic was created before PCB layout because the electrical connections need to be verified before deciding where the components should physically be placed.

At the schematic stage, we can clearly identify:

Which components are connected together
How the feedback network is connected
How the photodiode is connected
How the power supplies are connected
How the output is connected
Which components are connected to ground

The schematic therefore acts as the electrical blueprint of the PCB.

The PCB layout is then created from this electrical blueprint.

## 3. Moving from Schematic to PCB Layout

Once the schematic was completed in KiCad, the design was transferred to the PCB layout environment.

At this stage, the electrical connections from the schematic became physical connections that had to be implemented on the PCB.

The process involved:

Schematic → Footprints → Component placement → Board layout → Routing → Design verification

The schematic tells us:

What is connected to what?

The PCB layout determines:

Where should each component physically be placed and how should those connections be routed?

## 4. Assigning Component Footprints

Each schematic component needs a corresponding physical footprint before it can be placed on the PCB.

A footprint defines the physical characteristics required for mounting the component, including:

Component dimensions
Pad arrangement
Pad sizes
Pin locations
Physical mounting pattern

For example, the OPA657U requires an appropriate footprint corresponding to its package.

Similarly, the resistors, capacitors, connectors, and other components require their respective footprints.

This step connects the logical schematic component with its physical representation on the PCB.

## 5. Component Placement

After assigning footprints, the components were physically arranged on the PCB.

Component placement is particularly important for this project because this is a high-speed TIA.

The placement cannot simply be based on making the board look compact.

The physical location of components affects:

Parasitic capacitance
Parasitic inductance
Trace length
Signal integrity
Feedback behavior
Power-supply noise
High-frequency performance

Therefore, important components were placed close to the devices they directly interact with.

## 6. Placement of the OPA657

The OPA657U is the central amplifier of the TIA.

Therefore, its position is important because several critical connections originate from it.

The photodiode input connects to the amplifier's input, while the feedback network connects between the amplifier output and inverting input.

Because of this, the OPA657 was placed so that these critical connections could be kept short.

This is especially important because the project operates at high frequency.

## 7. Placement of R1 and C1

The feedback components are:

R1 = 80 Ω
C1 = 15 pF

These components form the feedback network of the TIA.

They were placed close to the OPA657 because the feedback path is extremely important for the high-frequency behavior of the amplifier.

A long feedback connection can introduce additional:

Parasitic capacitance
Parasitic inductance
Trace impedance

These parasitics can affect the stability and frequency response of the TIA.

Therefore:

The feedback path between the OPA657 and R1/C1 should be kept as short as practical.

This is one of the most important PCB-layout considerations in the design.

## 8. Photodiode Input — J1

J1 is the input connector associated with the photodiode.

The photodiode produces a small photocurrent, so the signal at this point is particularly sensitive to unwanted noise and parasitic effects.

Therefore, the input path from J1 to the OPA657 input was kept short and carefully routed.

The objective is to minimize unwanted parasitic capacitance and interference before the signal reaches the TIA input.

This is particularly important because the photodiode already contributes approximately 80 pF of capacitance.

Additional PCB parasitics therefore need to be minimized as much as practical.

## 9. Input-Side Components

The input section contains:

R2 = 100 Ω
C2 = 2.2 nF
C3 = 0.1 µF

These components are associated with the photodiode input/bias environment.

Their physical placement was therefore considered together with the J1 input and the photodiode connection.

The objective is to maintain the intended electrical behavior of the input network while keeping unnecessary trace lengths and parasitics low.

## 10. Power Supply Connections

The OPA657 operates using:

+5 V / GND / −5 V

The PCB therefore needs connections for the positive supply, ground, and negative supply.

A 3-pin power connector, J3, is used to provide these connections.

The power traces must be routed carefully because the amplifier is a high-speed device and supply noise can affect circuit performance.

## 11. Supply Decoupling Capacitors

Decoupling capacitors were placed close to the OPA657 supply pins.

The purpose of these capacitors is to provide a clean and stable local supply to the amplifier.

A high-speed amplifier can experience rapid changes in current demand.

If the supply path contains significant resistance or inductance, the voltage at the amplifier's supply pin can momentarily fluctuate.

The nearby capacitors help provide the required current locally and reduce high-frequency supply disturbances.

Therefore:

The decoupling capacitors should be physically close to the VS+ and VS− supply pins of the OPA657.

## 12. Four-Layer PCB

The PCB was designed as a 4-layer board.

Using multiple layers provides additional flexibility for:

Signal routing
Power distribution
Grounding
Maintaining compact connections
Controlling high-speed signal paths

A multilayer PCB also allows the signal and power connections to be organized more effectively than a simple single-layer board.

For a high-speed TIA, this helps create a more controlled electrical environment.

## 13. Ground Plane

A proper ground structure is important for a high-speed analog circuit.

The PCB uses a ground plane to provide a low-impedance return path for currents.

A continuous and well-planned ground structure helps reduce:

Unwanted loop area
Noise
Ground impedance
Parasitic effects

The ground plane is particularly important for the high-frequency behavior of the TIA because currents at high frequencies follow the available low-impedance return paths.

## 14. Routing

After component placement, the electrical connections from the schematic were physically routed on the PCB.

Routing determines how copper traces connect the components.

For this project, routing had to be done with particular attention to:

Short signal paths
Feedback path length
Input path
Output path
Power connections
Ground connections
High-frequency behavior

The objective was not simply to connect every component.

The routing also had to preserve the intended high-speed electrical behavior of the circuit.

## 15. High-Speed Signal Routing

The TIA is designed for approximately 120 MHz operation, so PCB traces cannot always be treated as ideal wires.

At high frequencies, the physical PCB becomes part of the electrical circuit.

Trace length, parasitic capacitance, parasitic inductance, and return paths can influence the circuit response.

Therefore, high-speed signal paths were kept as short and direct as practical.

This is especially important for:

Photodiode input
OPA657 input
Feedback network
OPA657 output
Output connector
## 16. Feedback Routing

The feedback path is one of the most sensitive parts of the PCB.

The connection between:

OPA657 output → R1/C1 → OPA657 inverting input

was therefore kept short.

The reason is that unwanted parasitic elements in the feedback path can change the effective feedback network.

Since the feedback network controls the high-frequency behavior and stability of the TIA, unnecessary trace length should be avoided.

Therefore:

Short feedback routing is a critical PCB-layout requirement for this design.

## 17. Output Routing

The OPA657 output is connected through:

OPA657 → R3 = 50 Ω → J2

J2 provides the high-speed output connection to external measurement equipment through a coaxial/SMA connection.

The 50 Ω output environment is important because high-frequency measurement equipment and coaxial cables commonly use 50 Ω impedance.

Therefore, the output routing was designed with the high-speed interface in mind.

## 18. PCB Parasitics

One of the major reasons PCB layout is important in this project is that the PCB itself introduces unwanted electrical elements.

These are called parasitics.

Examples include:

Parasitic capacitance

Small unintended capacitances can exist between:

PCB traces
Pads
Ground planes
Component terminals
Parasitic inductance

Traces, vias, and connections have small amounts of inductance.

At high frequencies, these parasitic elements become significant.

The PCB therefore cannot be considered completely separate from the circuit.

The physical layout becomes part of the electrical design.

## 19. Trace Length

Trace length is particularly important around the high-speed TIA.

Longer traces generally introduce more parasitic inductance and capacitance.

Therefore, critical paths were kept short.

The most important paths include:

Photodiode → OPA657 input

OPA657 → R1/C1 → OPA657 feedback

OPA657 output → R3 → J2

Supply pins → decoupling capacitors

These paths require careful placement and routing because they directly influence high-frequency performance.

## 20. Via Usage

Vias are used to connect traces between different PCB layers.

Although vias are necessary in multilayer PCB designs, they also introduce small parasitic effects.

Therefore, unnecessary vias should be avoided in critical high-speed signal paths where possible.

For important high-frequency connections, a simpler and shorter routing path is preferred.

## 21. Design Verification in KiCad

After completing the PCB layout, the design needs to be checked before fabrication.

KiCad provides Design Rule Checking (DRC) to identify potential PCB-layout problems.

The design can be checked for issues such as:

Incorrect clearances
Unconnected connections
Routing violations
Pad and track spacing problems
Other manufacturing-related design-rule violations

The PCB was therefore checked using KiCad's design verification tools before moving toward fabrication.

## 22. Final PCB Design

After completing the placement, routing, grounding, power connections, and design-rule checks, the PCB layout represented the physical implementation of the finalized TIA circuit.

The final PCB contains:

Photodiode input interface
OPA657 TIA
Feedback network
Input filtering/bias components
Supply decoupling
±5 V power interface
50 Ω output interface
Ground structure
High-speed routing

The resulting PCB design was then ready for the next stage:

PCB fabrication → Physical assembly → Testing

Overall PCB Design Flow

The complete process can therefore be summarized as:

Finalized Circuit
       ↓
Schematic in KiCad
       ↓
Assign Component Footprints
       ↓
Transfer to PCB Layout
       ↓
4-Layer Board Setup
       ↓
Component Placement
       ↓
Critical Component Placement
       ↓
High-Speed Routing
       ↓
Power & Ground Routing
       ↓
Ground Plane
       ↓
Design Rule Check (DRC)
       ↓
Final PCB
       ↓
Fabrication
       ↓
Physical Testing
