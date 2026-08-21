# Simulation and Design Verification

## What is the purpose of the simulation stage?

We already decided that our TIA needs to work at high frequency, around the 120 MHz target.

But a high-speed TIA is not as simple as:

> "Put an op-amp + resistor + capacitor and it will work."

The photodiode itself has about **80 pF capacitance**.

The OPA657 also has its own input characteristics.

Then we have:

- Feedback resistor
- Feedback capacitor
- PCB parasitic capacitance
- Wiring/layout effects

All of these interact.

So even if a calculation says:

> "80 Ω and 15 pF should work"

we still need to ask:

> **Does the complete circuit actually behave properly at high frequency?**

That's what simulation tells us.

---

# 1. Why did we simulate the TIA?

We already decided that our TIA needs to work at **high frequency**, around the **120 MHz target**.

But a high-speed TIA is not as simple as:

> "Put an op-amp + resistor + capacitor and it will work."

The photodiode itself has about **80 pF capacitance**.

The OPA657 also has its own input characteristics.

Then we have:

- the feedback resistor
- the feedback capacitor
- PCB parasitic capacitance
- wiring/layout effects

All of these interact.

So even if a calculation says:

> "80 Ω and 15 pF should work"

we still need to ask:

> **Does the complete circuit actually behave properly at high frequency?**

That's what simulation tells us.

---

# 2. What exactly were we trying to find out?

We were mainly interested in four things:

## 1. Gain

The TIA converts photocurrent into voltage.

Approximately:

\[
V_{out}=I_{photo}\times R_f
\]

So with:

\[
R_f=80\Omega
\]

we know the approximate transimpedance gain.

But we also want to see how that gain behaves as frequency increases.

---

## 2. Bandwidth

Our target is approximately:

**120 MHz**

So we want to know:

> Does the TIA continue responding properly up to around 120 MHz?

If the circuit's response starts falling too early, then our design isn't meeting the requirement.

---

## 3. Gain Peaking

This is very important.

Imagine the ideal response is approximately:

