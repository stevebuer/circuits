# RF Front-End Design - 10m Receiver

## Design Overview

The RF front-end consists of:
1. **10m Bandpass Filter** - Rejects out-of-band signals and images
2. **RF Preamp (Optional)** - Improves sensitivity and noise figure

## 1. 10m Bandpass Filter Design

### Specifications
- **Center Frequency**: 28.85 MHz (center of 10m band)
- **-3dB Bandwidth**: ~3 MHz (27.5 - 30.5 MHz)
- **Passband Ripple**: < 0.5 dB
- **Stopband Attenuation**: > 40 dB at ±10 MHz from center
- **Input/Output Impedance**: 50Ω
- **Topology**: 5th-order Chebyshev bandpass (good selectivity with reasonable component count)

### Filter Topology Selection

For HF receivers, common choices are:
- **Double-tuned coupled resonators**: Simple, good for wide bandwidth
- **Chebyshev ladder filter**: Better selectivity, steeper skirts
- **Crystal/ceramic filters**: Excellent selectivity but fixed frequency

For our wideband 10m coverage, we'll use a **5th-order Chebyshev ladder filter** (0.5dB ripple).

### Circuit Configuration

```
                L1      L2              L3      L4
    50Ω IN ──┬──┴┴┴──┬──┴┴┴──┬──────┬──┴┴┴──┬──┴┴┴──┬── 50Ω OUT
             │       │       │      │       │       │
             C1      C2      C3     C4      C5      C6
             │       │       │      │       │       │
            GND     GND     GND    GND     GND     GND
```

This is a 5th-order LC ladder filter (5 reactive elements in signal path).

### Component Calculations

For 50Ω impedance, 28.85 MHz center frequency, 3 MHz bandwidth (Q = 9.6):

**Normalized Chebyshev values (0.5dB ripple, 5th order):**
- g0 = 1.0000 (source impedance)
- g1 = 1.7058
- g2 = 1.2296
- g3 = 2.5408
- g4 = 1.2296
- g5 = 1.7058
- g6 = 1.0000 (load impedance)

**Bandpass transformation:**
- $\omega_0 = 2\pi f_0 = 2\pi \times 28.85 \times 10^6 = 181.3 \times 10^6$ rad/s
- $BW = 2\pi \times 3 \times 10^6 = 18.85 \times 10^6$ rad/s
- $FBW = BW/\omega_0 = 0.104$ (fractional bandwidth)

**Component formulas:**
- Series inductors: $L_s = \frac{R \cdot g}{2\pi f_0 \cdot FBW}$
- Series capacitors: $C_s = \frac{FBW}{2\pi f_0 \cdot R \cdot g}$
- Shunt inductors: $L_p = \frac{R \cdot FBW}{2\pi f_0 \cdot g}$
- Shunt capacitors: $C_p = \frac{g}{2\pi f_0 \cdot R \cdot FBW}$

**Calculated values (R = 50Ω):**

**Series elements (odd-numbered):**
- L1 = (50 × 1.7058) / (2π × 28.85×10⁶ × 0.104) = **454 nH** → **~11 turns on T50-6**
- L3 = (50 × 2.5408) / (2π × 28.85×10⁶ × 0.104) = **676 nH** → **~13 turns on T50-6**
- L5 = (50 × 1.7058) / (2π × 28.85×10⁶ × 0.104) = **454 nH** → **~11 turns on T50-6**

**Shunt capacitors (odd-numbered):**
- C2 = 1.2296 / (2π × 28.85×10⁶ × 50 × 0.104) = **130 pF** → **Use 120 or 150 pF silver mica**
- C4 = 1.2296 / (2π × 28.85×10⁶ × 50 × 0.104) = **130 pF** → **Use 120 or 150 pF silver mica**

**Input/Output matching capacitors:**
- C1, C6 = 1.7058 / (2π × 28.85×10⁶ × 50 × 0.104) = **181 pF** → **Use 180 pF silver mica**

### Practical Component Values (E12 series)

