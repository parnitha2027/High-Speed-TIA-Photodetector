# The OPA657 TIA: The Engineering Story Behind Every Component

The whole idea is:

**Problem → requirement → constraint → component choice → component value → final circuit**

The important thing is that the components were not supposed to be chosen randomly.

The photodiode creates the main problem:

- It gives us **current**, not voltage.
- The current can be very small.
- We want to measure a very fast optical signal.
- The photodiode has about **80 pF capacitance**.
- We want approximately **120 MHz-class bandwidth**.
- Therefore, the amplifier and all the surrounding components have to be selected around those requirements.

So every component in the schematic is basically there to solve a particular problem.

---

# 0. The signal chain problem, stated plainly

Our complete system is basically:

**Laser → Phase Modulator → Optical System/Cavity → Photodiode → TIA → Voltage → Oscilloscope**

The laser produces the optical signal.

The phase modulator changes the phase of that optical signal.

The optical system then produces the optical behavior we are interested in.

The photodiode detects that optical signal.

But the photodiode does **not** directly give us a convenient voltage.

It produces a **photocurrent**.

That current can be very small, and we want to observe the changing signal on electrical equipment such as an oscilloscope.

So we need something that converts:

**small optical-generated current → usable electrical voltage**

That is the job of the TIA.

But there is another important problem.

The photodiode is not just a perfect current source. It also has approximately **80 pF of capacitance** when reverse biased at 5 V.

That capacitance becomes extremely important when we want to operate at high frequency.

So our actual problem is:

> We need to take a very small, fast-changing current from a capacitive photodiode and convert it into a clean voltage without losing the high-frequency information or making the amplifier unstable.

That is why this isn't simply a matter of connecting a photodiode to an ordinary op-amp.

---

# 1. Why the OPA657U?

## What the photodiode imposes on the amplifier

The photodiode can be thought of electrically as:

**current source + capacitor**

The important capacitor here is the photodiode's junction capacitance.

For our SM05PD4A, the value we're working with is approximately:

**80 pF at 5 V reverse bias.**

This 80 pF is extremely important because it is connected to the TIA's sensitive inverting-input node.

At high frequency, capacitance affects how the feedback loop behaves.

So we cannot simply say:

> "Let's choose any fast op-amp."

We need an op-amp with particular characteristics.

### First requirement: High GBW

GBW means **gain-bandwidth product**.

Our target is around **120 MHz bandwidth**.

Therefore, the op-amp itself needs to be considerably faster than 120 MHz because the photodiode's 80 pF capacitance makes the feedback problem more difficult.

The OPA657 has approximately:

**1.6 GHz GBW**

which gives us a lot more bandwidth capability than our ~120 MHz target.

---

### Second requirement: Low input capacitance

Remember that the photodiode already gives us:

**80 pF**

If we choose an amplifier that itself has a very large input capacitance, the total capacitance becomes even larger.

That makes the TIA harder to stabilize and reduces the achievable bandwidth.

OPA657 has approximately:

**5 pF effective input capacitance**

So compared with the 80 pF from the photodiode, the amplifier adds relatively little.

---

### Third requirement: Low input bias current

The op-amp has some tiny current entering its input.

If this current is large, it can create an unwanted output voltage.

OPA657 is a **JFET-input amplifier**, so its input bias current is extremely small, around the pA range.

That is useful for a photodiode circuit because we're trying to measure tiny currents.

---

### Fourth requirement: Low noise

We are measuring a small signal.

Therefore, if the amplifier itself produces a lot of noise, we might end up measuring the amplifier's noise instead of the optical signal.

OPA657 has relatively low voltage and current noise, which makes it appropriate for a high-speed photodetector amplifier.

---

## Why the OPA657U specifically

The important OPA657 characteristics being used in this design are:

- **GBW ≈ 1.6 GHz**
- **Input bias current ≈ 1–2 pA**
- **Input voltage noise ≈ 4.8 nV/√Hz**
- **Input current noise ≈ 1.3 fA/√Hz**
- **Input capacitance ≈ 5 pF**

So the basic reason is:

**We have a fast photodiode with significant capacitance, and we want a ~120 MHz TIA.**

OPA657 gives us the high speed, low input capacitance, low bias current, and low noise that are appropriate for that job.

