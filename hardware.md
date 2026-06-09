# Understanding How Computers Really Work
My notes and learnings on computer architecture, covering CPUs, memory, registers, cache, control units, and instruction execution. A beginner's exploration of how computers work internally.


Think of it as the blueprint of a computer.

It includes things like:

1. CPU (Processor) – executes instructions
2. Memory (RAM) – stores data temporarily
3. Storage (SSD/HDD) – stores data permanently
4. Input/Output Devices – keyboard, mouse, monitor, etc.
5. Instruction Set Architecture (ISA) – the commands the CPU understands

**these parts makes a computer.**


# CPU (central processing unit)

The CPU (Central Processing Unit) is the brain of the computer. Its job is to **Read** instructions, **Understand (decode)** them, **Execute** them.
every program every process every maths problem is done by cpu.

![03a7fcdd7be29428fea19eeea8242316.png](../_resources/03a7fcdd7be29428fea19eeea8242316.png)

### A cpu consist of:
1. ALU (Arithmetic Logic Unit)
2. Registers
3. Control Unit (CU)
4. Cache
5. Clock

## ALU (Arithmetic Logic Unit)
ALU (Arithmetic Logic Unit) is a part of the CPU that performs arithmetic operations and logical operations on data. it uses logic gates and stuff to calculate and compare.

It takes input from registers, processes the data, and stores the result in a destination register or memory.



![687272661a77b94baa1f457b56f41499.png](../_resources/687272661a77b94baa1f457b56f41499.png)

it can perform :

1. **arithmetic operations** like addition,subrtraction,multiplication and division.

2. **logical operation** like AND , OR , NOT , XOR.

3. **comparison operation** like Equal (==), Greater than (>), Less than(<) ,greater than equal (>=) and less than equal (<=).

4. **shift operation** working with binaries like shift left (<<) , shift right (>>).

5. **Status Flag Generation** like Common flags: Zero (Z), Carry (C), Sign (S), Overflow (V).

*how alu multiply?*
most of cpu's alu just run the addition x times cause multiplication is addition many times . thats how many microvave dorebell adn electronic uses multiplication and advanced cpu like phone,computer have arthermetic multiplication

## Registers
Registers are small, fast storage elements inside the CPU used to temporarily hold data, addresses, or instructions. They are directly connected to the data path and ALU, enabling high-speed access

![ca17691734bcd659d3628c777e51f8f1.png](../_resources/ca17691734bcd659d3628c777e51f8f1.png)
**Special-Purpose Registers (The Internal Hardware Control)**

These registers have dedicated, unchangeable jobs hardwired into the CPU architecture. Programmers cannot directly use them to store arbitrary data

- **Program Counter (PC)** – stores the address of the next instruction.
- **Instruction Register (IR)** – stores the current instruction.
- **Status/Flag Register** – As mentioned when looking at the ALU, this register holds individual 1-bit flags (Zero, Carry, Overflow, Negative).
- **Memory Address Register (MAR)** – Holds the physical RAM address that the CPU wants to read from or write to
- **Memory Data Register (MDR)** – stores data being transferred to or from memory.

 ### General-Purpose Registers (GPRs)

General-Purpose Registers (GPRs) are registers inside the CPU used to store data, memory addresses, and intermediate results during program execution.
These are the registers that assembly language programmers and compilers interact with directly.

## Cache

Cache is a small, extremely fast memory located inside or very close to the CPU. It stores frequently used data and instructions so the CPU can access them faster than RAM.

Memory hierarchy:
```

Registers
↓
L1 Cache
↓
L2 Cache
↓
L3 Cache
↓
RAM
↓
SSD
```
## Control Unit (CU)

The Control Unit (CU) is the part of the CPU that controls and coordinates all other parts of the CPU.

The Control Unit fetches instructions, decodes them, and tells the CPU components what to do.

### CLOCK
A CPU clock (or clock speed) is an internal timing signal that dictates how many operations a processor can execute per second. 

Measured in gigahertz (GHz), it acts like the heartbeat of your computer—A higher clock speed means the CPU can perform more clock cycles per second.
![09b5d1470859368c7e227e46e66fce7f.png](../_resources/09b5d1470859368c7e227e46e66fce7f.png)
On each tick, parts of the CPU can perform work.

##
## Fetch–Decode–Execute Cycle

![b49596522425b7f5304fe0e17466702e.png](../_resources/b49596522425b7f5304fe0e17466702e.png)
The Fetch–Decode–Execute Cycle is the continuous process by which a CPU runs a program. The CPU repeatedly fetches an instruction from memory, decodes it to determine what operation is required, and then executes it.

 #### **Fetch**
The CPU retrieves the next instruction from memory.

- The **Program Counter (PC)** contains the address of the next instruction.
- The address is sent to memory.
- The instruction is fetched and stored in the **Instruction Register (IR).**
- The PC is updated to point to the next instruction.

#### **Decode**
The Control Unit (CU) interprets the fetched instruction.

It determines:
- Which operation to perform
- Which registers are involved
- Whether memory access is required

#### **Execute**

The CPU performs the operation.

Examples:
- ALU performs arithmetic or logical operations.
- LSU loads or stores data.
- Registers are updated with results.

### LETS RUN IT (CYCLE)
Lets run its:
```
MOV R1, 5
MOV R2, 3
ADD R1, R2

```
Our ram: 
```
Address  Instruction
100      MOV R1, 5
104      MOV R2, 3
108      ADD R1, R2
```

our Program Counter (PC):
`PC = 100`

**Step 1: Fetch**
The CPU : "What instruction should I execute next?"

1. The Program Counter contains: PC = 100
2. The Control Unit sends address 100 to memory.
3. Memory returns: ``MOV R1, 5``
4. The instruction is placed in the **Instruction Register (IR)**
5. The PC is updated to 104

**Step 2: Decode**
1. The Control Unit examines: `IR = MOV R1, 5`
2. and breaks it into pieces:
```
Opcode = MOV
Destination = R1
Value = 5
```
3. The Control Unit now understands: `Put 5 into R1`

**Step 3: Execute**
1. The Control Unit sends signals: `Write 5 into R1`
2. Result: ``R1 = 5``
3. Next Cycle:  `PC = 104`
`MOV R2, 3`
After execution:
`R2 = 3`
4.Third Cycle:
Fetch: `ADD R1, R2`
Execute:

The Control Unit tells the **ALU**:
```
Input A = R1
Input B = R2
Operation = ADD
```
Calculates:
5 + 3 = 8
Stores result:
`R1 = 8`

What Does the Clock Do?

Nothing would happen without the clock.

```
Tick 1 → Fetch
Tick 2 → Decode
Tick 3 → Execute

```
Every operation occurs in sync with clock cycles.

```
Cycle 1
Fetch MOV R1, 5
Decode
Execute

Cycle 2
Fetch MOV R2, 3
Decode
Execute

Cycle 3
Fetch ADD R1, R2
Decode
Execute
```

*Note: This is a simplified model. Real CPUs may use multiple clock cycles for each stage and can execute multiple stages simultaneously using pipelining.*
