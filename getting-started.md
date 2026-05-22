# Getting Started with NeuraDock EEG Workstation

This guide helps you get started with **NeuraDock EEG Workstation**, a 7-channel dry-electrode EEG development kit for researchers, developers, and makers working with brain signals.

NeuraDock EEG Workstation combines a wearable dry-electrode EEG headset, a compact acquisition module, recording software, Python tools, example workflows, and an open-source EEG analysis agent.

## What You Can Do First

If you are new to NeuraDock, we recommend starting with one of the following workflows:

| Goal | Recommended Starting Point |
|---|---|
| Learn what NeuraDock EEG Workstation is | [eeg-workstation](https://github.com/Neuradock/eeg-workstation) |
| Set up the device and software | This guide |
| Understand the recorded EEG text file format | [Data Format](./data-format.md) |
| Read saved EEG text files with Python | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Stream EEG data in real time | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Run EEG example workflows | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) |
| Use public sample EEG data | [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) |
| Try agent-based EEG analysis workflows | [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent) |
| Review hardware interface specifications | [eeg-workstation-hardware](https://github.com/Neuradock/eeg-workstation-hardware) |

## System Overview

NeuraDock EEG Workstation includes:

- A 7-channel dry-electrode EEG headset
- A compact EEG acquisition module
- USB and Bluetooth data streaming
- Recording software for EEG data collection
- Python tools for reading and processing EEG data
- Example workflows for EEG experiments and signal analysis
- NeuraDock Agent for natural-language EEG analysis workflows
- Hardware interface and port specifications for third-party integration

## Electrode Layout

The default 7-channel electrode layout is:

- O1
- O2
- Oz
- PO3
- PO4
- CP5
- CP6

This montage is designed for occipital and parietal-occipital EEG experiments, including visual and attention-related paradigms such as SSVEP, cVEP, P300, and other EEG-based interaction prototypes.

## Recommended Reading Order

If this is your first time using NeuraDock EEG Workstation, we recommend going through the materials in this order:

1. [Getting Started](./getting-started.md)
2. [Software Installation](./software-installation.md)
3. [Data Format](./data-format.md)
4. [Hardware Interface](./hardware-interface.md)
5. [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md)
6. [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md)
7. [Read USB Text Files](./tutorials/read-txt-usb.md)
8. [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)
9. [Signal Quality Check](./tutorials/signal-quality.md)
10. [SSVEP Workflow](./tutorials/ssvep.md)

## Basic Setup Workflow

A typical first-time setup process includes the following steps:

1. Prepare the NeuraDock headset and acquisition module.
2. Connect the acquisition module through USB or Bluetooth.
3. Wear the headset and check electrode contact.
4. Open the NeuraDock recording software.
5. Start EEG data recording or real-time streaming.
6. Save the EEG data as a text file if using the recording workflow.
7. Use Python examples to read, inspect, and analyze the data.
8. Run example workflows such as eyes-open/eyes-closed, band power, PSD, or SSVEP analysis.

## Hardware Preparation

Before recording EEG data, make sure that:

- The acquisition module is powered on.
- The headset is worn correctly.
- The dry electrodes are positioned according to the default electrode layout.
- The acquisition module is connected through USB or Bluetooth.
- The recording software can detect the device.
- The signal quality is checked before running an experiment.

For hardware interface information, see:

- [Hardware Interface](./hardware-interface.md)
- [eeg-workstation-hardware](https://github.com/Neuradock/eeg-workstation-hardware)

## Software Preparation

The recommended software workflow includes:

1. Install the NeuraDock recording software.
2. Install Python and required Python packages.
3. Clone or download the Python tools repository.
4. Open the example notebooks or scripts.
5. Load sample data or recorded NeuraDock EEG data.
6. Run the example analysis workflow.

Related materials:

- [Software Installation](./software-installation.md)
- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)
- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

## Python Environment

The Python examples are maintained in:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

A typical setup process is:

```bash
git clone https://github.com/Neuradock/eeg-workstation-python.git
cd eeg-workstation-python
python -m venv .venv
pip install -r requirements.txt
```

Depending on your operating system and Python environment, you may need to activate the virtual environment before installing dependencies.

## First Python Examples

Recommended first examples include:

| Example | Purpose | Repository |
|---|---|---|
| Read USB text file | Load and parse EEG text files recorded through USB | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Read Bluetooth text file | Load and parse EEG text files recorded through Bluetooth | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| USB real-time streaming | Read EEG data from NeuraDock through USB in real time | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Bluetooth real-time streaming | Read EEG data from NeuraDock through Bluetooth in real time | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Offline preprocessing | Clean and prepare recorded EEG data for analysis | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Signal quality check | Inspect basic EEG signal quality | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) |

