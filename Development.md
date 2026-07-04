# Development stages

Roughly speaking, there are 4 stages - PoC, building an advanced adapter,
creating flash chip emulation and creating an open-source FTL for
block devices with raw NAND flashes.

It should be noted that the order of these stages could change if
required or just so desired.

## Proof-of-concept proxy & adapter

1. Raw NAND initialization & I/O PoC over Olimex A20 Olinuxino board
2. `ufedm` kernel module PoC

    - Basic functionality on top of `nandsim`
    - Tests with actual NAND hardware
    - Implemeting these set of features for advanced handling of NAND chips:
        - Passthrough mode:
            Skip userspace intervention (useful for tests and "pure-caching" mode)
        - Cache mode - keep a RAM-backed cache of a MTD device, so reading is fast I/O

3. TSOP48 NAND Socket soldering & testing
    - Might require tweaking NAND flash controller timings

## Build an advanced adapter (USB NAND flash controller)

4. USB-to-NAND flash controller as a Linux MTD device (`usbnfc`)
    - Should use other board than the Olimex, maybe something that's not Linux!
    - A transparent "raw" NAND flash controller should be required
    - Probably requires a special kernel module for this to work
5. Testing `ufedm` on top of `usbnfc` in various conditions.

At that point, there are many possibilities to build many adapters -
something like an STM32-based solution with its FMC silicon, a prebuilt
solution like SEGGER NAND flash evaluator board or maybe even an FPGA
board.

Different adapters should allow supporting NAND flash chips with bus
width of x8 or x16, a separate VCCQ from VCC input and different
packages (TSOP56, BGA packages, etc) and SPI chips as well.


## NAND flash chip emulation

6. Creating gateware & firmware for emulating a NAND flash chip with
   adjustable parameters such as page geometry, internal ECC algorithm
   and in-page data layout, bus width and VCC inputs.

## Building an open-source flash translation layer

Building a block device (USB thumb drive) with raw NAND flash and eMMC
or MMC cards using an embedded NAND flash controller with appropriate
firmware for a microcontroller, or maybe even gateware to speed things
up with FPGA.

7. USB to SPI/Parallel flash using an embedded microcontroller
8. USB to eMMC flash using an embedded microcontroller
    - Mainly as a comparing reference to the raw flash variant.
