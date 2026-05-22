# Frequently Asked Questions

This page collects frequently asked questions about **NeuraDock EEG Workstation**.

This FAQ is currently a general placeholder and will be expanded as the documentation, software tools, examples, and public release materials are finalized.

## General Questions

### What is NeuraDock EEG Workstation?

NeuraDock EEG Workstation is a 7-channel dry-electrode EEG development kit for researchers, developers, and makers working with brain signals.

It combines wearable EEG hardware, a compact acquisition module, recording software, Python tools, example workflows, and an open-source EEG analysis agent.

Related repository:

- [eeg-workstation](https://github.com/Neuradock/eeg-workstation)

### Who is NeuraDock EEG Workstation for?

NeuraDock EEG Workstation is intended for:

- EEG researchers
- Brain-computer interface developers
- Human-computer interaction researchers
- AI and neurotechnology developers
- Interactive media developers
- Educators and students working with EEG data
- Makers and developers building brain-signal-based prototypes

### Is NeuraDock EEG Workstation a medical device?

No. NeuraDock EEG Workstation is intended for research, development, education, and prototyping use cases.

It is not intended to provide medical diagnosis, clinical treatment, or medical decision-making.

## Hardware Questions

### How many EEG channels does NeuraDock EEG Workstation have?

NeuraDock EEG Workstation uses a 7-channel EEG layout.

The default electrode layout is:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

### What kinds of experiments is this layout suitable for?

The default montage is designed for occipital and parietal-occipital EEG experiments, including visual and attention-related paradigms such as:

- SSVEP
- cVEP
- P300
- Visual attention workflows
- EEG-based interaction prototypes

### Does NeuraDock publish full hardware schematics and PCB design files?

No.

The current public hardware scope focuses on:

```text
Hardware interface and port specifications for third-party integration
```

Full hardware schematics, PCB design files, BOM, and manufacturing files are not included in the current public release scope.

Related document:

- [Hardware Interface](./hardware-interface.md)

## Software Questions

### Where can I download NeuraDock Recording Software?

NeuraDock Recording Software will be distributed through:

- [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software)

Release packages should be available from:

- [NeuraDock Recording Software Releases](https://github.com/Neuradock/eeg-workstation-software/releases)

### What does NeuraDock Recording Software do?

NeuraDock Recording Software is intended to support workflows such as:

- Connecting to NeuraDock EEG Workstation
- Checking device connection status
- Recording EEG data
- Exporting EEG data as text files
- Supporting USB and Bluetooth workflows, depending on the software version

### What operating systems are supported?

Supported operating systems will be finalized before public release.

The documentation currently assumes a general development environment using Windows, macOS, or Linux, but official support should be confirmed in the software release notes.

Related document:

- [Software Installation](./software-installation.md)

## Data Questions

### What data format does NeuraDock use?

NeuraDock EEG data may be accessed through:

- Recorded text files
- USB real-time data streaming
- Bluetooth real-time data streaming
- Public sample datasets

Related document:

- [Data Format](./data-format.md)

### What is the recommended parsed data structure?

The recommended parsed table structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

The exact exported file structure should be confirmed against the official recording software and Python readers before public release.

### Are USB and Bluetooth data formats the same?

USB and Bluetooth workflows may have different raw export or streaming structures.

However, the recommended parser output should use the same final 7-channel structure:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

Related document:

- [Data Format](./data-format.md)

## Python Questions

### Where are the Python tools?

Python tools, notebooks, and readers are maintained in:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

### What can I do with the Python tools?

The Python tools are intended for workflows such as:

- Reading recorded EEG text files
- Parsing USB and Bluetooth data
- Inspecting EEG signals
- Running preprocessing examples
- Plotting EEG data
- Running basic analysis workflows
- Supporting examples such as signal quality checks, PSD, band power, and SSVEP

### What Python version should I use?

Python 3.9 or later is currently recommended.

The official supported Python version range will be finalized before public release.

Related document:

- [Software Installation](./software-installation.md)

## Example Workflow Questions

### Where are the example workflows?

Example workflows are maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

Planned and in-progress examples include:

- Eyes-open / eyes-closed
- Band power analysis
- PSD visualization
- SSVEP demo
- cVEP demo
- Signal quality check
- Real-time marker workflow
- Visual reconstruction demo

### Do I need a device to run the examples?

Some examples may require a NeuraDock device for real-time acquisition.

Offline examples should be runnable with public sample datasets once the sample data repository is available.

Sample data will be maintained in:

- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

## NeuraDock Agent Questions

### What is NeuraDock Agent?

NeuraDock Agent is an open-source EEG analysis workflow layer designed to help users process EEG data through natural-language instructions.

It may support workflows such as:

- Loading recorded EEG files
- Running preprocessing steps
- Calculating band power features
- Generating time-frequency visualizations
- Creating signal quality summaries
- Producing analysis reports from EEG data

Related repository:

- [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent)

### Is NeuraDock Agent an open-source large language model?

No.

NeuraDock Agent is not an open-source large language model. It is an open-source EEG analysis workflow layer designed to work with EEG data, tools, prompts, and analysis pipelines.

## Documentation Questions

### Where should I start?

If you are new to NeuraDock EEG Workstation, start with:

1. [Getting Started](./getting-started.md)
2. [Software Installation](./software-installation.md)
3. [Data Format](./data-format.md)
4. [Hardware Interface](./hardware-interface.md)

### Why are some pages marked as in progress?

The NeuraDock GitHub organization is currently being prepared for public release.

Some tutorials, notebooks, sample datasets, software releases, and hardware interface notes are still being organized, translated, or finalized.

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation](https://github.com/Neuradock/eeg-workstation) | Main project overview and repository navigation |
| [eeg-workstation-docs](https://github.com/Neuradock/eeg-workstation-docs) | Documentation, setup guides, data format notes, tutorials, and hardware interface notes |
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent) | EEG Agent workflows, prompts, and analysis pipelines |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
| [eeg-workstation-hardware](https://github.com/Neuradock/eeg-workstation-hardware) | Hardware interface and port specifications for third-party integration |

## Links

- Website: [neuradock.com](https://neuradock.com)
- Crowd Supply: [NeuraDock EEG Workstation](https://www.crowdsupply.com/neuradock/neuradock-eeg-workstation)
- YouTube: [@NeuraDock](https://www.youtube.com/@NeuraDock)
- Discord: NeuraDock Community