Using standard values and your available parts:
- **L1, L5**: 470 nH → **11 turns on T50-6 toroid** (26 AWG wire)
- **L3**: 680 nH → **13 turns on T50-6 toroid** (26 AWG wire)
- **C1, C6**: 180 pF silver mica (NP0/C0G ceramic also OK)
- **C2, C4**: 120 or 150 pF silver mica (120 pF for slightly higher frequency)

**Note:** Silver mica capacitors are ideal for this application - very stable, high Q, low loss. If using toroids, see toroid-winding-guide.md for detailed winding instructions.

### Alternate Simplified Design: 3rd Order

For easier construction, a 3rd-order filter:

```
              L1              L2
    50Ω IN ──┴┴┴──┬────────┬──┴┴┴── 50Ω OUT
                  │        │
                  C1       C2
                  │        │
                 GND      GND
```

**Component values (3r→ **13 turns on T50-6 toroid** (26 AWG wire, close-wound)
- C1, C2 = **150 pF silver mica**

This provides good performance with fewer components. Perfect for hand-wound toroids!
This provides good performance with fewer components.

---

## Alternative Bandpass Filter Designs

### Option 1: Double-Tuned Coupled Resonator (Classic Design)

**Best for:** Simplicity, easy tuning, wide bandwidth, **EXCELLENT WITH TOROIDS AND SILVER MICA**
**Pros:** Only 2 tuned circuits, adjustable coupling, forgiving design, hand-windable
**Cons:** Moderate selectivity, requires alignment

**⭐ RECOMMENDED if you have toroids and silver mica capacitors! ⭐**

```
            C1                    C3
    IN ─────││──┬──────────────┬──││───── OUT
    50Ω        │              │         50Ω
               │    M (k)     │
              L1 ◄────────► L2
               │              │
              C2              C4
               │              │
              GND            GND
```

**Design for 28.85 MHz, Q = 10:**

Each resonator tuned to 28.85 MHz:
- **L = 1 µH** (air-wound coil or **16 turns on T50-6 toroid** - see toroid-winding-guide.md)
- **C = 30 pF** (use **27-33 pF silver mica**, can add trimmer for fine tuning)
- Total C ≈ 30 pF → $f_0 = \frac{1}{2\pi\sqrt{LC}} = 28.85$ MHz

**With toroids:**
- **Coil winding:** 16 turns of 26 AWG magnet wire on T50-6 (yellow/white)
- Close-wound or slightly spaced (1-2mm between turns)
- Identical winding for both L1 and L2
- See toroid-winding-guide.md for detailed instructions

**Coupling coefficient k:**
- For 3 MHz bandwidth: k = BW/f₀ = 0.104
- Mutual inductance: M = k√(L1×L2) = 0.104 × 1µH = **104 nH**
- Physical spacing: ~0.5× coil diameter
**16 turns on T50-6, 26 AWG magnet wire**)
- C2, C4: **27-33 pF silver mica** (main tuning) + optional 5-30 pF trimmer
- C1, C3: **10 pF silver mica** (coupling/matching)

**Building notes:**
- Wind both inductors identically for best matching
- Mount toroids 15-20mm apart (center-to-center)
- Orient at 90° to minimize unwanted coupling, or adjust spacing for desired coupling
- Silver mica capacitors are ideal - very stable, high Q, low loss
- No trimmer? Use nearest silver mica value and adjust by ±1 turn on coilsght from ground
- Or use C1, C3 as capacitive dividers: C1 = C3 = 10 pF

**Components:**
- L1, L2: 1 µH (hand-wound: 15T on T50-6, 0.5mm wire)
- C2, C4: 30 pF + 10 pF trimmer (for tuning)
- C1, C3: 10 pF (coupling/matching)

---

### Option 2: Simple Pi-Network (1st Order, Very Simple)

**Best for:** Beginners, breadboard testing, minimal parts
**Pros:** Only 3 components, easy to build, low insertion loss
**Cons:** Poor selectivity, minimal out-of-band rejection

```
    50Ω IN ──┬──┴┴┴──┬── 50Ω OUT
             │   L   │
             C1      C2
             │       │
            GND     GND
```

**Design for 28.85 MHz:**

