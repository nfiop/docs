# Why should I care about this?

## In short

The goals of this "modest" project are around the NAND flash chips' world.

In short, this is a project that strives to give these possibilites
on NAND flash chips:
 - Perform data extraction from various kinds of flash chips, by taking
   into consideration many types of ECC and data layouts that might be
   possible.

 - Perform data cloning from one chip to another (assuming same model)

 - Conduct experiments on ECC algorithms & data layouts

 - Conduct experiments & implemetation of a new flash translation layer
   for fast & reliable USB storage, using open-source components as much
   as possible.

 - Simulate a full-blown MTD stack on a certain chip, and being able to
   trigger cases and edge-cases that occur when dealing with flash chips
   in real-world scenarios (like uncorrectable errors, bitflips, 
   bad blocks) in a controlled environment.

 - Emulate a NAND flash chip without having the original chip, by using
   inexpesive hardware.

## How do I intend to implement this?

Using an appropriate hardware and hand-crafted software for this kind of
task.

The main component in development is a combination of `ufedm` kernel
module with `buildroot` environment for Olimex LIME2 Olinuxino A20 board
as an "adapter" for NAND flash chips.

This component is treated is a PoC, but can be used for completing some
goals of this project.

It should be noted that `ufedm` is built right now for the Olimex A20
platform, the idea is that it can be easily compiled for a general x86
system as well, and be tested with other planned adapters.

There are also 3 components in planning:
 - `mtd2usb` which should include firmware for a custom USB adapter to
   NAND flash chips, bundled with appropriate Linux kernel driver.
    - A NAND flash eval board (like the SEGGER one) seems promising.
    - Maybe an STM32-based solution could be possible as well.
 - `nandemu` which should include firmware and gateware for emulating
    various kinds of parallel/SPI NAND flash chips.
 - `usb2nand` which emulate a full-blown NAND flash controller for
    connecting a raw NAND flash chip and treat it as a block device,
    essentially creating an open-source flash translation layer, which
    evolves into what most people will call at that point a "USB thumb
    drive".

A variant of `usb2nand` for SPI NAND flash chips is called `usb2spinand`.

Another planned & niche component is `usb2mmc` - because eMMC & MMC
cards don't rely on an external NAND flash controller or FTL, the main
idea is to compare `usb2nand` and `usb2mmc` in aspects of endurance and
performance.

# The purposes of this project (in depth)

## 1. Full-fledged NAND flash chip reader and writer

There are many products that _can_ read and write to NAND flash chips,
like programming solution brands - XGecu, BeeProg, DediProg.

There are niche programmers based on CH341A chip, or RT809x programmers.

Some support lots of NAND flash chips, some support SPI nand flash chips
only - it's really up to the vendor to implement support (and charge
money for such features).

What I was mostly disappointed by all of these solutions is the **almost**
complete lack of support for ECC engines - it's up to you to fix errors
if they occur.

Some products allow you to erase the entire flash, without caring about
factory-marked bad blocks

BeeProg, for example, is only able to enable the internal ECC controller
of a flash chip (only if it exists, and if it does a terrible job, that
is _your damn problem_ to solve).

This project strives to provide a complete, full-fledged solution for
handling of NAND flash chips.

## A project for NAND flash chips (and only them)

The products I mentioned above are great! they do the job quite OK.

However, these products are intended for mass programming and are not
intuitive in cases when you need to modify a specific part or partition
in flash IC.

In some way, it seems like the products of these brands are struggling
with the quirks of these flash chips.

Their I/O operation granularity and UI are intended for mass programming
and not for tweaking and modifying small parts of a flash chip easily.

## Complete filesystem error-correction integration & simulation

UBIFS, YAFFS and JFFS all rely on some sort of ECC algorithm kicking in
when needed to ensure the written & read data is correct.

Because this platform allows ordinary ECC algorithms to be integrated
without any special modification of the filesystem implementation,
they can be technically mounted and tested against **real** flash chips.

While NANDSim provides a way to simulate a common set of NAND flash ICs,
you might be interested with working with real hardware, especially for
testing of actual hardware features implemented in the silicon die.

## Experimentation with data layouts & niche ECC algorithms

Linux has provided support for Hamming and BCH engines for quite a
while.

It is interesting however, to create benchmarks of software
implementations of niche ECC algorithms or unusual data layouts on the
actual NAND flash chip.
