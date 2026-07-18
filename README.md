# Linear-PSU-Isolated-5V

Şebeke beslemeli, izoleli 5V DC güç kaynağı modülü.

## Overview

Bu proje, 230V AC şebeke gerilimini alıp izole bir transformatör, tam dalga köprü doğrultucu ve anahtarlamalı step-down regülatör kullanarak kararlı **5V DC** çıkış üreten bir güç kaynağı kartıdır. Tasarım; aşırı akım koruması, gerilim darbe koruması ve görsel durum göstergesi (LED) içerir.

## Technical Specifications

| Parameter | Value |
|---|---|
| Input Voltage | 230V AC (50/60Hz) |
| Output Voltage | 5V DC (Regulated) |
| Regulator Efficiency | TSR3-2450N module, up to 90%+ (per manufacturer) |
| Isolation | Galvanic isolation via transformer |
| Protection | Input fuse (F1), MOV (RV1), output fuse (F2) |

## Circuit Architecture

1. **Input Protection:** F1 fuse (Line side) and RV1 varistor (B72314S2271K101, 275VAC MOV) protect against mains-borne voltage transients.
2. **Isolation:** TR1 (ASL171112) transformer provides galvanic isolation between AC and DC stages.
3. **Rectification:** D1 (GBU810 bridge rectifier) converts AC to DC.
4. **Filtering:** C1 (electrolytic) and C4 (film) capacitors filter the rectified voltage.
5. **Regulation:** U1 (TSR3-2450N, Traco Power) regulates the output to a stable 5V DC.
6. **Output Filtering & Indicator:** C2/C3 capacitors reduce output ripple; D2 LED (current-limited by R1, 1kΩ, 1% tolerance) serves as a power indicator.
7. **Output Protection:** F2 fuse provides a second layer of protection against load-side short circuits.

## Key Design Decisions

- **Fuse-varistor ordering:** F1 is placed before RV1 (on the Line side), ensuring the circuit fails safely open if the varistor fails short.
- **Dual-stage filtering:** Both input and output stages combine electrolytic and film capacitors to suppress low- and high-frequency noise together.
- **Simple, reliable topology:** Instead of a custom feedback loop, a certified off-the-shelf DC-DC module (TSR3-2450N) is used to reduce design complexity and failure risk.

## Bill of Materials (Summary)

| Reference | Component | MPN | Function |
|---|---|---|---|
| TR1 | Transformer | ASL171112 | Galvanic isolation |
| D1 | Bridge Rectifier | GBU810_T0_00601 | AC-DC conversion |
| U1 | Regulator | TSR3-2450N | 5V DC regulation |
| RV1 | Varistor | B72314S2271K101 | Voltage transient protection |
| F1 | Fuse (Input) | Fuse_Small | Overcurrent protection |
| F2 | Fuse (Output) | Fuse_Small | Load-side protection |
| R1 | Resistor | 1kΩ | LED current limiting |
| D2 | LED | - | Power indicator |

## Future Improvements

- Add EMI/EMC filtering (X/Y capacitors) on the input stage
- Measure and verify creepage/clearance distances against relevant safety standards
- Evaluate inrush current limiting on the transformer primary side

## Project Structure

- `/Hardware`: KiCad schematic and PCB layout source files
- `/Docs`: Schematic PDF and component datasheets
- `/Images`: Board renders and technical captures
