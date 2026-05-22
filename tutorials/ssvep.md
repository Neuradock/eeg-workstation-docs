# SSVEP Workflow

This tutorial explains how to build a basic **SSVEP workflow** with NeuraDock EEG Workstation using Python.

The tutorial includes two major parts:

1. **Online stimulation**: Use PsychoPy to present precise visual flicker stimuli and write event markers.
2. **Offline analysis**: Load recorded EEG data, preprocess the signal, segment the data into epochs, average trials, and inspect the power spectral density around the stimulation frequency.

## Related Code

The original tutorial code is available here:

- [NeuraDock SSVEP Tutorial Code](https://github.com/JunwenLuo/NeuraDock-Tutorials/tree/main/ssvep)

A cleaned and official version should later be maintained in:

- [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples)

## Overview

SSVEP stands for **steady-state visual evoked potential**.

In an SSVEP experiment, the user looks at a visual stimulus flickering at a specific frequency. The EEG signal, especially over occipital and parietal-occipital regions, may show frequency-domain activity related to the stimulation frequency and its harmonics.

NeuraDock's default 7-channel layout is suitable for this type of visual EEG workflow:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

## Workflow Summary

A complete SSVEP workflow includes:

1. Present a flickering visual stimulus.
2. Write an event marker when the stimulus starts.
3. Record EEG data with NeuraDock.
4. Load the recorded EEG data in Python.
5. Segment the data around stimulus events.
6. Apply basic preprocessing.
7. Compute frequency-domain features.
8. Check whether the stimulation frequency appears in the PSD.

## Part 1: Online Stimulation

The first part of the tutorial focuses on stimulus presentation.

The goal is to make a visual stimulus flicker at a precise frequency.

## Why Not Use `time.sleep()`?

A simple Python program may try to use `time.sleep()` to control flicker timing.

However, this is not reliable for precise screen flicker.

For visual stimulation, the timing should be tied to the monitor refresh rate. The actual image update happens when the screen refreshes, not exactly when `time.sleep()` returns.

Therefore, the existing tutorial uses **PsychoPy**, which is commonly used for psychology and neuroscience experiments.

## Refresh Rate and Flicker Frequency

To generate a flicker at a target frequency, the program needs to calculate how many screen frames correspond to one flicker cycle.

Example from the existing tutorial:

```text
Monitor refresh rate = 60 Hz
Target flicker frequency = 4 Hz
```

The number of frames per flicker cycle is:

```text
frames_per_cycle = refresh_rate / target_frequency
frames_per_cycle = 60 / 4 = 15 frames
```

For a simple square-wave flicker with a 50% duty cycle, the 15-frame cycle can be divided into two parts:

```text
Frames 0-7: show the target image
Frames 8-14: show the background image
```

For example:

- Target image: white square
- Background image: black square

This produces a 4 Hz flicker on a 60 Hz display.

## Frequency Selection

The target flicker frequency must be compatible with the monitor refresh rate.

For example, on a 60 Hz display, frequencies such as the following may be easier to implement cleanly:

```text
4 Hz, 6 Hz, 10 Hz, 12 Hz
```

If the monitor uses a higher refresh rate, such as 144 Hz or 240 Hz, more target frequencies may be available.

The exact frequency set should be selected based on:

- Monitor refresh rate
- Desired SSVEP target frequencies
- Experimental design
- Whether the number of frames per cycle is an integer
- Whether the visual stimulus timing is stable

## PsychoPy Implementation

The existing tutorial uses PsychoPy for visual stimulus presentation.

PsychoPy is used because it can synchronize stimulus updates with the screen refresh cycle.

The important concept is:

```text
win.flip()
```

`win.flip()` waits for the monitor's vertical synchronization signal before updating the display.

This helps ensure that the visual flicker follows the screen refresh timing rather than an approximate software timer.

## Event Markers

The existing tutorial writes a marker string such as:

```text
marker\n
```

to a text file as a simple software marker.

This marker is used to indicate when the visual stimulus event happens.

In more formal BCI or neuroscience systems, event markers may be sent through dedicated hardware triggers such as a parallel port or TTL signal.

However, for prototype validation and slower SSVEP workflows, a software marker can be acceptable, as long as the timing limitations are understood.

## Marker Timing Notes

Software markers may introduce timing uncertainty due to:

- System I/O delay
- File writing delay
- Operating system scheduling
- Software-side processing delay

For prototype and tutorial purposes, this approach is useful because it is easy to understand and implement.

For stricter experimental protocols, marker timing should be validated carefully.

## Online Stimulation Workflow

A typical online stimulation workflow is:

1. Open or start the NeuraDock data acquisition workflow.
2. Prepare the visual stimulus script.
3. Confirm monitor refresh rate.
4. Select the target flicker frequency.
5. Calculate frames per flicker cycle.
6. Present the target and background images alternately.
7. Write a marker when the stimulus begins.
8. Record EEG data during the stimulus.
9. Save the EEG data and marker information for offline analysis.

## Part 2: Offline Analysis

The second part of the tutorial focuses on offline EEG analysis.

After recording data, the goal is to check whether the SSVEP response can be observed in the frequency domain.

The typical offline workflow includes:

1. Load recorded EEG data.
2. Load or identify event markers.
3. Preprocess the EEG signal.
4. Segment the signal into epochs.
5. Average trials when needed.
6. Compute power spectral density.
7. Inspect whether the stimulation frequency appears as a peak.

## Signal Preprocessing

The existing tutorial describes a standard preprocessing pipeline.

Typical preprocessing steps may include:

- Bandpass filtering
- Removing low-frequency baseline drift
- Reducing high-frequency noise
- Preparing the data for epoch extraction
- Keeping the data in a consistent channel order

The exact preprocessing parameters should be confirmed in the official notebook before public release.

## Epoching

Epoching means cutting the continuous EEG data into smaller segments based on event markers.

For example, after a stimulus marker, the program may extract a fixed-length time window from the EEG data.

In an SSVEP workflow, each epoch should correspond to a time window during or after visual stimulation.

The existing tutorial discusses parameters such as:

```text
fs = 250
epoch_len = 1000
```

This means that, if the sampling rate is 250 Hz, an epoch length of 1000 samples corresponds to:

```text
1000 / 250 = 4 seconds
```

The tutorial also notes that if the intended goal is to capture only 1 second of data after stimulus onset, an epoch length of 250 samples would correspond to 1 second at 250 Hz.

The exact epoch length should be selected according to the experimental goal.

## Trial Averaging

The tutorial includes the idea of averaging across trials.

Trial averaging can help improve signal visibility by reducing random noise.

In EEG analysis, weak but time-locked or frequency-locked responses may become easier to observe after averaging multiple trials.

For SSVEP, trial averaging can help make the frequency-domain response more visible.

## PSD Analysis

The tutorial uses power spectral density analysis to inspect the frequency-domain structure of the EEG signal.

The goal is to check whether the stimulation frequency appears as a visible peak in the PSD.

For example, if the stimulus flickers at 4 Hz, the PSD may show activity around:

```text
4 Hz
```

It may also show harmonics such as:

```text
8 Hz
12 Hz
```

These harmonics are normal in SSVEP analysis.

## Why Use Welch PSD?

The existing tutorial discusses why Welch PSD can be more useful than applying a single FFT directly.

EEG data is noisy, and a single FFT over one long segment may produce unstable results.

Welch's method improves stability by:

1. Splitting the signal into smaller windows.
2. Applying FFT to each window.
3. Averaging the resulting spectra.

This can produce a smoother and more stable PSD estimate.

## Harmonics in SSVEP

SSVEP signals may show peaks not only at the fundamental stimulation frequency, but also at harmonic frequencies.

For a 4 Hz visual flicker, possible peaks may appear around:

```text
4 Hz
8 Hz
12 Hz
```

Seeing harmonics does not necessarily mean the result is wrong.

Harmonics can be part of a normal SSVEP response.

## Recommended Offline Analysis Workflow

A typical offline SSVEP analysis workflow is:

1. Load the recorded EEG data.
2. Parse the data into the default 7-channel structure.
3. Load or identify event markers.
4. Apply basic preprocessing.
5. Segment the data into epochs based on stimulus markers.
6. Average trials if needed.
7. Compute PSD using Welch's method.
8. Check whether the stimulation frequency and possible harmonics appear in the result.

## Recommended Parsed Format

For analysis, EEG data should be converted into a table-like structure.

Recommended columns:

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

If timestamp or marker information is not available in the exported file, those fields can be omitted or generated by the parser.

The minimum useful parsed structure should include:

```text
O1, O2, Oz, PO3, PO4, CP5, CP6
```

## Running the Original Tutorial Code

The existing tutorial code is located at:

- [NeuraDock SSVEP Tutorial Code](https://github.com/JunwenLuo/NeuraDock-Tutorials/tree/main/ssvep)

The original workflow includes files such as:

```text
NeuraDock_socket.py
Neuradock_lib.py
ssvep.py
ssvep_result.py
black.png
white.png
```

A typical workflow is:

1. Open NeuraDock software.
2. Start the data source or data stream service.
3. Obtain the IP and port information.
4. Modify the relevant Python files according to the local IP and port settings.
5. Run the SSVEP stimulation script.
6. Record EEG data.
7. Run the offline result or analysis script.

The exact command-line steps should be confirmed in the cleaned official example.

## Expected Outputs

The official SSVEP example should ideally provide:

- Visual flicker stimulus
- Marker output
- Parsed EEG data
- Preprocessed EEG signal
- Epoch-level data
- PSD plot
- Frequency-domain result around the stimulation frequency

The exact figures and outputs should be confirmed in the official example repository.

## Notes for Developers

When preparing or modifying the SSVEP workflow, keep the following points in mind:

- Do not use `time.sleep()` as the main method for precise flicker timing.
- Use monitor refresh rate to calculate frame-based stimulus timing.
- Use PsychoPy or another timing-appropriate visual presentation tool.
- Document the monitor refresh rate.
- Document the target stimulation frequency.
- Document marker timing and limitations.
- Preserve the default channel order.
- Clearly document preprocessing steps.
- Clearly document epoch length and sampling rate.
- Check both the stimulation frequency and harmonics in the PSD.
- Avoid treating the tutorial result as a clinical or diagnostic result.

## To Be Confirmed

The following technical details should be confirmed before this tutorial is finalized for public release:

| Item | Status |
|---|---|
| Official cleaned SSVEP example location | To be confirmed |
| Exact command to run the stimulation script | To be confirmed |
| Exact command to run the offline analysis script | To be confirmed |
| Official required Python dependencies | To be confirmed |
| Exact monitor refresh rate used in the public example | To be confirmed |
| Exact stimulation frequency or frequencies | To be confirmed |
| Exact marker file format | To be confirmed |
| Exact EEG sampling rate | To be confirmed |
| Exact preprocessing parameters | To be confirmed |
| Exact epoch length used in the public example | To be confirmed |
| Exact PSD parameters | To be confirmed |
| Official sample dataset location | To be confirmed |

## Related Documentation

| Document | Description |
|---|---|
| [Getting Started](../getting-started.md) | First-time setup guide for NeuraDock EEG Workstation |
| [Software Installation](../software-installation.md) | Software environment setup and installation guide |
| [Data Format](../data-format.md) | EEG text file structure, channel layout, USB data format, and Bluetooth data format |
| [USB Real-Time Data Streaming](./real-time-streaming-usb.md) | Read real-time EEG data through the USB workflow |
| [Bluetooth Real-Time Data Streaming](./real-time-streaming-bluetooth.md) | Read real-time EEG data through the Bluetooth workflow |
| [Real-Time Marker Workflow](./real-time-marker.md) | Stream EEG data with event markers for experiment synchronization |
| [Signal Quality Check](./signal-quality.md) | Evaluate basic EEG signal quality from recorded NeuraDock data |
| [Troubleshooting](../troubleshooting.md) | Common connection, data recording, and software issues |

## Related Repositories

| Repository | Description |
|---|---|
| [eeg-workstation-software](https://github.com/Neuradock/eeg-workstation-software) | NeuraDock Recording Software releases and software usage notes |
| [eeg-workstation-python](https://github.com/Neuradock/eeg-workstation-python) | Python tools, notebooks, and EEG data reading examples |
| [eeg-workstation-examples](https://github.com/Neuradock/eeg-workstation-examples) | Example EEG demos and signal processing workflows |
| [eeg-workstation-sample-data](https://github.com/Neuradock/eeg-workstation-sample-data) | Public sample EEG datasets for tutorials and examples |
