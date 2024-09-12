---
title: Learning Electronics Through Gaming - TC - 05 - CPU (2)
date: 2024-09-23
draft: false
author: Richey Ward
summary: Completing the CPU - Part 2
tags:
    - Turing Complete
categories:
    - Digital Electronics
description: Completing the CPU - Part 2
series:
    - Electronics Through Gaming - TC
series_order: 5
slug: 05-cpu-2
lastmod: 2024-09-04T12:48:24.071Z
---

## Introduction

In this section, we continue building and refining the **Overture** CPU. With the ability to load instructions from memory and manage immediate values, the design starts to take shape. We also introduce conditional logic, making the CPU capable of handling jumps, loops, and custom scripting.

---

## Program

The first step is to replace the manual instruction input with a RAM block. This block will store instructions that can be loaded and eventually edited, allowing for custom scripting. Connect the RAM block where the input was previously located, and add a `Counter` to serve as a pointer, determining which instruction is fetched.

![Program](program.png)

---

## Immediate Values

To allow the loading of immediate values into a register, the `IMMEDIATE` instruction is introduced. When this instruction is flagged, the next 6 bits of the instruction are stored directly in `REG 0`. Although the last two bits should technically be ignored, simulations show that using them as-is works fine. An additional enable condition is wired to `REG 0`, allowing input only when the `IMMEDIATE` flag is set, with the input value piped into the register.

![Immediate Values](<immediate values.png>)

---

## Turing Complete

The final piece needed to complete the **Overture** CPU is the conditional logic component, previously designed. This component checks the value of `REG 3` and, if the specified condition is met, resets the `COUNTER` to the value in `REG 0`. This enables loops and jumps within the program.

To implement this, connect the `CONDITIONAL` component to the diagram, feeding the instruction and `REG 3` into its inputs. The output bit and conditional flag are sent to an `AND` gate, ensuring that the `COUNTER` is overridden only when both conditions are met. `REG 0` is then connected to the `COUNTER`.

![Complete 1](<complete 1.png>)

![Complete 2](<complete 2.png>)

---

## Overture Complete

At this point, we’ve reached a similar stage as the final design in MHRD, but Turing Complete is just beginning to expand. The **Overture** architecture is still basic, but it provides a foundation for more complex designs and operations in the future.

---

## ADD 5

With the **Overture** CPU completed, we now move on to writing some machine code to run on it. The goal is to create a program that reads an input, adds 5 to it, and then outputs the result.

### 1 - Read Input

To write the code, click the pencil icon on the `RAM` block to open the binary editor. Using the "View Instructions Definitions" helper, we can determine the correct instruction for copying the input to `REG 1`. This instruction value is `177`.

![add5 - Read Input](<add5 read input.png>)

### 2 - Add 5

Next, we need to load the value `5` into `REG 2`, which requires two steps. First, use the `IMMEDIATE` instruction to store `5` in `REG 0`, then copy the value from `REG 0` to `REG 2`.

![add5 - add5-1](<add5 add 5-1.png>)

![add5 - add5-2](<add5 add 5-2.png>)

### 3 - Perform Calculation

Now perform an `ADD` operation using the `COMPUTE` instruction.

![add5 - perform calc](<add5 - perform calc.png>)

### 4 - Output Result

Finally, copy the result from `REG 3` (where the result of the `ADD` is stored) to the `OUTPUT`.

![add5 - output](<add5 - output.png>)

After running the program, the output will display the result of adding 5 to the input. This unlocks the IDE, which will be useful for future challenges.

![add5 Solution](<add5 solution.png>)

---

## Calibrating Laser Cannons

The next challenge involves calculating the circumference of an asteroid using the equation `2*π*r`, where `r` is the input. For simplicity, we round `π` to 3, so the calculation becomes multiplying the input by 6.

### Handling Multiplication

There is no multiplication component, but we can simulate multiplication through repeated additions.

### Assembly Editor

The IDE makes coding for this task easier by allowing us to map operation codes (opcodes) to custom names. We’ll create a simple multiplication routine using repeated addition.

### Understanding Register Usage

