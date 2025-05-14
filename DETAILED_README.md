# Pic0Rick: Open-Source Ultrasound Pulse-Echo System

![GitHub repo size](https://img.shields.io/github/repo-size/kelu124/pic0rick?style=plastic)
![GitHub language count](https://img.shields.io/github/languages/count/kelu124/pic0rick?style=plastic)
![GitHub top language](https://img.shields.io/github/languages/top/kelu124/pic0rick?style=plastic)
![GitHub last commit](https://img.shields.io/github/last-commit/kelu124/pic0rick?color=red&style=plastic)

[![Join our Slack](https://badgen.net/badge/icon/slack?icon=slack&label)](https://join.slack.com/t/usdevkit/shared_invite/zt-2g501obl-z53YHyGOOMZjeCXuXzjZow)
[![made-with-Markdown](https://img.shields.io/badge/Made%20with-Markdown-1f425f.svg)](http://commonmark.org)
[![Patreon](https://img.shields.io/badge/patreon-donate-orange.svg)](https://www.patreon.com/kelu124)
[![Kofi](https://badgen.net/badge/icon/kofi?icon=kofi&label)](https://ko-fi.com/G2G81MT0G)

## Table of Contents

1. [Introduction](#introduction)
2. [Project Overview](#project-overview)
3. [Hardware Components](#hardware-components)
   - [Main Board](#main-board)
   - [Pulser Board](#pulser-board)
   - [Double PMOD Connector](#double-pmod-connector)
   - [High Voltage Module](#high-voltage-module)
   - [Multiplexer Support](#multiplexer-support)
4. [Software/Firmware](#softwarefirmware)
   - [rp2350_simple](#rp2350_simple)
   - [rp2350_mux](#rp2350_mux)
   - [rp2040_vga](#rp2040_vga)
   - [rp2040_shell](#rp2040_shell)
5. [Getting Started](#getting-started)
   - [Hardware Assembly](#hardware-assembly)
   - [Software Setup](#software-setup)
   - [Basic Usage](#basic-usage)
6. [Project Structure](#project-structure)
7. [Latest Developments](#latest-developments)
   - [Recent Accomplishments](#recent-accomplishments)
   - [Upcoming Changes](#upcoming-changes)
8. [Examples & Demonstrations](#examples--demonstrations)
9. [Related Projects](#related-projects)
10. [License & Credits](#license--credits)
11. [Community & Support](#community--support)
12. [FAQ](#faq)

## Introduction

The pic0rick is an open-source ultrasound pulse-echo system designed by Luc Jonveaux (kelu124). This project provides all the necessary hardware designs and software to create a functional, customizable ultrasound system based on the RP2040 microcontroller (the same chip used in Raspberry Pi Pico). The system is designed to be modular, with separate boards that connect via PMOD connectors, allowing for flexibility in configuration and upgrades.

## Project Overview

The pic0rick system consists of three main components that work together to generate and capture ultrasound signals:

1. **Main Board**: The central component based on RP2040 microcontroller with ADC and signal processing
2. **Pulser Board**: Generates the ultrasound pulses
3. **High Voltage Module**: Provides the necessary higher voltages for the pulser

The system follows a modular design philosophy, allowing users to mix and match components as needed. It has evolved from previous open-source ultrasound projects by kelu124, including echOmods, un0rick, and lit3rick.

## Hardware Components

### Main Board

**Latest version: v2**

The main board is the central component of the system, featuring:

- RP2040 microcontroller
- 60Msps, 10-bit ADC for high-speed signal acquisition
- Protected front-end against high-voltage pulses
- Time-gain compensation (TGC) system using an AD8331 amplifier (7.5 dB to 55.5dB)
- MCP4812 SPI DAC for controlling the gain
- Single PMOD connector for pulser interface
- Double PMOD connector for additional functionality (e.g., VGA output)

The board is designed with easy-to-solder SMD components, making it accessible for hobbyists and researchers alike.

### Pulser Board

**Latest version: v2**

The pulser board connects to the main board via the single PMOD connector and is responsible for generating the ultrasound pulses. Features include:

- Three-level pulse generation
- Uses MD1210 + TC6320 chips for pulse generation
- Connection point for the high voltage module
- Designed to interface seamlessly with the main board

### Double PMOD Connector

The double PMOD connector on the main board can be used for various additional functionalities:

- VGA output for real-time display of acquisitions
- Potential for other add-ons and extensions

### High Voltage Module

The high voltage module attaches to the pulser board and generates the necessary voltage levels:

- Generates ±25V for the pulser
- Simple design that stacks on top of the pulser board

### Multiplexer Support

The latest firmware supports the MAX14866 multiplexer, allowing for:

- Multiple channel acquisitions
- Control of switches for complex transducer setups

## Software/Firmware

The project includes several firmware versions tailored for different use cases:

### rp2350_simple

The basic firmware for the main board (latest RP2350 version) that provides:

- Basic acquisition capabilities
- Simple commands for controlling pulses and reading data
- DAC control for gain adjustment

Commands include:
- `start acq X1 X2 X3` - Start acquisition with pulse parameters (PP, PN, PD in ns)
- `write dac X` - Set the DAC value (gain control)
- `read` - Read the 8000-point acquisition data

### rp2350_mux

Enhanced version of the firmware with multiplexer support:

- All features of rp2350_simple
- Support for the MAX14866 multiplexer
- Additional commands for controlling mux switches

Additional commands:
- `write mux XXXX` - Send control word to the MAX14866 multiplexer
- `clear mux` - Open all switches
- `set mux` - Close all switches

### rp2040_vga

Firmware that enables VGA output:

- Uses one PIO for acquisition
- Uses another PIO for VGA signal generation
- Displays acquisition data on a VGA monitor
- Shows gain value on screen

### rp2040_shell

Basic shell interface for controlling the system:

- Simple command-line interface
- Basic control of the system

## Getting Started

### Hardware Assembly

1. **Assemble the Main Board**:
   - Follow the KiCad design files to order or manufacture the PCB
   - Solder the components according to the BOM
   - The v2 version is recommended for new builds

2. **Assemble the Pulser Board**:
   - Similar to the main board, use the provided design files
   - Ensure proper orientation when connecting to the main board

3. **Assemble the High Voltage Module**:
   - Stack on top of the pulser board
   - Verify connections before powering on

4. **Connect the Boards**:
   - Attach the pulser board to the main board via the single PMOD connector
   - Attach the high voltage module to the pulser board
   - Optionally connect a VGA display to the double PMOD connector

5. **Connect to a Transducer**:
   - Use appropriate connectors to attach an ultrasound transducer
   - Common transducers in the 2-10MHz range are suitable

### Software Setup

1. **Flash the Firmware**:
   - Choose the appropriate firmware (.uf2 file) based on your needs:
     - For basic usage: rp2350_simple.uf2
     - For multiplexer support: rp2350_w_mux.uf2
     - For VGA output: FIRMWARE_VGA.uf2
     - For shell interface: pio_adc_dac.uf2
   - Put the RP2040 into bootloader mode by holding BOOTSEL while connecting USB
   - Drag and drop the .uf2 file onto the mounted drive

2. **Prepare the Python Interface**:
   - The project includes Python scripts (pico.py) for controlling the system
   - Use the Jupyter notebooks in the repository for examples

### Basic Usage

1. **Connect to the Device**:
   ```python
   from pico import Pic0rick
   p = Pic0rick()
   ```

2. **Set Gain**:
   ```python
   p.write_dac(200)  # Set gain value (0-400 range)
   ```

3. **Acquire Data**:
   ```python
   # Start acquisition with pulse parameters (PP, PN, PD in ns)
   p.start_acq(100, 100, 1000)
   data = p.read()  # Read acquisition data
   ```

4. **Control Multiplexer** (if using rp2350_mux):
   ```python
   p.write_mux("FFFF")  # Write control word to mux
   p.clear_mux()  # Open all switches
   p.set_mux()  # Close all switches
   ```

5. **Visualize Data**:
   ```python
   import matplotlib.pyplot as plt
   plt.plot(data)
   plt.show()
   ```

## Project Structure

The project repository is organized as follows:

- **documentation/** - Contains images, datasheets, and reference materials
- **hardware/**
  - **adc/** - Main board design files (v1 and v2)
  - **pulser/** - Pulser board design files
  - **mux/** - Multiplexer board design files
  - **vga/** - VGA interface design files
- **software/**
  - **rp2350_simple/** - Basic firmware for the RP2350 board
  - **rp2350_mux/** - Enhanced firmware with multiplexer support
  - **rp2040_vga/** - Firmware for VGA output
  - **rp2040_shell/** - Shell interface firmware
  - **imgs/** - Example images and data

## Latest Developments

### Recent Accomplishments

- **Improved Pulse Timing**: Pulses are now tightly synchronized with acquisition start
- **Multiplexer Support**: Added support for the MAX14866 multiplexer
- **MUX Code Optimization**: Faster MUX control code (May 2025)

### Upcoming Changes

- **Main Board Tweaks**: Modifications to allow more space for PMODs (planned for Oct 2024)
- **WebAPI for Pico 2**: Development in progress
- **Version/Hash in Binary**: For better tracking of firmware versions

## Examples & Demonstrations

### Basic Acquisition

The system can perform basic pulse-echo ultrasound acquisitions with adjustable gain:

![Example Acquisition](/software/imgs/pico_shell/pic0gain_at_6.jpg)

### VGA Output

The VGA output can display real-time acquisitions on a standard VGA monitor:

![VGA Demo](/documentation/images/VGA_demo.gif)

### Compact Assembly

The system can be assembled into a compact form factor:

![Compact Assembly](/documentation/images/compact_assembly.jpg)

## Related Projects

The pic0rick builds upon several previous open-source ultrasound projects by kelu124:

1. [echOmods Project](https://github.com/kelu124/echomods/)
2. [un0rick Project](https://doi.org/10.5281/zenodo.377054)
3. [lit3rick Project](https://doi.org/10.5281/zenodo.5792245)

These projects share a common philosophy of open hardware and software for ultrasound imaging.

## License & Credits

This work is based on three previous TAPR projects and is licensed under:

- **Hardware**: TAPR Open Hardware License (www.tapr.org/OHL)
- **Software**: GNU General Public License v3
- **Documentation**: Creative Commons Attribution-ShareAlike 3.0 Unported License

Copyright Luc Jonveaux (kelu124@gmail.com) 2024

## Community & Support

- **Join the Slack Channel**: [Join our Slack](https://join.slack.com/t/usdevkit/shared_invite/zt-2g501obl-z53YHyGOOMZjeCXuXzjZow)
- **Support the Project**: 
  - [Patreon](https://www.patreon.com/kelu124)
  - [Ko-fi](https://ko-fi.com/G2G81MT0G)
- **Contributors**:
  - Abdelrahman
  - Lap
  - And other community members

## FAQ

### What transducers work with this system?

Most standard ultrasound transducers in the 2-10MHz range are compatible. The system has been tested with both commercial transducers and DIY piezoelectric elements.

### Can I use this for medical imaging?

While the system is capable of basic ultrasound imaging, it is **NOT** certified for medical use. It is intended for educational, research, and experimental purposes only.

### What's the resolution/depth capability?

With the 60Msps ADC, the system can achieve axial resolutions dependent on the transducer frequency. Typical depth penetration is in the range of 2-10cm depending on the medium and transducer.

### How do I add my own functionality?

The RP2040 has additional available resources beyond what's used for the core functionality. You can modify the firmware to add your own features, and the modular design allows for hardware extensions via the PMOD connectors.

### Is assembly difficult?

The main board uses SMD components but is designed to be relatively easy to solder with standard equipment. The project includes detailed BOMs and assembly instructions.

---

## Disclaimer

This project is distributed WITHOUT ANY EXPRESS OR IMPLIED WARRANTY, INCLUDING OF MERCHANTABILITY, SATISFACTORY QUALITY AND FITNESS FOR A PARTICULAR PURPOSE.
