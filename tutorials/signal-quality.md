# Signal Quality Check

This tutorial explains how to evaluate basic EEG signal quality using recorded data from **NeuraDock EEG Workstation**.

## Overview

To help users evaluate NeuraDock before purchase or before running their own experiments, NeuraDock provides a technical evaluation package.

The package includes:

- A real EEG dataset collected with a 7-channel dry-electrode NeuraDock device
- Matching Python analysis code
- A notebook for inspecting signal quality and basic EEG data structure

Users do not need NeuraDock hardware to run this evaluation workflow. They can download the code and data, run the notebook locally, and inspect the data quality directly.

## Related Code

The original tutorial notebook is available here:

- [9.signal_quality_check.ipynb](https://github.com/JunwenLuo/NeuraDock-Tutorials/blob/main/9.signal_quality_check.ipynb)

A cleaned and official version should later be maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)
- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## What This Tutorial Helps Users Evaluate

This workflow is intended to help users inspect and understand:

- Raw EEG data quality
- Basic data structure
- Data preprocessing workflow
- Resting-state EEG data
- Task-state EEG data
- Whether the recorded data can be loaded and analyzed with Python
- Whether NeuraDock data is suitable for the user's own development or research workflow

## Input Data

The original tutorial materials include EEG data collected with NeuraDock's 7-channel dry-electrode device.

The current tutorial material appears to include files such as:

```text
rest_20251024160452_2m12s.txt
task_20251024160748_2m33s.txt
```

These files are used to compare different recording states, such as resting-state and task-state data.

For the official public GitHub release, the sample data should be maintained in:

- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

## Channel Layout

The default 7-channel electrode layout is:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

The signal quality workflow should preserve this channel order during parsing, preprocessing, and visualization.

## Recommended Workflow

A typical signal quality evaluation workflow is:

1. Download the sample EEG data.
2. Open the signal quality notebook.
3. Load the recorded `.txt` EEG file.
4. Parse the file into a Python-readable structure.
5. Confirm that the data contains the expected 7 EEG channels.
6. Inspect the raw data.
7. Run the preprocessing steps provided in the notebook.
8. Compare resting-state and task-state recordings if both are available.
9. Review the output plots or summary results.
10. Decide whether the signal quality is suitable for the intended development or research workflow.

## What to Inspect

When reviewing the signal quality output, users should check:

- Whether all 7 channels are present
- Whether the file can be parsed correctly
- Whether the raw signal is continuous
- Whether any channels look unusually flat or unstable
- Whether obvious noise or malformed rows are present
- Whether preprocessing produces a usable signal
- Whether rest and task recordings show reasonable differences for the intended workflow

This tutorial does not define a universal pass/fail threshold. Signal quality should be interpreted in the context of the user's experiment, environment, and analysis goal.

## Resting-State and Task-State Data

The original tutorial material mentions both resting-state and task-state information.

A typical comparison workflow may include:

| Data Type | Purpose |
|---|---|
| Resting-state data | Provides a baseline recording condition |
| Task-state data | Provides a recording condition during a task or stimulus |
| Comparison between states | Helps users inspect whether the data changes under different conditions |

The exact task design and recording protocol should be documented together with the official sample dataset.

## No Hardware Required for This Workflow

This evaluation workflow is designed so that users can run the analysis locally without owning the hardware.

Users need:

- Python environment
- Jupyter Notebook or JupyterLab
- Sample EEG text files
- Signal quality notebook
- Required Python dependencies

Users do not need:

- NeuraDock hardware
- Live USB connection
- Live Bluetooth connection
- Real-time data streaming setup

## Recommended Python Environment

The Python setup should follow:

- [Software Installation](../software-installation.md)

The official code should later be organized under:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

The official example workflow should later be organized under:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

## Suggested Output

The signal quality notebook should ideally produce outputs such as:

- Parsed data preview
- Basic channel-level visualization
- Raw data inspection
- Preprocessed data inspection
- Resting-state and task-state comparison, if applicable

The exact output figures and metrics should be confirmed against the official notebook before public release.

## Notes for Developers

When preparing the official version of this tutorial, keep the following points in mind:

- Do not modify the raw sample data directly.
- Keep raw data and processed data separate.
- Clearly document the channel order.
- Clearly document whether the data is raw, filtered, or preprocessed.
- Clearly document the source and condition of each sample file.
- Make the notebook runnable without hardware.
- Avoid using the workflow as a medical or clinical quality assessment.
- Make sure all file paths are relative or clearly explained.
- Keep the official sample data in [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data).

## To Be Confirmed

The following technical details should be confirmed before this tutorial is finalized for public release:

| Item | Status |
|---|---|
| Official cleaned notebook location | To be confirmed |
| Official sample data location | To be confirmed |
| Exact sample data file names | To be confirmed |
| Recording condition for each sample file | To be confirmed |
| Sampling rate | To be confirmed |
| Raw value unit or conversion rule | To be confirmed |
| Exact preprocessing steps | To be confirmed |
| Exact output plots or metrics | To be confirmed |
| Whether marker information is included | To be confirmed |
| Whether the data is raw or already preprocessed | To be confirmed |

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Read USB Text Files](./read-txt-usb.md) | Read and parse EEG text files recorded from the USB workflow |
| [Read Bluetooth Text Files](./read-txt-bluetooth.md) | Read and parse EEG text files recorded from the Bluetooth workflow |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