---

# LTC6268 — why it was considered, and the concrete reason it doesn't fit here

This is important because you asked:

> Why didn't we just use LTC6268?

The LTC6268 was actually a very attractive option.

In some respects, it is even better than OPA657 for TIA applications.

For example, it has:

- lower input capacitance
- low noise
- very low input bias current
- and the LTC6268-10 variant has very high GBW.

So the reason is **not**:

> "OPA657 is simply better than LTC6268."

That would be incorrect.

The major confirmed issue is the **supply voltage**.

The standard LTC6268 operates from approximately:

**3.1 V to 5.25 V total supply**

while our OPA657 circuit uses:

**+5 V and −5 V**

which gives a total supply span of:

**10 V**

The standard LTC6268 is not rated for that ±5 V supply arrangement.

Therefore, it cannot simply be substituted into this exact circuit.

So the documented engineering story is:

**LTC6268 → excellent TIA characteristics, but incompatible with the ±5 V supply used in this design.**

**OPA657 → suitable high-speed TIA amplifier and compatible with the ±5 V supply.**

One thing the document specifically says we **cannot claim without additional project records** is that we tested many other amplifiers and rejected each one for specific reasons. The confirmed comparison in the document is primarily **LTC6268 vs OPA657**.

---

# 2. Why R1 = 80 Ω and C1 = 15 pF?

This is probably the most important component-selection part of the design.

Here:

**R1 = Rf = 80 Ω**

and

**C1 = Cf = 15 pF**

They are connected in parallel in the feedback path.

They work together.

---

## What Rf (R1) does

The feedback resistor determines the **transimpedance gain**.

Very simply:

**Output voltage = Photocurrent × Rf**

So if:

**Rf = 80 Ω**

and the photodiode produces:

**1 µA**

then approximately:

**Vout = 1 µA × 80 Ω = 80 µV**

So increasing Rf would give us more voltage for the same photocurrent.

That sounds good.

But there is a problem.

### Higher Rf = higher gain but lower bandwidth.

### Lower Rf = lower gain but higher bandwidth.

Our project is focused on **high bandwidth**, around 120 MHz.

Therefore, we cannot simply choose a large resistor such as:

**1 kΩ**

**10 kΩ**

**100 kΩ**

because the high photodiode capacitance combined with a large feedback resistance would make it much harder to maintain the required bandwidth and stability.

Therefore:

**80 Ω was chosen as a relatively small feedback resistance to prioritize speed.**

The important trade-off is:

> We sacrificed some voltage gain in order to achieve much higher bandwidth.

---

# Why Cf (C1) is needed at all

Now we have:

**R1 = 80 Ω**

But we cannot just use the resistor alone.

Why?

Because the photodiode gives us approximately:

**80 pF**

and the OPA657 adds approximately:

**5 pF**

So our input capacitance is already around:

**85 pF**

before considering additional PCB parasitic capacitance.

That capacitance interacts with the feedback network.

At high frequency, this can make the amplifier's noise gain rise in an undesirable way.

Eventually, the amplifier can lose phase margin.

And when phase margin becomes insufficient, we can get:

- ringing
- peaking
- oscillation
- instability

So we add **Cf** across Rf.

That is our:

**C1 = 15 pF**

Its job is to control the high-frequency behavior of the feedback loop and make the amplifier stable.

So remember:

**R1 → primarily determines transimpedance gain**

**C1 → controls high-frequency feedback/stability and bandwidth**

---

# How 15 pF was arrived at

There is a standard starting-point formula:

**Cf ≈ √(Cin / (2π × Rf × GBW))**

The important variables are:

- Cin = total input capacitance
- Rf = feedback resistance
- GBW = amplifier gain-bandwidth product

For our design:

- Photodiode capacitance ≈ 80 pF
- OPA657 input capacitance ≈ 5 pF
- PCB parasitic capacitance adds some more
- Rf = 80 Ω
- GBW = 1.6 GHz

This puts the required feedback capacitance in approximately the **10–15 pF region** as a starting point.

So 15 pF is not an arbitrary number. It is in the appropriate range for stability.

---

## But C1 also affects bandwidth

The feedback resistor and capacitor also give us an approximate bandwidth relationship:

