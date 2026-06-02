# Software Installation

This document provides software setup guidance for **NeuraDock EEG Workstation**.

NeuraDock EEG Workstation supports EEG data recording, text file export, USB and Bluetooth data workflows, Python-based data reading, example EEG analysis workflows, and agent-assisted EEG analysis.

This guide covers the recommended software environment for working with NeuraDock EEG data and related developer resources.

## Software Components

NeuraDock EEG Workstation software resources are organized across several repositories.

| Component | Description | Related Repository |
|---|---|---|
| NeuraDock Recording Software | Desktop software for connecting the device, recording EEG data, and exporting EEG text files | [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) |
| Python tools | Python scripts, notebooks, and readers for EEG data files and streams | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Example workflows | Example EEG analysis workflows such as SSVEP, PSD, band power, signal quality, and markers | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) |
| Sample data | Public EEG sample datasets for tutorials and examples | [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) |
| NeuraDock Agent | Agent workflows, prompts, and EEG analysis automation examples | [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent) |
| Documentation | Setup guides, data format notes, tutorials, and hardware interface documentation | [eeg-workstation-docs](https://github.com/Neuradock/eeg-workstation-docs) |

## Recommended Setup Order

If this is your first time using NeuraDock EEG Workstation, we recommend setting up the software in this order:

1. Install or download **NeuraDock Recording Software**.
2. Connect NeuraDock EEG Workstation through USB or Bluetooth.
3. Record or stream EEG data.
4. Export recorded EEG data as text files.
5. Install the Python environment.
6. Run Python examples for reading and inspecting EEG data.
7. Run example workflows such as signal quality check, band power, PSD, or SSVEP.
8. Explore NeuraDock Agent workflows for natural-language EEG analysis.

## Recommended System Requirements

The exact system requirements will be finalized before public release.

Recommended development environment:

| Item | Recommendation |
|---|---|
| Operating system | Windows |
| Python version | Python 3.9 or later recommended |
| Development environment | VS Code, Jupyter Notebook, JupyterLab, or equivalent |
| USB support | Required for USB data streaming workflows |
| Bluetooth support | Required for Bluetooth data streaming workflows |
| Git | Recommended for cloning repositories |

To be finalized before public release:

- Official supported operating systems
- Official Python version range
- Required driver installation steps, if any
- Device firmware compatibility notes
- Recording software release link
- USB driver notes
- Bluetooth pairing requirements

## NeuraDock Recording Software

**NeuraDock Recording Software** is used to:

- Connect to NeuraDock EEG Workstation
- Check device connection status
- Record EEG data
- Export EEG data as text files
- Support USB and Bluetooth workflows, depending on the software version
- Support later analysis through Python tools and example workflows

The recording software will be distributed through the software release repository:

- [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software)

The installation package should be uploaded to:

- [NeuraDock Recording Software Releases](https://github.com/Neuradock/eeg-workstation-software/releases)

The software package should be uploaded as a GitHub Release asset, not committed directly into the documentation repository.

## Recording Software Download

The recommended download location is:

- [NeuraDock Recording Software Releases](https://github.com/Neuradock/eeg-workstation-software/releases)

Please download the latest release for your operating system.

Example release asset names:

```text
NeuraDock-Recording-Software-Windows-v0.1.0.zip
```

If only one operating system is supported in the first public release, only that installer should be provided.

To be finalized before public release:

- Official software version number
- Supported operating systems
- Installer file names
- Release notes
- Known limitations
- Required drivers, if any
- Whether the software is distributed as `.exe`, `.zip`, `.dmg`, or another package format

## Recording Software Installation

The exact installation process depends on the operating system and release package format.

General workflow:

1. Go to [NeuraDock Recording Software Releases](https://github.com/Neuradock/eeg-workstation-software/releases).
2. Download the latest release package for your operating system.
3. Extract or install the software package.
4. Launch NeuraDock Recording Software.
5. Connect the NeuraDock acquisition module through USB or Bluetooth.
6. Confirm that the software detects the device.
7. Start recording or streaming EEG data.
8. Export the recorded EEG data if using an offline analysis workflow.

To be finalized before public release:

- Windows installation steps
- USB driver installation notes
- Bluetooth permission notes

## Recording Software Output

The recording software may export EEG data as text files.

The exported files are intended to be used with:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)
- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)
- [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent)

Related documentation:

- [Data Format](./data-format.md)
- [Read USB Text Files](./tutorials/read-txt-usb.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)



## Install Example Workflows

Example workflows are maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

Clone the examples repository:

```bash
git clone https://github.com/Neuradock/eeg-workstation-examples.git
cd eeg-workstation-examples
```

Each example folder should include its own `README.md` with setup and usage notes.

Planned and in-progress examples include:

| Example | Description |
|---|---|
| Eyes-open / eyes-closed | Basic EEG comparison workflow for signal inspection and alpha activity observation |
| Band power analysis | Frequency band power calculation and visualization |
| PSD visualization | Power spectral density visualization for EEG signals |
| SSVEP demo | Visual stimulation and offline SSVEP analysis workflow |
| cVEP demo | Code-modulated visual evoked potential demo workflow |
| Signal quality check | Basic signal quality evaluation workflow |
| Real-time marker workflow | EEG data streaming with event markers and trigger alignment |
| Visual reconstruction demo | Advanced EEG visual reconstruction research example |

## Install Sample Data

Public sample data will be maintained in:

- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

Clone the sample data repository:

```bash
git clone https://github.com/Neuradock/eeg-workstation-sample-data.git
```

Sample data is intended for:

- Testing Python readers
- Reproducing tutorials
- Running offline analysis examples
- Inspecting NeuraDock EEG text file structure
- Trying example workflows without recording new data

Each dataset should include metadata such as:

- Recording condition
- Channel layout
- Sampling rate
- File format
- Task description
- Data usage notes
- Whether the data is raw or preprocessed

## USB Workflow Preparation

For USB workflows, make sure that:

- The acquisition module is powered on.
- The device is connected to the computer through USB.
- The operating system can detect the device.
- Any required driver is installed.
- The correct port or device path is selected in the software or Python script.
- The Python reader is configured for the correct USB workflow.

Related tutorials:

- [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md)
- [Read USB Text Files](./tutorials/read-txt-usb.md)

Related repositories:

- [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software)
- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

To be finalized before public release:

- USB driver requirement
- Device port naming examples
- USB connection troubleshooting notes
- Real-time streaming script name
- Export workflow from NeuraDock Recording Software

## Bluetooth Workflow Preparation

For Bluetooth workflows, make sure that:

- The acquisition module is powered on.
- Bluetooth is enabled on the host computer.
- The device is paired or discoverable according to the official workflow.
- The correct Bluetooth device is selected in the software or Python script.
- The Python reader is configured for the correct Bluetooth workflow.

Related tutorials:

- [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)

Related repositories:

- [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software)
- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

To be finalized before public release:

- Bluetooth pairing workflow
- Supported Bluetooth mode
- Device naming convention
- Bluetooth connection troubleshooting notes
- Real-time streaming script name
- Export workflow from NeuraDock Recording Software

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation](https://github.com/Neuradock/eeg-workstation) | Main project overview and repository navigation |
| [eeg-workstation-docs](https://github.com/Neuradock/eeg-workstation-docs) | Documentation, setup guides, data format notes, tutorials, and hardware interface notes |
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
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