Using online calculators or formulas:
- L = **820 nH** (series)
- C1 = C2 = **220 pF** (shunt)

This provides impedance matching but minimal filtering. Good for initial testing.

---

### Option 3: Hairpin Bandpass Filter (PCB Microstrip)

**Best for:** PCB implementation, reproducible manufacturing, compact
**Pros:** No discrete inductors needed, PCB trace inductors, very repeatable
**Cons:** Requires PCB design, substrate dependent, harder to tune

For FR-4 PCB (1.6mm, εr = 4.5):

```
    ╔═════════╗    ╔═════════╗    ╔═════════╗
IN ═╣ Hairpin ╠═══╣ Hairpin ╠═══╣ Hairpin ╠═ OUT
    ╚═════════╝    ╚═════════╝    ╚═════════╝
       Res 1          Res 2          Res 3
```

Each hairpin resonator is a U-shaped trace:
- Length: λ/4 at 28.85 MHz ≈ **70 mm** (accounting for εr)
- Width: 2-3 mm (for 50Ω impedance)
- Spacing between hairpins: 1-2 mm (controls coupling)

**Advantages:**
- Professional appearance
- Consistent performance unit-to-unit
- No hand-winding coils
- Good for production

**Design resources:**
- Use Qucs, Sonnet, or online microstrip calculators
- AppCAD (free from Keysight) for initial design

---

### Option 4: Higher-Order Chebyshev (7th Order, Better Selectivity)

**Best for:** Strong adjacent signal rejection, narrow IF
**Pros:** Very steep skirts, excellent stopband rejection (>60 dB)
**Cons:** More components, more expensive, requires careful layout

```
                L1    L2    L3           L4    L5    L6
    50Ω IN ──┬─┴┴┴─┬─┴┴┴─┬─┴┴┴─┬──────┬─┴┴┴─┬─┴┴┴─┬─┴┴┴─┬── 50Ω OUT
             │     │     │     │      │     │     │     │
             C1    C2    C3    C4     C5    C6    C7    C8
             │     │     │     │      │     │     │     │
            GND   GND   GND   GND    GND   GND   GND   GND
```

**7th-order Chebyshev (0.5dB ripple) normalized values:**
- g1 = 1.9841, g2 = 1.0644, g3 = 2.9002, g4 = 0.9971
- g5 = 2.9002, g6 = 1.0644, g7 = 1.9841

**Calculated values:** (similar process as 5th order)
- Series L: 350-750 nH range
- Shunt C: 100-180 pF range

Use filter design software (e.g., RFSim99, Elsie, FilterWiz) for exact values.

---

### Option 5: Crystal Filter (Narrowband, Fixed Frequency)

**Best for:** CW operation, very narrow bandwidth, excellent selectivity
**Pros:** Extremely sharp selectivity, stable, predictable
**Cons:** Fixed frequency, narrow bandwidth (~2-5 kHz), requires crystals

```
        L1              L2              L3
IN ─────┴┴┴──┬─────────┴┴┴──┬─────────┴┴┴───── OUT
             │              │
            [Y1]           [Y2]
         28.850 MHz    28.850 MHz
             │              │
            GND            GND
```

**For 28.850 MHz crystals:**
- Use series-mode crystal filter
- L1, L2, L3: Calculate based on crystal series resistance (50-100Ω)
- Typical values: 10-47 µH
- Bandwidth: Determined by crystal spacing and coupling

**Note:** Crystals at 28+ MHz are less common. Alternative:
- Use 10 MHz crystals with upconversion
- Use ceramic resonators (wider bandwidth but available)

---

### Option 6: Parallel Trap (Notch) Filter

**Best for:** Rejecting specific interferers (broadcast, pagers, etc.)
**Pros:** Targets specific frequencies, low cost
**Cons:** Not a complete bandpass solution

```
                  ┌─┴┴┴─┐
                  │  L  │
    IN ───────────┤     ├───────── OUT
                  │  C  │
                  └──┬──┘
                    GND
       (Parallel resonant @ notch freq)
```

Use before main bandpass filter to reject strong out-of-band signals:
- FM broadcast rejection: Tune to 88-108 MHz
- Pager rejection: Tune to specific frequency

