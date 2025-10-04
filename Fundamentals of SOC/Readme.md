# Fundamentals of SoC Design

## Objective

The primary objective of this project is to gain a comprehensive understanding of System-on-Chip (SoC) fundamentals and functional modelling of
the BabySoC using simulation tools (Icarus Verilog & GTKWave).
The project emphasizes the importance of functional modelling as the first step in the SoC design flow.

## System on Chip (SoC)

A System-on-Chip (SoC) is an integrated circuit that integrates most or all components of a complete electronic or computing system onto a single silicon chip (or substrate). 
Some key motivations and benefits:

- Size, power, cost: by integrating multiple functions on one chip, board-level complexity decreases, parasitics are reduced, and power/area overhead of inter-chip communication is eliminated.

- Performance: on-chip communication can be faster with lower latency than discrete chips.

- Integration: allows combining digital logic, memory, peripherals, interfaces, even analog / mixed-signal blocks.

- But tradeoffs: complexity, design difficulty, sometimes compromises (e.g. analog circuits or large memories may be harder to scale) 


An SoC is more than “just a chip” — it is a system (hardware + software + interconnect) collapsed into one substrate.

In embedded, mobile, IoT, or edge systems, SoCs are ubiquitous (smartphones, wearables, network devices, etc.).

### Block Diagram of a SoC

<div align="center">
<img width="318" height="158" alt="image" src="https://github.com/user-attachments/assets/beccb0fe-6832-419a-83ff-1cc6bf8f3d8c" />
</div>

## Components of a Typical SoC

|  Component |  Role in the System | Notes / Variations |
|--------------|-----------------------|----------------|
|  **CPU** | Executes instructions, runs software (OS, applications) | Could be single core or multicore (heterogeneous cores). Could be general-purpose (RISC, ARM, RISC-V) or domain-specific. |
|  **Memory (RAM/ROM/Flash)** | On-chip memory for data, instructions, caches | Types include SRAM, register files, L1/L2 caches; sometimes ROM/flash on chip or off-chip memory interface. |
|  **Peripherals / I/O interfaces** | Interfaces to external world (UART, SPI, I2C, USB, Ethernet, GPIO, timers, interrupt controllers, DMA, etc.) | May include accelerators (e.g. crypto, DSP, video, AI) as “peripheral-like” custom blocks. |
|  **Interconnect / Bus / Network-on-Chip (NoC)** | The “glue” for connecting cores, memory, peripherals — enabling data transfer, arbitration, etc. | Simple buses (AMBA, AHB, AXI) or more scalable NoC fabrics for high-performance SoCs. |
|  **Clock, Reset, Power Management** | Distributes clock signals, handles resets, regulates voltages, handles power gating / dynamic voltage/frequency scaling | Critical in low-power designs. |
|  **Control / System Logic** | Bus arbiters, crossbars, memory controllers, DMA controllers, interrupt controllers, glue logic | Helps with coordination, resource sharing. |
|  **Analog / Mixed-signal blocks** | ADCs, DACs, PLLs, oscillators, power regulators, RF front ends | Not every SoC includes these; if analog is required, the process becomes more complex. |

## How they interact / flow:

- The CPU or cores generate memory or I/O read/write requests.
- These requests travel via interconnect (bus, crossbar, or NoC) to memory or peripheral blocks.
- Peripherals respond or produce data.
- DMA engines may transfer bulk data without CPU involvement.
- Interrupt lines or events notify the CPU of peripheral activity.
- Power management logic may dynamically turn off parts of the chip or adjust voltage/frequency.

## Design Flow

<div align="center">
<img width="960" height="720" alt="image" src="https://github.com/user-attachments/assets/1eaa31cd-b24b-41f2-8f50-0b0e9c72681e" />
</div>

  
<details>
  <summary>Components of a SoC</summary>

A modern SoC usually consists of the following blocks:

### 1.Processor (CPU / MCU / MPU)

- Acts as the central brain of the SoC.

- Executes instructions, runs the operating system (if present), and controls other components.