There are 6 `Register` components, each with specific usage characteristics. It’s important to document these so that we can manage the storage and retrieval of data effectively:

```txt
REG 0 -> Stores immediate inputs and resets the counter.
REG 1 -> First operand for calculations.
REG 2 -> Second operand for calculations.
REG 3 -> Stores the output of calculations and input for conditionals.
REG 4/5 -> Can be used for long-term storage.
```

### Solution

Here’s a simple solution to multiply the input by 6:

```txt
Grab input (e.g. 10)
Double it (20)
Add the original input (30)
Double again (60)
```

The code to achieve this:

```matlab
in_to_r1  # grab input
r1_to_r2  # copy to r2
ADD       # r1 + r2
r3_to_r1  # copy output to r1
r1_to_r2  # copy to r2
ADD       # r1 + r2
r3_to_r1  # copy output to r1
ADD       # r1 + r2
r3_to_out # result out
```

This solution unlocks the `Robotron 9000`, a controllable robot.

---

## Spacial Invasion

Looking over the documentation, two things are mentioned that will become useful, first, the use of `const` which can declare a constant in the IDE. This makes life easier when trying to keep track of the Robotron's actions. Second is the `label` which is very useful for creating loops.  When using the `label` with a keyword, the address of the label is noted.

The Robotron starts out behind some boxes and the mission is to shoot all the "rats". This is similar to Space Invaders.  Running the code empty will show that the rats make their way down the screen until they reach a certain point and the game is over.  It should also be noted that the Robotron can 'see' directly in front of itself. If there is nothing, then a value of `0` is shown, however if there is something like a rat, then a value is shown, and the robot should then shoot.

![Spacial Invasion](<spacial invasion.png>)

### Starting Position

The Robotron must move to a more effective position before engaging the rats. Using the `OUTPUT` command, the Robotron can turn right, move forward, turn left, and then move forward again.

```matlab
const LEFT = 0
const FORWARD = 1
const RIGHT = 2
const WAIT = 3
const SHOOT = 5

# Move to position
RIGHT
r0_to_out
FORWARD
r0_to_out
r0_to_out
LEFT
r0_to_out
FORWARD
r0_to_out
```

### Waiting and Shooting

![Spacial Invastion - After Moving](<spacial invastion - after moving.png>)

Next, Rob will wait at that position until something appears directly in front of him. The pseudocode for this would be: check in front, if nothing then wait another turn, or if there is something then shoot. As mentioned, loops are very useful for this part.

Two new instructions also: `JEZ` means "Jump if Zero" which is perfect when the input is zero and `JMP` is just "Jump always"

```matlab
label wait
WAIT         # Do nothing
r0_to_out
label shoot  # Come back here after shooting
in_to_r3     # Check input
wait
JEZ          # Jump back if Zero
SHOOT        # Shoot if not
r0_to_out
shoot
JMP          # Jump
```

Rob waits patiently for a rat to appear where he quickly dispatches it until they are all gone.

## Storage Cracker

A simple challenge where a PIN has to be cracked.  I've read some complex solutions on this, and if we wanted to be efficient, this could be an option however I am lazy, so this code is also lazy.

This just simply outputs a number and increments it until the right one is found. No need to fancy tactics.

```matlab
1
r0_to_r2

label check
r3_to_out
r3_to_r1
ADD
check
JMP
```

---

## Masking Time

The goal here is to find the day of the week based on a given date. The task simplifies to finding the `mod 4` of the date.

### Modulus

If you're familiar with basic division, there can be instances where a remainder is returned, i.e. `14 / 5 = 2 remainder 4`. With modulus, it doesn't matter what the result is, only the remainder. So `mod 5` of `14` is `4`. If there is no remainder left, then the value is `0`. The `mod` calculation is represented with a `%`.

### Getting MOD 4

To get `mod 4`, we use the first two bits of the date value:

```txt
27 % 4 = 3
27     - 0b00011011
3      - 0b00000011
```

An `AND` operation with `0b11` (3) isolates these bits. The code is simple:

```matlab
3
r0_to_r2
in_to_r1
AND
r3_to_out
```

---

This concludes the section. In the next part, we’ll explore more advanced functionality for the **Overture** CPU.