**Example - Reject 100 MHz:**
- L = 100 nH
- C = 25 pF
- Forms high impedance (notch) at 100 MHz

---

## Comparison Table

| Filter Type | Components | Selectivity | Tunable | Difficulty | Cost |
|-------------|-----------|-------------|---------|------------|------|
| 3rd-order Chebyshev | 4 | Good | No | Easy | $ |
| 5th-order Chebyshev | 6 | Better | No | Medium | $$ |
| 7th-order Chebyshev | 8 | Excellent | No | Hard | $$$ |
| Double-tuned | 6-8 | Good | Yes | Easy | $ |
| Pi-network | 3 | Poor | No | Very Easy | $ |
| Hairpin (PCB) | 0 discrete | Good | No* | Medium | $$ |
| Crystal | 2-4 + crystals | Excellent | No | Medium | $$$ |
| Trap notch | 2 | N/A | Yes | Very Easy | $ |

*Hairpin filters can be tuned by changing trace dimensions or adding trimmer caps

---

## Recommended Design Strategy
If you have toroids and silver mica capacitors:**
1. **START HERE:** Build **double-tuned coupled resonator** - easiest with your parts!
2. Wind 2× identical 16-turn coils on T50-6 cores
3. Use 27-33 pF and 10 pF silver mica caps
4. Adjustable spacing lets you tune for perfect response
5. See toroid-winding-guide.md for complete instructions

**
**For initial prototyping:**
1. Start with **Pi-network** or **double-tuned** for easy breadboarding
2. Test with signal generator and receiver
3. Evaluate if selectivity is sufficient

**For final design:**
1. If moderate selectivity OK: **3rd-order Chebyshev** (good compromise)
2. If strong adjacent signals: **5th or 7th-order Chebyshev**
3. If PCB manufacturing: **Hairpin filter** (professional, repeatable)
4. For CW only: **Crystal filter** with upconversion

**Hybrid approach:**
- Use simple 3rd-order Chebyshev for main filtering
- Add parallel trap for specific strong interferer (e.g., local FM station)
- Keep it simple unless you have specific rejection requirements

---

## Filter Design Tools

**Free software:**
- **Elsie** - Excellent LC filter design tool (Windows)
- **RFSim99** - RF circuit simulator with filter wizard
- **Qucs** - Open-source RF simulation
- **FilterWiz** - Online filter calculator
- **Python/SciPy** - signal.iirfilter() for custom designs

**Online calculators:**
- RF-Tools.com
- Chebyshev-tools.com
- Microwaves101.com

---

## 2. RF Preamp Design

### Specifications
- **Frequency**: 28-30 MHz
- **Gain**: 15 dB (voltage gain ≈ 5.6)
- **Noise Figure**: < 3 dB (target < 2 dB)
- **Input/Output Impedance**: 50Ω
- **Supply Voltage**: +12V
- **Power Consumption**: < 50 mA

### Transistor Selection

**BFP420** (NPN RF transistor, SOT-23):
- fT = 25 GHz
- NF = 1.2 dB @ 900 MHz
- Gain: 18 dB @ 1.8 GHz
- Excellent for HF/VHF

**Alternative: 2N5109** (TO-39, classic HF preamp transistor):
- fT = 600 MHz
- Lower cost, through-hole
- Proven HF performance

### Circuit Topology: Common Emitter with Feedback

```
                +12V
                 │
                 ├─── C7 (10µF, bypass)
                 │
                R3 (RFC or 100Ω)
                 │
                 ├─── L3 (RFC, 1µH)
                 │
          ┌──────┴──────┐
          │      C      │
    L1    │    BFP420   │    L4
IN ─┴┴┴─C1┤   (NPN)     ├C3─┴┴┴─ OUT
    50Ω   │             │         50Ω
          │    E        │
          └─────┬───────┘
                │
           R1   │   R2
          ┌─────┴─────┐
          │           │
         GND    C2    │
               100pF  │
                 │   GND
                GND
```

### Design Calculations