- Can be a general-purpose CPU (like ARM Cortex-A/R/M cores) or a microcontroller core for simpler systems.

### 2.Memory Subsystem

Provides data storage and fast access.

Types of memory:

- **ROM / Flash**: Stores firmware or permanent programs.

- **RAM (SRAM/DRAM)**: Temporary data storage for active processes.

- **Cache**: High-speed memory for quick access to frequently used instructions/data.

### 3.Bus Interconnect / Network-on-Chip (NoC)

- Acts like the nervous system that connects all components.

- Examples: AMBA (AXI, AHB, APB), Wishbone, or proprietary NoC architectures.

- Ensures efficient data transfer between CPU, memory, and peripherals.

### 4.Peripheral Interfaces

Provide communication with the outside world.

Common interfaces include:

- **I²C, SPI, UART**: For communication with sensors and modules.

- **USB, Ethernet, PCIe**: For external device connectivity.

- **GPIOs (General Purpose I/O)**: For controlling LEDs, switches, etc.

### 5.Specialized Processing Units

Offload specific tasks from the CPU:

- **GPU (Graphics Processing Unit)**: Handles graphics rendering, display control, parallel computations.

- **DSP (Digital Signal Processor)**: For audio, image, and signal processing.

- **NPU / AI Accelerator**: For machine learning and neural network tasks.

### 6.Analog / Mixed-Signal Blocks

Allow the SoC to interact with real-world signals.

Examples:

- **ADC (Analog-to-Digital Converter)**: Converts sensor signals to digital.

- **DAC (Digital-to-Analog Converter)**: Converts digital data to analog output.

- **PLL/Clock Generators**: Generate stable timing signals.

### 7.Power Management Unit (PMU)

- Regulates power consumption, especially in battery-operated devices.

- Includes voltage regulators, clock gating, and sleep/wake-up logic.

### 8.Security Modules

- Provide data protection and secure boot.

- Examples: cryptographic accelerators, secure key storage, trusted execution environments (TEE).
</details>

<details>
<summary>BabySoC</summary>

### What is a BabySoC?

A **BabySoC** is a simplified System-on-Chip (SoC) model that is intentionally designed to be small, easy to understand, and suitable for learning SoC concepts.

It is not an industrial-grade chip but rather an educational prototype.

BabySoC integrates only the essential blocks of an SoC — a CPU core, memory, a basic interconnect, and a couple of peripherals — so that students and beginners can grasp the architecture and workflow of SoC design without being overwhelmed by complexity.

### Key Features of BabySoC

- **CPU Core**:
A simple RISC-V-based processor core that executes instructions.

- **Memory**:
Small ROM (for programs) and SRAM (for temporary data).

- **Peripherals**:
A 10-bit DAC to generate analog signals from digital outputs and simple GPIOs or timers.

- **Clock Management**:
A behavioural PLL to illustrate how SoCs generate internal clocks from an external source.

- **Interconnect**:
A lightweight memory-mapped bus connecting CPU, memory, and peripherals.

### Why "BabySoC"?

Compared to real-world SoCs (millions of gates, multiple cores, high-speed interfaces), BabySoC is tiny and minimalistic.

It’s meant as a teaching tool for:

- Understanding how different blocks of an SoC interact.

- Practicing functional modelling and simulation using tools like Icarus Verilog & GTKWave.

- Building confidence before moving on to RTL design and physical implementation (e.g., Sky130).

### Functional Modelling of BabySoC

Functional modelling is the first step in designing the BabySoC, where the focus is on what the system does, not on how it is implemented in hardware. It provides a behavioral representation of the SoC’s functionality to validate the design concept before writing RTL or performing physical design.

#### Purpose

- To verify the overall operation of the BabySoC.

- To test instruction execution, data flow, and control logic in a simplified model.

- To allow early software development without requiring the final hardware.

- To serve as a reference model for later RTL verification.


#### Approach

- Often implemented in high-level languages like C, C++, or SystemC.

- Focuses on correct functionality, ignoring timing, clock cycles, and hardware constraints.

