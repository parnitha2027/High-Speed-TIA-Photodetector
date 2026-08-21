## What is the overall project about?

The basic purpose of this project is to detect and electrically measure an optical signal.

The system starts with a laser, which produces optical light. This light is passed through a phase modulator and then through the optical setup. The resulting optical signal is detected by a photodiode.

The photodiode converts the detected optical signal into a very small photocurrent. Since this current is small and is not directly convenient to observe using an oscilloscope, a Transimpedance Amplifier (TIA) is used.

The TIA converts the photocurrent into a measurable voltage. This voltage can then be observed using an oscilloscope or another electrical measurement instrument.

# Laser – Generation of the Optical Signal

The first component in the system is the laser.
The laser is the source of the optical signal that we want to work with and eventually detect.

In our setup, the laser wavelength is approximately:
λ≈1064 nm

and the laser power is approximately:
P≈223.5 mW
So, at the beginning of the system, we have an optical signal, not an electrical signal.

# Phase Modulator: Controlling the Phase of the Optical Signal

The next component in the optical setup is the phase modulator. In our project, a lithium-niobate phase modulator is used.

The phase modulator is used to change the phase of the optical signal produced by the laser. It does not select, remove, or filter any particular portion of the laser light. Instead, it modifies the phase of the optical wave while the light passes through the modulator.

The phase of a light wave describes its position within one complete cycle. By applying a suitable electrical signal to the phase modulator, the phase of the optical signal can be changed in a controlled manner.

The phase modulator is therefore used to intentionally modify the optical signal so that the response of the optical system can be studied and measured.

#Optical System / Cavity

After the phase modulator, the modified optical signal continues through the optical setup.

In your project, the optical system involves an optical cavity/interferometric setup, where the behavior of the optical signal is of interest.

The reason we care about the phase is that phase plays an important role in interference and optical cavity behavior.

Therefore, changing the phase of the incoming optical signal can produce a measurable change in the optical response of the system.


# Photodiode - Converting Light into Current
We are using the Thorlabs SM05PD4A photodiode.

Our optical wavelength is approximately:

1064 nm

while the photodiode operates over approximately:

900−1700 nm

Therefore, the 1064 nm laser wavelength lies within the useful wavelength range of the photodiode.

The job of the photodiode is to detect the incoming optical signal and convert it into an electrical current.
The photocurrent produced by the photodiode contains information about the optical signal that was detected.
If the amount of detected optical power changes, the photocurrent also changes.
Therefore, the photocurrent is essentially the electrical representation of the detected optical signal.
However, there is a problem.
The photocurrent can be very small.
For example, it could be in the microampere or even smaller range depending on the optical power and photodiode responsivity.
So we need a circuit capable of handling this small current accurately.
That is where the TIA comes in.

# What exactly is a TIA?
The TIA (Transimpedance Amplifier) is used because the photodiode produces a very small current, while our measuring instruments like an oscilloscope are designed to measure voltage.
So, the TIA converts the photodiode's small current into a measurable voltage.
In simple terms:
Photodiode → small photocurrent → TIA → measurable voltage → Oscilloscope
