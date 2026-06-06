# NeuraDock EEG Workstation Documentation

This repository contains documentation for **NeuraDock EEG Workstation**, a 7-channel dry-electrode EEG development kit for researchers, developers, and makers working with brain signals.

The documentation covers setup guides, data format notes, software usage, Python workflows, example tutorials, and hardware interface specifications for third-party integration.

## Documentation Overview

| Document | Description | Status |
|---|---|---|
| [Getting Started](./getting-started.md) | First-time setup guide for NeuraDock EEG Workstation | avaliable |
| [Data Format](./data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format | avaliable |
| [Hardware Interface](./hardware-interface.md) | Hardware interface and port specifications for third-party integration | avaliable |
| [Software Installation](./software-installation.md) | Software environment setup, Python dependencies, and recommended tools | avaliable |
| [FAQ](./faq.md) | Frequently asked questions about NeuraDock EEG Workstation | avaliable |
| [Troubleshooting](./troubleshooting.md) | Common connection, data recording, and software issues | avaliable |

## Tutorials

The following tutorials are being prepared based on existing NeuraDock technical materials.

| Tutorial | Description | Related Repository |Status |
|---|---|---|---|
| [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md) | Read real-time EEG data from NeuraDock through USB | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | available |
| [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md) | Read real-time EEG data from NeuraDock through Bluetooth | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | available |
| [Read USB Text Files](./tutorials/read-txt-usb.md) | Read and parse EEG text files recorded from the USB workflow | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | available |
| [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md) | Read and parse EEG text files recorded from the Bluetooth workflow | [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | available |
| [Real-Time Marker Workflow](./tutorials/real-time-marker.md) | Stream EEG data with event markers for experiment synchronization | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | available |
| [Signal Quality Check](./tutorials/signal-quality.md) | Evaluate basic EEG signal quality from recorded NeuraDock data | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | available |
| [SSVEP Workflow](./tutorials/ssvep.md) | Run visual stimulation and offline SSVEP analysis workflows | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | available |
| [cVEP Workflow](https://github.com/Neuradock/eeg-workstation-examples/tree/main/neuradock_cvep) | Run code-modulated visual evoked potential workflows | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | available |
| [Visual Reconstruction Demo](https://github.com/Neuradock/eeg-workstation-examples/tree/main/Visual-reconstruction) | Advanced EEG visual reconstruction research example | [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | available |

## Recommended Reading Order

If you are new to NeuraDock EEG Workstation, we recommend reading the documentation in this order:

1. [Getting Started](./getting-started.md)
2. [Software Installation](./software-installation.md)
3. [Data Format](./data-format.md)
4. [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md)
5. [Read USB Text Files](./tutorials/read-txt-usb.md)
6. [Signal Quality Check](./tutorials/signal-quality.md)
7. [SSVEP Workflow](./tutorials/ssvep.md)
8. [Hardware Interface](./hardware-interface.md)

## Electrode Layout

The default 7-channel electrode layout of NeuraDock EEG Workstation is:

- O1
- O2
- Oz
- PO3
- PO4
- CP5
- CP6

This montage is designed for occipital and parietal-occipital EEG experiments, including visual and attention-related paradigms such as SSVEP, cVEP, P300, and other EEG-based interaction prototypes.

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

## Open Source Scope

NeuraDock is preparing open resources for EEG developers and researchers, including:

- EEG data reading examples
- Python-based analysis workflows
- Tutorial notebooks
- Example EEG experiments
- Agent workflows and prompts
- Hardware interface and port specifications for third-party integration

For hardware, the current public scope focuses on **hardware interface and port specifications**. Full hardware schematics, PCB design files, BOM, and manufacturing files are not included in the current public release scope.

## Hardware Interface Notes

The hardware-related public materials are intended to support third-party integration at the interface level.

Planned public hardware interface materials include:

- Data streaming interface notes
- USB data format notes
- Bluetooth data format notes
- Port and integration specifications where applicable

These materials are intended for developers who want to connect NeuraDock EEG data with their own software, experiments, applications, or interactive systems.

## Links

- Website: [neuradock.com](https://neuradock.com)
- Crowd Supply: [NeuraDock EEG Workstation](https://www.crowdsupply.com/neuradock/neuradock-eeg-workstation)
- YouTube: [@NeuraDock](https://www.youtube.com/@NeuraDock)
- Discord: [NeuraDock Community](https://discord.gg/YdQp8puZjz)

## License

- Hardware design files: CERN-OHL-W
- Mechanical CAD files: CC BY-SA 4.0
- Software (SDK and tools): MIT License
  
All resources will be published on our GitHub repository. We welcome contributions from the community, including software improvements, new analysis workflows, and hardware adaptations.