- Outputs from the functional model are used as a golden reference to verify RTL behavior.

#### Benefits

- Detects architectural errors early.

- Reduces time and cost by minimizing redesigns in RTL.

- Enables parallel hardware-software co-development.

- Provides a clear understanding of BabySoC behavior before physical implementation.

In short, BabySoC is a small, open-source, educational SoC that gives learners a hands-on introduction to digital-analog integration and SoC fundamentals.

</details>


<details>
  <summary>Challenges</summary>

### 1. Architectural & Functional Challenges

- **Complexity of Integration**: Modern SoCs pack CPUs, GPUs, AI accelerators, memory, and many peripherals on a single chip. Coordinating them without conflicts is difficult.

- **Specification Ambiguity**: Misinterpretation of system requirements at early stages can lead to costly redesigns.

- **IP (Intellectual Property) Reuse & Compatibility**: Different third-party IP blocks may have different standards, requiring careful adaptation and verification.

### 2. Design & Verification Challenges

- **Functional Verification Overhead**: Up to 70% of design time goes into verifying that RTL matches specifications. Ensuring correctness of billions of transistors is not trivial.

- **Hardware-Software Co-Design**: SoCs often run complex operating systems and apps, requiring parallel development of hardware and firmware. Synchronization between the two is challenging.

- **RTL vs Functional Model Mismatches**: Keeping the high-level model, RTL, and software in sync is difficult.

### 3. Performance Challenges

- **Power Consumption**: Critical in smartphones, IoT, and wearables. Designers must balance performance with battery life.

- **Thermal Management**: High integration leads to localized heating (“hotspots”), which can affect reliability.

- **Scalability**: Adding more cores or accelerators stresses the interconnect (NoC/bus) and memory bandwidth.

### 4. Physical Implementation Challenges

- **Timing Closure**: Ensuring all signals meet required setup/hold times across the chip.

- **Area Constraints**: Fitting everything within a reasonable die size to optimize cost.

- **Manufacturing Variability**: As feature sizes shrink (5nm, 3nm), process variations affect yield and reliability.

- **Signal Integrity & Crosstalk**: Densely packed wires can cause unwanted interference.

### 5. Security Challenges

- **Hardware Security**: Preventing side-channel attacks, secure boot failures, and IP piracy.

- **Data Protection**: Ensuring encryption and isolation of sensitive data (especially in IoT and automotive systems).

### 6. Time-to-Market & Cost Challenges

- **Short Product Cycles**: Mobile and consumer devices demand quick releases, leaving little time for exhaustive testing.

- **High NRE (Non-Recurring Engineering) Costs**: Tape-out (final manufacturing) costs for advanced nodes can reach tens of millions of dollars. Any bug after tape-out is extremely costly.

- **Ecosystem Support**: Ensuring software tools, drivers, and compilers are ready when the SoC is launched.
  
</details>

## Role of Functional Modelling in SoC Design

### What is Functional Modelling?

Functional modelling is the early stage representation of an SoC’s behavior, where the focus is on what the system does rather than how it is implemented.

- Typically done using high-level languages (C, C++, SystemC, Python, MATLAB).

- Describes the intended functionality and data flow, without worrying about clock cycles, registers, or hardware constraints.

- Example: Modeling a CPU instruction set simulator (ISS) to check correctness of instruction execution before designing the RTL.

### Why Functional Modelling Comes Before RTL

RTL (Register Transfer Level) describes hardware at the cycle-accurate level (Verilog/VHDL). Jumping straight into RTL without validating functionality can lead to costly design errors.

Functional modelling ensures:

- **Early Validation of Specifications**
  
Confirms that the SoC’s architecture (datapath, control, memory hierarchy, communication protocols) works as intended.

- **System-Level Simulation**

Enables fast, abstract simulations to test algorithms, throughput, latency, and overall performance.

- **Exploration of Design Trade-offs**

Helps architects evaluate different approaches (e.g., CPU vs. accelerator, memory sizes, bus widths) without committing to RTL.