## Data Format

NeuraDock EEG data can be accessed through:

- Recorded text files
- USB real-time data streaming
- Bluetooth real-time data streaming

The data format documentation covers:

- Channel order
- Sampling structure
- USB text file format
- Bluetooth text file format
- Timestamp and marker information where applicable
- Notes for parsing recorded data in Python

Read more:

- [Data Format](./data-format.md)
- [Read USB Text Files](./tutorials/read-txt-usb.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)

## Example Workflows

Example workflows are maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

Planned and in-progress workflows include:

| Workflow | Description |
|---|---|
| Eyes-open / eyes-closed | Basic EEG comparison workflow for signal inspection and alpha activity observation |
| Band power analysis | Frequency band power calculation and visualization |
| PSD visualization | Power spectral density visualization for EEG signals |
| SSVEP demo | Visual stimulation and offline SSVEP analysis workflow |
| cVEP demo | Code-modulated visual evoked potential demo workflow |
| Signal quality check | Basic signal quality evaluation workflow |
| Real-time marker workflow | EEG data streaming with event markers and trigger alignment |
| Visual reconstruction demo | Advanced research example based on EEG visual reconstruction workflows |

## Sample Data

Public sample EEG data will be maintained in:

- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

Sample data is intended for:

- Testing the Python readers
- Reproducing tutorials
- Running offline analysis examples
- Understanding NeuraDock EEG text file structure
- Trying example workflows without recording new data

Each sample dataset should include metadata such as:

- Recording condition
- Channel layout
- Sampling rate
- File format
- Task description
- Data usage notes

## NeuraDock Agent

NeuraDock Agent is an open-source EEG analysis agent designed to help users process EEG data through natural-language instructions.

It can support workflows such as:

- Loading recorded EEG files
- Running preprocessing steps
- Calculating band power features
- Generating time-frequency visualizations
- Creating signal quality summaries
- Producing analysis reports from EEG data

Related repository:

- [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent)

NeuraDock Agent is not an open-source large language model. It is an open-source EEG analysis workflow layer designed to work with EEG data, tools, prompts, and analysis pipelines.

## Hardware Interface Scope

The hardware-related public materials are intended to support third-party integration at the interface level.

The current public hardware scope focuses on:

- Hardware interface and port specifications
- USB data format notes
- Bluetooth data format notes
- Data streaming interface notes
- Integration information for third-party software and interactive systems

Full hardware schematics, PCB design files, BOM, and manufacturing files are not included in the current public release scope.

Related repository:

- [eeg-workstation-hardware](https://github.com/Neuradock/eeg-workstation-hardware)

## Troubleshooting

If you run into issues, check:

- [Troubleshooting](./troubleshooting.md)
- [FAQ](./faq.md)

Common issues may include:

- Device not detected
- Bluetooth connection failure
- USB connection failure
- Missing Python dependencies
- Incorrect file path
- Unexpected EEG text file format
- Poor electrode contact
- Noisy signal
- Marker synchronization issues

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation](https://github.com/Neuradock/eeg-workstation) | Main project overview and repository navigation |
| [eeg-workstation-docs](https://github.com/Neuradock/eeg-workstation-docs) | Documentation, setup guides, data format notes, tutorials, and hardware interface notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent) | EEG Agent workflows, prompts, and analysis pipelines |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos, including eyes-open/closed, PSD, band power, SSVEP, cVEP, signal quality, and real-time marker workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
| [eeg-workstation-hardware](https://github.com/Neuradock/eeg-workstation-hardware) | Hardware interface and port specifications for third-party integration |

## Links

- Website: [neuradock.com](https://neuradock.com)
- Crowd Supply: [NeuraDock EEG Workstation](https://www.crowdsupply.com/neuradock/neuradock-eeg-workstation)
- YouTube: [@NeuraDock](https://www.youtube.com/@NeuraDock)
- Discord: NeuraDock Community

## Project Status

This documentation is currently being prepared for public release.

Some tutorials, notebooks, sample datasets, and hardware interface notes may still be in progress.
