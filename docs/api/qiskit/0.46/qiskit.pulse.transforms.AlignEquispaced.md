---
title: AlignEquispaced
description: API reference for qiskit.pulse.transforms.AlignEquispaced
in_page_toc_min_heading_level: 1
python_api_type: class
python_api_name: qiskit.pulse.transforms.AlignEquispaced
---

# AlignEquispaced

<span id="qiskit.pulse.transforms.AlignEquispaced" />

`qiskit.pulse.transforms.AlignEquispaced(duration)`[GitHub](https://github.com/qiskit/qiskit/tree/stable/0.46/qiskit/pulse/transforms/alignments.py "view source code")

Bases: [`AlignmentKind`](pulse#qiskit.pulse.transforms.AlignmentKind "qiskit.pulse.transforms.alignments.AlignmentKind")

Align instructions with equispaced interval within a specified duration.

Instructions played on different channels are also arranged in a sequence. This alignment is convenient to create dynamical decoupling sequences such as PDD.

Create new equispaced context.

**Parameters**

**duration** ([*int*](https://docs.python.org/3/library/functions.html#int "(in Python v3.12)")  *|*[*ParameterExpression*](qiskit.circuit.ParameterExpression "qiskit.circuit.parameterexpression.ParameterExpression")) – Duration of this context. This should be larger than the schedule duration. If the specified duration is shorter than the schedule duration, no alignment is performed and the input schedule is just returned. This duration can be parametrized.

## Attributes

<span id="qiskit.pulse.transforms.AlignEquispaced.duration" />

### duration

Return context duration.

<span id="qiskit.pulse.transforms.AlignEquispaced.is_sequential" />

### is\_sequential

## Methods

### align

<span id="qiskit.pulse.transforms.AlignEquispaced.align" />

`align(schedule)`

Reallocate instructions according to the policy.

Only top-level sub-schedules are aligned. If sub-schedules are nested, nested schedules are not recursively aligned.

**Parameters**

**schedule** ([*Schedule*](qiskit.pulse.Schedule "qiskit.pulse.schedule.Schedule")) – Schedule to align.

**Returns**

Schedule with reallocated instructions.

**Return type**

[*Schedule*](qiskit.pulse.Schedule "qiskit.pulse.schedule.Schedule")
