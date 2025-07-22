# MacSnap SCSI Reloaded

This is a simple design clone to the original, using mostly all through hole components. It's designed this way to be period correct and easy to build for people who don't like soldering SMD components. The only SMD component is the 2.85V voltage regulator for SCSI termination.
Footprint is very similar. Like the original, the SCSI is provided through a LPT header.

The board has a few improvements:
- provides power to SCSI devices
- active SCSI termination
- voltage supervisor / reset ciruit

This folder contains the source KiKad files for this board.

![PCB](MacSnap%20SCSI.png)

## ![Button BOM]
### Mandatory

Here is the BOM for the board. Part number are what was tested on prototypes board but you may find alternatives easily, especially resistors and sockets

| Reference(s)          | Value      | Quantity | Notes                                  | Part number              |
|-----------------------|------------|----------|----------------------------------------|--------------------------|
| C1-2, C6, C7, C13-14  | 100nF      | 6        | Axial ceramic capacitor X7R            | Kemet C412C104K5R5TA7200 |
| C4                    | 1500pF     | 1        | Axial ceramic capacitor X7R 10%        | Kemet C410C152K1R5TA     |
| C8-11                 | 10uF       | 4        | Tantalum radial capacitor 2.5mm pitch  | Thomsen 481726           |
| D2                    | 1N5817     | 1        | Schottky Diode DO-15 20V               | Diotec 1N5817            |
| J1                    | 26 pins    | 1        | low profile shrouded IDC header        | TE Conn 5103309-6        |
| R1, R3                | 10kΩ       | 2        | standard 0.25W carbon film resistor    | TRU CFR0W4J0103          |
| R4                    | 470Ω       | 1        | standard 0.25W carbon film resistor    | TRU CFR0W4J0471          |
| R5, R6                | 47Ω        | 2        | standard 0.25W carbon film resistor    | TRU CFR0W4J0470          |
| RN1, RN2              | 110Ω       | 2        | 10 pins bussed resistor array          | Bourns 4610X-101-111LF   |
| U5, U6                | 16V8       | 2        | Programmable Logic Devices 15ns        | Microchip ATF16V8B-15PU  |
| U7                    | 53C80      | 1        | SCSI Interface IC 3MB/SEC CMOS SCSI I  | Zilog Z53C8003VSG        |
| U8                    | LDO        | 1        | 800mA SCSI-5 Active Terminator         | onsemi MC34268DR2G       |
| U10                   | RESET      | 1        | Supervisory Circuits 5V EconoReset     | DS1233D-15+              |
| -                     | LPT        | 1        | Parallel DB25 Female LPT to IDC 26-pin | ?? eBay / Amazon         |
| -                     | 14 pins    | 4        | Machine Pin break away header          | ?? eBay / Amazon         |
| Socket                | DIP-20     | 2        | THT DIP-20 socket                      | TRU TC-A 20-LC-TT-203    |
| Socket                | DIP-28     | 2        | THT DIP-28 socket                      | TRU TC-A 28-LC-TT-203    |
| Socket                | PLCC-44    | 1        | THT PLCC-44 socket                     | TRU TC-A-CCS 044-Z-T-203 |

Using sockets is recommended because it will allow you to reclaim both ROMs (to put back into factory) and PLDs

### Alternate

Both the 53C80 SCSI chip and MC34268 are obsolete components. NCR5380 in PLCC package is a working substitute. As for the active terminator LDO, there is an alternate build which consist of the following components:

| Reference(s)          | Value      | Quantity | Notes                                  | Part number              |
|-----------------------|------------|----------|----------------------------------------|--------------------------|
| U9                    | LDO        | 1        | LDO Voltage Regulators 800mA 2.85V     | A.D. LT1117CST-2.85      |
| C12                   | 10uF       | 1        | Tantalum radial capacitor 2.5mm pitch  | Thomsen 481726           |

When the LT1117CST build variant is used, do not populate U8

## ![Button Building]

PCB is 2 layers, the gerbers are avaible in the [release section](https://github.com/demik/MacSnap-SCSI/releases/download/v2.1/MacSnap_SCSI_Reloaded_2.1.zip)
You should be able to use any mainstream PCB manufacturer. Nothing special about this PCB, thickness should be 1.6mm

On JLCPCB, select _Create 2D barcode (Serial Number)_ to the option _Remove Order Number_. Others options to set:
- Printing: 2D barcode Only
- Code Type: QR Code
- Serial Number: MACSNAPSCSI
- QR Code Size: 10mm * 10mm
- QR Code Position: Specify Position

### Assembling

Building is straightworfard. You will need to start with the machine pin headers, then the following order is recommended:
- PDIP sockets
- axial resistors, axial capacitors
- resistors networks
- PLCC socket
- IDC header
- anything else left

### PLDs

You will need the following .jed files to program both ATF with this board:
- [DOV1b](../../PLD/DOV1B.jed) as DOV1b
- [DOV2_low](../../PLD/DOV2_low.jed) as DOV2

A TL866 II or T48 can program ATF16V8Bs just fine. Avoid ATF16V8BQL or ATF16V8C as this has not been tested and might lead into issues

### ROMs

Using real OEM ROMs is recommended, as replica ROMs do have different access timing, as well as source and sink power capabilities, and thus may not work with the MacSnap SCSI.
The following OEM sets have been tested without issues: 342-0341-B & 342-0342-A, 342-0341-C + 342-0342-B

<!-------------------------------[ Buttons ]----------------------------------->

[Button BOM]: https://img.shields.io/badge/BOM-FFDD21?style=for-the-badge&logoColor=white&logo=BookStack
[Button Building]: https://img.shields.io/badge/Building-FFDD21?style=for-the-badge&logoColor=white&logo=opensourcehardware
