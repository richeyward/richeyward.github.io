---
title: Learning Electronics Through Gaming - TC - 02 - Arithmetic
date: 2024-09-02
draft: false
author: Richey Ward
summary: Building arithmetic components
tags:
    - Turing Complete
categories:
    - Digital Electronics
description: Building arithmetic components
series:
    - Electronics Through Gaming - TC
series_order: 2
slug: 02-arithmetic
lastmod: 2024-09-12T17:14:48.641Z
---

## Introduction

Next, we focus on the arithmetic challenges in *Turing Complete*. These exercises involve building more advanced gates that will serve as the foundation for future tasks. While some binary-based challenges are also included, we’ll focus on the key arithmetic components here.

---

## Double Trouble

This challenge introduces a new concept in TC. You are presented with four inputs, and the output is positive if two or more inputs are positive.

Initially, I considered designing something intricate to solve this, but a simpler approach emerged. Since the condition for a positive output is at least two positive inputs, the solution involves using `AND` gates for each possible pair of inputs [(1,2), (1,3), (1,4), (2,3), (2,4), (3,4)]. The outputs of these `AND` gates are then fed into `OR` gates. The recently unlocked three-input `OR` gates are especially useful here.

![Double Trouble](<double trouble diagram.png>)

---

## Odd Number of Signals

This challenge is similar to Double Trouble, except the condition is to output true if there’s an odd number of positive inputs.

Despite the seemingly complex nature of the problem, the solution lies in a simple trick. An `XOR` gate can determine whether two bits are either both positive (even) or one is positive (odd). This principle extends to more inputs. With a constraint on the number of components (3), the solution is elegantly handled using three `XOR` gates.

![Odd Number](<odd number diagram.png>)

---

## Counting Signals

This challenge builds on the previous two, asking you to count the number of positive signals. The output is a multi-bit result, as seen in the bus-type challenges in MHRD.

The truth table may seem complex at first, but with the knowledge gained from the Double Trouble and Odd Number of Signals challenges, it becomes manageable.

![Counting Signals Truth Table](<counting signals truth table.png>)

### 1st Bit

The first output bit represents the lowest bit. By examining its permutations, you’ll notice they match the output of the Odd Number of Signals challenge. Use that logic for the first bit.

### 2nd Bit

This bit is identical to the Double Trouble solution. Connect it accordingly to the second output bit.

### 3rd Bit

The third bit is only positive when all inputs are true, which makes it a simple `AND` chain.

To correct a small issue (the 2nd bit remains true when all bits are positive), use an `XOR` gate to negate the 2nd bit when the 3rd bit is active.

![Counting Signals Diagram](<counting signals diagram.png>)

---

## Half Adder

The Half Adder is identical to the one in MHRD, where the `SUM` output is generated using an `XOR` gate, and the `CARRY` output is produced by an `AND` gate.

![Half Adder](<halfadder diagram.png>)

---

## Double the Number

This challenge introduces bytes in TC. Recall that a byte is simply a collection of 8 bits, each representing a value. The task here is to double the input value.

To achieve this, use a `Byte Splitter` to break down the input into individual bits and an `8-bit Maker` to reconstruct the output. The solution is straightforward: for each bit, the input is shifted to the next higher bit. The last bit can be ignored.

This challenge unlocks the `splitter` and `maker` for future use.

![Double The Number](<double the number.png>)

---

## Full Adder

Similar to the Half Adder, the Full Adder combines inputs but with an additional `carryIn` input. Unlike in MHRD, we don’t have `MUX` components here, but a simple solution still exists.

![Full Adder Truth Table](<full adder truth table.png>)

### SUM Output

The `SUM` output behaves like an `XOR` gate for the first 4 ticks, but the second 4 ticks produce the inverted result. Using the concept of an `XOR` gate as a bit flipper, we can use a second `XOR` gate to achieve this.

### CARRY Output

The `CARRY` output is straightforward. Connect each pair of inputs to an `AND` gate, and feed the results into a 3-input `OR` gate. This ensures that the `CARRY` output is positive when two or more inputs are positive. This unlocks the `Full Adder` component.

![Full Adder Diagram](<fulladder diagram.png>)

---

## Bit Switch

The `Switch` component is introduced in this challenge. The switch only outputs the input when the `enable` signal is active. This is useful when combining multiple outputs to avoid conflicts.

The task here is to build an `XOR` gate using only two switches and two `NOT` gates. To do this, route the first input into a `switch`. For the second input, route it through a `NOT` gate, then into another `switch`. The outputs from both switches can be joined together. This unlocks both the `Switch` and `8-bit Switch` components.

![Bit Switch](<bit switch.png>)

---

## Byte OR

For the Byte OR challenge, perform a bitwise `OR` operation on all 8 bits of two inputs. Split the bytes using a `splitter`, perform the `OR` operation on each bit, and then use a `maker` to reconstruct the output.

![Byte OR](<byte or.png>)

---

## Byte NOT

The Byte NOT challenge is similar to Byte OR, but here you negate all 8 bits of a single input using `NOT` gates.

![Byte NOT](<byte not.png>)

---

## Adding Bytes

This challenge is essentially an 8-bit full adder, with two input bytes and a `carryIn`. Start by splitting the bytes, then connect the corresponding bits to 1-bit full adders. Chain the `carryOut` from one adder to the `carryIn` of the next. The final `carryOut` is connected to the output. This unlocks the `Byte Adder`.

![Adding Bytes](<adding bytes.png>)

---

## Signed Negator

Signed numbers are back. The task is to negate a signed byte. As we’ve learned, the *most significant bit* (MSB) represents the sign. To negate the value, flip all the bits and add 1.

Feed the input into an `8-bit NOT`, then into a `Byte Adder`, with a `carryIn` set to 1. This will increment the value by 1, completing the negation. This unlocks the `Negator` component.

![Signed Negator](<signed negator.png>)

---

## Conclusion

With the Arithmetic section completed, we’re ready to tackle the next major topic: memory components.
