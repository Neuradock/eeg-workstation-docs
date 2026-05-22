# NeuraDock EEG Data Format

This document describes the EEG data format used by **NeuraDock EEG Workstation**.

NeuraDock EEG data can be accessed through recorded text files, USB real-time data streaming, and Bluetooth real-time data streaming. This document provides a public-facing overview of the data structure, channel layout, parsing notes, and related examples.

## Overview

NeuraDock EEG Workstation provides EEG data through the following workflows:

| Data Source | Description | Related Materials |
|---|---|---|
| Recorded text files | EEG data saved from the NeuraDock recording software | [Read USB Text Files](./tutorials/read-txt-usb.md), [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md) |
| USB real-time stream | Real-time EEG data streamed from the acquisition module through USB | [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md), [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Bluetooth real-time stream | Real-time EEG data streamed from the acquisition module through Bluetooth | [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md), [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) |
| Sample datasets | Public sample EEG data for tutorials and offline analysis examples | [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) |

## Channel Layout

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

The channel order used in recorded files and Python examples should follow this default layout unless otherwise stated.

This montage is designed for occipital and parietal-occipital EEG experiments, including visual and attention-related paradigms such as SSVEP, cVEP, P300, and other EEG-based interaction prototypes.

## Data Types

NeuraDock data may appear in the following forms depending on the workflow:

| Data Type | Description |
|---|---|
| Raw EEG samples | EEG values directly recorded or streamed from the device |
| Parsed EEG samples | EEG values loaded and structured by Python readers |
| Preprocessed EEG data | EEG data after filtering, cleaning, segmentation, or normalization |
| Event markers | Optional trigger or event labels used for experiment synchronization |
| Derived features | Features such as band power, PSD, time-frequency power, or signal quality metrics |

## Recorded Text File Format

NeuraDock recording workflows can export EEG data as text files.

A recorded text file usually contains:

- EEG sample values
- Channel data
- Optional timestamp or sample index information
- Optional marker information, depending on the workflow
- Metadata or header information, depending on the recording software version

The exact exported structure may vary between USB and Bluetooth workflows. Use the corresponding tutorial and Python reader for parsing.

Related tutorials:

- [Read USB Text Files](./tutorials/read-txt-usb.md)
- [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md)

Related repository:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## USB Text File Format

The USB text file workflow is intended for data recorded through the USB connection.

A typical parsed USB sample should include:

| Field | Description |
|---|---|
| Sample index or timestamp | Sample sequence or time reference |
| O1 | EEG value from O1 |
| O2 | EEG value from O2 |
| Oz | EEG value from Oz |
| PO3 | EEG value from PO3 |
| PO4 | EEG value from PO4 |
| CP5 | EEG value from CP5 |
| CP6 | EEG value from CP6 |
| Marker | Optional event marker, if available |

The recommended parsed data table structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

If a field is not available in the exported file, the Python reader should either omit it or generate it during parsing.

### Notes

- The final field names should match the official Python reader.
- Timestamp availability depends on the recording workflow.
- Marker availability depends on whether the recording was performed with event markers.
- Sampling rate and unit conversion should be documented together with the sample data and Python examples.

To be finalized before public release:

- Exact USB exported column names
- Exact delimiter
- Sampling rate
- Unit conversion rule
- Header and metadata structure

## Bluetooth Text File Format

The Bluetooth text file workflow is intended for data recorded through the Bluetooth connection.

Bluetooth data may be grouped differently from USB data during export or streaming. In the existing NeuraDock tutorial materials, Bluetooth text parsing is handled separately from USB text parsing.

A parsed Bluetooth sample should eventually be converted into the same channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

The recommended final parsed data table structure is:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

If the Bluetooth export groups multiple EEG samples in a single line or packet, the parser should expand the grouped data into one row per EEG sample.

### Notes

- Bluetooth files should be parsed with the Bluetooth-specific reader.
- The parsed output should use the same channel order as the USB workflow.
- If multiple samples are stored in one line or packet, the parser should preserve the original sample order.
- Marker and timestamp behavior should be documented according to the recording workflow.

To be finalized before public release:

- Exact Bluetooth exported column names
- Exact delimiter
- Whether one text line contains one sample or multiple samples
- Number of samples per line or packet, if grouped
- Sampling rate
- Unit conversion rule
- Header and metadata structure

## Real-Time USB Streaming Format

USB real-time streaming provides live EEG data from NeuraDock to a host computer.

The USB streaming parser should output structured EEG samples with the default channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

Recommended parsed stream output:

| Field | Description |
|---|---|
| Sample index | Incremental sample count generated by the reader or device |
| Timestamp | Host-side or device-side timestamp, depending on implementation |
| EEG channels | O1, O2, Oz, PO3, PO4, CP5, CP6 |
| Marker | Optional event marker, if supported by the workflow |

Related tutorial:

- [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md)

Related repository:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## Real-Time Bluetooth Streaming Format

Bluetooth real-time streaming provides live EEG data from NeuraDock to a host computer or compatible software workflow.

The Bluetooth streaming parser should output structured EEG samples with the default channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

Recommended parsed stream output:

| Field | Description |
|---|---|
| Sample index | Incremental sample count generated by the reader or device |
| Timestamp | Host-side or device-side timestamp, depending on implementation |
| EEG channels | O1, O2, Oz, PO3, PO4, CP5, CP6 |
| Marker | Optional event marker, if supported by the workflow |

Related tutorial:

- [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md)

Related repository:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

## Units and Scaling

The public Python examples should clearly indicate whether EEG values are represented as:

- Raw digital values
- Converted voltage values
- Microvolt-level EEG values
- Normalized or preprocessed values

To avoid ambiguity, each example should state:

| Item | Description |
|---|---|
| Input unit | Unit of the original recorded or streamed values |
| Output unit | Unit of the parsed or converted values |
| Scaling formula | Conversion rule, if applicable |
| Filtering state | Whether the signal is raw, filtered, or otherwise preprocessed |

To be finalized before public release:

- Raw-to-voltage conversion rule
- Whether default examples output raw values or converted values
- Recommended unit naming in Python examples

## Sampling Rate

The sampling rate should be documented consistently across:

- Recorded text files
- USB real-time streaming
- Bluetooth real-time streaming
- Sample datasets
- Python examples
- Tutorial notebooks

To be finalized before public release:

- Default sampling rate
- Whether USB and Bluetooth workflows use the same sampling rate
- Whether sampling rate can be configured
- How sampling rate is stored or inferred in recorded files

## Markers and Event Synchronization

Some workflows may include event markers for experiment synchronization.

Markers are useful for:

- Stimulus onset labeling
- Trial boundary labeling
- Task condition labeling
- SSVEP / cVEP experiment synchronization
- Offline segmentation and epoch extraction
- Real-time interactive experiments

Recommended marker table structure:

```text
sample_index, timestamp, marker
```

or, if embedded in the EEG table:

```text
sample_index, timestamp, O1, O2, Oz, PO3, PO4, CP5, CP6, marker
```

Related tutorial:

- [Real-Time Marker Workflow](./tutorials/real-time-marker.md)

Related repository:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

To be finalized before public release:

- Marker format
- Marker timing rule
- Marker alignment behavior
- Whether markers are stored in the same file or a separate file
- Recommended marker parsing function

## Recommended Parsed DataFrame Format

For Python workflows, we recommend converting both USB and Bluetooth data into a common table format.

Recommended DataFrame columns:

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

Example:

| sample_index | timestamp | O1 | O2 | Oz | PO3 | PO4 | CP5 | CP6 | marker |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 0.000 | ... | ... | ... | ... | ... | ... | ... | none |
| 1 | 0.004 | ... | ... | ... | ... | ... | ... | ... | none |
| 2 | 0.008 | ... | ... | ... | ... | ... | ... | ... | stimulus_onset |

The example above is illustrative. Actual timestamps and sample intervals should follow the confirmed sampling rate and reader implementation.

## Recommended File Organization

For public sample data, we recommend the following structure:

```text
eeg-workstation-sample-data/
├── README.md
├── usb/
│   └── example_data_usb.txt
├── bluetooth/
│   └── example_data_bluetooth.txt
├── eyes-open-closed/
│   └── open_closed_eye_sample.txt
├── rest-task/
│   ├── rest_sample.txt
│   └── task_sample.txt
└── metadata/
    ├── channel-layout.md
    ├── data-dictionary.md
    └── recording-conditions.md
```

Each dataset should include metadata describing:

- Recording condition
- Channel layout
- Sampling rate
- File format
- Task description
- Device connection mode
- Data usage notes
- Whether the data is raw or preprocessed

Related repository:

- [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data)

## Parsing Recommendations

When writing parsers or analysis scripts, we recommend the following practices:

1. Preserve the original channel order.
2. Convert USB and Bluetooth data into a shared parsed format.
3. Keep raw data and processed data separate.
4. Store metadata together with sample data.
5. Do not overwrite the original recorded text files.
6. Document filtering and preprocessing steps clearly.
7. Use explicit column names instead of unnamed numeric columns.
8. Validate the number of channels before analysis.
9. Check for missing values, dropped samples, or malformed rows.
10. Record whether timestamps are device-side or host-side.

## Python Examples

Python readers, notebooks, and example scripts are maintained in:

- [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python)

Planned and in-progress examples include:

| Example | Purpose |
|---|---|
| USB text file reader | Parse recorded USB EEG text files |
| Bluetooth text file reader | Parse recorded Bluetooth EEG text files |
| USB real-time stream reader | Read live EEG data through USB |
| Bluetooth real-time stream reader | Read live EEG data through Bluetooth |
| Offline preprocessing | Clean and prepare recorded EEG data |
| Signal quality check | Inspect basic EEG signal quality |
| Time-frequency visualization | Generate time-frequency plots from EEG data |
| Band power analysis | Calculate EEG frequency band power |

## Related Tutorials

| Tutorial | Description |
|---|---|
| [Read USB Text Files](./tutorials/read-txt-usb.md) | Read and parse EEG text files recorded from the USB workflow |
| [Read Bluetooth Text Files](./tutorials/read-txt-bluetooth.md) | Read and parse EEG text files recorded from the Bluetooth workflow |
| [USB Real-Time Data Streaming](./tutorials/real-time-streaming-usb.md) | Read real-time EEG data from NeuraDock through USB |
| [Bluetooth Real-Time Data Streaming](./tutorials/real-time-streaming-bluetooth.md) | Read real-time EEG data from NeuraDock through Bluetooth |
| [Real-Time Marker Workflow](./tutorials/real-time-marker.md) | Stream EEG data with event markers for experiment synchronization |
| [Signal Quality Check](./tutorials/signal-quality.md) | Evaluate basic EEG signal quality from recorded NeuraDock data |

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

## Notes for Public Release

Before public release, this document should be checked against the official Python readers and exported sample files.

The following items must be confirmed by the technical team:

- Exact USB text file format
- Exact Bluetooth text file format
- Sampling rate
- Unit conversion rule
- Delimiter and header structure
- Marker format
- Timestamp behavior
- Whether Bluetooth text files group multiple samples per line
- Whether all public examples use raw, converted, or preprocessed EEG values

## Links

- Website: [neuradock.com](https://neuradock.com)
- Crowd Supply: [NeuraDock EEG Workstation](https://www.crowdsupply.com/neuradock/neuradock-eeg-workstation)
- YouTube: [@NeuraDock](https://www.youtube.com/@NeuraDock)
- Discord: NeuraDock Community