**Biasing (for BFP420):**
- Target IC = 10 mA (low noise, good linearity)
- VCE = 6V (mid-supply for maximum swing)
- VBE ≈ 0.7V

**Emitter resistor:**
- VE = 2V (provides stability)
- RE = VE / IC = 2V / 10mA = **200Ω**
- Use 220Ω standard value

**Collector resistor (if using resistive load):**
- VC = VCC - VCE - VE = 12 - 6 - 2 = 4V drop
- RC = 4V / 10mA = **400Ω**
- However, we'll use an RFC (radio frequency choke) instead for better RF performance

**Base bias (voltage divider):**
- VB = VE + VBE = 2 + 0.7 = 2.7V
- R2 = 10 × RE = 2.2kΩ (rule of thumb for stability)
- R1 = (VCC - VB) / (VB/R2) = (12 - 2.7) / (2.7/2200) = **7.6kΩ**
- Use R1 = 8.2kΩ, R2 = 2.2kΩ

**Impedance Matching:**

For 50Ω input/output:
- Use inductor/capacitor networks for conjugate matching
- Alternatively, use small-value series inductors (L1, L4) with shunt capacitors

**Simplified matching (broadband):**
- L1 = 680 nH (input series matching)
- C1 = 100 pF (AC coupling + part of match)
- L4 = 680 nH (output series matching)
- C3 = 100 pF (AC coupling)

### Component List - RF Preamp

- Q1: BFP420 (SOT-23) or 2N5109 (TO-39)
- R1: 8.2kΩ, 1/4W
- R2: 2.2kΩ, 1/4W
- RE: 220Ω, 1/4W
- L1, L4: 680 nH (air-wound or shielded inductor)
- L3: 1µH RF choke (RFC)
- C1, C3: 100 pF NP0/C0G
- C2: 100 pF bypass (NP0/C0G)
- C7: 10µF, 16V electrolytic (power supply bypass)
- Additional 100nF ceramic near VCC pin

### Expected Performance

- **Gain**: ~15 dB (depends on matching)
- **Noise Figure**: ~2 dB
- **Input IP3**: +10 dBm (with 10mA bias)
- **Power consumption**: 120 mW (10mA @ 12V)

---

## Combined RF Front-End

### Block Diagram with Component Values

```
        BPF (3rd order)              Preamp               To Tayloe
ANT ─────┤ 680nH/150pF ├──────────┤ BFP420 15dB ├────────┤ Detector
50Ω      └─────────────┘           └─────────────┘        50Ω
```

### Optional: Attenuator Switch

For strong signals, add switchable 10-20 dB attenuator between filter and preamp:
- Protects against overload
- Improves dynamic range on strong signals
- Can be PIN diode switched or mechanical relay

### Layout Considerations

1. **Keep RF traces short** (< 1 inch if possible)
2. **Use ground plane** on both sides of PCB
3. **Shield between filter and preamp** if close together
4. **NP0/C0G capacitors only** for RF paths (stable, low-loss)
5. **Toroid or shielded inductors** to minimize coupling
6. **Star grounding** for DC power, solid plane for RF ground
7. **Decoupling**: 100nF ceramic + 10µF electrolytic at power input

---

## Testing and Tuning

### Equipment Needed
- Signal generator (20-40 MHz)
- Spectrum analyzer or RF power meter
- 50Ω dummy load
- Multimeter

### Procedure
1. **DC bias check**: Verify VCE ≈ 6V, IC ≈ 10mA without RF applied
2. **Filter response**: Sweep 20-40 MHz, measure S21 (insertion loss)
3. **Gain measurement**: Measure output vs input power at 28.5 MHz
4. **Adjust**: Tweak L/C values if needed for desired response

### Tuning Notes
- If filter is too narrow: Increase capacitor values slightly
- If filter is too wide: Decrease capacitor values
- For gain adjustment: Change emitter resistor (lower RE = more gain, but higher distortion)

---

## Next Steps

1. Build filter on prototype board first
2. Test filter response before adding preamp
3. Add preamp and optimize bias point
4. Characterize noise figure and gain
5. Transfer to PCB layout with proper RF techniques
