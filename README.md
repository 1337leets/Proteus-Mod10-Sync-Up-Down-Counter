# Proteus Mod-10 Synchronous Up/Down Counter

A 4-digit synchronous decimal (BCD) up/down counter built in Proteus from
discrete 74-series logic, using JK flip-flops, gates, multiplexers and
7-segment decoders. Carry-in and carry-out let it be cascaded further. It was
a self-directed summer project, designed from scratch.

![4-digit counter schematic](images/4_digit_counter.png)

## Features

- Counts 0000 to 9999 up or down, selected with a single `UP/DOWN` input
- Fully synchronous, since every flip-flop shares the same `CLK` and nothing ripples between digits
- `CARRY-IN` enables counting, and `CARRY-OUT` lets another counter block be chained after the last digit
- Each digit drives its own common-anode 7-segment display

## Structure

The four digits are identical stages, and each stage is made of the parts below.

| Part | Chip | Role |
|------|------|------|
| 4 × JK flip-flop | 2 × 74HC76 | Holds the BCD digit |
| Next-state logic | 74HC08 (AND), 74HC11 (3-input AND), 74HC21 (4-input AND), 74HC32 (OR) | J/K inputs for counting up and for counting down, and the terminal-count decode that enables the next digit |
| Up/down selection | 74HC157 (quad 2:1 MUX) | Switches the flip-flop inputs between the up and the down logic with `UP/DOWN` |
| Display driver | 74LS247 + 330 Ω resistors | BCD to 7-segment decoder for the common-anode display |

The asynchronous set and reset inputs of every flip-flop are tied inactive,
so the wrap-around from 9 to 0 (and from 0 to 9 when counting down) is not
done by resetting the flip-flops. Instead, the J/K equations for both
directions were derived with Karnaugh maps so that the next-state logic itself
only walks through the ten BCD states.

`CARRY-IN` gates the J/K inputs of the first digit, so it works as a count
enable. A digit only advances when every lower digit is at its terminal count
(9 when counting up, 0 when counting down) and `CARRY-IN` is high. The same
condition on the last digit produces `CARRY-OUT`.

The design uses JK flip-flops. A modern implementation would more likely use
D flip-flops or a single programmable device, but building it from JK
flip-flops and gates was the point of the exercise.

## Development History

### Early attempts (`history/early/`)

Before the mod-10 line, the counters were built in several smaller steps.

| File | Step |
|------|------|
| `jk-ff-counter-5.pdsprj` | First JK flip-flop counter, 74LS series with a 74LS47 display decoder |
| `senkron-counter-10.pdsprj` | Synchronous decade counter |
| `3-digit-counter.pdsprj` | Three cascaded digits |
| `1-digit-up-down-counter.pdsprj` | First up/down digit, moved to the 74HC series |
| `3-digit-up-down-counter.pdsprj` | Three up/down digits |

### Mod-10 line (`history/`)

Each version adds one step toward the final design.

| File | Step |
|------|------|
| `mod-10-synch-counter.pdsprj` | Single synchronous mod-10 digit with the up/down selection multiplexers |
| `mod-10-sync-2-digit-up-down-counter.pdsprj` | Two cascaded digits, up/down |
| `mod-10-sync-2-digit-up-down-counter-with-carry.pdsprj` | Two digits with carry-in / carry-out |
| `mod-10-sync-4-digit-up-down-counter-with-carry.pdsprj` (root) | Four digits with carry, final version |

## Tools
Proteus 9
