---
title: Learning Electronics Through Gaming - TC - 02 - Arithemetic
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
lastmod: 2024-09-01T14:52:52.913Z
---

## Initial

Our focus turns to the arithmetic challenges next. These will build more enhanced gates for future use. There are some binary based challenges also but are not discussed in this challenge.

## Double Trouble

For me, this is the first new real challenge in TC. There are 4 inputs, and if 2 or more inputs are positive, then the output is positive.

I had thought about making some intricate design to solve this however, it became obvious that a basic one was available. As the only conditon for a positive output was a minimum of 2 positive inputs, a simple `AND` connected to every permutation of inputs is all that is needed [(1,2), (1,3), (1,4), (2,3), (2,4), (3,4)], and each output of the `AND` circuits is fed into `OR` circuits. As we have recently discovered three input `OR` gates, they will come in handy.

![Double Trouble](<double trouble diagram.png>)

## Odd Number Of Signals

A similar challenge to the above in which 4 inputs are presented with one output being the conditional check of if the count of positive inputs are true are not.

Despite how difficult this can appear, there is already a simple way to check if two bits positivity are even or odd, by using an `XOR` gate. If only one bit is positive (odd), then the output is true. Using that to our advantage is the key.

It's also worth noting that their is a constaint on number of components used (3).  Three `XOR` gates can be used to solve this.

![Odd Number](<odd number diagram.png>)

## Counting Signals

This challenge adds to the previous two challenges. The documentation mentions that this is not to be expected to be a neat solve.  This is also our first introduction into a milti-bit output like seen in the bus type challengtes in MHRD.

The truth table is initially complex to absorb but when we apply the previous two challenges, it becomes a lot clearer.

![Counting Signals Truth Table](<counting signals truth table.png>)

### 1st bit

There are three inputs for the output, the first to represent the lowest bit. Focusing on the permutations of this bit, it matches the output of the previous "Odd Number of Signals" challenge. Using this logic, the first output bit is complete.

### 2nd Bit

This is identical to the Double Trouble output. Build that and connect to the 2nd bit.

### 3rd Bit

As the 3rd bit is only positive when all inputs are true, then this is a simple `AND` chain.

There is a small fix needed however.  When all bits are positive, the 2nd bit reports true which is not the expected input.  To fix this, an `XOR` gate is used to negate the 2nd bit when the 3rd bit is positive.

![Counting Signals Diagram](<counting signals diagram.png>)

## Half Adder

Identical to the MHRD solution where the `SUM` output is an `XOR` of the two inputs and the `CARRY` is an `AND` gate.

![Half Adder](<halfadder diagram.png>)

## Double The Number

This is our first challenge dealing with *bytes* in TC. Remember that a *byte* is just a collection of 8 bits, which each bit corresponding to a certain value.  This was observed earlier in the binary racer challenge.  There's also only two components available for this chellange, a `Byte Splitter` which converts the byte input to individual bits, and an `8-bit Maker` which does the opposite.

The goal here is to double the input and the solution is trivial.  If the input value is `1`, then the expected output should be `2`. To wire this up, the first output pin of the splitter is mapped to the *second* input of the maker.  Same logic applies for an input of any other bit. The last pin in the splitter can be ignored. This unlocks the `splitter` and `maker` for future use.

![Double The Number](<double the number.png>)

## Full Adder

Another familiar piece from MHRD, however unlike the last time, there are no `MUX` components available. Not to worry, a simple solution exists.

![Full Adder Truth Table](<full adder truth table.png>)

### SUM Output

For the first 4 ticks, the output is the same as an `XOR` however the second 4 is the inverse.  If you recall, an `XOR` can be thought of as a bit flipper, i.e. whatever the value of the first input is, it will be inverted when the second input is true. Using this logic, a second `XOR` gate can be used.

### CARRY Output

Simple solution for this. Connect each input permutation to an `AND`, and connect the inputs to 3-pin `OR`. When there are two or more inputs positive, the `CARRY` should output positive so this is an elegant way to achieve this.  This unlocks the `Full Adder` component.

![Full Adder Diagram](<fulladder diagram.png>)

## Bit Switch

This introduces a new component, a `Switch` that will only output the input when the `enable` input is set.  This is particularly useful when joining multiple outputs together so that there are no errors.  This challenge asks us to make an `XOR` using only two switches and two `NOT` gates.

The solution for this is simple. Looking at the first input, routing this into a `switch` will not output, however taking the second input and using a `NOT`, then connecting it to the `switch` will output correctly. Same wiring goes for the second input.

Both outputs of the switches can be joined together instead of having to use an `OR` for example.  This unlocks a `switch` and an `8-bit switch`.

![Bit Switch](<bit switch.png>)

## Byte OR

Moving onto the `Byte OR`. Perform a bitwise `OR` operation on all 8 bits of two inputs.  Firstly, split the input bytes using a splitter, then connect each bit back to a maker.

![Byte OR](<byte or.png>)

## Byte NOT

Applying similar logic to the above except for one input and 8 `NOT` gates.

![Byte NOT](<byte not.png>)

## Adding Bytes

Another similar style challenge, this time there are two inputs to be added together, as well as a carry in, so this appears to make an 8-bit full adder. Starting out as before, split the input bytes to bits. Next, line out 8 1-bit full adders. Connect up each corresponding bit into each adder, then the chaining of the carry bits begins.  Start by hooking up the `Carry` input to the full adder. Next, connect the `Carry` output of the first adder to the `Carry` input of the second adder. Continue this chain up to the last adder, where the `Carry` of the eigth adder is outputted to the `Carry` out. This unlocks the `Byte Adder`.

![Adding Bytes](<adding bytes.png>)

## Signed Negator

Signed bytes have returned.  this component is a negator, i.e. flip the sign of the input.  As learned previously, the *most significant bit* represents the sign of a signed byte. To negate a number like before, just flip all the bits, then add 1.  First, take the input and feed it into an `8-bit NOT`. Next feed the output to a `Byte Adder` with the `carry` input being fed a positive bit. This will increment the value by 1. This unlocks the `Negator` component.

![Signed Negator](<signed negator.png>)

## Conclusion

We've gone as far as we can in the Arithmetic section.  Next our attention will turn to the memory components.
