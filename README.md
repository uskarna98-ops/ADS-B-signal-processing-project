# ADS-B Signal Processing and Decoder Using Python

## Overview

This project implements an offline Automatic Dependent Surveillance–Broadcast (ADS-B) receiver in Python. The decoder processes raw complex I/Q samples recorded from a 1090 MHz ADS-B transmission, detects valid Mode S packets, recovers the transmitted binary payload, and decodes aircraft metadata including the Downlink Format (DF), Capability (CA), ICAO address, and message type.

The project demonstrates the application of Digital Signal Processing (DSP), wireless communications, and binary protocol decoding using Python.

---

## Project Motivation

Aircraft equipped with ADS-B continuously broadcast information about their identity, position, and status. These broadcasts are transmitted using Mode S Extended Squitter messages at 1090 MHz.

The goal of this project was to build a receiver capable of processing raw recorded radio signals without relying on existing ADS-B decoding libraries. Every stage of the decoding process—from signal acquisition to message interpretation—was implemented using custom Python code.

---

## Features

- Read large ADS-B WAV recordings
- Extract complex In-phase (I) and Quadrature (Q) samples
- Remove DC offset from the received signal
- Compute FFT for frequency-domain analysis
- Detect signal bursts using adaptive thresholding
- Validate ADS-B Mode S preambles
- Recover 112-bit ADS-B payloads
- Convert binary payloads into hexadecimal messages
- Decode ADS-B header fields:
  - Downlink Format (DF)
  - Capability (CA)
  - ICAO Address
  - Message Type (Type Code)
- Visualize the received signal and decoded packets

---

## Signal Processing Pipeline

```
Raw WAV Recording
        │
        ▼
Read I/Q Samples
        │
        ▼
Magnitude Calculation
        │
        ▼
DC Offset Removal
        │
        ▼
FFT Spectrum Analysis
        │
        ▼
Burst Detection
        │
        ▼
ADS-B Preamble Validation
        │
        ▼
Payload Extraction
        │
        ▼
Bit Recovery
        │
        ▼
Hexadecimal Message
        │
        ▼
ADS-B Header Decoding
```

---

## Technologies Used

- Python
- NumPy
- SciPy
- Matplotlib
- Wave

---

## Project Structure

```
ADSB_Project/
│
├── ADSB_python_project_file.py
├── audio_data.wav
├── README.md
└── figures/
    ├── iq_samples.png
    ├── fft_spectrum.png
    ├── magnitude.png
    ├── preamble.png
    └── decoded_packet.png
```

---

## Methodology

### 1. Read the Recorded Signal

The ADS-B recording is stored as a stereo WAV file containing complex I/Q samples sampled at 2 MHz.

The first channel represents the In-phase (I) component while the second channel represents the Quadrature (Q) component.

---

### 2. Construct the Complex Signal

The received complex signal is formed as

\[
x[n] = I[n] + jQ[n]
\]

where

- I = In-phase component
- Q = Quadrature component

---

### 3. Compute Signal Magnitude

The signal magnitude is calculated as

\left|x\left[n\right]\right|=\ \sqrt{I^{2}\ +\ Q^{2}}

This simplifies burst detection by removing phase information.

---

### 4. Remove DC Offset

The mean of both I and Q channels is removed before performing frequency analysis.

---

### 5. Frequency Analysis

A 262,144-point Fast Fourier Transform (FFT) is computed to inspect the received spectrum and verify the presence of ADS-B transmissions.

---

### 6. Burst Detection

Candidate ADS-B packets are identified using a threshold based on the signal statistics.

Threshold:

```
Threshold = Mean + 3 × Standard Deviation
```

Samples exceeding the threshold are considered potential ADS-B bursts.

---

### 7. Preamble Validation

Detected bursts are compared against the expected ADS-B Mode S preamble pulse locations.

Expected pulse locations:

```
0
3
8
11
```

Only bursts matching this pattern are accepted.

---

### 8. Payload Extraction

After detecting the 8 μs preamble, the decoder extracts the following 224 signal samples corresponding to the 112-bit ADS-B payload.

---

### 9. Bit Recovery

Each ADS-B bit occupies two samples.

Decision rule:

```
Sample 1 > Sample 2  →  Bit = 1

Otherwise            →  Bit = 0
```

---

### 10. ADS-B Header Decoding

Recovered bits are converted into hexadecimal and decoded according to the ADS-B specification.

Decoded fields include:

- Downlink Format
- Capability
- ICAO Address
- Type Code

---

## Example Output

Decoded ADS-B message:

```
Message:
8A572C75376C4D5AAE198A5FA899

Downlink Format : 17
Capability      : 2
ICAO Address    : 572C75
Type Code       : 6

Message Type:
Surface Position
```

This indicates a valid ADS-B Extended Squitter message transmitted by an aircraft while operating on the airport surface.

---

## Results

The decoder successfully:

- Processed over two million complex I/Q samples
- Identified ADS-B signal bursts
- Recovered valid 112-bit ADS-B packets
- Decoded Mode S Extended Squitter messages
- Extracted aircraft metadata directly from recorded radio transmissions

---

## Challenges

Some of the major engineering challenges encountered during development included:

- Processing large WAV files efficiently
- Removing receiver DC offset
- Detecting weak ADS-B bursts
- Correctly identifying ADS-B preambles
- Recovering aligned binary payloads
- Eliminating bit-shift errors during decoding

These challenges were resolved through iterative debugging and signal processing improvements.

---

## Future Improvements

Possible extensions include:

- CRC verification
- Aircraft callsign decoding
- Airborne position decoding
- Altitude decoding
- Velocity decoding
- Live RTL-SDR support
- Real-time aircraft tracking
- Interactive visualization dashboard

---

## Skills Demonstrated

- Digital Signal Processing
- FFT Analysis
- Wireless Communications
- Software Defined Radio Concepts
- Binary Protocol Decoding
- Python Programming
- Scientific Computing
- Data Visualization
- Signal Detection
- Complex I/Q Processing

---

## References

- ICAO Annex 10
- RTCA DO-260B Minimum Operational Performance Standards
- ADS-B Documentation
- NumPy Documentation
- SciPy Documentation
- Mode S and ADS-B Technical References

---

## Author

Uttakarsh Karna

Electrical Engineering Student

North Carolina State University

Focus Areas:

- Signal Processing
- Wireless Communications
- RF Systems
- Embedded Systems. 
