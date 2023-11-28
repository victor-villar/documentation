---
title: pulse
description: API reference for qiskit.pulse
in_page_toc_min_heading_level: 1
python_api_type: module
python_api_name: qiskit.pulse
---

<span id="module-qiskit.pulse" />

<span id="qiskit-pulse" />

# Pulse

<span id="module-qiskit.pulse" />

`qiskit.pulse`

Qiskit-Pulse is a pulse-level quantum programming kit. This lower level of programming offers the user more control than programming with [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit")s.

Extracting the greatest performance from quantum hardware requires real-time pulse-level instructions. Pulse answers that need: it enables the quantum physicist *user* to specify the exact time dynamics of an experiment. It is especially powerful for error mitigation techniques.

The input is given as arbitrary, time-ordered signals (see: [Instructions](#pulse-insts)) scheduled in parallel over multiple virtual hardware or simulator resources (see: [Channels](#pulse-channels)). The system also allows the user to recover the time dynamics of the measured output.

This is sufficient to allow the quantum physicist to explore and correct for noise in a quantum system.

<span id="module-qiskit.pulse.instructions" />

<span id="pulse-insts" />

## Instructions

<span id="module-qiskit.pulse" />

`qiskit.pulse.instructions`

The `instructions` module holds the various [`Instruction`](#qiskit.pulse.instructions.Instruction "qiskit.pulse.instructions.Instruction")s which are supported by Qiskit Pulse. Instructions have operands, which typically include at least one [`Channel`](#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel") specifying where the instruction will be applied.

Every instruction has a duration, whether explicitly included as an operand or implicitly defined. For instance, a [`ShiftPhase`](qiskit.pulse.instructions.ShiftPhase "qiskit.pulse.instructions.ShiftPhase") instruction can be instantiated with operands *phase* and *channel*, for some float `phase` and a [`Channel`](#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel") `channel`:

```python
ShiftPhase(phase, channel)
```

The duration of this instruction is implicitly zero. On the other hand, the [`Delay`](qiskit.pulse.instructions.Delay "qiskit.pulse.instructions.Delay") instruction takes an explicit duration:

```python
Delay(duration, channel)
```

An instruction can be added to a [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule"), which is a sequence of scheduled Pulse `Instruction` s over many channels. `Instruction` s and `Schedule` s implement the same interface.

|                                                                                                                                      |                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Acquire`](qiskit.pulse.instructions.Acquire "qiskit.pulse.instructions.Acquire")(duration, channel\[, mem\_slot, ...])             | The Acquire instruction is used to trigger the ADC associated with a particular qubit; e.g.                                                                                   |
| [`Call`](qiskit.pulse.instructions.Call "qiskit.pulse.instructions.Call")(subroutine\[, value\_dict, name])                          | Pulse `Call` instruction.                                                                                                                                                     |
| [`Reference`](qiskit.pulse.instructions.Reference "qiskit.pulse.instructions.Reference")(name, \*extra\_keys)                        | Pulse compiler directive that refers to a subroutine.                                                                                                                         |
| [`Delay`](qiskit.pulse.instructions.Delay "qiskit.pulse.instructions.Delay")(duration, channel\[, name])                             | A blocking instruction with no other effect.                                                                                                                                  |
| [`Play`](qiskit.pulse.instructions.Play "qiskit.pulse.instructions.Play")(pulse, channel\[, name])                                   | This instruction is responsible for applying a pulse on a channel.                                                                                                            |
| [`SetFrequency`](qiskit.pulse.instructions.SetFrequency "qiskit.pulse.instructions.SetFrequency")(frequency, channel\[, name])       | Set the channel frequency.                                                                                                                                                    |
| [`ShiftFrequency`](qiskit.pulse.instructions.ShiftFrequency "qiskit.pulse.instructions.ShiftFrequency")(frequency, channel\[, name]) | Shift the channel frequency away from the current frequency.                                                                                                                  |
| [`SetPhase`](qiskit.pulse.instructions.SetPhase "qiskit.pulse.instructions.SetPhase")(phase, channel\[, name])                       | The set phase instruction sets the phase of the proceeding pulses on that channel to `phase` radians.                                                                         |
| [`ShiftPhase`](qiskit.pulse.instructions.ShiftPhase "qiskit.pulse.instructions.ShiftPhase")(phase, channel\[, name])                 | The shift phase instruction updates the modulation phase of proceeding pulses played on the same [`Channel`](#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel"). |
| [`Snapshot`](qiskit.pulse.instructions.Snapshot "qiskit.pulse.instructions.Snapshot")(label\[, snapshot\_type, name])                | An instruction targeted for simulators, to capture a moment in the simulation.                                                                                                |

These are all instances of the same base class:

<span id="qiskit.pulse.instructions.Instruction" />

`Instruction(operands, name=None)`

The smallest schedulable unit: a single instruction. It has a fixed duration and specified channels.

Instruction initializer.

**Parameters**

*   **operands** (`Tuple`) – The argument list.
*   **name** (`Optional`\[`str`]) – Optional display name for this instruction.

<span id="module-qiskit.pulse.library" />

## Pulse Library

<span id="module-qiskit.pulse" />

`qiskit.pulse.library`

This library provides Pulse users with convenient methods to build Pulse waveforms.

A pulse programmer can choose from one of several [Pulse Models](#pulse-models) such as [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform") and [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse") to create a pulse program. The [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform") model directly stores the waveform data points in each class instance. This model provides the most flexibility to express arbitrary waveforms and allows a rapid prototyping of new control techniques. However, this model is typically memory inefficient and might be hard to scale to large-size quantum processors. Several waveform subclasses are defined by [Waveform Pulse Representation](#waveforms), but a user can also directly instantiate the [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform") class with `samples` argument which is usually a complex numpy array or any kind of array-like data.

In contrast, the [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse") model only stores the function and its parameters that generate the waveform in a class instance. It thus provides greater memory efficiency at the price of less flexibility in the waveform. This model also defines a small set of pulse subclasses in [Parametric Pulse Representation](#symbolic-pulses) which are commonly used in superconducting quantum processors. An instance of these subclasses can be serialized in the [QPY Format](qpy#qpy-format) while keeping the memory-efficient parametric representation of waveforms. Note that [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform") object can be generated from an instance of a [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse") which will set values for the parameters and sample the parametric expression to create the [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").

<Admonition title="Note" type="note">
  QPY serialization support for [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse") is currently not available. This feature will be implemented soon in Qiskit terra version 0.21.
</Admonition>

<span id="id1" />

### Pulse Models

|                                                                                                                           |                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform")(samples\[, name, epsilon, ...])               | A pulse specified completely by complex-valued samples; each sample is played for the duration of the backend cycle-time, dt. |
| [`SymbolicPulse`](qiskit.pulse.library.SymbolicPulse "qiskit.pulse.library.SymbolicPulse")(pulse\_type, duration\[, ...]) | The pulse representation model with parameters and symbolic expressions.                                                      |
| [`ParametricPulse`](qiskit.pulse.library.ParametricPulse "qiskit.pulse.library.ParametricPulse")(duration\[, name, ...])  | The abstract superclass for parametric pulses.                                                                                |

<span id="waveforms" />

### Waveform Pulse Representation

|                                                                                                                                                      |                                                                                                                                                            |
| ---------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`constant`](qiskit.pulse.library.constant#qiskit.pulse.library.constant "qiskit.pulse.library.constant")(duration, amp\[, name])                    | Generates constant-sampled [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                    |
| [`zero`](qiskit.pulse.library.zero "qiskit.pulse.library.zero")(duration\[, name])                                                                   | Generates zero-sampled [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                        |
| [`square`](qiskit.pulse.library.square "qiskit.pulse.library.square")(duration, amp\[, freq, phase, name])                                           | Generates square wave [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                         |
| [`sawtooth`](qiskit.pulse.library.sawtooth "qiskit.pulse.library.sawtooth")(duration, amp\[, freq, phase, name])                                     | Generates sawtooth wave [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                       |
| [`triangle`](qiskit.pulse.library.triangle "qiskit.pulse.library.triangle")(duration, amp\[, freq, phase, name])                                     | Generates triangle wave [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                       |
| [`cos`](qiskit.pulse.library.cos "qiskit.pulse.library.cos")(duration, amp\[, freq, phase, name])                                                    | Generates cosine wave [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                         |
| [`sin`](qiskit.pulse.library.sin "qiskit.pulse.library.sin")(duration, amp\[, freq, phase, name])                                                    | Generates sine wave [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                           |
| [`gaussian`](qiskit.pulse.library.gaussian#qiskit.pulse.library.gaussian "qiskit.pulse.library.gaussian")(duration, amp, sigma\[, name, zero\_ends]) | Generates unnormalized gaussian [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                               |
| [`gaussian_deriv`](qiskit.pulse.library.gaussian_deriv "qiskit.pulse.library.gaussian_deriv")(duration, amp, sigma\[, name])                         | Generates unnormalized gaussian derivative [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                    |
| [`sech`](qiskit.pulse.library.sech "qiskit.pulse.library.sech")(duration, amp, sigma\[, name, zero\_ends])                                           | Generates unnormalized sech [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                   |
| [`sech_deriv`](qiskit.pulse.library.sech_deriv "qiskit.pulse.library.sech_deriv")(duration, amp, sigma\[, name])                                     | Generates unnormalized sech derivative [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                        |
| [`gaussian_square`](qiskit.pulse.library.gaussian_square "qiskit.pulse.library.gaussian_square")(duration, amp, sigma\[, ...])                       | Generates gaussian square [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform").                                                     |
| [`drag`](qiskit.pulse.library.drag#qiskit.pulse.library.drag "qiskit.pulse.library.drag")(duration, amp, sigma, beta\[, name, ...])                  | Generates Y-only correction DRAG [`Waveform`](qiskit.pulse.library.Waveform "qiskit.pulse.library.Waveform") for standard nonlinear oscillator (SNO) \[1]. |

<span id="symbolic-pulses" />

### Parametric Pulse Representation

|                                                                                                                             |                                                                                                                                                          |
| --------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Constant`](qiskit.pulse.library.Constant "qiskit.pulse.library.Constant")(duration, amp\[, name, limit\_amplitude])       | A simple constant pulse, with an amplitude value and a duration:                                                                                         |
| [`Drag`](qiskit.pulse.library.Drag "qiskit.pulse.library.Drag")(duration, amp, sigma, beta\[, name, ...])                   | The Derivative Removal by Adiabatic Gate (DRAG) pulse is a standard Gaussian pulse with an additional Gaussian derivative component and lifting applied. |
| [`Gaussian`](qiskit.pulse.library.Gaussian "qiskit.pulse.library.Gaussian")(duration, amp, sigma\[, name, ...])             | A lifted and truncated pulse envelope shaped according to the Gaussian function whose mean is centered at the center of the pulse (duration / 2):        |
| [`GaussianSquare`](qiskit.pulse.library.GaussianSquare "qiskit.pulse.library.GaussianSquare")(duration, amp, sigma\[, ...]) | A square pulse with a Gaussian shaped risefall on both sides lifted such that its first sample is zero.                                                  |

<span id="module-qiskit.pulse.channels" />

<span id="pulse-channels" />

## Channels

<span id="module-qiskit.pulse" />

`qiskit.pulse.channels`

Pulse is meant to be agnostic to the underlying hardware implementation, while still allowing low-level control. Therefore, our signal channels are *virtual* hardware channels. The backend which executes our programs is responsible for mapping these virtual channels to the proper physical channel within the quantum control hardware.

Channels are characterized by their type and their index. Channels include:

*   transmit channels, which should subclass `PulseChannel`
*   receive channels, such as [`AcquireChannel`](qiskit.pulse.channels.AcquireChannel "qiskit.pulse.channels.AcquireChannel")
*   non-signal “channels” such as [`SnapshotChannel`](qiskit.pulse.channels.SnapshotChannel "qiskit.pulse.channels.SnapshotChannel"), [`MemorySlot`](qiskit.pulse.channels.MemorySlot "qiskit.pulse.channels.MemorySlot") and `RegisterChannel`.

Novel channel types can often utilize the [`ControlChannel`](qiskit.pulse.channels.ControlChannel "qiskit.pulse.channels.ControlChannel"), but if this is not sufficient, new channel types can be created. Then, they must be supported in the PulseQobj schema and the assembler. Channels are characterized by their type and their index. See each channel type below to learn more.

|                                                                                                                        |                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| [`DriveChannel`](qiskit.pulse.channels.DriveChannel "qiskit.pulse.channels.DriveChannel")(index)                       | Drive channels transmit signals to qubits which enact gate operations.                         |
| [`MeasureChannel`](qiskit.pulse.channels.MeasureChannel "qiskit.pulse.channels.MeasureChannel")(index)                 | Measure channels transmit measurement stimulus pulses for readout.                             |
| [`AcquireChannel`](qiskit.pulse.channels.AcquireChannel "qiskit.pulse.channels.AcquireChannel")(index)                 | Acquire channels are used to collect data.                                                     |
| [`ControlChannel`](qiskit.pulse.channels.ControlChannel "qiskit.pulse.channels.ControlChannel")(index)                 | Control channels provide supplementary control over the qubit to the drive channel.            |
| [`RegisterSlot`](qiskit.pulse.channels.RegisterSlot "qiskit.pulse.channels.RegisterSlot")(index)                       | Classical resister slot channels represent classical registers (low-latency classical memory). |
| [`MemorySlot`](qiskit.pulse.channels.MemorySlot "qiskit.pulse.channels.MemorySlot")(index)                             | Memory slot channels represent classical memory storage.                                       |
| [`SnapshotChannel`](qiskit.pulse.channels.SnapshotChannel "qiskit.pulse.channels.SnapshotChannel")(\*args, \*\*kwargs) | Snapshot channels are used to specify instructions for simulators.                             |

All channels are children of the same abstract base class:

<span id="qiskit.pulse.channels.Channel" />

`Channel(index)`

Base class of channels. Channels provide a Qiskit-side label for typical quantum control hardware signal channels. The final label -> physical channel mapping is the responsibility of the hardware backend. For instance, `DriveChannel(0)` holds instructions which the backend should map to the signal line driving gate operations on the qubit labeled (indexed) 0.

When serialized channels are identified by their serialized name `<prefix><index>`. The type of the channel is interpreted from the prefix, and the index often (but not always) maps to the qubit index. All concrete channel classes must have a `prefix` class attribute (and instances of that class have an index attribute). Base classes which have `prefix` set to `None` are prevented from being instantiated.

To implement a new channel inherit from [`Channel`](#qiskit.pulse.channels.Channel "qiskit.pulse.channels.Channel") and provide a unique string identifier for the `prefix` class attribute.

Channel class.

**Parameters**

**index** (`int`) – Index of channel.

And classical IO channels are children of:

<span id="qiskit.pulse.channels.ClassicalIOChannel" />

`ClassicalIOChannel(index)`

Base class of classical IO channels. These cannot have instructions scheduled on them.

Channel class.

**Parameters**

**index** (`int`) – Index of channel.

<span id="module-qiskit.pulse.schedule" />

## Schedules

Schedules are Pulse programs. They describe instruction sequences for the control hardware. The Schedule is one of the most fundamental objects to this pulse-level programming module. A `Schedule` is a representation of a *program* in Pulse. Each schedule tracks the time of each instruction occuring in parallel over multiple signal *channels*.

|                                                                                                    |                                                                                                                                                                         |
| -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule")(\*schedules\[, name, metadata])        | A quantum program *schedule* with exact time constraints for its instructions, operating over all input signal *channels* and supporting special syntaxes for building. |
| [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock")(\[name, metadata, ...]) | Time-ordered sequence of instructions with alignment context.                                                                                                           |

<span id="module-qiskit.pulse.transforms" />

## Pulse Transforms

<span id="module-qiskit.pulse" />

`qiskit.pulse.transforms`

The pulse transforms provide transformation routines to reallocate and optimize pulse programs for backends.

<span id="pulse-alignments" />

### Alignments

The alignment transforms define alignment policies of instructions in [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock"). These transformations are called to create [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule")s from [`ScheduleBlock`](qiskit.pulse.ScheduleBlock "qiskit.pulse.ScheduleBlock")s.

|                                                                                                                  |                                                                          |
| ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [`AlignEquispaced`](qiskit.pulse.transforms.AlignEquispaced "qiskit.pulse.transforms.AlignEquispaced")(duration) | Align instructions with equispaced interval within a specified duration. |
| [`AlignFunc`](qiskit.pulse.transforms.AlignFunc "qiskit.pulse.transforms.AlignFunc")(duration, func)             | Allocate instructions at position specified by callback function.        |
| [`AlignLeft`](qiskit.pulse.transforms.AlignLeft "qiskit.pulse.transforms.AlignLeft")()                           | Align instructions in as-soon-as-possible manner.                        |
| [`AlignRight`](qiskit.pulse.transforms.AlignRight "qiskit.pulse.transforms.AlignRight")()                        | Align instructions in as-late-as-possible manner.                        |
| [`AlignSequential`](qiskit.pulse.transforms.AlignSequential "qiskit.pulse.transforms.AlignSequential")()         | Align instructions sequentially.                                         |

These are all subtypes of the abstract base class [`AlignmentKind`](#qiskit.pulse.transforms.AlignmentKind "qiskit.pulse.transforms.AlignmentKind").

<span id="qiskit.pulse.transforms.AlignmentKind" />

`AlignmentKind(context_params)`

An abstract class for schedule alignment.

Create new context.

<span id="pulse-canonical-transform" />

### Canonicalization

The canonicalization transforms convert schedules to a form amenable for execution on OpenPulse backends.

|                                                                                                                                               |                                                                                                                        |
| --------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------- |
| [`add_implicit_acquires`](qiskit.pulse.transforms.add_implicit_acquires "qiskit.pulse.transforms.add_implicit_acquires")(schedule, meas\_map) | Return a new schedule with implicit acquires from the measurement mapping replaced by explicit ones.                   |
| [`align_measures`](qiskit.pulse.transforms.align_measures "qiskit.pulse.transforms.align_measures")(schedules\[, inst\_map, ...])             | Return new schedules where measurements occur at the same physical time.                                               |
| [`block_to_schedule`](qiskit.pulse.transforms.block_to_schedule "qiskit.pulse.transforms.block_to_schedule")(block)                           | Convert `ScheduleBlock` to `Schedule`.                                                                                 |
| [`compress_pulses`](qiskit.pulse.transforms.compress_pulses "qiskit.pulse.transforms.compress_pulses")(schedules)                             | Optimization pass to replace identical pulses.                                                                         |
| [`flatten`](qiskit.pulse.transforms.flatten "qiskit.pulse.transforms.flatten")(program)                                                       | Flatten (inline) any called nodes into a Schedule tree with no nested children.                                        |
| [`inline_subroutines`](qiskit.pulse.transforms.inline_subroutines "qiskit.pulse.transforms.inline_subroutines")(program)                      | Recursively remove call instructions and inline the respective subroutine instructions.                                |
| [`pad`](qiskit.pulse.transforms.pad "qiskit.pulse.transforms.pad")(schedule\[, channels, until, inplace])                                     | Pad the input Schedule with `Delay``s on all unoccupied timeslots until ``schedule.duration` or `until` if not `None`. |
| [`remove_directives`](qiskit.pulse.transforms.remove_directives "qiskit.pulse.transforms.remove_directives")(schedule)                        | Remove directives.                                                                                                     |
| [`remove_trivial_barriers`](qiskit.pulse.transforms.remove_trivial_barriers "qiskit.pulse.transforms.remove_trivial_barriers")(schedule)      | Remove trivial barriers with 0 or 1 channels.                                                                          |

<span id="pulse-dag" />

### DAG

The DAG transforms create DAG representation of input program. This can be used for optimization of instructions and equality checks.

|                                                                                                      |                                              |
| ---------------------------------------------------------------------------------------------------- | -------------------------------------------- |
| [`block_to_dag`](qiskit.pulse.transforms.block_to_dag "qiskit.pulse.transforms.block_to_dag")(block) | Convert schedule block instruction into DAG. |

<span id="pulse-transform-chain" />

### Composite transform

A sequence of transformations to generate a target code.

|                                                                                                                                                        |                                                                   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- |
| [`target_qobj_transform`](qiskit.pulse.transforms.target_qobj_transform "qiskit.pulse.transforms.target_qobj_transform")(sched\[, remove\_directives]) | A basic pulse program transformation for OpenPulse API execution. |

<span id="module-qiskit.pulse.builder" />

<span id="id2" />

## Pulse Builder

Use the pulse builder DSL to write pulse programs with an imperative syntax.

<Admonition title="Warning" type="caution">
  The pulse builder interface is still in active development. It may have breaking API changes without deprecation warnings in future releases until otherwise indicated.
</Admonition>

The pulse builder provides an imperative API for writing pulse programs with less difficulty than the [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule") API. It contextually constructs a pulse schedule and then emits the schedule for execution. For example, to play a series of pulses on channels is as simple as:

```python
from qiskit import pulse

dc = pulse.DriveChannel
d0, d1, d2, d3, d4 = dc(0), dc(1), dc(2), dc(3), dc(4)

with pulse.build(name='pulse_programming_in') as pulse_prog:
    pulse.play([1, 1, 1, 0, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 1, 1, 1], d0)
    pulse.play([1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0], d1)
    pulse.play([1, 0, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0], d2)
    pulse.play([1, 0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 1, 0], d3)
    pulse.play([1, 1, 1, 1, 0, 1, 1, 1, 0, 1, 1, 1, 0, 1, 0, 1, 0, 1, 1, 1, 0, 0, 1, 0], d4)

pulse_prog.draw()
```

![../\_images/pulse\_0\_0.png](/images/api/qiskit/0.39/pulse_0_0.png)

To begin pulse programming we must first initialize our program builder context with [`build()`](qiskit.pulse.builder.build "qiskit.pulse.builder.build"), after which we can begin adding program statements. For example, below we write a simple program that [`play()`](qiskit.pulse.builder.play "qiskit.pulse.builder.play")s a pulse:

```python
from qiskit import execute, pulse

d0 = pulse.DriveChannel(0)

with pulse.build() as pulse_prog:
    pulse.play(pulse.Constant(100, 1.0), d0)

pulse_prog.draw()
```

![../\_images/pulse\_1\_0.png](/images/api/qiskit/0.39/pulse_1_0.png)

The builder initializes a [`pulse.Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule"), `pulse_prog` and then begins to construct the program within the context. The output pulse schedule will survive after the context is exited and can be executed like a normal Qiskit schedule using `qiskit.execute(pulse_prog, backend)`.

Pulse programming has a simple imperative style. This leaves the programmer to worry about the raw experimental physics of pulse programming and not constructing cumbersome data structures.

We can optionally pass a [`Backend`](qiskit.providers.Backend "qiskit.providers.Backend") to [`build()`](qiskit.pulse.builder.build "qiskit.pulse.builder.build") to enable enhanced functionality. Below, we prepare a Bell state by automatically compiling the required pulses from their gate-level representations, while simultaneously applying a long decoupling pulse to a neighboring qubit. We terminate the experiment with a measurement to observe the state we prepared. This program which mixes circuits and pulses will be automatically lowered to be run as a pulse program:

```python
import math

from qiskit import pulse
from qiskit.providers.fake_provider import FakeOpenPulse3Q

# TODO: This example should use a real mock backend.
backend = FakeOpenPulse3Q()

d2 = pulse.DriveChannel(2)

with pulse.build(backend) as bell_prep:
    pulse.u2(0, math.pi, 0)
    pulse.cx(0, 1)

with pulse.build(backend) as decoupled_bell_prep_and_measure:
    # We call our bell state preparation schedule constructed above.
    with pulse.align_right():
        pulse.call(bell_prep)
        pulse.play(pulse.Constant(bell_prep.duration, 0.02), d2)
        pulse.barrier(0, 1, 2)
        registers = pulse.measure_all()

decoupled_bell_prep_and_measure.draw()
```

![../\_images/pulse\_2\_0.png](/images/api/qiskit/0.39/pulse_2_0.png)

With the pulse builder we are able to blend programming on qubits and channels. While the pulse schedule is based on instructions that operate on channels, the pulse builder automatically handles the mapping from qubits to channels for you.

In the example below we demonstrate some more features of the pulse builder:

```python
import math

from qiskit import pulse, QuantumCircuit
from qiskit.pulse import library
from qiskit.providers.fake_provider import FakeOpenPulse2Q

backend = FakeOpenPulse2Q()

with pulse.build(backend) as pulse_prog:
    # Create a pulse.
    gaussian_pulse = library.gaussian(10, 1.0, 2)
    # Get the qubit's corresponding drive channel from the backend.
    d0 = pulse.drive_channel(0)
    d1 = pulse.drive_channel(1)
    # Play a pulse at t=0.
    pulse.play(gaussian_pulse, d0)
    # Play another pulse directly after the previous pulse at t=10.
    pulse.play(gaussian_pulse, d0)
    # The default scheduling behavior is to schedule pulses in parallel
    # across channels. For example, the statement below
    # plays the same pulse on a different channel at t=0.
    pulse.play(gaussian_pulse, d1)

    # We also provide pulse scheduling alignment contexts.
    # The default alignment context is align_left.

    # The sequential context schedules pulse instructions sequentially in time.
    # This context starts at t=10 due to earlier pulses above.
    with pulse.align_sequential():
        pulse.play(gaussian_pulse, d0)
        # Play another pulse after at t=20.
        pulse.play(gaussian_pulse, d1)

        # We can also nest contexts as each instruction is
        # contained in its local scheduling context.
        # The output of a child context is a context-schedule
        # with the internal instructions timing fixed relative to
        # one another. This is schedule is then called in the parent context.

        # Context starts at t=30.
        with pulse.align_left():
            # Start at t=30.
            pulse.play(gaussian_pulse, d0)
            # Start at t=30.
            pulse.play(gaussian_pulse, d1)
        # Context ends at t=40.

        # Alignment context where all pulse instructions are
        # aligned to the right, ie., as late as possible.
        with pulse.align_right():
            # Shift the phase of a pulse channel.
            pulse.shift_phase(math.pi, d1)
            # Starts at t=40.
            pulse.delay(100, d0)
            # Ends at t=140.

            # Starts at t=130.
            pulse.play(gaussian_pulse, d1)
            # Ends at t=140.

        # Acquire data for a qubit and store in a memory slot.
        pulse.acquire(100, 0, pulse.MemorySlot(0))

        # We also support a variety of macros for common operations.

        # Measure all qubits.
        pulse.measure_all()

        # Delay on some qubits.
        # This requires knowledge of which channels belong to which qubits.
        # delay for 100 cycles on qubits 0 and 1.
        pulse.delay_qubits(100, 0, 1)

        # Call a quantum circuit. The pulse builder lazily constructs a quantum
        # circuit which is then transpiled and scheduled before inserting into
        # a pulse schedule.
        # NOTE: Quantum register indices correspond to physical qubit indices.
        qc = QuantumCircuit(2, 2)
        qc.cx(0, 1)
        pulse.call(qc)
        # Calling a small set of standard gates and decomposing to pulses is
        # also supported with more natural syntax.
        pulse.u3(0, math.pi, 0, 0)
        pulse.cx(0, 1)


        # It is also be possible to call a preexisting schedule
        tmp_sched = pulse.Schedule()
        tmp_sched += pulse.Play(gaussian_pulse, d0)
        pulse.call(tmp_sched)

        # We also support:

        # frequency instructions
        pulse.set_frequency(5.0e9, d0)

        # phase instructions
        pulse.shift_phase(0.1, d0)

        # offset contexts
        with pulse.phase_offset(math.pi, d0):
            pulse.play(gaussian_pulse, d0)
```

The above is just a small taste of what is possible with the builder. See the rest of the module documentation for more information on its capabilities.

|                                                                                                     |                                                                          |
| --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| [`build`](qiskit.pulse.builder.build "qiskit.pulse.builder.build")(\[backend, schedule, name, ...]) | Create a context manager for launching the imperative pulse builder DSL. |

### Channels

Methods to return the correct channels for the respective qubit indices.

```python
from qiskit import pulse
from qiskit.providers.fake_provider import FakeArmonk

backend = FakeArmonk()

with pulse.build(backend) as drive_sched:
    d0 = pulse.drive_channel(0)
    print(d0)
```

```python
DriveChannel(0)
```

|                                                                                                               |                                                                    |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------ |
| [`acquire_channel`](qiskit.pulse.builder.acquire_channel "qiskit.pulse.builder.acquire_channel")(qubit)       | Return `AcquireChannel` for `qubit` on the active builder backend. |
| [`control_channels`](qiskit.pulse.builder.control_channels "qiskit.pulse.builder.control_channels")(\*qubits) | Return `ControlChannel` for `qubit` on the active builder backend. |
| [`drive_channel`](qiskit.pulse.builder.drive_channel "qiskit.pulse.builder.drive_channel")(qubit)             | Return `DriveChannel` for `qubit` on the active builder backend.   |
| [`measure_channel`](qiskit.pulse.builder.measure_channel "qiskit.pulse.builder.measure_channel")(qubit)       | Return `MeasureChannel` for `qubit` on the active builder backend. |

### Instructions

Pulse instructions are available within the builder interface. Here’s an example:

```python
from qiskit import pulse
from qiskit.providers.fake_provider import FakeArmonk

backend = FakeArmonk()

with pulse.build(backend) as drive_sched:
    d0 = pulse.drive_channel(0)
    a0 = pulse.acquire_channel(0)

    pulse.play(pulse.library.Constant(10, 1.0), d0)
    pulse.delay(20, d0)
    pulse.shift_phase(3.14/2, d0)
    pulse.set_phase(3.14, d0)
    pulse.shift_frequency(1e7, d0)
    pulse.set_frequency(5e9, d0)

    with pulse.build() as temp_sched:
        pulse.play(pulse.library.Gaussian(20, 1.0, 3.0), d0)
        pulse.play(pulse.library.Gaussian(20, -1.0, 3.0), d0)

    pulse.call(temp_sched)
    pulse.acquire(30, a0, pulse.MemorySlot(0))

drive_sched.draw()
```

![../\_images/pulse\_5\_0.png](/images/api/qiskit/0.39/pulse_5_0.png)

|                                                                                                                               |                                                                                                                                         |
| ----------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| [`acquire`](qiskit.pulse.builder.acquire "qiskit.pulse.builder.acquire")(duration, qubit\_or\_channel, ...)                   | Acquire for a `duration` on a `channel` and store the result in a `register`.                                                           |
| [`barrier`](qiskit.pulse.builder.barrier "qiskit.pulse.builder.barrier")(\*channels\_or\_qubits\[, name])                     | Barrier directive for a set of channels and qubits.                                                                                     |
| [`call`](qiskit.pulse.builder.call "qiskit.pulse.builder.call")(target\[, name, value\_dict])                                 | Call the subroutine within the currently active builder context with arbitrary parameters which will be assigned to the target program. |
| [`delay`](qiskit.pulse.builder.delay "qiskit.pulse.builder.delay")(duration, channel\[, name])                                | Delay on a `channel` for a `duration`.                                                                                                  |
| [`play`](qiskit.pulse.builder.play "qiskit.pulse.builder.play")(pulse, channel\[, name])                                      | Play a `pulse` on a `channel`.                                                                                                          |
| [`reference`](qiskit.pulse.builder.reference "qiskit.pulse.builder.reference")(name, \*extra\_keys)                           | Refer to undefined subroutine by string keys.                                                                                           |
| [`set_frequency`](qiskit.pulse.builder.set_frequency "qiskit.pulse.builder.set_frequency")(frequency, channel\[, name])       | Set the `frequency` of a pulse `channel`.                                                                                               |
| [`set_phase`](qiskit.pulse.builder.set_phase "qiskit.pulse.builder.set_phase")(phase, channel\[, name])                       | Set the `phase` of a pulse `channel`.                                                                                                   |
| [`shift_frequency`](qiskit.pulse.builder.shift_frequency "qiskit.pulse.builder.shift_frequency")(frequency, channel\[, name]) | Shift the `frequency` of a pulse `channel`.                                                                                             |
| [`shift_phase`](qiskit.pulse.builder.shift_phase "qiskit.pulse.builder.shift_phase")(phase, channel\[, name])                 | Shift the `phase` of a pulse `channel`.                                                                                                 |
| [`snapshot`](qiskit.pulse.builder.snapshot "qiskit.pulse.builder.snapshot")(label\[, snapshot\_type])                         | Simulator snapshot.                                                                                                                     |

### Contexts

Builder aware contexts that modify the construction of a pulse program. For example an alignment context like [`align_right()`](qiskit.pulse.builder.align_right "qiskit.pulse.builder.align_right") may be used to align all pulses as late as possible in a pulse program.

```python
from qiskit import pulse

d0 = pulse.DriveChannel(0)
d1 = pulse.DriveChannel(1)

with pulse.build() as pulse_prog:
    with pulse.align_right():
        # this pulse will start at t=0
        pulse.play(pulse.Constant(100, 1.0), d0)
        # this pulse will start at t=80
        pulse.play(pulse.Constant(20, 1.0), d1)

pulse_prog.draw()
```

![../\_images/pulse\_6\_0.png](/images/api/qiskit/0.39/pulse_6_0.png)

|                                                                                                                                                 |                                                                                |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| [`align_equispaced`](qiskit.pulse.builder.align_equispaced "qiskit.pulse.builder.align_equispaced")(duration)                                   | Equispaced alignment pulse scheduling context.                                 |
| [`align_func`](qiskit.pulse.builder.align_func "qiskit.pulse.builder.align_func")(duration, func)                                               | Callback defined alignment pulse scheduling context.                           |
| [`align_left`](qiskit.pulse.builder.align_left "qiskit.pulse.builder.align_left")()                                                             | Left alignment pulse scheduling context.                                       |
| [`align_right`](qiskit.pulse.builder.align_right "qiskit.pulse.builder.align_right")()                                                          | Right alignment pulse scheduling context.                                      |
| [`align_sequential`](qiskit.pulse.builder.align_sequential "qiskit.pulse.builder.align_sequential")()                                           | Sequential alignment pulse scheduling context.                                 |
| [`circuit_scheduler_settings`](qiskit.pulse.builder.circuit_scheduler_settings "qiskit.pulse.builder.circuit_scheduler_settings")(\*\*settings) | Set the currently active circuit scheduler settings for this context.          |
| [`frequency_offset`](qiskit.pulse.builder.frequency_offset "qiskit.pulse.builder.frequency_offset")(frequency, \*channels\[, ...])              | Shift the frequency of inputs channels on entry into context and undo on exit. |
| [`phase_offset`](qiskit.pulse.builder.phase_offset "qiskit.pulse.builder.phase_offset")(phase, \*channels)                                      | Shift the phase of input channels on entry into context and undo on exit.      |
| [`transpiler_settings`](qiskit.pulse.builder.transpiler_settings "qiskit.pulse.builder.transpiler_settings")(\*\*settings)                      | Set the currently active transpiler settings for this context.                 |

### Macros

Macros help you add more complex functionality to your pulse program.

```python
from qiskit import pulse
from qiskit.providers.fake_provider import FakeArmonk

backend = FakeArmonk()

with pulse.build(backend) as measure_sched:
    mem_slot = pulse.measure(0)
    print(mem_slot)
```

```python
MemorySlot(0)
```

|                                                                                                             |                                                                                                         |
| ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| [`measure`](qiskit.pulse.builder.measure "qiskit.pulse.builder.measure")(qubits\[, registers])              | Measure a qubit within the currently active builder context.                                            |
| [`measure_all`](qiskit.pulse.builder.measure_all "qiskit.pulse.builder.measure_all")()                      | Measure all qubits within the currently active builder context.                                         |
| [`delay_qubits`](qiskit.pulse.builder.delay_qubits "qiskit.pulse.builder.delay_qubits")(duration, \*qubits) | Insert delays on all of the `channels.Channel`s that correspond to the input `qubits` at the same time. |

### Circuit Gates

To use circuit level gates within your pulse program call a circuit with [`call()`](qiskit.pulse.builder.call "qiskit.pulse.builder.call").

<Admonition title="Warning" type="caution">
  These will be removed in future versions with the release of a circuit builder interface in which it will be possible to calibrate a gate in terms of pulses and use that gate in a circuit.
</Admonition>

```python
import math

from qiskit import pulse
from qiskit.providers.fake_provider import FakeArmonk

backend = FakeArmonk()

with pulse.build(backend) as u3_sched:
    pulse.u3(math.pi, 0, math.pi, 0)
```

|                                                                                   |                                               |
| --------------------------------------------------------------------------------- | --------------------------------------------- |
| [`cx`](qiskit.pulse.builder.cx "qiskit.pulse.builder.cx")(control, target)        | Call a `CXGate` on the input physical qubits. |
| [`u1`](qiskit.pulse.builder.u1 "qiskit.pulse.builder.u1")(theta, qubit)           | Call a `U1Gate` on the input physical qubit.  |
| [`u2`](qiskit.pulse.builder.u2 "qiskit.pulse.builder.u2")(phi, lam, qubit)        | Call a `U2Gate` on the input physical qubit.  |
| [`u3`](qiskit.pulse.builder.u3 "qiskit.pulse.builder.u3")(theta, phi, lam, qubit) | Call a `U3Gate` on the input physical qubit.  |
| [`x`](qiskit.pulse.builder.x "qiskit.pulse.builder.x")(qubit)                     | Call a `XGate` on the input physical qubit.   |

### Utilities

The utility functions can be used to gather attributes about the backend and modify how the program is built.

```python
from qiskit import pulse

from qiskit.providers.fake_provider import FakeArmonk

backend = FakeArmonk()

with pulse.build(backend) as u3_sched:
    print('Number of qubits in backend: {}'.format(pulse.num_qubits()))

    samples = 160
    print('There are {} samples in {} seconds'.format(
        samples, pulse.samples_to_seconds(160)))

    seconds = 1e-6
    print('There are {} seconds in {} samples.'.format(
        seconds, pulse.seconds_to_samples(1e-6)))
```

```python
Number of qubits in backend: 1
There are 160 samples in 3.5555555555555554e-08 seconds
There are 1e-06 seconds in 4500 samples.
```

|                                                                                                                                                          |                                                                                                    |
| -------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| [`active_backend`](qiskit.pulse.builder.active_backend "qiskit.pulse.builder.active_backend")()                                                          | Get the backend of the currently active builder context.                                           |
| [`active_transpiler_settings`](qiskit.pulse.builder.active_transpiler_settings "qiskit.pulse.builder.active_transpiler_settings")()                      | Return the current active builder context's transpiler settings.                                   |
| [`active_circuit_scheduler_settings`](qiskit.pulse.builder.active_circuit_scheduler_settings "qiskit.pulse.builder.active_circuit_scheduler_settings")() | Return the current active builder context's circuit scheduler settings.                            |
| [`num_qubits`](qiskit.pulse.builder.num_qubits "qiskit.pulse.builder.num_qubits")()                                                                      | Return number of qubits in the currently active backend.                                           |
| [`qubit_channels`](qiskit.pulse.builder.qubit_channels "qiskit.pulse.builder.qubit_channels")(qubit)                                                     | Returns the set of channels associated with a qubit.                                               |
| [`samples_to_seconds`](qiskit.pulse.builder.samples_to_seconds "qiskit.pulse.builder.samples_to_seconds")(samples)                                       | Obtain the time in seconds that will elapse for the input number of samples on the active backend. |
| [`seconds_to_samples`](qiskit.pulse.builder.seconds_to_samples "qiskit.pulse.builder.seconds_to_samples")(seconds)                                       | Obtain the number of samples that will elapse in `seconds` on the active backend.                  |

## Configuration

|                                                                                                         |                                                                                                                                                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`InstructionScheduleMap`](qiskit.pulse.InstructionScheduleMap "qiskit.pulse.InstructionScheduleMap")() | Mapping from [`QuantumCircuit`](qiskit.circuit.QuantumCircuit "qiskit.circuit.QuantumCircuit") [`qiskit.circuit.Instruction`](qiskit.circuit.Instruction "qiskit.circuit.Instruction") names and qubits to [`Schedule`](qiskit.pulse.Schedule "qiskit.pulse.Schedule") s. In particular, the mapping is formatted as type::. |

## Exceptions

<span id="qiskit.pulse.PulseError" />

`PulseError(*message)`

Errors raised by the pulse module.

Set the error message.
