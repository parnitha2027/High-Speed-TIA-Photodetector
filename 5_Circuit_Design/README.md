# What comes next after understanding the design?

You can think of the entire project as:

**Optical requirement → Electrical requirement → Component selection → Circuit design → Simulation → Schematic → PCB layout → Fabrication → Testing**

You have already worked through the first three stages.

Now we start the actual **circuit construction/design process**.

---

# 1. Start with the electrical requirements

Before drawing the schematic, we first establish what the TIA must achieve.

For your project, the important requirements are approximately:

- Photodiode: **SM05PD4A**
- Optical wavelength: **1064 nm**
- Photodiode capacitance: approximately **80 pF at 5 V**
- Target bandwidth: around **120 MHz**
- Op-amp: **OPA657U**
- Supply: **+5 V / −5 V**
- Feedback resistor: **80 Ω**
- Feedback capacitor: **15 pF**

The important point is:

> We don't start by randomly placing components.

We start with the required **performance**, and then choose the circuit values that can achieve that performance.

---

# 2. Understand the basic TIA structure

The core of the circuit is very simple:

**Photodiode → OPA657 → feedback network → output voltage**

The feedback network contains:

- **R1 = 80 Ω**
- **C1 = 15 pF**

R1 and C1 are connected between the **OPA657 output** and its **inverting input**.

The reason they are there is already what we discussed:

### R1

Controls the **transimpedance gain**.

Approximately:

$$
V_{out} = I_{photo} \times R_f
$$

So with:

$$
R_f = 80\Omega
$$

the circuit produces a voltage corresponding to the photodiode current.

### C1

Controls the high-frequency behavior and helps maintain **stability** in the presence of the photodiode's large capacitance.

So the central TIA is:

**Photodiode → OPA657 → Feedback network → Output**

That is the **heart of the PCB**.

---

# 3. Then deal with the photodiode side

Now we look at the input.

Your photodiode is not an ideal current source.

It has its own:

- capacitance
- bias requirements
- electrical parasitics

The approximately **80 pF photodiode capacitance** is particularly important because the TIA is intended to operate at high frequency.

So we need to provide the appropriate biasing and filtering around the photodiode.

This is where your input-side components come in.

You have:

- **R2 = 100 Ω**
- **C2 = 2.2 nF**
- **C3 = 0.1 µF**
- **J1 = photodiode/coaxial input**

These aren't part of the feedback gain-setting mechanism.

They are associated with the **photodiode bias/input environment**.

The important conceptual separation is:

### Feedback components

**R1 + C1**

→ determine TIA gain/stability/high-frequency response.

### Input/bias components

**R2 + C2 + C3**

→ provide the required biasing/filtering environment for the photodiode.

That distinction is important when you're explaining the design.

---

# 4. Then design the power-supply section

The OPA657 is operated from:

**+5 V**

and

**−5 V**

with ground as the reference.

So your PCB needs three supply connections:

**+5 V / GND / −5 V**

That's why J3 in your schematic is a **3-pin power connector**.

---

## Why do we put capacitors near VS+ and VS−?

This is your next section.

### Positive supply

**OPA657 VS+ → +5 V**

with a capacitor close to the supply pin.

### Negative supply

**OPA657 VS− → −5 V**

with another capacitor close to the supply pin.

These are **decoupling/bypass capacitors**.

Their job is to provide a clean local supply to the high-speed op-amp.

A high-speed amplifier can suddenly demand current when its output changes rapidly.

The power supply wires are not perfect. They have resistance and inductance.

Therefore, instead of making the op-amp depend entirely on the distant power supply, we put capacitors physically close to the supply pins.

The two capacitor values in your schematic serve different frequency ranges.

The **smaller capacitor** is effective at very high frequencies.

The **larger capacitor** provides more bulk energy storage and handles lower-frequency supply variations.

And because your OPA657 is a **high-speed amplifier**, supply decoupling becomes particularly important.

---

# 5. Then look at the output

Your OPA657 output doesn't directly go to J2.

You have:

**OPA657 output → R3 = 50 Ω → J2**

This is the output interface.

J2 is your coaxial/SMA output connection.

The **50 Ω resistor** is there because high-speed signals are commonly transmitted through **50 Ω transmission systems**.

Your oscilloscope/input equipment and coaxial cables are generally designed around this 50 Ω environment.

So R3 helps the amplifier interface properly with the external high-frequency measurement system.

Conceptually:

**OPA657 output → 50 Ω → J2 → Coaxial cable → Oscilloscope / instrument**

This becomes especially important when you're working around **100+ MHz**, because at those frequencies the PCB traces and cables cannot simply be treated as ideal wires.

---

# 6. Now put everything together into the schematic

At this point we know **why every major section exists**.

So the schematic becomes much easier to understand.

Your circuit can be divided into five functional blocks:

### Block 1 — Photodiode input

**J1 → Photodiode → R2 → C2/C3**

Purpose:

**Photodiode connection + bias/filtering**

---

### Block 2 — TIA amplifier

**Photodiode → OPA657**

Purpose:

**Convert photocurrent into voltage**

---

### Block 3 — Feedback network

**R1 = 80 Ω + C1 = 15 pF**

connected between the OPA657 output and inverting input.

Purpose:

**Set transimpedance behavior and stabilize the high-speed TIA**

---

### Block 4 — Power supply

**+5 V → decoupling → VS+**

**−5 V → decoupling → VS−**

**GND = reference**

Purpose:

**Provide clean power to OPA657**

---

### Block 5 — Output interface

**OPA657 → 50 Ω → J2 → SMA/coax**

Purpose:

**Interface the high-speed output to the measurement equipment**

---

# 7. Then comes simulation

Only after the circuit topology is established do we verify:

> **Does this design actually behave the way we expect?**

This is where your LTspice/PSpice work comes in.

You simulate things such as:

### Gain

How much voltage do we get for a given photocurrent?

### Bandwidth

Does the circuit reach the required high-frequency response?

### Peaking

Does the response have an unwanted peak?

### Stability

Does the circuit remain well behaved, or does it show excessive ringing/oscillation?

### Frequency response

You sweep frequency and observe the output.

---

# 8. This is where your Rf/Cf sweep becomes important

You already have the table you showed me.

You tested combinations such as:

| Rf | Cf |
|---|---|
| 50 Ω | 19–22 pF |
| 60 Ω | 17–20 pF |
| 70 Ω | 15–18 pF |
| **80 Ω** | **13–16 pF** |
| 90 Ω | 13–16 pF |
| 100 Ω | 11–14 pF |

And you observed how the gain and peaking changed.

For example, at:

**Rf = 80 Ω**

you had:

- 13 pF → ~1.19 dB peaking
- 14 pF → ~0.71 dB
- **15 pF → ~0.31 dB**
- 16 pF → ~0.04 dB

This tells you that the feedback capacitor isn't just an arbitrary 15 pF value.

You investigated the behavior of the circuit and selected a value that gives a suitably flat/stable response.

That is the **design-validation stage**.
