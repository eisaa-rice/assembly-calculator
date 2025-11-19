# 🧮 boolean and hexadecimal calculator

this is a small calculator project i made for my assembly class. it runs in a windows console and uses the irvine32 library for input and output. nothing fancy, just a console menu, some bitwise operations, and a couple of hex math options.

## ▶️ how to run

open the project in visual studio and run it (`f5`). it’s already configured with the irvine32 files, so it should build and run without any additional setup.

## ✨ how it works

once running, the program will display a menu of available operations:

```
---- Boolean Calculator ----------

1. x AND y
2. x OR y
3. NOT x
4. NOT y
5. x XOR y
6. x XOR y XOR x
7. x XOR y XOR y
8. x + y
9. x - y
0. Exit program
```

enter a number from 0 to 9 and the program immediately switches to that operation (no need to press enter).

for a quick rundown of how the program flows:

- 📋 prints the menu
- ⌨️ waits for a single key press
- 🎯 uses a lookup table to figure out which function to run
- ⚙️ calls that function
- 🔢 prints the result in hex
- 🔁 loops back to the menu

the lookup table is basically a manual switch statement built out of bytes and function pointers, and each operation uses the same pattern:

- shows a label
- reads one or two hex numbers
- does the instruction (`and`, `or`, `xor`, `not`, `add`, `sub`)
- prints the result

most options ask for two 32 bit hex values. the **not** options only ask for one.

for all operations, the input hex numbers are converted to binary, and then the operations are performed on their binary representations. the results are converted back into hex before printing.

you can input up to 8 hexadecimal digits because each hex digit is 4 bits and the program works with a 32 bit register (`8 hex digits * 4 binary digits each = 32 bit register`).

## 🧾 examples

for the following examples, we're going to declare:

`x` = `F0F0F0F0` → `1111 0000 1111 0000 1111 0000 1111 0000`.  
`y` = `0FF00FF0` → `0000 1111 1111 0000 0000 1111 1111 0000`.

however, you’ll be asked to enter `x` and `y` each time you select an operation, which lets you retry the same operation with different values.

### 🔗 x and y

| **and** keeps a `1` only if _both_ digits are `1`. otherwise, the resulting digit will be a `0`:

`1111 0000 1111 0000 1111 0000 1111 0000`  
`0000 1111 1111 0000 0000 1111 1111 0000`  
\=  
`0000 0000 1111 0000 0000 0000 1111 0000`

converted back into hex, the result is `00F000F0`.

```
Boolean AND:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            00F000F0
```

### 🔀 x or y

| **or** will return a `1` if _either_ digit is a `1`. otherwise, if _neither_ digit is a 1, the result will be `0`:

`1111 0000 1111 0000 1111 0000 1111 0000`  
`0000 1111 1111 0000 0000 1111 1111 0000`  
\=  
`1111 1111 1111 0000 1111 1111 1111 0000`

converted back into hex, the result is `FFF0FFF0`.

```
Boolean OR:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            FFF0FFF0
```

### ❗ not x

| **not** will simply flip each binary digit to the opposite value. so if it's a `1`, the digit becomes `0`, and vice versa:

`1111 0000 1111 0000 1111 0000 1111 0000`  
\=  
`0000 1111 0000 1111 0000 1111 0000 1111`

converted back into hex, the result is `0F0F0F0F`.

```
Boolean NOT:

Input a 32-bit hexadecimal operand: F0F0F0F0
The 32-bit hexadecimal result is:   0000000F
```

_**note:** the **not** operation flips all bits, but the program only keeps the lowest byte afterward. this is part of the starter template for the assignment, so the output shows only the last two hex digits reversed._

### ❗ not y

| **not** will simply flip each binary digit to the opposite value. so if it's a `1`, the digit becomes `0`, and vice versa:

`0000 1111 1111 0000 0000 1111 1111 0000`  
\=  
`1111 0000 0000 1111 1111 0000 0000 1111`

converted back into hex, the result is `F00FF00F`.

```
Boolean NOT:

Input a 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:   0000000F
```

_**note:** the NOT operation flips all bits, but the program only keeps the lowest byte afterward. this is part of the starter template for the assignment, so the output shows only the last two hex digits reversed._

### ⚡ x xor y

| **xor** will return a `1` if the digits _don't_ match. if they _do match_, the result will be `0`:

`1111 0000 1111 0000 1111 0000 1111 0000`  
`0000 1111 1111 0000 0000 1111 1111 0000`  
\=    
`1111 1111 0000 0000 1111 1111 0000 0000`

converted back into hex, the result is `FF00FF00`.

```
Boolean XOR:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            FF00FF00
```

### ♻️ x xor y xor x

| **xor** has 2 simple identities: `x xor x = 0` and `x xor 0 = x`.

in `x xor y xor x`, the `x xor x` equals `0`, leading to `0 xor y`, which ends up in `y`.

so `x xor y xor x` will always return `y` since the two `x`s cancel out

in hex, the result is `0FF00FF0`.

```
Boolean X XOR Y XOR X:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            0FF00FF0
```

_**note:** in the code, `y` is simply returned here, instead of hassling with unnecessary logic._

### ♻️ x xor y xor y

| **xor** has 2 simple identities: `x xor x = 0` and `x xor 0 = x`.

in `x xor y xor y`, the `y xor y` equals `0`, leading to `x xor 0`, which ends up in `x`.

so `x xor y xor y` will always return `x` since the two `y`s cancel out.

in hex, the result is `F0F0F0F0`.

```
Boolean X XOR Y XOR Y:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            F0F0F0F0
```

_**note:** in the code, `x` is simply returned here, instead of hassling with unnecessary logic._

### ➕ x + y

| **addition** is the _sum_ of two values.

`1111 0000 1111 0000 1111 0000 1111 0000`  
`0000 1111 1111 0000 0000 1111 1111 0000`  
\=  
`0000 0000 1110 0001 0000 0000 1110 0000`

```
Hexadecimal Addition:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            00E100E0
```

### ➖ x - y

| **subtraction** is the _difference_ between two values.

`1111 0000 1111 0000 1111 0000 1111 0000`  
`0000 1111 1111 0000 0000 1111 1111 0000`  
\=  
`1110 0001 0000 0000 1110 0001 0000 0000`

```
Hexadecimal Subtraction:

Input the first 32-bit hexadecimal operand:  F0F0F0F0
Input the second 32-bit hexadecimal operand: 0FF00FF0
The 32-bit hexadecimal result is:            E100E100
```

### 🚫 exit program

| you would _never_ guess what this does.

## 🔧 building it

coded in visual studio, and all logic lies inside `main.asm`. all configuration should be set from the get-go.

## 📝 small note

this was mainly for learning how to work with input, output, and a jump table in assembly. we were also given a template and were assigned the task of filling it out, so there is some functionality i understood referentially but not on a deep level.

because of this it lacks some functionality that would normally be default in other programs, such as input validation and error handling.
