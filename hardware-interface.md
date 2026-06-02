# NeuraDock Hardware Interface

This document describes the public hardware interface scope for **NeuraDock EEG Workstation**.

NeuraDock EEG Workstation is a 7-channel dry-electrode EEG development kit that provides EEG data through USB and Bluetooth workflows. The hardware-related public materials are intended to help developers integrate NeuraDock EEG data with third-party software, experiments, applications, and interactive systems.

## Public Hardware Scope

The current public hardware scope focuses on:

- Hardware interface and port specifications for third-party integration
- USB data streaming interface notes
- Bluetooth data streaming interface notes
- Data format notes for software integration
- Connection workflow notes for developers
- Integration guidance for research and interactive applications

The goal of this documentation is to help developers access and work with NeuraDock EEG data at the interface level.

## What Is Included

The public hardware interface materials may include:

| Item | Description | Status |
|---|---|---|
| USB data interface notes | Notes for accessing EEG data through USB workflows | Available |
| Bluetooth data interface notes | Notes for accessing EEG data through Bluetooth workflows | Available |
| Data streaming format notes | Information about how EEG data is structured for software parsing | Available |
| Recorded data format notes | Notes about exported EEG text files | Available |
| Channel layout | Default 7-channel electrode layout | Available |
| Third-party integration notes | Guidance for connecting NeuraDock data with external software systems | In progress |
| Port and connector specifications | Interface-level information where applicable | To be finalized |

## What Is Not Included

The current public hardware scope does **not** include:

- Full hardware schematics
- PCB design files
- PCB layout files
- Manufacturing files
- BOM
- Gerber files
- Pick-and-place files
- Internal firmware source code, unless separately released
- Internal production test procedures
- Internal manufacturing documentation

NeuraDock is not currently publishing a full open-hardware manufacturing package.

## Open Source Hardware Boundary

For hardware, the current public release focuses on:

```text
Hardware interface and port specifications for third-party integration
```

This means the public materials are intended to support developers who want to connect NeuraDock EEG data with their own tools or applications.

It does not mean that the full hardware design is open-sourced for manufacturing, cloning, or PCB-level modification.

## Default Electrode Layout

The default 7-channel electrode layout is:

| Channel Index | Electrode |
|---|---|
| 1 | O1 |
| 2 | O2 |
| 3 | Oz |
| 4 | PO3 |
| 5 | PO4 |
| 6 | CP5 |
| 7 | CP6 |

This montage is designed for occipital and parietal-occipital EEG experiments, including visual and attention-related paradigms such as SSVEP, cVEP, P300, and other EEG-based interaction prototypes.

## Interface-Level Integration

NeuraDock hardware interface documentation is intended for developers who want to:

- Read EEG data from NeuraDock into custom software
- Build Python-based EEG analysis tools
- Connect NeuraDock data with experiment software
- Build EEG-based interaction prototypes
- Use NeuraDock data in real-time visualization systems
- Integrate NeuraDock data with AI-assisted analysis workflows
- Synchronize EEG data with external event markers or stimuli

Related repositories:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)
- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)
- [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent)

## USB Interface Notes

NeuraDock supports USB-based EEG data workflows.

The USB workflow may be used for:

- Real-time EEG data streaming
- Stable desktop-side data acquisition
- Python-based EEG data reading
- Recorded EEG data export
- Offline analysis workflows

Related documentation:

- [Data Format](./data-format.md)
- [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md)
- [Read USB Text Files](./tutorials/read-txt-usb.md)

Related repository:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

### USB Data Structure

USB data should be parsed into the default 7-channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

The recommended parsed table structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

To be finalized before public release:

- Exact USB data packet structure
- Exact USB text export format
- USB delimiter and header structure
- Sampling rate
- Timestamp behavior
- Marker behavior
- Unit conversion rule

## Bluetooth Interface Notes

NeuraDock supports Bluetooth-based EEG data workflows.

The Bluetooth workflow may be used for:

- Wireless EEG data streaming
- Portable EEG acquisition workflows
- Python-based data reading
- Recorded EEG data export
- Offline analysis workflows

Related documentation:

- [Data Format](./data-format.md)
- [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)

Related repository:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

### Bluetooth Data Structure

Bluetooth data should be parsed into the default 7-channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

The recommended parsed table structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

If Bluetooth data is grouped by packet or by line in the exported text file, the parser should expand the grouped data into one row per EEG sample while preserving the original sample order.

