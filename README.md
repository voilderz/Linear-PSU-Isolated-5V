# Linear-PSU-Isolated-5V (v1.1)

Şebeke beslemeli, izoleli 5V DC güç kaynağı modülü.

![PCB Top](./Images/Top.png)

---

## Overview

Bu proje, 230V AC şebeke gerilimini alıp izole bir transformatör, tam dalga köprü doğrultucu ve anahtarlamalı step-down regülatör kullanarak kararlı **5V DC** çıkış üreten bir güç kaynağı kartıdır. Tasarım; giriş/çıkış aşırı akım koruması, gerilim darbe koruması (MOV) ve görsel güç göstergesi (LED) içerir.

## Changelog — v1.1 (February 2026)

- **Surge Protection:** Mains girişine `B72314S2271K101` (275VAC MOV) entegre edildi.
- **Isolation Layout:** AC ve DC bölgeleri PCB üzerinde fiziksel olarak ayrıştırıldı.
- **Branding:** Silkscreen katmanına logo ve versiyon bilgisi eklendi.

## Technical Specifications

| Parameter | Value |
|---|---|
| Input Voltage | 230V AC (50/60Hz) |
| Output Voltage | 5V DC (Regulated) |
| Max Continuous Current | ~1.9 A |
| Isolation | Galvanic isolation via TR1 transformer |
| Regulator | TSR3-2450N (Traco Power), non-isolated step-down |

## Circuit Architecture

1. **Input Protection:** F1 (250mA slow-blow fuse) on the Line side, followed by RV1 (275VAC MOV) for surge/transient suppression. Fuse-before-varistor ordering ensures a safe open-circuit failure mode if the MOV fails short.
2. **Isolation:** TR1 (ASL171112) provides galvanic isolation between the AC primary and DC secondary.
3. **Rectification:** D1 (GBU810 full-wave bridge rectifier) converts AC to DC.
4. **Bulk Filtering:** C1 (6800µF electrolytic, low-ESR) smooths the rectified voltage; C4 (film) handles high-frequency ripple.
5. **Regulation:** U1 (TSR3-2450N) regulates the input to a stable 5V DC output.
6. **Output Filtering:** C2 (film) and C3 (polymer) suppress output ripple.
7. **Output Protection:** F2 (2.5A fast-acting fuse) protects against load-side short circuits.
8. **Power Indicator:** D2 LED, current-limited by R1 (1kΩ, 1% tolerance metal film resistor).

## Key Design Decisions

- **Fail-safe ordering:** F1 placed before RV1 so a shorted MOV blows the fuse instead of creating a sustained fault path.
- **Layered filtering:** Electrolytic + film + polymer capacitor combination across input and output stages targets both low-frequency ripple and high-frequency noise.
- **Off-the-shelf regulation:** A certified DC-DC module (TSR3-2450N) is used instead of a discrete feedback loop, reducing design complexity and failure risk.
- **Physical AC/DC separation:** Primary (AC) and secondary (DC) sections are laid out on separate regions of the PCB to reduce arcing/creepage risk.

## Bill of Materials (BOM)

| Reference | Component | MPN | Description |
|---|---|---|---|
| TR1 | Transformer | ASL171112 | 230V AC primary step-down transformer, galvanic isolation |
| D1 | Bridge Rectifier | GBU810_T0_00601 | Full-wave bridge rectifier |
| C1 | Capacitor (Electrolytic) | B41858C5688M000 | 6800µF, 25V, low-ESR bulk filtering |
| C2, C4 | Capacitor (Film) | MKS2C031001A00KSSD | High-frequency noise filtering |
| C3 | Capacitor (Polymer) | A7C5KN227M1EAAS029 | Low-noise output smoothing |
| U1 | Regulator | TSR 3-2450 | Non-isolated step-down DC-DC module |
| R1 | Resistor | MFR-25FTE52-1K | 1kΩ, 1% tolerance — LED current limiting |
| LED1 (D2) | LED | 3R4HD-7-TB | Red (660nm) power indicator |
| F1 | Fuse (Input) | 0697H0250 | 250mA slow-blow, primary overcurrent protection |
| F2 | Fuse (Output) | 80812500000 | 2.5A fast-acting, load-side protection |
| RV1 | Varistor | B72314S2271K101 | 275VAC MOV, surge/transient protection |
| J1, J2 | Connector | KF129V-5.08-2P | 5.08mm pitch screw terminal blocks |

> Full extended BOM: [`/BOM/BOM_Linear_PSU_Extended.csv`](./BOM/BOM_Linear_PSU_Extended.csv)

## Future Improvements

- Add EMI/EMC filtering (X/Y capacitors) on the AC input stage
- Verify creepage/clearance distances against IEC/IPC safety standards with physical measurement
- Evaluate inrush current limiting (e.g. NTC thermistor) on the transformer primary
- Confirm F2 fuse rating margin relative to the regulator's rated output current

## Project Structure

- `/Hardware`: KiCad schematic and PCB layout source files
- `/Production`: Gerber and drill files
- `/Docs`: Schematic PDF and component datasheets
- `/Images`: PCB renders (top/bottom) and technical captures

---

## Author

**Batın Sarı**
Linear-PSU-Isolated-5V

All Rights Reserved
