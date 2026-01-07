# 10m Transceiver Project

Compact portable 10 meter transceiver

## Phase 1: Softrock-Style Receiver

### Block Diagram

```
┌─────────────┐
│   Antenna   │
│   (10m)     │
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────────┐
│     RF Front-End                    │
│  ┌─────────────────────────────┐   │
│  │  10m Bandpass Filter        │   │
│  │  (28-29.7 MHz)              │   │
│  └────────────┬────────────────┘   │
│               │                     │
│               ▼                     │
│  ┌─────────────────────────────┐   │
│  │  RF Preamp (Optional)       │   │
│  │  ~10-20 dB gain             │   │
│  └────────────┬────────────────┘   │
└───────────────┼─────────────────────┘
                │
                ▼
┌─────────────────────────────────────┐
│  Tayloe Detector (QSD)              │
│  ┌──────────────────────────────┐  │
│  │  FST3253 Analog Switches     │  │
│  │  (Quadrature Sampling)       │  │
│  └────┬──────────────────┬──────┘  │
│       │                  │          │
│    I output          Q output       │
└───────┼──────────────────┼──────────┘
        │                  │
        │    ┌─────────────────────┐
        │    │  Quadrature LO      │
        │◄───┤  Si570 or Si5351    │
        │    │  (28-29.7 MHz)      │
        │    │  I2C Control        │
        │    └─────────────────────┘
        │                  │
        ▼                  ▼
┌────────────────────────────────────┐
│  I/Q Audio Processing              │
│  ┌───────────────┐ ┌─────────────┐│
│  │ I Channel     │ │ Q Channel   ││
│  │ - AC Coupling │ │ - AC Coupling││
│  │ - Gain Stage  │ │ - Gain Stage││
│  │ - LP Filter   │ │ - LP Filter ││
│  └───────┬───────┘ └──────┬──────┘│
└──────────┼─────────────────┼───────┘
           │                 │
           ▼                 ▼
┌────────────────────────────────────┐
│  USB Audio Interface               │
│  (or direct ADC to MCU)            │
│  - 48-96 kHz sample rate           │
│  - Stereo input (I/Q)              │
└────────────┬───────────────────────┘
             │
             ▼
      ┌─────────────┐
      │   USB to    │
      │     PC      │
      └─────────────┘
             │
             ▼
      ┌─────────────┐
      │  SDR        │
      │  Software   │
      │  (HDSDR,    │
      │   Quisk,    │
      │   etc.)     │
      └─────────────┘

┌────────────────────────────────────┐
│  Control Interface                 │
│  - I2C for LO tuning               │
│  - Optional: encoder, display      │
│  - MCU (Arduino, STM32, etc.)      │
└────────────────────────────────────┘
```

### Signal Flow Description

1. **RF Front-End**: 10m signals from antenna pass through bandpass filter (rejects out-of-band signals and images)
2. **Optional Preamp**: Low-noise amplifier if needed for sensitivity
3. **Tayloe Detector**: FST3253 switches sample RF at 4× LO frequency, producing I/Q baseband outputs
4. **Quadrature LO**: Programmable oscillator generates 0°, 90°, 180°, 270° clock phases
5. **I/Q Processing**: Op-amp stages provide gain and lowpass filtering for audio range
6. **USB Audio**: Stereo ADC digitizes I/Q signals for computer processing
7. **Control**: Microcontroller manages LO tuning via I2C

### Key Design Parameters

- **Frequency Range**: 28.0 - 29.7 MHz (10m amateur band)
- **LO Frequency**: Same as receive frequency (zero-IF architecture)
- **Baseband BW**: DC to ~24 kHz (covers SSB, CW, digital modes)
- **Sample Rate**: 48 or 96 kHz (stereo I/Q)
- **Dynamic Range**: Target 90+ dB with good layout and components

## Project Structure

```
10m-transceiver/
├── hardware/           # KiCad project files
│   ├── softrock-rx.kicad_pro    # KiCad project
│   ├── softrock-rx.kicad_sch    # Schematic
│   ├── softrock-rx.kicad_pcb    # PCB layout
│   └── BOM.md                    # Bill of materials
├── datasheets/         # Component datasheets
├── firmware/           # Microcontroller code (if used)
└── README.md           # This file
```

## Getting Started

1. Open KiCad and load `hardware/softrock-rx.kicad_pro`
2. Start with the schematic design
3. Refer to BOM.md for component selection
4. PCB layout after schematic is complete

## Next Steps

- [ ] Design RF bandpass filter (28-30 MHz)
- [ ] Complete Tayloe detector schematic
- [ ] Design I/Q amplifier stages
- [ ] Add USB audio interface
- [ ] Add LO control (Si5351A + I2C)
- [ ] Layout PCB with proper RF practices
- [ ] Order components and PCBs
- [ ] Assemble and test
