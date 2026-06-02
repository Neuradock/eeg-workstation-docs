# Read USB Text Files

This tutorial explains how to read EEG text files exported from the **USB workflow** of NeuraDock EEG Workstation and convert them into a Python-readable data structure.

## Overview

NeuraDock Recording Software can save recorded EEG data as `.txt` files.

The USB text file format is simpler than the Bluetooth text file format:

- In the Bluetooth workflow, one line may contain data from 5 sampling moments.
- In the USB workflow, one line contains data from 1 sampling moment.

This means that USB text files are generally easier to parse because each line corresponds to one moment of EEG data.

## Related Code

The original tutorial notebook is available here:

- [2.text_file_read_usb_version.ipynb](https://github.com/Neuradock/eeg-workstation-python/blob/main/examples/2.text_file_read_usb_version.ipynb)

## Data Structure

In the USB workflow, each line of the text file represents the current micro-state of the brain at one sampling moment.

A line of USB data can be understood as:

```text
Header information + EEG data + auxiliary value
```

Based on the existing tutorial, the structure can be described as:

```text
One row of data = Header information + 7-channel EEG data + auxiliary value
```

The 7 EEG channels follow the default NeuraDock electrode layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

## Why USB Text Files Are Easier to Parse

Compared with Bluetooth text files, USB text files do not need to unpack multiple sampling moments from one row.

In the Bluetooth workflow, one row may contain multiple data blocks, so the parser needs to split and reorganize the row into multiple samples.

In the USB workflow, each row corresponds to one sampling moment. Therefore, the parsing logic can be much simpler:

1. Read each line from the text file.
2. Extract the header information.
3. Extract the 7 EEG channel values.
4. Extract the auxiliary value.
5. Convert the result into a Python-readable structure.

## Notes for Developers

When implementing or modifying a USB text file reader, keep the following points in mind:

- Each USB row should represent one sampling moment.
- The parser should preserve the original channel order.
- The parser should clearly separate header information, EEG values, and auxiliary values.
- The parser should avoid overwriting the original raw `.txt` file.
- The parser should output a clean structure for later analysis.
- The parser should document whether the values are raw values, converted voltage values, or preprocessed values.

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