**f-3dB ≈ 1 / (2πRfCf)**

Putting:

**Rf = 80 Ω**

and

**Cf = 15 pF**

gives approximately:

**132.6 MHz**

That is very close to the project's approximately **120 MHz-class target**.

So 15 pF makes sense from two directions:

1. It helps stabilize the TIA.
2. It places the bandwidth in the required region.

That is why the choice is meaningful rather than random.

---

## What about 16 pF?

Your sweep data actually showed that increasing Cf can reduce gain peaking.

So 16 pF can give a slightly flatter response.

But increasing Cf also decreases bandwidth.

For 16 pF:

**f-3dB ≈ 124.3 MHz**

while 15 pF gives approximately:

**132.6 MHz**

So the design is basically balancing:

**stability/less peaking**

against

**higher bandwidth.**

The document notes that the exact hard peaking limit and the original sweep decision should be confirmed from the actual sweep records rather than invented.

---

# Summarizing the relationship between all the pieces

| Quantity | Value | Why it matters |
|---|---:|---|
| Photodiode capacitance | ~80 pF | Main input-capacitance constraint |
| OPA657 input capacitance | ~5 pF | Adds to total input capacitance |
| OPA657 GBW | 1.6 GHz | Gives enough speed for high-bandwidth operation |
| Rf/R1 | 80 Ω | Sets transimpedance gain and keeps bandwidth high |
| Cf/C1 | 15 pF | Helps stability and sets high-frequency response |

---

# 3. Why the capacitors around VS+ / VS−?

Now we move away from the signal feedback network.

These capacitors are doing a **completely different job**.

They are connected around the OPA657's:

**VS+ = +5 V**

and

**VS− = −5 V**

supply rails.

---

## Why ±5 V dual supply?

The OPA657 is being operated with:

**+5 V**

and

**−5 V**

So the total voltage across the amplifier is:

**10 V**

This is within the OPA657's specified total supply range.

Using ±5 V also means that:

**0 V/GND sits naturally between the two rails.**

That makes it convenient for a signal that needs to swing around ground.

---

# Decoupling vs. feedback capacitors — the important distinction

This is very important.

Do **not** confuse C1 with the supply capacitors.

### C1 = 15 pF

This is a **signal/feedback capacitor**.

It directly affects:

- feedback
- stability
- bandwidth
- gain response

### Supply capacitors

These are **power-supply decoupling capacitors**.

They don't intentionally process the signal.

Their job is to make sure the:

**+5 V and −5 V supply rails remain clean.**

---

# Why two different capacitor values: 0.1 µF and 6.8 µF?

The idea is:

**small capacitor = high-frequency filtering**

**large capacitor = lower-frequency/bulk filtering**

### 0.1 µF

This capacitor responds very quickly to high-frequency disturbances.

A high-speed amplifier can demand very fast changes in current.

The 0.1 µF capacitor provides a local high-frequency current path.

### 6.8 µF

This is the larger capacitor.

It acts more like a local energy reservoir.

It handles slower changes and prevents the supply voltage from drooping during larger current demands.

So:

**0.1 µF → fast/high-frequency**

**6.8 µF → slower/larger transient**

Together they cover a wider frequency range.

---

# Why proximity to the supply pins matters

This becomes particularly important because our amplifier is very fast.

If the capacitor is far away from the op-amp, the PCB trace itself has inductance.

At high frequency, that inductance matters.

So even though the capacitor might electrically be connected to +5 V, it may not behave like a good high-frequency bypass capacitor if it is physically far away.

Therefore:

**The decoupling capacitors should be placed physically close to the OPA657 supply pins.**

This reduces the unwanted parasitic inductance and keeps the supply clean where the amplifier actually needs it.

---

# 4. Why R3 = 50 Ω at the output?

Now we come to the output.

We have:

**OPA657 output → R3 = 50 Ω → J2 coaxial connector**

Why?

Because our output eventually goes to laboratory equipment through a coaxial cable.

Coaxial measurement systems commonly use:

**50 Ω characteristic impedance.**

So R3 is being used as a **series/source termination resistor**.

---

# What it's actually doing

There are two important reasons.

### 1. Signal integrity