To be finalized before public release:

- Exact Bluetooth data packet structure
- Whether one exported line contains one sample or multiple samples
- Number of samples per Bluetooth packet or exported line, if grouped
- Bluetooth delimiter and header structure
- Sampling rate
- Timestamp behavior
- Marker behavior
- Unit conversion rule

## Recorded Data Export

NeuraDock recording workflows may export EEG data as text files.

Recorded text files are intended for:

- Offline EEG analysis
- Tutorial reproduction
- Data format inspection
- Python-based parsing
- Signal quality checking
- Example workflow development

Related documentation:

- [Data Format](./data-format.md)
- [Read USB Text Files](./tutorials/read-txt-usb.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)

Related repositories:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)
- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

## Event Markers and Synchronization

Some workflows may use event markers for experiment synchronization.

Markers may be used for:

- Stimulus onset labeling
- Trial boundary labeling
- Task condition labeling
- SSVEP experiment synchronization
- cVEP experiment synchronization
- Offline segmentation and epoch extraction
- Real-time interaction prototypes

Recommended marker table structure:

```text
sample_index, timestamp, marker
```

or, if embedded in the EEG table:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

Related documentation:

- [Real-Time Marker Workflow](./tutorials/real-time-marker.md)
- [Data Format](./data-format.md)

Related repository:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

To be finalized before public release:

- Marker format
- Marker timing rule
- Marker alignment behavior
- Whether markers are stored in the same file or a separate file
- Recommended marker parsing function

## Third-Party Software Integration

NeuraDock data can be integrated with third-party software through parsed EEG data streams or exported files.

Possible integration targets include:

- Python analysis scripts
- Jupyter notebooks
- Real-time visualization systems
- Experimental stimulus software
- Interactive media systems
- BCI research prototypes
- AI-assisted EEG analysis workflows
- Custom data logging tools

Recommended integration workflow:

1. Connect NeuraDock through USB or Bluetooth.
2. Record or stream EEG data.
3. Parse the data into the default 7-channel structure.
4. Validate channel order and sampling behavior.
5. Apply preprocessing or signal quality checks.
6. Integrate the parsed data with the target software workflow.
7. Document any filtering, conversion, marker, or synchronization steps.

## Recommended Parsed Data Structure

For software integration, both USB and Bluetooth workflows should ideally be converted into a shared parsed structure.

Recommended table columns:

```text
sample_index
timestamp
O1
O2
Oz
PO3
PO4
CP5
CP6
marker
```

This shared structure makes it easier to reuse the same Python examples, tutorials, and agent workflows across different connection modes.

Related documentation:

- [Data Format](./data-format.md)

## Hardware Interface and Software Examples

The hardware interface documentation is closely connected to the Python tools and example workflows.

| Repository | Purpose |
|---|---|
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
| [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent) | EEG Agent workflows, prompts, and analysis pipelines |

## Notes for Developers

When building with NeuraDock EEG data, we recommend:

1. Use the official Python readers where available.
2. Preserve the default channel order.
3. Clearly document whether the data is raw, converted, filtered, or preprocessed.
4. Keep original recorded files separate from processed files.
5. Validate the number of channels before analysis.
6. Check for missing values, dropped samples, or malformed rows.
7. Document the sampling rate used in each workflow.
8. Document marker timing and synchronization behavior.
9. Avoid assuming that USB and Bluetooth exports have identical raw file structures.
10. Convert both USB and Bluetooth workflows into a shared parsed format before downstream analysis.

## Safety and Use Disclaimer

NeuraDock EEG Workstation is intended for research, development, education, and prototyping use cases.

It is not intended to provide medical diagnosis, clinical treatment, or medical decision-making.

Developers should validate their own workflows, signal quality, data processing steps, and experimental protocols before using the system in research or product prototypes.

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](./getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Data Format](./data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Software Installation](./software-installation.md) | Software environment setup, Python dependencies, and recommended tools |
| [FAQ](./faq.md) | Frequently asked questions about NeuraDock EEG Workstation |
| [Troubleshooting](./troubleshooting.md) | Common connection, data recording, and software issues |

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

## License

The license for this hardware interface documentation will be provided before public release.

The current public hardware scope is limited to hardware interface and port specifications for third-party integration. Full hardware schematics, PCB design files, BOM, and manufacturing files are not included in the current public release scope.
