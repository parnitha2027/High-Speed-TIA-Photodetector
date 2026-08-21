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
That bump is gain peaking.

It means the circuit is amplifying some frequencies more than the nominal response.

We don't want excessive peaking because it indicates that the high-frequency response is becoming less controlled.

So during simulation we looked at:

How much peaking do we get for different feedback capacitor values?

## 3. Why were we changing C1?

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

## 4. Then why didn't we simply choose 16 pF?

This is an important question.

According to the particular sweep we had, 16 pF produced even less peaking than 15 pF.

So why is our circuit using 15 pF?

Because component selection isn't purely about finding the smallest simulation number.

We also have to consider:

Practical component availability
Standard capacitor values
The overall simulated response
The actual design target
The components available for fabrication

So 15 pF gave a suitably controlled response and was carried forward as the practical design value.

But one thing I want you to remember for interviews:

Don't say "15 pF was mathematically the perfect value."

Your simulation actually showed 16 pF had lower peaking.

A better explanation is:

"We performed an Rf/Cf sweep. For Rf = 80 Ω, increasing Cf reduced the gain peaking. 15 pF gave a suitably controlled response, so it was selected as the practical design value."

That's accurate.

## 5. What was the purpose of the Rf/Cf sweep?

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

## 6. What did LTspice do for us?

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

## 7. What about PSpice?

PSpice served as another simulation environment for the design.

The important thing here is that we wanted to simulate the circuit using the OPA657 model.

The OPA657 isn't just an ideal op-amp.

It has real characteristics such as:

Finite gain
Finite bandwidth
Input characteristics
High-speed behavior

The OPA657 macromodel allows the simulation to represent the actual amplifier more realistically than simply using an ideal op-amp.

So PSpice gave us another way to evaluate the circuit with the actual amplifier model.

## 8. So why did we use BOTH LTspice and PSpice?

Think of it like this:

LTspice

We used it to investigate the circuit behavior and particularly the Rf/Cf combinations and frequency response.

PSpice

We used it to evaluate the design using the OPA657 macromodel.

So they were part of the same overall goal:

Verify the TIA design before physically building it.

It's not:

"LTspice is for one circuit and PSpice is for a completely different circuit."

They are both tools used during the design verification stage.

## 9. What happens if simulation shows the circuit is bad?

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

## 10. What does "stability" mean here?

This is something you should understand because your project is a high-speed TIA.

The photodiode has approximately:

80 pF capacitance

That capacitance appears at the op-amp's input.

At low frequencies, this may not cause a major problem.

But as frequency increases, the capacitor's effect becomes significant.

The TIA can then develop unwanted:

Peaking
Ringing
Oscillation
Reduced phase margin

The feedback capacitor Cf helps control this behavior.

That's why we don't just choose Rf based on gain.

We have to consider the entire high-frequency feedback system.

## 11. Overall purpose of the simulation stage

The simulation stage can therefore be understood as:

Before physically constructing the TIA PCB, we used LTspice and PSpice to predict and verify how the circuit would behave at high frequency.

We investigated:

Transimpedance gain
Frequency response
Target bandwidth
Gain peaking
Feedback resistor values
Feedback capacitor values
Stability
OPA657 behavior using its macromodel

The Rf/Cf sweep was especially important because it allowed us to compare different combinations rather than selecting the feedback components arbitrarily.

The final design then moved from:

theoretical design → simulation verification → component-value selection → schematic → PCB layout → fabrication → physical testing.