A high-frequency signal travelling through a coax cable behaves like a transmission-line signal.

If the source impedance is not appropriate, reflections can occur.

The 50 Ω series resistor helps make the source look like approximately 50 Ω.

That reduces reflections and ringing.

---

### 2. It protects/isolate the OPA657 output

The coaxial cable has capacitance.

A very fast op-amp can have problems when directly driving capacitive loads.

The resistor provides isolation between:

**OPA657 output**

and

**cable/load**

which helps the amplifier remain stable.

---

## R3 is NOT a gain resistor

This is important.

R3 is **not** controlling the TIA transimpedance gain.

The gain resistor is:

**R1 = 80 Ω**

R3 is outside the feedback loop.

So:

**R1 → gain**

**R3 → output interface/signal integrity**

---

## One important thing about measuring the output

If the oscilloscope is terminated with:

**50 Ω**

then you have:

**R3 = 50 Ω**

and:

**Scope input = 50 Ω**

So they form a voltage divider.

Therefore, the scope can see approximately **half the actual OPA657 output voltage**.

This needs to be considered when interpreting the measured gain.

---

# 5. Why R2 = 100 Ω, and the 2.2 nF / 0.1 µF near J1?

This section is about the **photodiode bias network**.

It is important not to confuse this with the OPA657 supply decoupling.

They look somewhat similar, but they have different purposes.

### OPA657 supply capacitors

→ clean the amplifier's power supply.

### R2 + 2.2 nF + 0.1 µF

→ clean the **photodiode's +5 V bias supply**.

---

# Why the photodiode is reverse-biased at all

The photodiode is given approximately:

**5 V reverse bias.**

Why?

Because reverse bias reduces the photodiode's junction capacitance.

Remember our biggest problem:

**photodiode capacitance ≈ 80 pF**

Reducing this capacitance helps the photodiode respond faster and makes the TIA easier to stabilize.

The datasheet's approximately 80 pF and response characteristics are specified at this reverse-bias condition.

So the 5 V reverse bias is important to achieving the high-speed operation we want.

---

# Why R2 = 100 Ω

R2 sits between:

**+5 V supply → R2 → photodiode bias**

The resistor provides **isolation** between the main +5 V supply and the photodiode.

Why is that useful?

Imagine there is noise on the +5 V rail.

If the photodiode were connected directly to that noisy supply, some of that noise could appear at the photodiode.

The photodiode is connected to the extremely sensitive TIA input.

Therefore, supply noise could eventually become output noise.

R2 helps isolate the photodiode bias node.

Together with the capacitors, it forms an **RC filtering network**.

The document does make an important distinction here:

The **general reason for using 100 Ω is clear**, but the exact project-specific calculation that says:

> "It must be exactly 100 Ω rather than 50 Ω or 200 Ω"

is not documented in the information currently available.

So we shouldn't invent a calculation that we don't actually have.

---

# Why 2.2 nF and 0.1 µF?

Again, we're using **two capacitor values** because we want to filter noise over different frequency ranges.

### 0.1 µF

Provides broader/lower-frequency bypassing.

### 2.2 nF

Provides faster/high-frequency bypassing.

Together with:

**R2 = 100 Ω**

they form a filtering network for the photodiode's bias supply.

The goal is:

**+5 V supply noise → filtered → clean photodiode bias**

instead of:

**+5 V supply noise → photodiode → sensitive TIA input**

---

# Why they're located close to the photodiode/input section

The TIA's inverting input is an extremely sensitive point.

Remember:

The photodiode's current goes into this node.

The TIA then converts that current into voltage through Rf.

So if unwanted current/noise enters this node, the TIA doesn't know whether it is:

**real optical signal**

or

**unwanted electrical noise.**

It amplifies whatever current appears there.

Therefore, the bias filtering components are kept close to the photodiode/input area.

This minimizes the PCB trace length over which unwanted high-frequency noise can travel.

---

# 6. The complete design story, start to finish

This is the part that connects **everything together**.

## 1. The problem

We have an optical system producing a modulated optical signal.

We want to detect and electrically measure it.

We want to preserve fast changes in that signal.

Therefore, we have approximately a:

**120 MHz-class bandwidth target.**

---

## 2. Why a photodiode, and why that creates a current, not a voltage

