# Real-Time Marker Workflow

This tutorial explains a real-time marker workflow for **NeuraDock EEG Workstation**.

## Overview

In BCI experiment system development, the system architecture often needs to handle two high-priority tasks at the same time:

- High-frequency EEG data streaming
- High-precision stimulus rendering or experiment control

A traditional single-threaded blocking architecture can easily lead to data backlog, delayed rendering, or data loss.

The existing NeuraDock tutorial introduces an asynchronous data management approach based on Python `threading` and `queue.Queue`.

The core idea is to use an `EEGThreadManager` class:

- A background thread continuously receives EEG data.
- The main experiment thread controls stimulus logic and updates the current trigger state.
- The background thread injects the current trigger state into each EEG data packet.
- The processed data is passed through a thread-safe queue.

This decouples the EEG data stream from the experiment logic while keeping the implementation simple.

## Related Code

The existing tutorial material refers to a real-time marker workflow implemented with a Python manager class.

The current code file appears to be:

```text
7.neuradock_marker.py
```

A cleaned and official version should later be maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

Related Python tools should later be maintained in:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## Problem

In an EEG experiment, the system may need to do both of the following:

1. Continuously receive EEG data.
2. Render stimuli and update experiment states.

If both tasks are handled in one blocking loop, several problems may occur:

- EEG data receiving may block stimulus rendering.
- Stimulus rendering may delay EEG data receiving.
- TCP/IP parsing may interfere with the UI or experiment loop.
- Data may accumulate faster than the program can process it.
- Trigger timing may become difficult to align with the EEG stream.

## Design Goal

The goal of the real-time marker workflow is to:

- Keep EEG data receiving stable.
- Avoid blocking the main experiment loop.
- Preserve trigger information together with EEG data.
- Reduce timestamp synchronization complexity.
- Allow stimulus logic to update markers with minimal code changes.
- Provide a thread-safe data path for downstream analysis or logging.

## Core Architecture

The existing tutorial describes the architecture as a non-blocking producer-consumer model.

The system includes:

| Component | Role |
|---|---|
| Background EEG thread | Continuously receives and processes EEG data |
| Main experiment thread | Runs stimulus logic and updates trigger state |
| `current_trigger` | Stores the current experiment marker state |
| `queue.Queue` | Transfers processed EEG data safely between threads |
| Data consumer | Reads processed EEG data for storage, plotting, or analysis |

## Producer-Consumer Model

The `EEGThreadManager` class is designed as a producer-consumer model.

It addresses two engineering problems:

### 1. I/O Isolation

EEG data receiving and TCP/IP packet parsing should not block the main experiment or rendering loop.

By running the EEG stream in a background thread, the main thread can focus on experiment logic and stimulus presentation.

### 2. Synchronization

The trigger marker should be associated with the EEG data as directly as possible.

Instead of using a separate timestamp log and aligning it later, the workflow injects the current trigger value into the EEG data packet during background processing.

This reduces the risk of cross-thread timestamp mismatch.

## State Injection Mechanism

The existing tutorial uses a state injection mechanism.

The main thread maintains a global or shared trigger state:

```text
current_trigger
```

Whenever the experiment state changes, the main thread updates `current_trigger`.

The background thread reads the current value of `current_trigger` when each EEG data group is received, then appends the trigger value to the EEG data as an additional column.

In other words:

```text
EEG data group + current trigger state = marked EEG data group
```

## Trigger State Control

The main experiment thread updates the trigger state through a method such as:

```python
def set_trigger(self, code):
    self.current_trigger = int(code)
```

Example usage:

```python
eeg_mgr.set_trigger(1)  # Stimulus A starts
eeg_mgr.set_trigger(0)  # Inter-trial interval
eeg_mgr.set_trigger(2)  # Stimulus B starts
```

The exact trigger codes should be defined by the experiment design.

For example:

| Trigger Code | Meaning |
|---|---|
| 0 | No active stimulus or inter-trial interval |
| 1 | Stimulus A |
| 2 | Stimulus B |

The trigger code definitions should be documented for each experiment.

## Background Worker Thread

The background worker thread is responsible for receiving EEG data and adding the current trigger state.

The existing tutorial describes a worker loop similar to:

```python
while not self.stop_event.is_set():
    data_group = blocking_read()

    if data_group is None:
        continue

    arr = np.array(data_group)

    marker = np.full(
        (arr.shape[0], 1),
        self.current_trigger
    )

    merged_data = np.concatenate([arr, marker], axis=1)

    self.data_queue.put(merged_data)
```

This means that every row in the received EEG data group receives the same current trigger value.

## Resulting Data Structure

