# Static Random Access Memory-Architecture
 
Implemented a 4-word × 4-bit static RAM (16 bits total) built from D flip-flops, basic logic gates and tri-state output buffers. Designed and simulated in **Logisim Evolution**.

## Architecture
![Circuit](circuit.png)

## Components
| Block | Parts | Function |
|---|---|---|
| Address decoder | 4 NOT + 4 AND | Turns A1 A0 into one active word select line |
| Write control | 3-input AND + NOT on RD | Produces CS · RD̅ · CLK, the write pulse |
| Write gates | 4 AND (one per word) | Clock only the selected row |
| Memory array | 16 D flip-flops | Store the bits; data inputs are shared down each column |
| Read gates | 16 AND | Pass a cell's Q only if its word is selected |
| Bit combiners | 4 OR (4-input) | Merge the 4 words of each column into one bit |
| Output stage | 4 controlled buffers | Drive `o3`–`o0` only when CS · RD · OE = 1 |
 
## Features
 
- 4 words × 4 bits (16 bits of storage)
- 2-bit address decoder (2-to-4)
- Clock-driven write: data is stored on the rising edge of the clock
- Chip Select (CS), Read/Write (RD) and Output Enable (OE) controls
- Tri-state output buffers, so the outputs are disconnected (Hi-Z) when not reading

## Signals
 
| Signal | Direction | Description |
|---|---|---|
| `A1`, `A0` | Input | Address, selects 1 of 4 words |
| `I3`–`I0` | Input | Data in |
| `o3`–`o0` | Output | Data out (LEDs) |
| `CS` | Input | Chip Select, enables the chip |
| `RD` | Input | 1 = read, 0 = write |
| `OE` | Input | Output Enable, drives the outputs when 1 |
| `CLK` | Clock | Write clock |
 
## Operation
 
| Operation | CS | RD | OE | Result |
|---|---|---|---|---|
| Write | 1 | 0 | X | Input data is stored in the selected word on the clock's rising edge |
| Read | 1 | 1 | 1 | The selected word appears on `o3`–`o0` |
| Idle | 0 | X | X | Nothing is written, and the outputs are Hi-Z |
 
Address decoding:
 
| A1 | A0 | Word selected |
|---|---|---|
| 0 | 0 | Word 0 |
| 0 | 1 | Word 1 |
| 1 | 0 | Word 2 |
| 1 | 1 | Word 3 |
 
## How to use (Logisim Evolution)
 
1. Open `ram_4x4.circ` in Logisim Evolution.
2. Enable the clock: **Simulate → Tick Enabled** (`Ctrl+K`). The clock is set to 1 Hz. Use **Tick Once** (`Ctrl+T`) to step it by hand.
3. Use the **Poke tool** (hand) to toggle the inputs.
### Example: write `1010` to word 0, then read it back
 
1. Set `A1 A0 = 00` and `I3 I2 I1 I0 = 1010`.
2. Set `CS = 1`, `RD = 0` and wait for a clock tick. The data is stored.
3. Set `CS = 0` (or `RD = 1`) to stop writing.
4. Set `CS = 1`, `RD = 1`, `OE = 1`. The LEDs show `o3 o2 o1 o0 = 1010`.
## Notes
 
- Always set `CS = 0` or `RD = 1` before changing the address or data. While `CS = 1` and `RD = 0` with the clock running, the selected word is rewritten on every clock pulse.
- Flip-flops start undefined, so write a word before reading it.
- The output LEDs are off when `CS = 0`, `RD = 0` or `OE = 0`, because the tri-state buffers are disabled.
 