The photodiode converts light into current.

The problem is that the current is small.

And the photodiode has approximately:

**80 pF capacitance.**

If we simply connected a photodiode to a resistor and tried to measure the voltage, the capacitance and resistance would form an RC low-pass response.

That would severely restrict our bandwidth.

The TIA solves this differently.

The op-amp keeps the photodiode input near a **virtual ground**.

That allows us to convert the photocurrent into voltage through the feedback resistor while maintaining much better high-frequency performance.

---

## 3. Why reverse bias

We apply:

**5 V reverse bias**

to the photodiode.

This reduces its capacitance to approximately:

**80 pF**

and improves its response speed.

That helps us achieve the required bandwidth.

---

## 4. Why that 80 pF forces a high-speed, low-Cin, low-noise amplifier

Now everything starts connecting.

The photodiode gives us:

**80 pF**

Therefore, we need an amplifier that can handle this capacitance without becoming unstable.

OPA657 gives us:

**1.6 GHz GBW**

and approximately:

**5 pF input capacitance.**

Therefore, it is suitable for this high-speed TIA design.

---

## 5. Why Rf = 80 Ω

We deliberately keep Rf small.

Why?

Because:

**small Rf → lower gain but higher bandwidth**

and our priority is:

**high bandwidth.**

So:

**80 Ω**

is chosen as part of the trade-off between gain and bandwidth.

---

## 6. Why Cf = 15 pF

Cf is needed to stabilize the TIA.

It also controls the high-frequency response.

With:

**Rf = 80 Ω**

and

**Cf = 15 pF**

the approximate bandwidth is:

**132.6 MHz**

which is close to the desired ~120 MHz-class bandwidth.

So the value makes sense from both:

**stability**

and

**bandwidth**

perspectives.

---

## 7. Why ±5 V supplies

The OPA657 is operated using:

**+5 V**

and

**−5 V**

This gives a total supply span of:

**10 V**

which is within the OPA657's specified supply range.

It also allows the signal to naturally be referenced around:

**0 V/GND.**

---

## 8. Why supply decoupling

The OPA657 is a very fast amplifier.

Therefore, its supply rails need to be clean.

We use:

**0.1 µF + 6.8 µF**

to handle different frequency ranges of supply disturbances.

And they need to be physically close to the amplifier's supply pins.

---

## 9. Why the photodiode bias network

The photodiode also needs a clean +5 V reverse-bias supply.

Therefore we use:

**R2 = 100 Ω**

plus:

**2.2 nF**

and

**0.1 µF**

to filter the +5 V supply before it reaches the photodiode.

This prevents supply noise from entering the most sensitive part of the circuit.

---

## 10. Why R3 = 50 Ω

Finally, the TIA's voltage output has to travel through coax to the measurement equipment.

Therefore:

**R3 = 50 Ω**

provides source termination and helps with:

- signal integrity
- reducing reflections
- isolating the op-amp from cable capacitance
- interfacing with standard 50 Ω laboratory equipment

---

# How it all locks together

This is ultimately the most important thing to understand.

The **photodiode** is what creates the main constraint.

It gives us:

**small photocurrent + approximately 80 pF capacitance**

That leads to the need for:

**high-speed, low-capacitance OPA657**

Then the amplifier choice and photodiode capacitance influence:

**Rf and Cf**

which are:

**R1 = 80 Ω**

and

**C1 = 15 pF**

Then because the amplifier is extremely fast, we need clean power:

**0.1 µF + 6.8 µF decoupling**

Then because the photodiode needs reverse bias, we need:

**R2 = 100 Ω + 2.2 nF + 0.1 µF**

to keep that bias clean.

And finally, because we need to send the high-speed voltage through coax to the measurement equipment:

**R3 = 50 Ω**

is used at the output.

So the entire design can be remembered as:

**Photodiode → creates the problem**

**OPA657 → solves the high-speed amplification problem**

**R1/C1 → solve gain + stability + bandwidth**

**Supply capacitors → keep the amplifier power clean**

**R2 + capacitors → keep the photodiode bias clean**

**R3 → makes the high-speed output compatible with the measurement system**

That is the complete engineering story behind the schematic, without getting into PCB construction or schematic-drawing procedure.