The original EEG data group may contain only EEG channels.

After marker injection, the output contains:

```text
EEG channels + trigger marker column
```

For NeuraDock's default 7-channel layout, the marked data can be organized as:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

If timestamp or sample index information is available, the parsed output can be extended to:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

## Thread-Safe Queue

The workflow uses a thread-safe `queue.Queue` to pass processed data from the background EEG thread to the main program or data logger.

This helps separate:

- Data receiving
- Data marking
- Data storage
- Data visualization
- Downstream analysis

Conceptually:

```text
EEG stream
    ↓
Background EEG thread
    ↓
Add current trigger
    ↓
queue.Queue
    ↓
Data consumer / logger / visualizer
```

## Example Usage

A typical usage workflow is:

```python
eeg_mgr = EEGThreadManager()
eeg_mgr.start_stream()

# Experiment loop
eeg_mgr.set_trigger(1)  # Stimulus A starts

# Run stimulus A logic here

eeg_mgr.set_trigger(0)  # Inter-trial interval

# Run rest or blank screen logic here

eeg_mgr.set_trigger(2)  # Stimulus B starts

# Run stimulus B logic here
```

This approach allows the experiment code to remain simple.

The main thread does not need to manually handle low-level EEG receiving during every stimulus frame.

## Recommended Experiment Workflow

A typical real-time marker experiment workflow is:

1. Start NeuraDock EEG data streaming.
2. Initialize `EEGThreadManager`.
3. Start the background EEG stream thread.
4. Define trigger codes for each experiment condition.
5. Run the experiment or stimulus loop in the main thread.
6. Call `set_trigger(code)` whenever the experiment state changes.
7. Let the background worker inject the current trigger into incoming EEG data.
8. Store or consume the marked EEG data from the queue.
9. Stop the stream safely after the experiment.

## Why This Approach Is Useful

This workflow is useful because it:

- Avoids blocking the stimulus loop with EEG stream parsing.
- Keeps EEG data receiving active in the background.
- Reduces the need for separate timestamp log alignment.
- Keeps marker information attached to the EEG data matrix.
- Requires only minimal changes in the experiment logic.
- Supports real-time experiment and BCI prototype development.

## Timing Considerations

This method attaches the current trigger value to the EEG data group when the background thread processes incoming data.

This is simple and useful for many prototyping workflows.

However, users should understand that timing precision depends on:

- EEG stream timing
- Host computer scheduling
- Thread scheduling
- Buffer size
- Data group length
- When the main thread updates `current_trigger`
- When the background thread reads `current_trigger`

For strict timing experiments, marker timing should be validated carefully.

## Use Cases

The real-time marker workflow can be used for:

- SSVEP experiments
- cVEP experiments
- P300-style prototypes
- Online neurofeedback
- Real-time EEG visualization with event labels
- Stimulus-response experiments
- Interactive media systems
- BCI experiment prototyping

## Notes for Developers

When implementing or modifying the real-time marker workflow, keep the following points in mind:

- Run EEG data receiving in a background thread.
- Use a thread-safe queue for data transfer.
- Keep the main experiment thread focused on stimulus logic.
- Define trigger codes clearly before running the experiment.
- Set the trigger state whenever the experiment state changes.
- Append the trigger state to the EEG data as an additional column.
- Keep raw data and marked data clearly separated.
- Document the marker code definitions.
- Document whether marker values are injected per sample or per data group.
- Validate timing if the experiment requires precise synchronization.

## To Be Confirmed

The following technical details should be confirmed before this tutorial is finalized for public release:

| Item | Status |
|---|---|
| Official cleaned script location | To be confirmed |
| Exact class name and API | To be confirmed |
| Exact stream input format | To be confirmed |
| Exact output data structure | To be confirmed |
| Whether marker is injected per sample or per data group | To be confirmed |
| Whether timestamp is device-side, software-side, or host-side | To be confirmed |
| Recommended queue size, if any | To be confirmed |
| Recommended stop / cleanup behavior | To be confirmed |
| Official example trigger code definitions | To be confirmed |
| Whether this workflow supports USB, Bluetooth, or both | To be confirmed |

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [USB Real-Time Data Streaming](./real-time-streaming-usb.md) | Read real-time EEG data through the USB workflow |
| [Bluetooth Real-Time Data Streaming](./real-time-streaming-bluetooth.md) | Read real-time EEG data through the Bluetooth workflow |
| [Signal Quality Check](./signal-quality.md) | Evaluate basic EEG signal quality from recorded NeuraDock data |
| [SSVEP Workflow](./ssvep.md) | Run visual stimulation and offline SSVEP analysis workflows |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