- **Software/Hardware Co-Development**
  
Embedded software teams can start development using the functional model even before RTL is available.

### Bridge to RTL Design

Once functionality is validated, the functional model acts as a golden reference during RTL development:

- RTL is written to match the functional model behavior.

- Verification engineers compare RTL simulation results against the functional model outputs.

- Ensures that functionality is correct before timing and physical constraints are applied.

### Why Before Physical Design?

- Physical design (place-and-route, clock tree synthesis, timing closure, etc.) deals with chip layout and manufacturing constraints.

- If functionality errors are discovered this late, fixing them is extremely expensive.

- Functional modelling avoids such late-stage rework by catching logic/architectural bugs early.

Functional modelling plays a critical role in verifying correctness, exploring architectures, and enabling early software development before investing in RTL and physical implementation.









# VSDBabySoC

## Overview

**VSDBabySoC** is an open-source, mixed-signal System-on-Chip (SoC) designed to integrate digital and analog components on a single chip. It serves as an educational tool for understanding and implementing SoC designs, particularly in the context of **RISC-V architecture**.

**VSDBabySoC** is part of the VSD Hardware Design Program (HDP), which aims to provide hands-on experience in SoC design, from RTL (Register Transfer Level) to GDSII (Graphic Data System II). The project emphasizes the importance of integrating digital and analog components, offering insights into the challenges and solutions in mixed-signal design.

## Key Features

- **RVMYTH Processor**: A simple RISC-V-based CPU core developed by RedwoodEDA and VLSI System Design (VSD). It was introduced in workshops aimed at teaching SoC design concepts. 

- **8x Phase-Locked Loop (PLL)**: Generates a stable clock signal for the SoC, ensuring synchronized operation of digital components.

- **10-bit Digital-to-Analog Converter (DAC)**: Facilitates communication with analog devices by converting digital signals to analog form.

<div align="center">
<img width="829" height="481" alt="image" src="https://github.com/user-attachments/assets/b632e80f-3e72-434c-b770-f8b0c6f1d98c" />

</div>


## 🎯 Why VSDBabySoC?

1.Educational Purpose

   - Helps beginners understand how different SoC components (CPU, clocking, memory) fit together.

   - Provides a real RTL-to-simulation flow using open-source EDA tools (Icarus Verilog, GTKWave, Yosys, etc.).

2.Bridging Core & SoC Design

   - Many students learn CPU design (like RVMyth) but don’t see how it plugs into a full SoC.

   - BabySoC bridges that gap by integrating the CPU into a complete system.

3.Demonstrates Integration Challenges

   - Clocking with PLL.

   - Memory interfacing with CPU.

   - Reset, testbenching, and SoC-level verification.

4.Open-Source Ecosystem

   - Encourages learning and research without expensive proprietary tools.

   - Can be extended for advanced research (FPGA prototyping, Sky130 tapeout, peripheral addition).

5.Scalable Learning

   - “Baby” SoC is small and simple, but the same concepts scale up to industrial SoC design.

## Conclusion

This repository provides a comprehensive overview of the fundamentals of System-on-Chip (SoC) design, covering key concepts such as SoC components, functional modelling, RTL design, interconnects, peripherals, and verification strategies. It highlights the importance of modular design, early functional validation, and careful consideration of performance, power, and security challenges in SoC development.

The BabySoC serves as a miniature, manageable SoC, allowing learners to go through conceptual design → functional modelling → RTL → verification, building a solid foundation for advanced SoC development and embedded system design.

The VSDBabySoC project successfully integrates a PLL (Phase Locked Loop), the RVMyth RISC-V core, and on-chip memory into a working System-on-Chip (SoC). The pre-synthesis simulation waveform (shown above) demonstrates the correct operation of all major blocks:

- The PLL locks onto the reference clock and generates a stable, higher-frequency CPU clock.
- The RVMyth core fetches and executes instructions from memory, showcasing proper pipeline operation and memory access.
- The SoC output (OUT) reflects the expected results of the executed program, confirming functional correctness.
