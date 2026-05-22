# Troubleshooting

This page provides general troubleshooting guidance for **NeuraDock EEG Workstation**.

This troubleshooting guide is currently a general placeholder and will be expanded as the recording software, Python tools, example workflows, and public release materials are finalized.

## Quick Checklist

Before troubleshooting a specific issue, check the following:

- The acquisition module is powered on.
- The headset is worn correctly.
- The dry electrodes are positioned according to the default layout.
- The device is connected through USB or Bluetooth.
- NeuraDock Recording Software is installed.
- The correct device or port is selected.
- Python dependencies are installed if using Python tools.
- The correct sample data or recorded data file path is used.
- The data file format matches the selected parser.
- The documentation page you are following matches your workflow.

Default channel layout:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

## Device Connection Issues

### Device is not detected through USB

Possible causes:

- USB cable is not connected properly.
- The USB cable only supports charging and not data transfer.
- The device is not powered on.
- The operating system does not recognize the device.
- Required drivers are not installed.
- The wrong port is selected in the software or script.

Suggested actions:

1. Check that the device is powered on.
2. Try a different USB data cable.
3. Try a different USB port.
4. Restart NeuraDock Recording Software.
5. Check whether the device appears in the operating system device list.
6. Confirm whether any driver is required.
7. Check the USB workflow notes in [Software Installation](./software-installation.md).

Related documentation:

- [Software Installation](./software-installation.md)
- [Hardware Interface](./hardware-interface.md)
- [Data Format](./data-format.md)

### Device is not found through Bluetooth

Possible causes:

- Bluetooth is not enabled on the host computer.
- The device is not powered on.
- The device is not discoverable.
- The device is already connected to another host.
- Bluetooth permissions are blocked by the operating system.
- The wrong Bluetooth device is selected.

Suggested actions:

1. Make sure Bluetooth is enabled.
2. Restart the device.
3. Restart Bluetooth on the host computer.
4. Check whether the device appears in the Bluetooth device list.
5. Remove old pairing records and pair again if needed.
6. Make sure the correct device is selected in the software or script.
7. Check the Bluetooth workflow notes in [Software Installation](./software-installation.md).

Related documentation:

- [Software Installation](./software-installation.md)
- [Hardware Interface](./hardware-interface.md)

## Recording Software Issues

### NeuraDock Recording Software does not launch

Possible causes:

- The wrong software package was downloaded.
- The operating system is not supported.
- Required dependencies are missing.
- The software package was not extracted properly.
- System permissions are blocking the application.

Suggested actions:

1. Download the latest release from [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software/releases).
2. Confirm that the release matches your operating system.
3. Extract the package before launching, if required.
4. Check release notes for known issues.
5. Restart the computer if required by the installer.
6. Try running the software with appropriate system permissions.

Related repository:

- [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software)

### Recording software cannot export data

Possible causes:

- Recording was not started.
- No valid data was received.
- The export path is invalid.
- The software does not have write permission.
- The selected export format is not supported.

Suggested actions:

1. Confirm that EEG data is being received before exporting.
2. Choose a valid export folder.
3. Avoid paths with special characters if export fails.
4. Check that the export file is not open in another program.
5. Check the data format documentation after export.

Related documentation:

- [Data Format](./data-format.md)

## Signal Quality Issues

### EEG signal looks noisy

Possible causes:

- Poor electrode contact.
- Headset is not positioned correctly.
- User movement or muscle artifacts.
- Environmental electrical noise.
- Loose device connection.
- Dry electrodes are not making stable contact.

Suggested actions:

1. Reposition the headset.
2. Check electrode contact.
3. Reduce head and body movement.
4. Move away from strong electrical noise sources.
5. Check the connection between headset and acquisition module.
6. Run a signal quality check workflow when available.

Related repository:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

### Expected EEG pattern is not visible

Possible causes:

- Experimental condition is not controlled.
- Signal quality is poor.
- Wrong channel order.
- Wrong analysis script.
- Sampling rate or filtering settings are incorrect.
- The task or stimulus is not aligned with markers.

Suggested actions:

1. Confirm the channel order.
2. Check signal quality before analysis.
3. Confirm the task condition and timing.
4. Check whether markers are aligned correctly.
5. Compare with a known sample dataset.
6. Review the corresponding example workflow.

## Marker and Synchronization Issues

### Markers are missing

Possible causes:

- Marker workflow was not enabled.
- Marker file was not saved.
- Markers are stored separately from EEG data.
- The parser does not support marker columns yet.
- The wrong data file was loaded.

Suggested actions:

1. Confirm whether the recording workflow supports markers.
2. Check whether markers are saved in the same file or a separate file.
3. Inspect the exported files.
4. Use the marker-specific example workflow when available.
5. Check [Data Format](./data-format.md).

Related tutorial:

- [Real-Time Marker Workflow](./tutorials/real-time-marker.md)

### Markers are not aligned with EEG data

Possible causes:

- Timestamp mismatch.
- Host-side and device-side timestamps are mixed.
- Marker timing rule is unclear.
- Sampling rate is incorrect.
- Dropped samples or malformed rows exist.

Suggested actions:

1. Confirm the sampling rate.
2. Check marker timestamps or sample indices.
3. Confirm whether timestamps are host-side or device-side.
4. Validate the number of samples.
5. Check for dropped or malformed rows.
6. Compare with a known example workflow.

## Agent Workflow Issues

### NeuraDock Agent cannot load the EEG file

Possible causes:

- Unsupported file format.
- Incorrect file path.
- Missing sample data.
- Data has not been parsed into the expected structure.
- Required Python dependencies are missing.

Suggested actions:

1. Confirm the file path.
2. Check the data format.
3. Parse the data using the official Python tools first.
4. Confirm the expected input format for the agent workflow.
5. Check the agent repository documentation.

Related repository:

- [eeg-workstation-agent](https://github.com/Neuradock/eeg-workstation-agent)

### Agent output looks incorrect

Possible causes:

- Input data is noisy.
- Wrong channel order.
- Missing metadata.
- Wrong task instruction.
- Unsupported analysis workflow.
- Preprocessing steps are not documented.

Suggested actions:

1. Check the input data quality.
2. Confirm the channel layout.
3. Provide clear task instructions.
4. Review the workflow prompt.
5. Compare with official example outputs when available.
6. Avoid using agent outputs as medical or clinical conclusions.

## Common Error Table

| Issue | Likely Cause | Suggested Action |
|---|---|---|
| Device not detected | USB, Bluetooth, driver, or power issue | Check connection and device status |
| Recording software does not launch | Wrong package or OS issue | Check [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) release notes |
| Python command not found | Python not installed or PATH issue | Install Python and restart terminal |
| Dependency installation fails | Environment or package conflict | Use a clean virtual environment |
| Notebook cannot find data | Incorrect file path | Check working directory and sample data location |
| File parser fails | Wrong format or parser | Check [Data Format](./data-format.md) |
| Signal is noisy | Poor contact or movement artifact | Check headset placement and reduce movement |
| Markers missing | Marker workflow not enabled | Check marker workflow documentation |
| Agent cannot load file | Unsupported input format | Parse data with Python tools first |

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](./getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](./software-installation.md) | Software environment setup, Python dependencies, and recommended tools |
| [Data Format](./data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [Hardware Interface](./hardware-interface.md) | Hardware interface and port specifications for third-party integration |
| [FAQ](./faq.md) | Frequently asked questions about NeuraDock EEG Workstation |

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
