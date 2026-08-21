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

