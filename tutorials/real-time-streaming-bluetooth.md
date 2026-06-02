# Bluetooth Real-Time Data Streaming

This tutorial explains how to read real-time EEG data from **NeuraDock EEG Workstation** through the Bluetooth workflow.


## Overview

NeuraDock EEG Workstation supports real-time EEG data streaming through Bluetooth.

The real-time data stream can be understood as a continuous flow of data. Instead of loading a complete file all at once, the program receives EEG data continuously as it arrives.

In the existing NeuraDock workflow, developers can open the real-time data stream API port from the NeuraDock software side. The host-side Python program then reads the streamed EEG data in real time.

This workflow is useful for:

- Real-time EEG visualization
- Online signal monitoring
- Neurofeedback prototypes
- Interactive EEG applications
- Real-time BCI experiment development
- Online feature extraction and model inference

## Related Code

The original tutorial notebook is available here:

- [3.online_data_stream_bluetooth.ipynb](https://github.com/Neuradock/eeg-workstation-python/blob/main/examples/3.online_data_stream_bluetooth.ipynb)

## Real-Time Streaming Concept

A real-time EEG stream is different from reading a saved file.

When reading a saved file, the full dataset already exists.

When reading a real-time stream, the program receives data continuously. The stream may arrive in chunks, and the receiving program must decide how to buffer, parse, and group incoming data.

A typical Bluetooth real-time workflow is:

1. Connect NeuraDock EEG Workstation through Bluetooth.
2. Open or enable the real-time data stream API from the NeuraDock software workflow.
3. Configure the host-side Python script with the correct IP and port.
4. Receive incoming data chunks.
5. Buffer incomplete data.
6. Split complete rows.
7. Parse each row into EEG samples.
8. Group samples into batches.
9. Send each batch to visualization, analysis, or feedback logic.

## Core Challenges

The existing tutorial highlights two core challenges in the Bluetooth real-time stream workflow.

## 1. TCP Packet Splitting and Concatenation

Network transmission does not always arrive in complete logical rows.

Sometimes the program may receive:

- Half of one row
- One and a half rows
- Multiple rows at once
- A partial row at the end of a received chunk

Therefore, the reader should not assume that each received chunk is exactly one complete EEG row.

The solution used in the existing tutorial is to maintain a buffer.

The basic idea is:

1. Receive a chunk from the socket.
2. Add the chunk to the existing buffer.
3. Split the buffer by newline characters.
4. Process only complete lines.
5. Keep the last incomplete line in the buffer for the next receive cycle.

Conceptually:

```text
Receive new chunk
Append chunk to buffer
Split buffer by newline
Process complete lines
Keep the last incomplete line for the next round
```

In the existing tutorial, the logic is described using this pattern:

```python
lines = self.buffer_str.split('\n')

# lines[:-1] are complete lines that can be processed
complete_lines = lines[:-1]

# lines[-1] may be an incomplete line, so keep it in the buffer
self.buffer_str = lines[-1]
```

This ensures that the program does not process broken or incomplete data rows.

## 2. Real-Time Yielding and Batch Size Control

Real-time algorithms cannot always process every single sample immediately.

If the program processes one sample at a time, it may be too slow.

If the program waits for too much data before processing, the delay may become too high.

The existing tutorial uses a variable such as:

```text
data_group_len
```

to control how many data points are grouped before being passed to the downstream algorithm.

For example, a workflow may group a small batch of data points and then immediately send that batch to the algorithm.

This approach balances:

- Processing speed
- Real-time responsiveness
- Algorithm input size
- System stability
- Latency

## DataStream Class

The existing tutorial wraps the real-time Bluetooth stream workflow into a `DataStream` class.

The class is intended to make the stream easy to use, similar to turning on a faucet and receiving data continuously.

The main responsibilities of the class include:

- Connecting to the real-time stream endpoint
- Receiving incoming socket data
- Buffering incomplete rows
- Splitting complete lines
- Parsing EEG values
- Grouping samples into batches
- Yielding each batch to downstream logic

## Initialization and Connection

In the existing tutorial, the initialization and connection logic defines:

- IP address
- Port
- Data shape
- Number of packet groups
- Number of total channels
- Number of used EEG channels

The tutorial notes the following values:

```text
pkg_groups = 5
total_channels = 8
used_channels = 7
```

This means:

- One Bluetooth data row contains 5 time points.
- The hardware or raw stream provides 8 values per time point.
- The workflow uses the first 7 EEG channels.

The default 7 EEG channels are:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

## IP and Port

The IP and port values should match the settings used by the NeuraDock software-side stream or forwarding workflow.

The existing tutorial notes that the software-side workflow is responsible for connecting to Bluetooth and forwarding the data through a TCP-style stream.

The Python script should use the same IP and port as the software-side configuration.

To be confirmed before public release:

- Exact IP configuration
- Exact port configuration
- Whether the port is fixed or user-configurable
- Whether a separate receiver or forwarding script is required
- Official software-side setup steps

## Core Buffering Logic

The core logic is inside the receiving loop.

The existing tutorial describes a loop that repeatedly receives socket data:

```python
chunk = self.socket.recv(...)
```

Because each received chunk may contain incomplete rows, the stream reader should use the buffer strategy described above.

The logic can be summarized as:

```text
while streaming:
    receive chunk
    append chunk to buffer
    split buffer by newline
    process complete lines
    keep incomplete final line
    parse complete rows
    group parsed samples
    yield data group
```

## Grouped Data Output

The Bluetooth real-time stream should be parsed into the default 7-channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

A recommended parsed output structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

If timestamp or marker information is not available in the stream, those fields can be omitted or generated by the receiving program.

## Using the Stream

The existing tutorial describes the stream as something the user can use simply, without needing to manage all low-level socket details directly.

A typical usage pattern is:

```python
stream = DataStream(...)
for data_group in stream:
    # process data_group
    pass
```

The tutorial also notes that this loop is blocking.

This means that if no new data arrives, the program may wait at this point instead of continuing to execute later code.

This behavior is expected in a real-time stream workflow because the downstream algorithm needs data before it can continue.

## Blocking Behavior

The following pattern waits for incoming data:

```python
for data_group in stream:
    ...
```

This is useful for real-time workflows, but developers should understand that it blocks the current execution flow.

If the application needs to perform other tasks at the same time, the stream reader may need to run in a separate thread, process, or asynchronous workflow.

The exact recommended implementation should be confirmed in the official Python examples.

## Recommended Python Workflow

A typical Python workflow for Bluetooth real-time streaming is:

1. Configure the IP and port.
2. Initialize the `DataStream` object.
3. Connect to the stream.
4. Receive chunks from the socket.
5. Use a buffer to reconstruct complete rows.
6. Parse complete rows into EEG samples.
7. Group samples according to `data_group_len`.
8. Yield each `data_group`.
9. Send each `data_group` to plotting, feature extraction, model inference, or feedback logic.

Conceptually:

```text
Initialize stream
Connect to IP and port
Receive chunk
Append to buffer
Split by newline
Process complete lines
Keep incomplete line
Parse EEG samples
Group samples
Yield data_group
```

## Difference from USB Streaming

| Item | USB Real-Time Streaming | Bluetooth Real-Time Streaming |
|---|---|---|
| Row structure | One row contains one data block | One row may contain 5 time points |
| Main parsing challenge | Simpler row parsing | Buffering, packet splitting, and grouped sample parsing |
| `pkg_groups` | Not the main concern in the USB tutorial | Existing tutorial notes `pkg_groups = 5` |
| `used_channels` | 7 EEG channels | 7 EEG channels |
| Final channel layout | O1, O2, Oz, PO3, PO4, CP5, CP6 | O1, O2, Oz, PO3, PO4, CP5, CP6 |
| Typical use case | Stable real-time USB workflow | Wireless real-time Bluetooth workflow |

## Use Cases

Bluetooth real-time streaming can be used as a standard template for:

- Real-time plotting
- Online neurofeedback
- Real-time EEG monitoring
- Wireless EEG experiment workflows
- Interactive EEG systems
- Real-time feature extraction
- BCI prototype development

## Notes for Developers

When implementing or modifying the Bluetooth real-time streaming workflow, keep the following points in mind:

- Do not assume that each socket receive call returns one complete row.
- Use a buffer to handle incomplete rows.
- Split buffered data by newline characters only after appending new chunks.
- Process only complete lines.
- Keep the final incomplete line for the next receive cycle.
- One Bluetooth row may contain 5 time points.
- The existing workflow uses 7 EEG channels out of 8 raw values.
- Preserve the default channel order.
- Choose `data_group_len` according to latency and algorithm requirements.
- Be aware that `for data_group in stream` is blocking.
- Document whether timestamps are device-side, software-side, or host-side.

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Read Bluetooth Text Files](./read-txt-bluetooth.md) | Read and parse EEG text files recorded from the Bluetooth workflow |
| [USB Real-Time Data Streaming](./real-time-streaming-usb.md) | Read real-time EEG data through the USB workflow |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
