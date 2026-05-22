# Read Bluetooth Text Files

This tutorial explains how to read EEG text files exported from the **Bluetooth workflow** of NeuraDock EEG Workstation and convert them into a Python-readable data structure.


## Overview

NeuraDock Recording Software can save recorded EEG data as `.txt` files.

In the Bluetooth workflow, data is grouped to reduce bandwidth usage. Instead of storing one sampling moment per line, one line may contain multiple sampling moments.

Based on the existing tutorial:

```text
One Bluetooth row = 2 header fields + 5 x (7 EEG channels + 1 auxiliary value)
```

This means that each Bluetooth row contains:

- 2 header fields
- 5 sampling blocks
- Each sampling block contains 7 EEG channel values
- Each sampling block also contains 1 auxiliary value

The parser needs to unpack each row and convert it into one row per sampling moment.

## Related Code

The original tutorial notebook is available here:

- [1.text_file_read_bluetooth_version.ipynb](https://github.com/JunwenLuo/NeuraDock-Tutorials/blob/main/1.text_file_read_bluetooth_version.ipynb)

A cleaned and official version should later be maintained in:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## Data Structure

In the Bluetooth workflow, each line can be understood as a packet-like row.

The structure is:

```text
Header information + Payload
```

The header contains 2 fields.

According to the existing tutorial, these two fields represent:

- Timestamp
- Packet sequence information

The payload contains 5 sampling blocks.

Each sampling block contains:

```text
7 EEG channel values + 1 auxiliary value
```

Therefore, the full row can be described as:

```text
2 header fields + 5 x (7 EEG channels + 1 auxiliary value)
```

## Channel Layout

The 7 EEG channels follow the default NeuraDock electrode layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

Each sampling block should be unpacked into this channel order.

## Why Bluetooth Parsing Is Different from USB Parsing

The Bluetooth text file format is more compact than the USB text file format.

In the USB workflow:

```text
One row = 1 sampling moment
```

In the Bluetooth workflow:

```text
One row = 5 sampling moments
```

This means that the Bluetooth parser needs one extra step:

1. Read one row from the text file.
2. Extract the 2 header fields.
3. Split the payload into 5 sampling blocks.
4. For each block, extract 7 EEG channel values and 1 auxiliary value.
5. Convert the 5 blocks into 5 separate parsed samples.

## Block Structure

Based on the existing tutorial, each data block has a stride of 8 values:

```text
block_stride = 8
```

Each block contains:

```text
7 EEG channel values + 1 auxiliary value
```

Because one Bluetooth row contains 5 blocks, the parser needs to loop through 5 blocks:

```text
number_of_blocks = 5
```

The parsing logic can be summarized as:

```text
For each row:
    Read 2 header fields
    For each of the 5 blocks:
        Extract 7 EEG channel values
        Extract 1 auxiliary value
        Save as one parsed sample
```

## Example Parsed Output

A parsed Bluetooth row should be expanded into multiple samples.

For example, one raw Bluetooth line contains 5 sampling blocks:

```text
Raw Bluetooth row
├── Header field 1
├── Header field 2
├── Block 1: 7 EEG values + 1 auxiliary value
├── Block 2: 7 EEG values + 1 auxiliary value
├── Block 3: 7 EEG values + 1 auxiliary value
├── Block 4: 7 EEG values + 1 auxiliary value
└── Block 5: 7 EEG values + 1 auxiliary value
```

After parsing, this should become 5 data samples:

```text
Parsed sample 1
Parsed sample 2
Parsed sample 3
Parsed sample 4
Parsed sample 5
```

Each parsed sample should contain the 7 EEG channels in the default order.

## Comparison with USB Text Files

| Item | USB Workflow | Bluetooth Workflow |
|---|---|---|
| Row structure | One row contains one sampling moment | One row may contain 5 sampling moments |
| Parsing complexity | Simpler | Requires unpacking grouped blocks |
| Main parsing step | Extract one sample per row | Split each row into multiple samples |
| Channel layout after parsing | O1, O2, Oz, PO3, PO4, CP5, CP6 | O1, O2, Oz, PO3, PO4, CP5, CP6 |
| Recommended final format | One parsed row per sample | One parsed row per sample |

## Notes for Developers

When implementing or modifying a Bluetooth text file reader, keep the following points in mind:

- One Bluetooth row may contain multiple sampling moments.
- The parser should preserve the original sample order.
- The parser should split each row into 5 sampling blocks.
- Each block should be parsed as 7 EEG channel values plus 1 auxiliary value.
- The final parsed output should use the same channel order as the USB workflow.
- The parser should clearly separate header fields, EEG values, and auxiliary values.
- The parser should avoid overwriting the original raw `.txt` file.
- The parser should document whether the values are raw values, converted voltage values, or preprocessed values.

## To Be Confirmed

The following technical details should be confirmed before this tutorial is finalized for public release:

| Item | Status |
|---|---|
| Exact Bluetooth text file delimiter | To be confirmed |
| Exact timestamp field definition | To be confirmed |
| Exact packet sequence field definition | To be confirmed |
| Exact auxiliary value meaning | To be confirmed |
| Whether every Bluetooth row always contains 5 sampling blocks | To be confirmed |
| Whether marker information is included in the exported file | To be confirmed |
| Raw value unit or conversion rule | To be confirmed |
| Official cleaned notebook location | To be confirmed |

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Read USB Text Files](./read-txt-usb.md) | Read and parse EEG text files recorded from the USB workflow |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
