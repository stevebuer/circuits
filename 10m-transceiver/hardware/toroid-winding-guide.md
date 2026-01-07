# Toroid Winding Guide for 10m Bandpass Filter

## Toroid Selection for 28-29 MHz

### Recommended Cores

**T50 Series (0.5" diameter):**
- **T50-6** (Yellow/White) - Best for 10-50 MHz, µ = 8, Q > 200
- **T50-2** (Red) - Good for 2-30 MHz, µ = 10, higher inductance/turn
- **T50-7** (White/Red) - Good alternative, µ = 5

**T37 Series (0.37" diameter, more compact):**
- **T37-6** (Yellow/White) - Excellent for 10m, smaller than T50
- **T37-2** (Red) - Also works well

**T68 Series (0.68" diameter, higher Q):**
- **T68-6** (Yellow/White) - Maximum Q, best performance
- Use if you have them available

### Inductance vs Turns Formula

For powdered iron toroids:

$$L(\mu H) = \frac{A_L \times N^2}{1000}$$

Where:
- $A_L$ = Inductance index (nH per turn²)
- $N$ = Number of turns

**AL values:**
- T50-6: AL = 40 nH/turn²
- T37-6: AL = 30 nH/turn²
- T68-6: AL = 57 nH/turn²
- T50-2: AL = 49 nH/turn²

### Winding Calculations

**For 680 nH on T50-6 (recommended):**

$$N = \sqrt{\frac{L \times 1000}{A_L}} = \sqrt{\frac{680 \times 1000}{40}} = \sqrt{17000} \approx 13 \text{ turns}$$

**For 680 nH on T37-6:**
$$N = \sqrt{\frac{680 \times 1000}{30}} \approx 15 \text{ turns}$$

**For 1 µH on T50-6 (double-tuned design):**
$$N = \sqrt{\frac{1000 \times 1000}{40}} = \sqrt{25000} \approx 16 \text{ turns}$$

## Practical Winding Instructions

### Wire Selection
- **26 AWG** (0.4mm) - Good balance, easy to handle
- **28 AWG** (0.3mm) - More turns fit, slightly higher Q
- **24 AWG** (0.5mm) - Fewer turns, lower DC resistance
- Use enameled magnet wire (not insulated hookup wire)

### Winding Technique

1. **Prepare wire:**
   - Cut ~30cm (12") length
   - Scrape enamel from last 1cm on each end with knife or sandpaper

2. **Start winding:**
   - Insert wire through toroid center
   - Leave 5cm (2") tail for connection
   - Wind in same direction (clockwise when viewed from top)

3. **Spacing:**
   - **Close-wound**: Turns touching (maximum inductance)
   - **Spaced**: Small gaps between turns (lower capacitance, higher Q at HF)
   - For 10m: Close-wound is fine, or space by 1-2mm

4. **Distribution:**
   - Spread turns around 270° of toroid (3/4 of circumference)
   - Leave small gap for start/finish wires
   - Even spacing for best performance

5. **Finish:**
   - Secure with small dab of glue or clear nail polish
   - Measure inductance if you have LC meter
   - Fine-tune by adding/removing turns

### Tapped Coils (for impedance matching)

For double-tuned filter with tap points:

```
     ──── Full coil (16T)
      ↑
      │
      ├── Tap at 5T (1/3 height from ground)
      │
      ↓
     GND
```

**Winding:**
- Wind complete coil (16 turns)
- Count from ground end
- Scrape enamel at tap point (turn 5)
- Solder tap wire
- Cover with glue/coating after testing

## Filter Designs Using Toroids and Silver Mica Caps

### Design A: 3rd-Order Chebyshev (Original Design)

**Advantages with your parts:**
- Silver mica caps are perfect (stable, low-loss, high Q)
- Toroids are ideal for 680 nH inductors
- Very predictable, repeatable results

**Components:**
- L1, L2: 680 nH → **13 turns on T50-6** (close-wound, 26 AWG)
- C1, C2: 150 pF → Use **silver mica** (±5% tolerance is fine)

**Alternative with available values:**
If you have these silver mica values:
- 100 pF: Use two in parallel (200 pF total) - slightly wider bandwidth
- 150 pF: Perfect match
- 180 pF: Slightly lower frequency, can compensate with fewer turns

### Design B: Double-Tuned Coupled Resonator (Highly Recommended)

**Perfect for your components!**

```
         C1                           C3
  IN ────││──┬──────────────────┬────││──── OUT
  50Ω       │                   │           50Ω
            │    k ~ 0.1        │
           L1 ◄────────────────► L2
            │                   │
         ┌──┴──┐             ┌──┴──┐
         │ C2  │             │ C4  │
         └──┬──┘             └──┬──┘
           GND                  GND
```

**Components:**
- **L1, L2**: 1 µH → **16 turns on T50-6** (26 AWG wire)
  - Wind both coils identically for best matching
- **C2, C4** (main tuning): 30 pF + variable trimmer
  - Use **27-33 pF silver mica** as main capacitor
  - Add 5-30 pF trimmer cap if you have them (for tuning)
  - Or just use 30 pF silver mica if close enough
- **C1, C3** (coupling/matching): 10 pF silver mica

**Physical layout:**
- Mount L1 and L2 on PCB or breadboard
- Space coils ~15-20mm apart (center-to-center)
- Orient coils at 90° to each other to minimize coupling
- Or use same-axis spacing to control coupling

**Coupling adjustment:**
- Closer spacing = tighter coupling = wider bandwidth
- 90° orientation = looser coupling = narrower bandwidth
- Start with ~20mm spacing, adjust for desired bandwidth

### Design C: Higher Inductance Option (Fewer Standard Values)

If you have limited silver mica values, use higher inductance:

**For 2.2 µH coils:**
- Turns: N = √(2200×1000/40) ≈ **23 turns on T50-6**
- Tuning cap: 13-15 pF (easier to get or parallel combination)
- Bandwidth controlled by coupling

**For 4.7 µH coils:**
- Turns: N = √(4700×1000/40) ≈ **34 turns on T50-6**
- Tuning cap: 6-7 pF
- Very easy to tune (less sensitive to cap tolerance)

## Silver Mica Capacitor Selection

### Typical Available Values (E12 series)
10, 12, 15, 18, 22, 27, 33, 39, 47, 56, 68, 82, 100, 120, 150, 180, 220, 270, 330, 390, 470...

### Creating Custom Values

**Series combination** (reduces capacitance):
$$C_{total} = \frac{C_1 \times C_2}{C_1 + C_2}$$

Example: 100 pF + 100 pF in series = 50 pF

**Parallel combination** (increases capacitance):
$$C_{total} = C_1 + C_2$$

Example: 100 pF + 50 pF in parallel = 150 pF

### Design Strategy

1. **Check what values you have**
2. **Calculate needed L/C ratio** for 28.85 MHz resonance
3. **Choose nearest silver mica values**
4. **Calculate required inductance** (adjust turns accordingly)
5. **Wind and test**

### Example Design Process

Let's say you have: 27 pF, 33 pF, 47 pF, 100 pF, 150 pF, 220 pF silver mica caps

**For double-tuned design:**
- Target: 1 µH with 30 pF → 28.85 MHz
- Available: 27 pF or 33 pF
- Choose 27 pF: Need L = 1.13 µH → **17 turns on T50-6**
- Choose 33 pF: Need L = 0.92 µH → **15 turns on T50-6**

Both will work! The filter will be centered slightly off 28.85 MHz but within 10m band.

## Building Tips with Your Components

### Layout Considerations
1. **Silver mica orientation**: Mount horizontally for lowest stray inductance
2. **Ground plane**: Important for stability, reduces stray capacitance
3. **Toroid mounting**: Stand vertically or horizontally, consistent orientation
4. **Lead length**: Keep capacitor leads short (<5mm if possible)

### Testing Without Equipment

**Resonance check (with receiver):**
1. Connect filter to antenna and receiver input
2. Tune receiver across 10m band
3. Look for maximum signal strength in desired frequency range
4. Adjust coupling between coils if needed

**Rough frequency check:**
- Connect signal generator/antenna to input
- Connect diode detector + voltmeter to output
- Sweep frequency, note peak (should be ~29 MHz)

### Tuning Procedure

1. **Build filter with calculated values**
2. **Test frequency response**
3. **If too high in frequency:**
   - Add 1 turn to each inductor, OR
   - Add small cap in parallel (5-10 pF)
4. **If too low in frequency:**
   - Remove 1 turn from each inductor, OR
   - Use smaller capacitor value
5. **Adjust coupling** (spacing between coils) for bandwidth

## Quick Reference Table

| Inductance | T50-6 Turns | T37-6 Turns | Wire (AWG) |
|------------|-------------|-------------|------------|
| 470 nH     | 11          | 13          | 26         |
| 680 nH     | 13          | 15          | 26         |
| 1.0 µH     | 16          | 18          | 26         |
| 1.5 µH     | 19          | 22          | 26         |
| 2.2 µH     | 23          | 27          | 26-28      |
| 3.3 µH     | 29          | 33          | 26-28      |
| 4.7 µH     | 34          | 40          | 28         |

All values are approximate - measure and adjust as needed!

## Recommended Build

**For easiest construction with toroids and silver mica:**

**Double-tuned coupled resonator filter:**
- Wind 2× identical coils: 16 turns on T50-6 or T37-6
- Use 27-33 pF silver mica for tuning (C2, C4)
- Use 10 pF silver mica for coupling (C1, C3)
- Space coils 15-20mm apart
- Tune by adjusting spacing or adding trimmer caps

**Advantages:**
- Only need 2-3 different capacitor values
- Hand-windable with your toroids
- Adjustable (can tune for best performance)
- Forgiving design (doesn't require exact values)
- High Q with silver mica and toroid construction

This will give you an excellent 10m bandpass filter with the parts you already have!