```text
Gain
 │
 │───────────────
 │              \
 │               \
 │                \
 └──────────────────── FrequencyThat's a reasonably controlled response.

But suppose instead it looks like:

Gain
 │
 │          /\
 │         /  \
 │────────/    \────
 │
 └──────────────────── Frequency

That bump is gain peaking.

It means the circuit is amplifying some frequencies more than the nominal response.

We don't want excessive peaking because it indicates that the high-frequency response is becoming less controlled.

So during simulation we looked at:

How much peaking do we get for different feedback capacitor values?

4. Stability

Stability is particularly important in a high-speed TIA.

The photodiode has approximately:

80 pF capacitance

That capacitance appears at the op-amp's input.

At low frequencies, this may not cause a major problem.

But as frequency increases, the capacitor's effect becomes significant.

The TIA can then develop unwanted:

peaking
ringing
oscillation
reduced phase margin

The feedback capacitor Cf helps control this behavior.

That's why we don't just choose Rf based on gain.

We have to consider the entire high-frequency feedback system.

3. Why were we changing C1?

This is the important part.

We had already selected:

R1/Rf = 80 Ω

But we still needed to determine the appropriate:

C1/Cf

Remember:

Rf controls transimpedance gain
Cf controls the high-frequency behavior and helps stabilize the TIA

So instead of blindly saying:

"Let's use 15 pF."

we simulated several values.

For example:

Cf	Gain Peaking
13 pF	1.19 dB
14 pF	0.71 dB
15 pF	0.31 dB
16 pF	0.04 dB

Now you can see what happened.

As we increased Cf:

13 pF → 14 pF → 15 pF → 16 pF

the peaking decreased.

So simulation showed us how the feedback capacitor affects the high-frequency response.

4. Then why didn't we simply choose 16 pF?

This is an important question.

According to the particular sweep we had, 16 pF produced even less peaking than 15 pF.

So why is our circuit using 15 pF?

Because component selection isn't purely about finding the smallest simulation number.

We also have to consider:

practical component availability
standard capacitor values
the overall simulated response
the actual design target
the components available for fabrication

So 15 pF gave a suitably controlled response and was carried forward as the practical design value.

But one thing I want you to remember for interviews:

Don't say "15 pF was mathematically the perfect value."

Your simulation actually showed 16 pF had lower peaking.

A better explanation is:

"We performed an Rf/Cf sweep. For Rf = 80 Ω, increasing Cf reduced the gain peaking. 15 pF gave a suitably controlled response, so it was selected as the practical design value."

That's accurate.

5. What was the purpose of the Rf/Cf sweep?

This is probably the most important thing to understand.

We didn't just simulate one circuit.

We investigated multiple possibilities.

For example:

Different Rf values
        ↓
50 Ω, 60 Ω, 70 Ω,
80 Ω, 90 Ω, 100 Ω
        ↓
Different Cf values
        ↓
Observe frequency response
        ↓
Compare gain/peaking
        ↓
Select suitable pair

So simulation became a design-selection tool.

It helped us answer:

"Which feedback resistor and capacitor combination gives us a suitable high-speed response?"

The sweep allowed us to study the relationship between the feedback resistor and feedback capacitor rather than choosing the values arbitrarily.

6. What did LTspice do for us?

LTspice was basically our virtual laboratory.

Instead of physically building:

Rf = 50 Ω → test
Rf = 60 Ω → test
Rf = 70 Ω → test
Rf = 80 Ω → test
etc.

we could simulate those combinations.

We could sweep frequency and see:

What happens to the output as frequency increases?

We could therefore investigate the circuit before manufacturing the PCB.

So LTspice helped us understand the frequency response and feedback-network behavior.

It allowed us to change component values and observe the resulting response without physically changing components on a fabricated board.

This was especially useful because this is a high-speed circuit, where small changes in the feedback network can affect the frequency response and stability.

7. What about PSpice?

PSpice served as another simulation environment for the design.

The important thing here is that we wanted to simulate the circuit using the OPA657 model.

The OPA657 isn't just an ideal op-amp.

It has real characteristics such as:

finite gain
finite bandwidth
input characteristics
high-speed behavior

The OPA657 macromodel allows the simulation to represent the actual amplifier more realistically than simply using an ideal op-amp.

So PSpice gave us another way to evaluate the circuit with the actual amplifier model.

The purpose was therefore not simply to draw the circuit again in another software.

The purpose was to evaluate the actual amplifier behavior using the OPA657 model and verify the design before moving toward hardware.

8. So why did we use BOTH LTspice and PSpice?

Think of it like this:

LTspice

We used it to investigate the circuit behavior and particularly the Rf/Cf combinations and frequency response.

We could change Rf and Cf, perform frequency sweeps, and observe the resulting gain and peaking.

PSpice

We used it to evaluate the design using the OPA657 macromodel.

This allowed us to work with a model representing the actual amplifier rather than treating the amplifier as an ideal component.

So they were part of the same overall goal:

Verify the TIA design before physically building it.

It's not:

"LTspice is for one circuit and PSpice is for a completely different circuit."

They are both tools used during the design verification stage.

9. What happens if simulation shows the circuit is bad?

This is why simulation comes before PCB fabrication.

Imagine we simulated the circuit and got:

Huge gain peaking
        ↓
Poor stability
        ↓
Bandwidth not sufficient

Then we wouldn't immediately manufacture the PCB.

We would go back and change something:

Rf
 ↓
Cf
 ↓
possibly amplifier
 ↓
simulate again

until we obtained an acceptable design.

So the design process is actually:

Requirement
      ↓
Choose OPA657
      ↓
Choose initial Rf/Cf
      ↓
Simulate
      ↓
Look at response
      ↓
Adjust if necessary
      ↓
Simulate again
      ↓
Finalize values
      ↓
Create schematic
      ↓
PCB

That's the important story.

The simulation stage therefore acts as a verification and iteration stage between the theoretical design and the physical implementation.

10. What does "stability" mean here?

This is something you should understand because your project is a high-speed TIA.

The photodiode has approximately:

80 pF capacitance

That capacitance appears at the op-amp's input.

At low frequencies, this may not cause a major problem.

But as frequency increases, the capacitor's effect becomes significant.

The TIA can then develop unwanted:

peaking
ringing
oscillation
reduced phase margin

The feedback capacitor Cf helps control this behavior.

That's why we don't just choose Rf based on gain.

We have to consider the entire high-frequency feedback system.

11. Why is the photodiode capacitance important in simulation?

The SM05PD4A photodiode has approximately 80 pF capacitance at 5 V bias.

This is significant because the photodiode is connected directly to the TIA input.

Therefore, the TIA does not see an ideal current source.

It sees a current source together with the photodiode's capacitance.

At high frequencies, that capacitance strongly affects the behavior of the circuit.

This is one of the reasons why the TIA cannot be designed by considering only the 80 Ω feedback resistor.

We have to consider:

Photodiode capacitance
        +
OPA657 input characteristics
        +
PCB parasitics
        +
Rf
        +
Cf
        ↓
Overall high-frequency response

That complete interaction is what we investigate during simulation.

12. Why is simulation especially important for this project?

The project is designed for a relatively high bandwidth of approximately 120 MHz.

At these frequencies, components and connections that may seem insignificant at low frequency can affect the circuit.

For example:

photodiode capacitance
op-amp input capacitance
PCB parasitic capacitance
feedback components
wiring
signal paths

Therefore, we cannot rely only on simple DC calculations.

We need to understand how the circuit behaves as frequency changes.

This is why frequency-domain simulation is an important part of the project.

13. What did the simulation ultimately help us decide?

The simulation helped us validate and refine the feedback network.

In particular, we investigated:

different Rf values
different Cf values
gain
frequency response
bandwidth
gain peaking
stability

The simulation showed how changing the feedback capacitor affected the high-frequency response.

For Rf = 80 Ω, the sweep showed:

Cf	Gain Peaking
13 pF	1.19 dB
14 pF	0.71 dB
15 pF	0.31 dB
16 pF	0.04 dB

This gave us evidence for selecting a practical feedback capacitor value.

The important point is that the component values were not selected completely arbitrarily.

They were investigated through simulation before the design was moved to the physical implementation.

14. What is the complete design flow?

The overall project can now be understood as:

Optical requirement
        ↓
Electrical requirement
        ↓
Select photodiode
        ↓
Select suitable high-speed op-amp
        ↓
Determine initial Rf/Cf
        ↓
LTspice / PSpice simulation
        ↓
Frequency-response analysis
        ↓
Gain / bandwidth / peaking / stability evaluation
        ↓
Adjust component values if necessary
        ↓
Finalize circuit values
        ↓
Create schematic
        ↓
Assign footprints
        ↓
PCB layout
        ↓
Fabrication
        ↓
Hardware testing

So simulation was not a separate activity unrelated to the PCB.

It was the stage where we asked:

"Before we physically build this circuit, does our proposed design actually meet the required high-speed behavior?"
