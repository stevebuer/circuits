# Bill of Materials - 10m Softrock Receiver

## Core Components

### Tayloe Detector
- **FST3253** - Quad SPDT analog switch (TSSOP-16)
  - Qty: 1
  - Note: Used for quadrature sampling detector
  - Alternative: FST3257, CD4053 (lower performance)

### Local Oscillator
- **Si5351A** - I2C Programmable Clock Generator (MSOP-10)
  - Qty: 1
  - Frequency range: 8 kHz to 160 MHz
  - Generates quadrature clocks
  - Alternative: Si570 (requires external divider for quadrature)
- **27 MHz Crystal** - For Si5351A reference
  - Qty: 1

### Op-Amps
- **OPA2134** - Dual low-noise audio op-amp (DIP-8 or SOIC-8)
  - Qty: 2 (one for each I/Q channel)
  - Alternative: NE5532, TL072 (lower performance)

### RF Front-End
- **BFP420** - NPN RF transistor for preamp (SOT-23)
  - Qty: 1 (optional)
  - Alternative: 2N5109, BFR93A
- **Inductors** - For bandpass filter
  - TBD based on filter design
- **Capacitors** - For bandpass filter
  - TBD based on filter design

### USB Audio Interface
- **PCM2902** - USB Audio Codec (SSOP-28)
  - Qty: 1
  - Built-in USB interface
  - Alternative: Use external USB sound card

### Microcontroller (Optional)
- **ATmega328P** or **STM32F103** 
  - For I2C control of Si5351A
  - Display and encoder interface
  - Alternative: Use PC control via USB-I2C adapter

## Passive Components

### Capacitors
- **100nF ceramic** - Decoupling (0805 or through-hole)
  - Qty: ~20
- **10µF electrolytic** - Power supply filtering
  - Qty: 5
- **1000pF ceramic** - RF coupling
  - Qty: 4
- **Various RF capacitors** - For filters and matching
  - TBD

### Resistors
- **49.9Ω 1%** - Termination resistors (0805)
  - Qty: 4
- **10kΩ** - Pull-ups, biasing
  - Qty: ~10
- **Various values** - For gain stages
  - TBD

### Connectors
- **SMA connector** - Antenna input
  - Qty: 1
- **USB Type-B or Mini-USB** - For audio and power
  - Qty: 1
- **Header pins** - Programming, I2C
  - As needed

## Power Supply
- **78L05** - 5V regulator (TO-92)
  - Qty: 1
- **ADP150** - 3.3V LDO regulator (TSOT-5)
  - Qty: 1 (for digital sections)

## Notes
- Component values to be finalized during schematic design
- Prefer low-ESR capacitors for RF sections
- Use 1% resistors for critical gain-setting positions
- All ICs available from Mouser, Digikey, LCSC
