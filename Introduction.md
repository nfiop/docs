## Introduction

## NAND flash chips

Wikipedia explains this in a much better way and in more depth.

What you should know is that there are two types of flash storage -
NOR flash and NAND flash and their names is because of the way they're
produced in the silicon-level - either as a "NOR-gate" cells in parallel
or a chain of transistors that are like a "chain" or "series" of them.

Again, Wikipedia explains this in depth [here](https://en.wikipedia.org/wiki/Flash_memory#Distinction_between_NOR_and_NAND_flash).

NAND flash chips are known for:
    - Having a lot of storage capacity
    - Not very reliable in terms of data integrity
    - Faster than NOR flashes in some cases

This is why we need things like ECC and bad block checking, but it gets
more complicated later on.

## Concepts in NAND flashes

Some of these concepts are also relevant for NOR flash chips, but we are
not interested about those chips in this project.

### Error correction codes

These are algorithms invented for the sake of storing and communicatin
data on a somewhat unreliable communication channel (noisy RF links,
NAND flash chips) and being able to fix a up-to-known number of errors
in a defined quantity of data in such channel (also known as the
strength of algorithm).

There are many algorithms, which each has its advantages and disadvantages.
It's expected to see a common list of those in some applications, although
experimentation is a valuable tool in this field.

### Bad blocks

Bad blocks are considered to be blocks that are unreliable enough for any sane
ECC algorithm to be able to fix errors in such block.

In other words, if a block has correctable errors, it might be considered still
functional, but once there are uncorrectable errors (even after repeated attempts)
then there's a need to mark that block as bad, to prevent potential data corruption
& loss.

Bad blocks, in that sense, are either:
 - blocks that are marked faulty by the factory
 - blocks that are weared, due to repeated erase-write cycles

While it's expected to see bad blocks being marked even on new chips,
seeing more & more **new** bad blocks is a good reason to worry on
state of the flash chip.
