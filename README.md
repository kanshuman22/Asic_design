# ASIC DESIGN  

- [Task 1](#task-1)
- [Task 2](#task-2)
- [Task 3](#task-3)
- [Task 4](#task-4)
- [Task 5](#task-5)
  
- [Task 6](#task-6)
- [Task 7](#task-7)
- [Task 8](#task-8)
- [Task 9](#task-9)
- [Task 10](#task-10)

- [Task 11](#task-11)
- [Task 12](#task-12)
- [Task 13](#task-13)

  


## TASK 1

### C Code 
A code to print sum of numbers from 1 to n(100)

Code:

![Screenshot from 2024-08-07 18-25-21](https://github.com/user-attachments/assets/ec83a5ce-ebac-472f-8f4e-d75fadee71c2)

### GCC compilation

Compilation and output of the code:

![Screenshot from 2024-08-07 18-28-18](https://github.com/user-attachments/assets/b366815d-2849-4e3e-98ce-68c9a332dd41)



### RISC-V compilation

* Compilation with flag set as O1:

![Screenshot from 2024-08-07 19-00-55](https://github.com/user-attachments/assets/d86ad928-17d3-45d1-b9a8-848426dc538f)



*  Object file creation shown below:

  ![image](https://github.com/user-attachments/assets/6c40b275-1bc0-41aa-9b8f-7de3c923d3f4)

* Execution of the program and corresponding output:

![image](https://github.com/user-attachments/assets/17392b91-bea8-40a7-aae3-b8e107f6e933)

* We will now focus on the main section . We want to calculate the total no. of instructions. To do this we will subtract the address of the first instruction in the main section from the first instruction in the next section. As it is a byte addressable memory so we will divide by 4 to get the no. of instructions.
Here the first instruction address for main is 10184 and the first instruction in the next section is 101c0 (Hexadecimal) . The difference is 60. So the no. of instructions is 60/4=15.
![image](https://github.com/user-attachments/assets/d81c195e-5f27-402c-b5f4-a916bcc7330e)

![image](https://github.com/user-attachments/assets/13419a40-f958-4ed5-99bc-51a1d2fd7751)

*Compiling with flag set as Ofast

![image](https://github.com/user-attachments/assets/99c481ac-8704-40e3-b851-3cd5bb7242dd)

![image](https://github.com/user-attachments/assets/6c1296ea-7eb5-43ad-8c8f-2b2d8cfcc657)

### Conclusion

It is observed that the no. of instructions reduces to 12 for Ofast from 15 for O1.
Ofast reduces  set of instructions  as Ofast applies aggressive optimizations that  removes redundancies and uses parallelism .So the instructions are less when we compare to O1 case.


## TASK 2

Using Ofast 

* Compiling the objdump file:
  
![image](https://github.com/user-attachments/assets/338d70da-37e7-4384-9e93-39f7424aac0b)

![image](https://github.com/user-attachments/assets/5256f187-b873-4567-bea5-2237499c0219)



### Debugging using SPIKE

Command used 

```bash
spike -d pk sum1ton.o
```

* We'll let Spike run the program until it reaches a specific point,which is the start of the main part of the program (marked by the instruction "100b0").

Once we get there, we'll observe the "a0 register". We'll check its value before and after a specific command runs.
We can observe that the instruction "lui a0,Ox21" changes the value of the register "a0" from  0x0000000000000001 to 0x0000000000021000
![image](https://github.com/user-attachments/assets/8815622e-7913-45d2-97e3-dbe0f4979bfa)

* Debugging the next instruction - "addi sp,sp,-16". The stack pointer (sp) is decremented by 16.
* We can observe that before execution of this instruction , sp register value was 0x0000003ffffffb50 , it gets updated to 0x0000003ffffffb40.
![image](https://github.com/user-attachments/assets/53d95e18-1a13-478a-bd28-4426e88caa23)

Using O1

Images for O1 case:

![image](https://github.com/user-attachments/assets/9cd33128-7f7d-423e-8d04-c09d741e536e)

![image](https://github.com/user-attachments/assets/738ac13d-b8ac-47f7-8005-7fbb2f984fa5)


## TASK 3

## RISC-V Instruction Formats

## Instruction Formats
# RISC-V Instruction Formats

This repository provides a detailed overview of the six primary instruction formats in the RISC-V architecture. Each format is designed for specific types of operations and includes fields that define how the instruction is encoded.

![image](https://github.com/user-attachments/assets/027bf444-c5e3-4766-8ca0-31a5e45cf4d1)


## Instruction Formats

# RISC-V Instruction Formats

This repository provides a detailed overview of the six primary instruction formats in the RISC-V architecture. Each format is designed for specific types of operations and includes fields that define how the instruction is encoded.

## Instruction Formats

### R-Type Instructions

- **Purpose**: R-Type instructions are used for arithmetic and logical operations involving three registers.
- **Format**:
  - **funct7 (7 bits)**: Function code for additional instruction differentiation.
  - **rs2 (5 bits)**: Second source register.
  - **rs1 (5 bits)**: First source register.
  - **funct3 (3 bits)**: Function code to specify the operation type.
  - **rd (5 bits)**: Destination register for storing the result.
  - **opcode (7 bits)**: Basic operation code, typically `0110011` for integer operations.
- **Examples**: ADD, SUB, OR, XOR, etc.


### I-Type Instructions

- **Purpose**: I-Type instructions involve an immediate value and one or two registers. They are used for operations like arithmetic with immediate values, loading data, and certain branches.
- **Format**:
  - **immediate (12 bits)**: Immediate value used in the operation.
  - **rs1 (5 bits)**: Source register.
  - **funct3 (3 bits)**: Function code for operation differentiation.
  - **rd (5 bits)**: Destination register.
  - **opcode (7 bits)**: Operation code for I-Type instructions.


### S-Type Instructions

- **Purpose**: S-Type instructions are used for storing data from a register into memory.
- **Format**:
  - **imm[11:5] (7 bits)**: Upper part of the immediate value (memory offset).
  - **rs2 (5 bits)**: Register containing the data to be stored.
  - **rs1 (5 bits)**: Base address register.
  - **funct3 (3 bits)**: Function code specifying the store operation.
  - **imm[4:0] (5 bits)**: Lower part of the immediate value.
  - **opcode (7 bits)**: Operation code for S-Type instructions.


### B-Type Instructions

- **Purpose**: B-Type instructions are used for conditional branching, allowing for program flow changes based on register comparisons.
- **Format**:
  - **imm[12] (1 bit)**: The 12th bit of the immediate value for branching.
  - **imm[10:5] (6 bits)**: Bits 10 through 5 of the immediate value.
  - **rs2 (5 bits)**: Second source register.
  - **rs1 (5 bits)**: First source register.
  - **funct3 (3 bits)**: Function code for branch operations.
  - **imm[4:1] (4 bits)**: Bits 4 through 1 of the immediate value.
  - **imm[11] (1 bit)**: The 11th bit of the immediate value.
  - **opcode (7 bits)**: Operation code for B-Type instructions.


### U-Type Instructions

- **Purpose**: U-Type instructions handle large immediate values, often for loading upper immediate values or computing addresses.
- **Format**:
  - **immediate[31:12] (20 bits)**: Upper 20 bits of the immediate value.
  - **rd (5 bits)**: Destination register.
  - **opcode (7 bits)**: Operation code for U-Type instructions.
- **Note**: The immediate value is stored in the upper 20 bits of a 32-bit word, with the lower 12 bits set to zero.


### J-Type Instructions

- **Purpose**: J-Type instructions are used for unconditional jumps, altering the control flow by jumping to a specific address, typically for function calls or loops.
- **Format**:
  - **imm[20] (1 bit)**: The 20th bit of the immediate value.
  - **imm[10:1] (10 bits)**: Bits 10 through 1 of the immediate value.
  - **imm[11] (1 bit)**: The 11th bit of the immediate value.
  - **imm[19:12] (8 bits)**: Bits 19 through 12 of the immediate value.
  - **rd (5 bits)**: Destination register for storing the return address.
  - **opcode (7 bits)**: Operation code for J-Type instructions.




### PART 1

## Instructions

### 1. ADD r4, r4, r4
- **Type**: R
- **32-bit Pattern**: `0000000 00100 00100 000 00100 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `000` (ADD)
  - **funct7**: `0000000`
  - **rs1**: `00100` (r4)
  - **rs2**: `00100` (r4)
  - **rd**: `00100` (r4)

### 2. SUB r4, r4, r4
- **Type**: R
- **32-bit Pattern**: `0000001 00100 00100 000 00100 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `000` (SUB)
  - **funct7**: `0100000`
  - **rs1**: `00100` (r4)
  - **rs2**: `00100` (r4)
  - **rd**: `00100` (r4)

### 3. AND r4, r4, r4
- **Type**: R
- **32-bit Pattern**: `0000000 00100 00100 111 00100 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `111` (AND)
  - **funct7**: `0000000`
  - **rs1**: `00100` (r4)
  - **rs2**: `00100` (r4)
  - **rd**: `00100` (r4)

### 4. OR r8, r4, r5
- **Type**: R
- **32-bit Pattern**: `0000000 00101 00100 110 01000 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `110` (OR)
  - **funct7**: `0000000`
  - **rs1**: `00100` (r4)
  - **rs2**: `00101` (r5)
  - **rd**: `01000` (r8)

### 5. XOR r8, r4, r4
- **Type**: R
- **32-bit Pattern**: `0000000 00100 00100 100 01000 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `100` (XOR)
  - **funct7**: `0000000`
  - **rs1**: `00100` (r4)
  - **rs2**: `00100` (r4)
  - **rd**: `01000` (r8)

### 6. SLT r00, r1, r4
- **Type**: R
- **32-bit Pattern**: `0000000 00100 00001 010 00000 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `010` (SLT)
  - **funct7**: `0000000`
  - **rs1**: `00001` (r1)
  - **rs2**: `00100` (r4)
  - **rd**: `00000` (r00)

### 7. ADDI r02, r2, 5
- **Type**: I
- **32-bit Pattern**: `000000000101 00010 000 00010 0010011`
- **Fields**:
  - **opcode**: `0010011` (I-type)
  - **funct3**: `000` (ADDI)
  - **rs1**: `00010` (r2)
  - **rd**: `00010` (r02)
  - **immediate**: `000000000101` (5)

### 8. SW r2, r0, 4
- **Type**: S
- **32-bit Pattern**: `000000000100 00000 010 00010 0100011`
- **Fields**:
  - **opcode**: `0100011` (S-type)
  - **funct3**: `010` (SW)
  - **rs1**: `00000` (r0)
  - **rs2**: `00010` (r2)
  - **imm[11:5]**: `0000000` (0)
  - **imm[4:0]**: `00100` (4)

### 9. SRL r06, r01, r1
- **Type**: R
- **32-bit Pattern**: `0000000 00001 00001 101 00110 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `101` (SRL)
  - **funct7**: `0000000`
  - **rs1**: `00001` (r01)
  - **rs2**: `00001` (r1)
  - **rd**: `00110` (r06)

### 10. BNE r0, r0, 20
- **Type**: B
- **32-bit Pattern**: `000000000101 00000 001 00000 1100011`
- **Fields**:
  - **opcode**: `1100011` (B-type)
  - **funct3**: `001` (BNE)
  - **rs1**: `00000` (r0)
  - **rs2**: `00000` (r0)
  - **imm[12]**: `0`
  - **imm[10:5]**: `000000`
  - **imm[4:1]**: `0101`
  - **imm[11]**: `0`

### 11. BEQ r0, r0, 15
- **Type**: B
- **32-bit Pattern**: `000000000111 00000 000 00000 1100011`
- **Fields**:
  - **opcode**: `1100011` (B-type)
  - **funct3**: `000` (BEQ)
  - **rs1**: `00000` (r0)
  - **rs2**: `00000` (r0)
  - **imm[12]**: `0`
  - **imm[10:5]**: `000000`
  - **imm[4:1]**: `0111`
  - **imm[11]**: `0`

### 12. LW r03, r01, 2
- **Type**: I
- **32-bit Pattern**: `000000000010 00001 010 00011 0000011`
- **Fields**:
  - **opcode**: `0000011` (I-type)
  - **funct3**: `010` (LW)
  - **rs1**: `00001` (r01)
  - **rd**: `00011` (r03)
  - **immediate**: `000000000010` (2)

### 13. SLL r05, r01, r1
- **Type**: R
- **32-bit Pattern**: `0000000 00001 00001 001 00101 0110011`
- **Fields**:
  - **opcode**: `0110011` (R-type)
  - **funct3**: `001` (SLL)
  - **funct7**: `0000000`
  - **rs1**: `00001` (r01)
  - **rs2**: `00001` (r1)
  - **rd**: `00101` (r05)
 
| Instruction         | Type | 32-bit Instruction                         | Hexadecimal |
|--------------------|-------|---------------------------------------------|-----------|
| `ADD r4, r4, r4`   | R    | 0000000 00100 00100 000 00100 0110011       | 0x00420233  |
| `SUB r4, r4, r4`   | R    | 0100000 00100 00100 000 00100 0110011       | 0x40420233  |
| `AND r4, r4, r4`   | R    | 0000000 00100 00100 111 00100 0110011       | 0x00427233  |
| `OR r8, r4, r5`    | R    | 0000000 00101 00100 110 01000 0110011       | 0x00526433  |
| `XOR r8, r4, r4`   | R    | 0000000 00100 00100 100 01000 0110011       | 0x00424433  |
| `SLT r00, r1, r4`  | R    | 0000000 00100 00001 010 00000 0110011       | 0x0040A033  |
| `ADDI r02, r2, 5`  | I    | 000000000101 00010 000 00010 0010011       | 0x00510113  |
| `SW r2, r0, 4`     | S    | 000000000100 00000 010 00010 0100011       | 0x00402123  |
| `SRL r06, r01, r1` | R    | 0000000 00001 00001 101 00110 0110011       | 0x0010D333  |
| `BNE r0, r0, 20`   | B    | 000000010100 00000 001 00000 1100011       | 0x01401063  |
| `BEQ r0, r0, 15`   | B    | 000000001111 00000 000 00000 1100011       | 0x00F00063  |
| `LW r03, r01, 2`   | I    | 000000000010 00001 010 00011 0000011       | 0x0020A183  |
| `SLL r05, r01, r1` | R    | 0000000 00001 00001 001 00101 0110011       | 0x001092B3  |

### PART 2

The RISC-V ISA and the Hardcoded ISA
| Operation           | Standard RISC-V ISA | Standard RISC-V ISA (Binary)               | Hardcoded ISA     | Hardcoded ISA (Binary)                  |
|---------------------|---------------------|--------------------------------------------|-------------------|-----------------------------------------|
| ADD R6, R2, R1      | 32'h00110333        | 000000000001 00010 000 00110 0110011       | 32'h02208300      | 000000100010 00000 100 00000 01100000  |
| SUB R7, R1, R2      | 32'h402083b3        | 010000000010 00000 000 00111 0110011       | 32'h02209380      | 000000100010 01000 100 10000 01100000  |
| AND R8, R1, R3      | 32'h0030f433        | 000000000011 00000 111 01000 0110011       | 32'h0230a400      | 000000100011 01000 101 00100 01100000  |
| OR R9, R2, R5       | 32'h005164b3        | 000000000101 00010 110 01001 0110011       | 32'h02513480      | 000000100101 00010 110 10100 01100000  |
| XOR R10, R1, R4     | 32'h0040c533        | 000000000100 00000 100 01010 0110011       | 32'h0240c500      | 000000100100 00000 100 11000 01100000  |
| SLT R1, R2, R4      | 32'h0045a0b3        | 000000000100 00010 010 00001 0110011       | 32'h02415580      | 000000100100 00010 101 01010 01100000  |
| ADDI R12, R4, 5     | 32'h004120b3        | 000000000101 00010 010 01100 0010011       | 32'h00520600      | 000000100100 00010 010 00010 01100000  |
| BEQ R0, R0, 15      | 32'h00000f63        | 000000000000 00000 000 00000 1100011       | 32'h00f00002      | 000000000000 00000 000 00000 11000000  |
| SW R3, R1, 2        | 32'h0030a123        | 000000000011 00000 010 00001 0100011       | 32'h00209181      | 000000100010 00000 010 00001 01100000  |
| LW R13, R1, 2       | 32'h0020a683        | 000000000010 00000 010 01101 0000011       | 32'h00208681      | 000000100010 00000 010 01100 01100000  |
| SRL R16, R14, R2    | 32'h0030a123        | 000000000011 00000 010 00001 0100011       | 32'h00271803      | 000000100010 00000 011 00110 01100000  |
| SLL R15, R1, R2     | 32'h002097b3        | 000000000010 00000 111 01111 0110011       | 32'h00208783      | 000000100010 00000 111 01111 01100000  |


We will be using gtkwave to analyse the waveforms

![image](https://github.com/user-attachments/assets/7a6bde53-89b5-4f62-8753-78d6360b6252)



Hardcode instructions output waveforms:

add r6, r1, r2
![image](https://github.com/user-attachments/assets/ae731f5b-c611-416f-886d-b60bd343b35b)

sub r7,r1,r2
![image](https://github.com/user-attachments/assets/ae3bd791-5cd9-4969-a904-ff20d60a02e8)


and r8,r1,r3
![image](https://github.com/user-attachments/assets/ea26a0c2-9b07-4395-92ba-7ce7334198aa)

or r9,r2,r5
![image](https://github.com/user-attachments/assets/1bf1974c-2856-4c98-b12d-b9d81e05e3c8)


xor r10,r1,r4
![image](https://github.com/user-attachments/assets/a0b2b8c7-6898-4510-bb47-917e3866196b)


slt r11,r2,r4
![image](https://github.com/user-attachments/assets/228558de-979b-4134-9898-5ef471876d59)


addi r12,r4,5
![image](https://github.com/user-attachments/assets/23082282-6362-473a-b943-69cda247db6e)


sw r3,r1,2
![image](https://github.com/user-attachments/assets/1d46c231-e6e5-4efc-a367-a036ad5e9671)


lw r13,r1,2
![image](https://github.com/user-attachments/assets/25346670-526b-499c-947d-d003fc8be0c3)



beq r0,r0,15
![image](https://github.com/user-attachments/assets/497e1fda-1b1b-4fcf-9760-f0e6c14316f5)


add r14,r2,r2
![image](https://github.com/user-attachments/assets/2df20fd0-4a57-4b33-a2c6-8095ede49cd4)


Corresponding waveforms for our instructions

![image](https://github.com/user-attachments/assets/95e5f151-1eb8-46c6-8577-f683a3fb6402)


![image](https://github.com/user-attachments/assets/b9ce5baf-5a62-4905-bb03-2d36058fdeca)


![image](https://github.com/user-attachments/assets/93a48b55-e33f-41e4-987c-ac9b2be7e8b8)


![image](https://github.com/user-attachments/assets/72b1b705-3418-446b-8481-91ccef9d72c9)


![image](https://github.com/user-attachments/assets/7a974386-08bc-4238-9ee1-278e14689667)


![image](https://github.com/user-attachments/assets/1efd009f-35f5-467c-8937-c52f1e334581)


![image](https://github.com/user-attachments/assets/69479bea-381f-44f5-90ba-c7a71fd18a64)

![image](https://github.com/user-attachments/assets/5b5aae57-340c-428c-bf7c-6de74d8ee92b)

![image](https://github.com/user-attachments/assets/a4dde053-9ad4-4953-8416-7d72b9333d17)

![image](https://github.com/user-attachments/assets/bb91d0fd-2b75-4b3d-a139-b1f7b75f4beb)

![image](https://github.com/user-attachments/assets/b848e154-b90d-47ff-aa3d-2bd614cee95e)

A difference in waveforms can be observed for both set of instructions

## TASK 4

A C code which utilizes the binary exponentiation algorithm to calculate the power of a number with time complexity O(log n). Binary exponentiation is an algorithm that reduces the number of multiplications needed to compute powers of a number by exploiting the properties of exponents in binary form.

### C Code

![image](https://github.com/user-attachments/assets/547081ab-bb02-4696-b3f2-ebb0a4161a23)

### GCC Compilation 

![image](https://github.com/user-attachments/assets/8cb7b108-dc4a-453f-9b40-d52560b5a402)

### RISC-V Compilation

We will use the O1 switch

![image](https://github.com/user-attachments/assets/a33e4e1f-aaa4-4151-b663-ec9489a7d96e)


Objdump file

![image](https://github.com/user-attachments/assets/acdcc33e-a0ea-45c3-ab2d-986e98fe7240)

![image](https://github.com/user-attachments/assets/3a72db72-20e7-4d2b-819e-eba241f603ef)

Counting no. of instructions:
101b4-10184
We will then divide by 4 the decimal value obtained .Doing this we get the no. of instructions. We get 12 instructions for base=5 and exponent=2
![image](https://github.com/user-attachments/assets/6ffadb57-a960-466a-a810-0c08d8c81519)



### Debugging with spike

![image](https://github.com/user-attachments/assets/28f9aa56-4fc3-400e-b47d-28aec549d08d)

We first bring the program counter to the start of the main function. We can see the registers have the corresponding values, the values 5,2 and 25 are present on their respective registers.


## TASK 6

### Pipelined RISC-V Processor


### CPU Components

This document provides an overview of the key components within a CPU and their functions. These components work together to execute machine instructions and carry out tasks required by a program.

#### Components

- **Program Counter (PC)**  
  A special register in the CPU that tracks the address of the next instruction to be fetched and executed. It advances with each instruction fetch, directing the instruction memory to supply the upcoming instruction.

- **Instruction Decoder**  
  An internal CPU circuit that interprets the binary machine instructions fetched from memory. It decodes these instructions and generates control signals for other CPU components to execute the instructions.

- **Instruction Memory**  
  A storage unit that holds the binary machine instructions of a program. It is read-only and provides instructions to the CPU based on the address given by the program counter.

- **Data Memory**  
  A storage component used for storing data that the CPU manipulates during program execution. Unlike instruction memory, data memory supports both read and write operations and holds variables, data arrays, and other necessary information.

- **ALU (Arithmetic Logic Unit)**  
  A critical CPU component that performs arithmetic and logical operations on data, such as addition, subtraction, multiplication, division, and bitwise operations. The ALU generates results used in various computations as specified by instructions.

- **Read Register File**  
  A set of registers within the CPU that hold data during instruction execution. Instructions specify which registers to read from, and this data is used as operands for ALU operations or other tasks.

- **Write Register File**  
  Responsible for storing the results of operations into registers. After an instruction is executed, the result is written back to the register file to ensure that the updated data is available for subsequent instructions.

### How They Work Together

These components work together to execute machine instructions:

1. The **Program Counter** directs the fetching of instructions.
2. The **Instruction Decoder** interprets these instructions.
3. The **ALU** performs the necessary computations.
4. The **Register Files** manage data storage and retrieval.
5. **Instruction Memory** and **Data Memory** provide and store the necessary instructions and data.

This coordinated effort allows the CPU to efficiently execute a program's instructions.


### Program Counter

![pc](https://github.com/user-attachments/assets/e735fcf1-69ca-4a46-bc0d-bd49f4e57589)


### Instruction fetch

![2](https://github.com/user-attachments/assets/b6a4bb4e-7306-4708-af47-7445d75b376b)



### Instruction decode

![3](https://github.com/user-attachments/assets/71416d87-f682-462d-a406-b678a22302d9)


![3_2](https://github.com/user-attachments/assets/13478985-0ad2-449f-892c-58df715363f3)



### Register File Read

![4](https://github.com/user-attachments/assets/ca097eb0-cfd0-4786-9f6c-ac5982b49c85)


### ALU 

![5](https://github.com/user-attachments/assets/4dfd08c7-1251-489d-a68d-d0507f4c7794)

![5_2](https://github.com/user-attachments/assets/501a4673-4866-4e0a-a5bd-4f2b0c84e486)


### Register File Write

![6](https://github.com/user-attachments/assets/a0be8a10-0dca-41ee-a294-353dec9789fb)


![6_2](https://github.com/user-attachments/assets/d2de9807-8f31-4e8c-8d97-a24d16235cec)


### Branch Instructions

![7](https://github.com/user-attachments/assets/ac22c481-1f21-4f69-93f0-02396c20da80)

![7_2](https://github.com/user-attachments/assets/3ac8aed0-71d3-4131-a5f5-2c44e4eb090d)




### CODE :
```bash

\m4_TLV_version 1d: tl-x.org
\SV
   // This code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 1 to 9 Program |
   // \====================/
   //
   // Program for MYTH Workshop to test RV32I
   // Add 1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1011)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   
   // Optional:
   // m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_ansh = *clk;
         //$pc[31:0] = >>1$reset ? 0 : ( >>1$pc + 31'h4 );
         
         $pc[31:0] = (>>1$reset) ? '0 :
                     (>>1$taken_br) ? >>1$br_tgt_pc : >>1$pc + 32'd4;

         
         $imem_rd_en = >>1$reset ? 0 : 1;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
      @1
         $instr[31:0] = $imem_rd_data[31:0];
         
         //decode
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] ==? 5'b11001;
         $is_r_instr = $instr[6:2] ==? 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] ==? 5'b10100;
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         $is_b_instr = $instr[6:2] ==? 5'b11000;
         $is_j_instr = $instr[6:2] ==? 5'b11011;
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         //imm decode
         $imm[31:0] = $is_i_instr ? {{21{$instr[31]}}, $instr[30:20]} :
                      $is_s_instr ? {{21{$instr[31]}}, $instr[30:25], $instr[11:7]} :
                      $is_b_instr ? {{20{$instr[31]}}, $instr[7], $instr[30:25], $instr[11:8], 1'b0} :
                      $is_u_instr ? {$instr[31:12], 12'b0} :
                      $is_j_instr ? {{12{$instr[31]}}, $instr[19:12], $instr[20], $instr[30:21], 1'b0} :
                      32'b0;
         
         //decode logic for other fields
         $rs2_valid = $is_r_instr || $is_s_instr || $is_b_instr;
         ?$rs2_valid
            $rs2[4:0] = $instr[24:20];
            
         $rs1_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$rs1_valid
            $rs1[4:0] = $instr[19:15];
         
         $funct3_valid = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         ?$funct3_valid
            $funct3[2:0] = $instr[14:12];
            
         $funct7_valid = $is_r_instr ;
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
            
         $rd_valid = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         ?$rd_valid
            $rd[4:0] = $instr[11:7];
            
         $opcode[6:0] = $instr[6:0];
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         $is_beq = $dec_bits ==? 11'bx_000_1100011;
         $is_bne = $dec_bits ==? 11'bx_001_1100011;
         $is_blt = $dec_bits ==? 11'bx_100_1100011;
         $is_bge = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu = $dec_bits ==? 11'bx_111_1100011;
         $is_add = $dec_bits ==? 11'b0_000_0110011;
         $is_addi = $dec_bits ==? 11'bx_000_0010011;
         
         //RF read and enable
         $rf_rd_en1 = $rs1_valid;
         $rf_rd_index1[4:0] = $rs1;
         $rf_rd_en2 = $rs2_valid;
         $rf_rd_index2[4:0] = $rs2;
         
         $src1_value[31:0] = $rf_rd_data1;
         $src2_value[31:0] = $rf_rd_data2;
         
         // ALU
         $result[31:0] = $is_addi ? $src1_value + $imm :
                         $is_add ? $src1_value + $src2_value : 32'bx ;
                         
         //RF write and enable
         $rf_wr_en = $rd_valid && $rd != 5'b0;
         $rf_wr_index[4:0] = $rd;
         $rf_wr_data[31:0] = $result;
         
         //branch
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) : 1'b0;
         
         $br_tgt_pc[31:0] = $pc + $imm;
         

         
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   *passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9) ;
   *passed = *cyc_cnt > 40;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@1, @1)  // Args: (read stage, write stage) - if equal, no register bypass is required
      //m4+dmem(@4)    // Args: (read/write stage)

   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic. @4 would work for all labs.
\SV
   endmodule

```
We now test our code using testbench ,   
*passed = |cpu/xreg[14]>>5$value == (1+2+3+4+5+6+7+8+9) ;   this is used , its inserted in @1 stage

![image](https://github.com/user-attachments/assets/a3605ab3-1158-40ee-bdee-65e947d975a7)


![image](https://github.com/user-attachments/assets/af2d5cd3-fa6f-4c98-905f-58cfd8ba84e9)



![image](https://github.com/user-attachments/assets/1486efe5-f43a-41e8-8e49-423146c9c6d3)

![image](https://github.com/user-attachments/assets/ab437faa-c8c0-4abe-81ac-fefc60cf2cfa)

![image](https://github.com/user-attachments/assets/608b82b4-7e25-49bf-84f8-f7b46ddc75ea)

We can observe the value of /xreg[14] it has a hexadecimal value of 2D which converts to 45 in decimal system which is the sum from 1 to 9.


We will now take a look at the concepts of day 5

### Pipelining Hazards

Pipelining in CPU architecture can introduce several types of hazards that disrupt the smooth execution of instructions.

- **Structural Hazard**  
  This occurs when multiple instructions compete for the same hardware resource, causing delays and stalls in the pipeline.

- **Data Hazard**  
  This arises when an instruction depends on the outcome of a preceding instruction that has not yet completed. Such dependencies can result in incorrect results or delays if the required data is not available in time.

- **Control Hazard (Branch Hazard)**  
  This happens due to uncertainty over whether a branch instruction will be taken. If the prediction about a branch is incorrect, the pipeline must be flushed and restarted with the correct instructions, leading to a performance penalty.

Understanding and managing these hazards useful for optimizing pipeline performance and ensuring efficient instruction execution.

Code:

```bash

\m4_TLV_version 1d: tl-x.org
\SV
   // Template code can be found in: https://github.com/stevehoover/RISC-V_MYTH_Workshop
   
   m4_include_lib(['https://raw.githubusercontent.com/BalaDhinesh/RISC-V_MYTH_Workshop/master/tlv_lib/risc-v_shell_lib.tlv'])

\SV
   m4_makerchip_module   // (Expanded in Nav-TLV pane.)
\TLV

   // /====================\
   // | Sum 0 to 9 Program |
   // \====================/
   //
   // Add 0,1,2,3,...,9 (in that order).
   //
   // Regs:
   //  r10 (a0): In: 0, Out: final sum
   //  r12 (a2): 10
   //  r13 (a3): 1..10
   //  r14 (a4): Sum
   // 
   // External to function:
   m4_asm(ADD, r10, r0, r0)             // Initialize r10 (a0) to 0.
   // Function:
   m4_asm(ADD, r14, r10, r0)            // Initialize sum register a4 with 0x0
   m4_asm(ADDI, r12, r10, 1010)         // Store count of 10 in register a2.
   m4_asm(ADD, r13, r10, r0)            // Initialize intermediate sum register a3 with 0
   // Loop:
   m4_asm(ADD, r14, r13, r14)           // Incremental addition
   m4_asm(ADDI, r13, r13, 1)            // Increment intermediate register by 1
   m4_asm(BLT, r13, r12, 1111111111000) // If a3 is less than a2, branch to label named <loop>
   m4_asm(ADD, r10, r14, r0)            // Store final result to register a0 so that it can be read by main program
   m4_asm(SW, r0, r10, 10000)           // Store r10 result in dmem
   m4_asm(LW, r17, r0, 10000)           // Load contents of dmem to r17
   m4_asm(JAL, r7, 00000000000000000000) // Done. Jump to itself (infinite loop). (Up to 20-bit signed immediate plus implicit 0 bit (unlike JALR) provides byte address; last immediate bit should also be 0)
   m4_define_hier(['M4_IMEM'], M4_NUM_INSTRS)

   |cpu
      @0
         $reset = *reset;
         $clk_ansh = *clk;
         
         
         $pc[31:0] = >>1$reset ? '0 : (>>3$valid_taken_br ? >>3$br_tgt_pc :
                                       >>3$valid_load     ? >>3$inc_pc[31:0] :
                                       >>3$jal_valid      ? >>3$br_tgt_pc :
                                       >>3$jalr_valid     ? >>3$jalr_tgt_pc :
                                                     (>>1$inc_pc[31:0]));
         // Access instruction memory using PC
         $imem_rd_en = ~ $reset;
         $imem_rd_addr[M4_IMEM_INDEX_CNT-1:0] = $pc[M4_IMEM_INDEX_CNT+1:2];
         
         
      @1
         //Getting instruction from IMem
         $instr[31:0] = $imem_rd_data[31:0];
         
         //Increment PC
         $inc_pc[31:0] = $pc[31:0] + 32'h4;
         
         //Decoding I,R,S,U,B,J type of instructions based on opcode [6:0]
         //Only [6:2] is used here because this implementation is for RV64I which does not use [1:0]
         $is_i_instr = $instr[6:2] ==? 5'b0000x ||
                       $instr[6:2] ==? 5'b001x0 ||
                       $instr[6:2] == 5'b11001;
         
         $is_r_instr = $instr[6:2] == 5'b01011 ||
                       $instr[6:2] ==? 5'b011x0 ||
                       $instr[6:2] == 5'b10100;
         
         $is_s_instr = $instr[6:2] ==? 5'b0100x;
         
         $is_u_instr = $instr[6:2] ==? 5'b0x101;
         
         $is_b_instr = $instr[6:2] == 5'b11000;
         
         $is_j_instr = $instr[6:2] == 5'b11011;
         
         //Immediate value decode
         $imm[31:0] = $is_i_instr ? { {21{$instr[31]}} , $instr[30:20]} :
                      $is_s_instr ? { {21{$instr[31]}} , $instr[30:25] , $instr[11:8] , $instr[7]} :
                      $is_b_instr ? { {20{$instr[31]}} , $instr[7] , $instr[30:25] , $instr[11:8] , 1'b0} :
                      $is_u_instr ? { $instr[31] , $instr[30:12] , { 12{1'b0}} } :
                      $is_j_instr ? { {12{$instr[31]}} , $instr[19:12] , $instr[20] , $instr[30:21] , 1'b0} :
                      >>1$imm[31:0];
         
         //Generate valid signals for each instruction fields
         $rs1_or_funct3_valid    = $is_r_instr || $is_i_instr || $is_s_instr || $is_b_instr;
         $rs2_valid              = $is_r_instr || $is_s_instr || $is_b_instr;
         $rd_valid               = $is_r_instr || $is_i_instr || $is_u_instr || $is_j_instr;
         $funct7_valid           = $is_r_instr;
         
         //Decode other fields of instruction - source and destination registers, funct, opcode
         ?$rs1_or_funct3_valid
            $rs1[4:0]    = $instr[19:15];
            $funct3[2:0] = $instr[14:12];
         
         ?$rs2_valid
            $rs2[4:0]    = $instr[24:20];
         
         ?$rd_valid
            $rd[4:0]     = $instr[11:7];
         
         ?$funct7_valid
            $funct7[6:0] = $instr[31:25];
         
         $opcode[6:0] = $instr[6:0];
         
         //Decode instruction in subset of base instruction set based on RISC-V 32I
         $dec_bits[10:0] = {$funct7[5],$funct3,$opcode};
         
         //Branch instructions
         $is_beq   = $dec_bits ==? 11'bx_000_1100011;
         $is_bne   = $dec_bits ==? 11'bx_001_1100011;
         $is_blt   = $dec_bits ==? 11'bx_100_1100011;
         $is_bge   = $dec_bits ==? 11'bx_101_1100011;
         $is_bltu  = $dec_bits ==? 11'bx_110_1100011;
         $is_bgeu  = $dec_bits ==? 11'bx_111_1100011;
         
         //Jump instructions
         $is_auipc = $dec_bits ==? 11'bx_xxx_0010111;
         $is_jal   = $dec_bits ==? 11'bx_xxx_1101111;
         $is_jalr  = $dec_bits ==? 11'bx_000_1100111;
         
         //Arithmetic instructions
         $is_addi  = $dec_bits ==? 11'bx_000_0010011;
         $is_add   = $dec_bits ==  11'b0_000_0110011;
         $is_lui   = $dec_bits ==? 11'bx_xxx_0110111;
         $is_slti  = $dec_bits ==? 11'bx_010_0010011;
         $is_sltiu = $dec_bits ==? 11'bx_011_0010011;
         $is_xori  = $dec_bits ==? 11'bx_100_0010011;
         $is_ori   = $dec_bits ==? 11'bx_110_0010011;
         $is_andi  = $dec_bits ==? 11'bx_111_0010011;
         $is_slli  = $dec_bits ==? 11'b0_001_0010011;
         $is_srli  = $dec_bits ==? 11'b0_101_0010011;
         $is_srai  = $dec_bits ==? 11'b1_101_0010011;
         $is_sub   = $dec_bits ==? 11'b1_000_0110011;
         $is_sll   = $dec_bits ==? 11'b0_001_0110011;
         $is_slt   = $dec_bits ==? 11'b0_010_0110011;
         $is_sltu  = $dec_bits ==? 11'b0_011_0110011;
         $is_xor   = $dec_bits ==? 11'b0_100_0110011;
         $is_srl   = $dec_bits ==? 11'b0_101_0110011;
         $is_sra   = $dec_bits ==? 11'b1_101_0110011;
         $is_or    = $dec_bits ==? 11'b0_110_0110011;
         $is_and   = $dec_bits ==? 11'b0_111_0110011;
         
         //Store instructions
         $is_sb    = $dec_bits ==? 11'bx_000_0100011;
         $is_sh    = $dec_bits ==? 11'bx_001_0100011;
         $is_sw    = $dec_bits ==? 11'bx_010_0100011;
         
         //Load instructions - support only 4 byte load
         $is_load  = $dec_bits ==? 11'bx_xxx_0000011;
         
         $is_jump = $is_jal || $is_jalr;
         
      @2
         //Get Source register values from reg file
         $rf_rd_en1 = $rs1_or_funct3_valid;
         $rf_rd_en2 = $rs2_valid;
         
         $rf_rd_index1[4:0] = $rs1[4:0];
         $rf_rd_index2[4:0] = $rs2[4:0];
         
         //Register file bypass logic - data forwarding from ALU to resolve RAW dependence
         $src1_value[31:0] = $rs1_bypass ? >>1$result[31:0] : $rf_rd_data1[31:0];
         $src2_value[31:0] = $rs2_bypass ? >>1$result[31:0] : $rf_rd_data2[31:0];
         
         //Branch target PC computation for branches and JAL
         $br_tgt_pc[31:0] = $imm[31:0] + $pc[31:0];
         
         //RAW dependence check for ALU data forwarding
         //If previous instruction was writing to reg file, and current instruction is reading from same register
         $rs1_bypass = >>1$rf_wr_en && (>>1$rd == $rs1);
         $rs2_bypass = >>1$rf_wr_en && (>>1$rd == $rs2);
         
      @3
         //ALU
         $result[31:0] = $is_addi  ? $src1_value +  $imm :
                         $is_add   ? $src1_value +  $src2_value :
                         $is_andi  ? $src1_value &  $imm :
                         $is_ori   ? $src1_value |  $imm :
                         $is_xori  ? $src1_value ^  $imm :
                         $is_slli  ? $src1_value << $imm[5:0]:
                         $is_srli  ? $src1_value >> $imm[5:0]:
                         $is_and   ? $src1_value &  $src2_value:
                         $is_or    ? $src1_value |  $src2_value:
                         $is_xor   ? $src1_value ^  $src2_value:
                         $is_sub   ? $src1_value -  $src2_value:
                         $is_sll   ? $src1_value << $src2_value:
                         $is_srl   ? $src1_value >> $src2_value:
                         $is_sltu  ? $sltu_rslt[31:0]:
                         $is_sltiu ? $sltiu_rslt[31:0]:
                         $is_lui   ? {$imm[31:12], 12'b0}:
                         $is_auipc ? $pc + $imm:
                         $is_jal   ? $pc + 4:
                         $is_jalr  ? $pc + 4:
                         $is_srai  ? ({ {32{$src1_value[31]}} , $src1_value} >> $imm[4:0]) :
                         $is_slt   ? (($src1_value[31] == $src2_value[31]) ? $sltu_rslt : {31'b0, $src1_value[31]}):
                         $is_slti  ? (($src1_value[31] == $imm[31]) ? $sltiu_rslt : {31'b0, $src1_value[31]}) :
                         $is_sra   ? ({ {32{$src1_value[31]}}, $src1_value} >> $src2_value[4:0]) :
                         $is_load  ? $src1_value +  $imm :
                         $is_s_instr ? $src1_value + $imm :
                                    32'bx;
         
         $sltu_rslt[31:0]  = $src1_value <  $src2_value;
         $sltiu_rslt[31:0] = $src1_value <  $imm;
         
         //Jump instruction target PC computation
         $jalr_tgt_pc[31:0] = $imm[31:0] + $src1_value[31:0]; 
         
         //Branch resolution
         $taken_br = $is_beq ? ($src1_value == $src2_value) :
                     $is_bne ? ($src1_value != $src2_value) :
                     $is_blt ? (($src1_value < $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bge ? (($src1_value >= $src2_value) ^ ($src1_value[31] != $src2_value[31])) :
                     $is_bltu ? ($src1_value < $src2_value) :
                     $is_bgeu ? ($src1_value >= $src2_value) :
                     1'b0;
         
         //Current instruction is valid if one of the previous 2 instructions were not (taken_branch or load or jump)
         $valid = ~(>>1$valid_taken_br || >>2$valid_taken_br || >>1$is_load || >>2$is_load || >>2$jump_valid || >>1$jump_valid);
         
         //Current instruction is valid & is a taken branch
         $valid_taken_br = $valid && $taken_br;
         
         //Current instruction is valid & is a load
         $valid_load = $valid && $is_load;
         
         //Current instruction is valid & is jump
         $jump_valid = $valid && $is_jump;
         $jal_valid  = $valid && $is_jal;
         $jalr_valid = $valid && $is_jalr;
         
         //Destination register update - ALU result or load result depending on instruction
         $rf_wr_en = (($rd != '0) && $rd_valid && $valid) || >>2$valid_load;
         $rf_wr_index[4:0] = $valid ? $rd[4:0] : >>2$rd[4:0];
         $rf_wr_data[31:0] = $valid ? $result[31:0] : >>2$ld_data[31:0];
         
      @4
         //Data memory access for load, store
         $dmem_addr[3:0]     =  $result[5:2];
         $dmem_wr_en         =  $valid && $is_s_instr;
         $dmem_wr_data[31:0] =  $src2_value[31:0];
         $dmem_rd_en         =  $valid_load;
         
      
         //Write back data read from load instruction to register
         $ld_data[31:0]      =  $dmem_rd_data[31:0];
         
      
      

      // Note: Because of the magic we are using for visualisation, if visualisation is enabled below,
      //       be sure to avoid having unassigned signals (which you might be using for random inputs)
      //       other than those specifically expected in the labs. You'll get strange errors for these.

   
   // Assert these to end simulation (before Makerchip cycle limit).
   //Checks if sum of numbers from 1 to 9 is obtained in reg[17] and runs 10 cycles extra after this is met
   *passed = |cpu/xreg[17]>>10$value == (1+2+3+4+5+6+7+8+9);
   //Run for 200 cycles without any checks
   //*passed = *cyc_cnt > 200;
   *failed = 1'b0;
   
   // Macro instantiations for:
   //  o instruction memory
   //  o register file
   //  o data memory
   //  o CPU visualization
   |cpu
      m4+imem(@1)    // Args: (read stage)
      m4+rf(@2, @3)  // Args: (read stage, write stage) - if equal, no register bypass is required
      m4+dmem(@4)    // Args: (read/write stage)
   
   m4+cpu_viz(@4)    // For visualisation, argument should be at least equal to the last stage of CPU logic
                       // @4 would work for all labs
\SV
   endmodule
```


![image](https://github.com/user-attachments/assets/d4778cc5-49d0-4c2c-add4-8b63140c2276)


![image](https://github.com/user-attachments/assets/1518df93-cd04-4c23-a050-c1bed78b4dda)

![image](https://github.com/user-attachments/assets/bc897566-f5a9-45d2-9a98-a2a0c7620f9e)


![image](https://github.com/user-attachments/assets/6c14e0b4-fbd8-4377-90b2-b10cc362208a)


We can observe the value of /xreg[14] it has a hexadecimal value of 2D which converts to 45 in decimal system which is the sum from 1 to 9.


### Task 7

We use the following commands:

``` !bash

$ sudo apt install make python3 python3 python3-pip git iverilog gtkwave

$ cd ~

$ sudo apt-get install python3-venv

$ python3 -m venv .venv

$ source ~/.venv/bin/activate

$ pip3 install pyyaml click sandpiper-saas

$ sudo apt install make python python3 python3-pip git iverilog gtkwave docker.io

$ sudo chmod 666 /var/run/docker.sock

$ cd ~

$ pip3 install pyyaml click sandpiper-saas

$ cd ~

$ git clone https://github.com/manili/VSDBabySoC.git

$ cd /home/vsduser/VSDBabySoC

$ make pre_synth_sim


$ sandpiper-saas -i ./src/module/rvmyth.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/

$ iverilog -o output/pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module
$ cd output
$ ./pre_synth_sim.out


```

We use a virtual environment to carry out our tasks .Through these commands we first setup the virtual environment. The provided tool is then used to generate a .v file for the corresponding .tlv file. Then using the testbench gtkwave simulation is used for analysis.

![image](https://github.com/user-attachments/assets/65b7e3cd-b5e6-4dd2-aa15-54433472e23e)



![image](https://github.com/user-attachments/assets/69916d23-bfde-4a0e-aee8-46d414dd3acd)



GTKWave simulation waveforms for clock , reset and output
![image](https://github.com/user-attachments/assets/d53aac1f-7b57-42a6-87e7-d397164d42b8)

Corresponding Makerchip simulations


![image](https://github.com/user-attachments/assets/d4778cc5-49d0-4c2c-add4-8b63140c2276)


![image](https://github.com/user-attachments/assets/1518df93-cd04-4c23-a050-c1bed78b4dda)




![image](https://github.com/user-attachments/assets/6c14e0b4-fbd8-4377-90b2-b10cc362208a)


Gradual Increment can be seen . It is observed that both the waveforms reach the value 45 .

## Task 8
Generating waveform for DAC and PLL for RISC-V processor

Iverilog , gtkwave , yosys and opensta installed.
![image](https://github.com/user-attachments/assets/a4dfd95a-c158-42c6-b020-940eedc29f5f)


We clone the given repository. Following which we edit the top level verilog code as instructed.
We then use the following commands:
```!bash
iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/
./pre_synth_sim.out
gtkwave pre_synth_sim.vcd
```

![image](https://github.com/user-attachments/assets/55366e4a-e206-4b48-81e1-f5701514f572)

Final waveforms:

![image](https://github.com/user-attachments/assets/b4913d8a-ed95-43ee-94e7-cd7365428fdd)


## Task 9

### Day 1

![day1lab1](https://github.com/user-attachments/assets/91916ffd-4bab-48b4-a7b3-9f802445e692)


![day1lab2_1](https://github.com/user-attachments/assets/662254ff-352e-4868-8597-52474ab91bab)


GTKwave simulation results:
![day1lab2_2](https://github.com/user-attachments/assets/ead14d43-3657-4835-8590-ce4d3b089505)

Code
![image](https://github.com/user-attachments/assets/b2d55509-c94a-4855-8177-76cedce8a4f5)

Testbench

![image](https://github.com/user-attachments/assets/777d3e97-b364-42d9-b0f9-8a0e314ca2e7)  


Yosys:

![image](https://github.com/user-attachments/assets/183a433c-6ff4-4c94-a36c-aacf88e27e30)


![image](https://github.com/user-attachments/assets/e7144b9d-42d2-475c-bfe1-f6e56783981f)


![image](https://github.com/user-attachments/assets/f2bf31f5-5d21-4698-8edc-70ed3e751d71)

Synthesis:
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verlog good_mux_netlist.v
write_verilog -noattr good_mux_netlist.v

```

Output
![image](https://github.com/user-attachments/assets/d249a67a-7724-48d8-b3c8-9f1ee6eef399)

using the command "show":


![image](https://github.com/user-attachments/assets/7b4fcdfb-0bfd-4bda-a8e9-05d24d5102bf)

![image](https://github.com/user-attachments/assets/e387bdcb-6292-45e4-8693-7ed83e50a4c3)

## Day 2

Contents inside .lib file

```bash
cd ../lib
vim sky130_fd_sc_hd__tt_025C_1v80.lib

```

![image](https://github.com/user-attachments/assets/54630a1d-c85f-4091-a886-78413ca71bd1)

The liberty (.lib) files contain PVT parameters (Process, Voltage, Temperature). Changes in these parameters can greatly influence circuit performance. 

Multiple and designs

![image](https://github.com/user-attachments/assets/847c954e-4385-4acb-ad8e-c0605d3fe19f)

![image](https://github.com/user-attachments/assets/034e8386-2901-4279-83b1-8b432b6e454a)

![image](https://github.com/user-attachments/assets/a5bb8b34-6ee1-42df-87b0-01438bdef7ef)

Observations:

    and2_0: Minimizes area but results in higher delay and lower power consumption.
    and2_1: Occupies more area, offers reduced delay, and consumes more power.
    and2_2: Has the largest area, incurs the greatest delay, and uses the highest amount of power.


Hierarchial and Flat Synthesis

Verilog Code 

```bash
module sub_module2 (input a, input b, output y);
    assign y = a | b;
endmodule

module sub_module1 (input a, input b, output y);
    assign y = a&b;
endmodule


module multiple_modules (input a, input b, input c , output y);
    wire net1;
    sub_module1 u1(.a(a),.b(b),.y(net1));  //net1 = a&b
    sub_module2 u2(.a(net1),.b(c),.y(y));  //y = net1|c ,ie y = a&b + c;
endmodule


```

Hierarchial Synthesis

![image](https://github.com/user-attachments/assets/71862750-793c-4c70-aeab-ad3ed5bfa2b2)


![image](https://github.com/user-attachments/assets/7e86841d-1d38-4eab-844a-581e2d55ac4d)


Netlist:
![image](https://github.com/user-attachments/assets/e92b5e9a-6d54-4e56-929e-a5240fe921c6)

Netlist Code:
![image](https://github.com/user-attachments/assets/5c11eb90-2273-4cf5-b5ce-0a3f31f9d721)


Flat Synthesis

![image](https://github.com/user-attachments/assets/f89a96df-f02f-48f1-870d-6264fb7480e3)

![image](https://github.com/user-attachments/assets/c47734a6-4b66-4b76-8f92-35ea0eb5ab55)

![image](https://github.com/user-attachments/assets/a443d7f5-0bd7-461a-8324-d76833b90487)


Netlist:

![image](https://github.com/user-attachments/assets/776e3513-4db4-4ea2-a9e1-35fe87fd5458)

Netlist Code:

![image](https://github.com/user-attachments/assets/4045365c-e2be-4616-83dd-2e40a57d1c61)


Sub module synthesis
```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
read_verilog multiple_modules.v 
synth -top sub_module1
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr multiple_modules_submodule1.v
```

![image](https://github.com/user-attachments/assets/9823b114-50e3-4cf3-a678-5d7e9f0621b9)

Netlist:

![image](https://github.com/user-attachments/assets/ed60706f-b0fc-4cc2-b7ef-a89b9420f9b7)

Netlist Code:
![image](https://github.com/user-attachments/assets/b393e0de-d0e5-4a9e-82ca-06ff227369b2)


Flip Flops

Asynchronous Reset Flip Flop

Code:

```bash
module dff_asyncres ( input clk ,  input async_reset , input d , output reg q );
always @ (posedge clk , posedge async_reset)
begin
	if(async_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule

```


Commands to view simulation

```bash
iverilog dff_asyncres.v tb_dff_asyncres.v
./a.out
gtkwave tb_dff_asyncres.vcd

```

Output 

![image](https://github.com/user-attachments/assets/afb94dbf-5095-4db3-bcbe-f4501b33ddd5)

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_asyncres.v
synth -top dff_asyncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

```

Netlist:

![image](https://github.com/user-attachments/assets/20af24c0-b176-416e-8159-231153c4832f)


Synchronous Reset Flip Flop

Code:

```bash

module dff_syncres ( input clk , input async_reset , input sync_reset , input d , output reg q );
always @ (posedge clk )
begin
	if (sync_reset)
		q <= 1'b0;
	else	
		q <= d;
end
endmodule

```

```bash

iverilog dff_syncres.v tb_dff_syncres.v
./a.out
gtkwave tb_dff_syncres.vcd

```

Output:
![image](https://github.com/user-attachments/assets/0a7dbe68-c193-4160-92f5-4f7365e13de7)


```bash

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_syncres.v
synth -top dff_syncres
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

```

![image](https://github.com/user-attachments/assets/c2e0fa3f-af9a-476a-a240-6213ace9cf24)


Netlist:
![image](https://github.com/user-attachments/assets/062e2ad9-1cfe-4945-8bbf-9180ef1bea07)


Aysnchronous set flip flop

Code:

```bash

module dff_async_set ( input clk ,  input async_set , input d , output reg q );
always @ (posedge clk , posedge async_set)
begin
	if(async_set)
		q <= 1'b1;
	else	
		q <= d;
end
endmodule

```
Output:

![image](https://github.com/user-attachments/assets/76629ffa-7b15-4042-b0f4-2db7075954a1)

Commands to run

```bash

yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_async_set.v
synth -top dff_async_set
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show

```

![image](https://github.com/user-attachments/assets/627ab794-a4f6-4162-bc8f-e47c645a7188)


Netlist:

![image](https://github.com/user-attachments/assets/a23f3c83-99a3-4c4e-8eff-eae74b62efbf)



Optimizations:

Example 1

Verilog file mult_2.v

Code:

```bash
module mul2 (input [2:0] a, output [3:0] y);
assign y = a * 2;
endmodule

```


Truth Table:

| a2 | a1 | a0 | y3 | y2 | y1 | y0 |
|---|---|---|---|---|---|---|
| 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 | 1 | 0 |
| 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 0 | 1 | 1 | 0 | 1 | 1 | 0 |
| 1 | 0 | 0 | 1 | 0 | 0 | 0 |
| 1 | 0 | 1 | 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 | 1 | 0 | 0 |
| 1 | 1 | 1 | 1 | 1 | 1 | 0 |


Multiplying a number by 2 doesn't require additional hardware; it simply involves appending zeroes to the least significant bits (LSBs) while keeping the other bits unchanged. This can be achieved by grounding the LSBs and properly connecting the input to the output.

Commands to run


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_2.v
synth -top mul2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_2_net.v
```


![image](https://github.com/user-attachments/assets/48de3ee4-1480-4b0e-9552-e81af7c61cc8)


Netlist:

![image](https://github.com/user-attachments/assets/d881934b-a271-463b-b7d7-50af6a419c19)

Netlist Code:

```bash
/* Generated by Yosys 0.44+60 (git sha1 c25448f1d, g++ 11.4.0-1ubuntu1~22.04 -fPIC -O3) */

module mul2(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [3:0] y;
  wire [3:0] y;
  assign y = { a, 1'h0 };
endmodule

```
Example 2


Verilog code "mult_8.v"

In this design, the 3-bit input number "a" is multiplied by 9, which can be expressed as (a×8)+a(a×8)+a. The term (a×8)(a×8) is equivalent to left-shifting the number aa by three bits. Let’s consider a=a2a1a0a=a2​a1​a0​. The result of (a×8)(a×8) will be a2a1a0000a2​a1​a0​000. Therefore, (a×9)=(a×8)+a=a2a1a0a2a1a0(a×9)=(a×8)+a=a2​a1​a0​a2​a1​a0​, which can be represented as aaaa in a 6-bit format. As a result, no additional hardware is needed for this operation. 

```
module mult8 (input [2:0] a , output [5:0] y);
	assign y = a * 9;
endmodule
```
Commands to run

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog mult_8.v
synth -top mult8
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr mult_8_net.v
```

![image](https://github.com/user-attachments/assets/80d965a4-1ea3-4dd1-9b81-35a218aa354b)


Netlist:
![image](https://github.com/user-attachments/assets/d03ab954-538c-44fe-8387-27e3698651e6)

Netlist Code:

```

module mult8(a, y);
  input [2:0] a;
  wire [2:0] a;
  output [5:0] y;
  wire [5:0] y;
  assign y = { a, a };
endmodule

```

## Day 3

Optimizations
There are two categories of optimizations: combinational and sequential. These optimizations are performed to create designs that are more efficient regarding area, power, and performance.

Combinational Optimization

    Constant Propagation (Direct Optimization)
    Boolean Logic Optimization (using  K-Maps or Quine-McCluskey)


Example 1:

Verllog code:

```
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule

```

The code above infers a multiplexer, and because one of its inputs is permanently connected to ground, it will optimize to an AND gate

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check.v
synth -top opt_check
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check_net.v
```

Netlist:
![image](https://github.com/user-attachments/assets/efb615e0-b5f4-461a-acff-fe92a850986a)

Netlist Code

```

module opt_check(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _3_ (
    .A(_1_),
    .B(_0_),
    .X(_2_)
  );
  assign _1_ = b;
  assign _0_ = a;
  assign y = _2_;
endmodule

```
Example 2

```
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```
In this scenario, the multiplexer’s output is consistently tied to logic 1, leading to an optimization that results in an OR gate. Importantly, the OR gate is implemented using a NAND configuration because NAND designs use stacked NMOS transistors, whereas NOR gates require stacked PMOS transistors. This design choice is commonly made to improve performance, as NMOS transistors generally exhibit lower resistance than PMOS transistors.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check2.v
synth -top opt_check2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check2_net.v
```
![image](https://github.com/user-attachments/assets/b433db36-07f2-479e-a240-4a7e73e1e9ad)


Netlist:
![image](https://github.com/user-attachments/assets/ce0c6611-3f85-4ffb-a932-fb6f6aaffaaf)

Netlist Code:
```

module opt_check2(a, b, y);
  wire _0_;
  wire _1_;
  wire _2_;
  input a;
  wire a;
  input b;
  wire b;
  output y;
  wire y;
  sky130_fd_sc_hd__or2_0 _3_ (
    .A(_0_),
    .B(_1_),
    .X(_2_)
  );
  assign _0_ = a;
  assign _1_ = b;
  assign y = _2_;
endmodule

```

Example 3

code:

```
module opt_check3 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
Post optimization, the design simplifies to a 3-input AND gate.


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check3.v
synth -top opt_check3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check3_net.v
```


![image](https://github.com/user-attachments/assets/8465bd3e-2f81-4c15-8d3e-d57d5bb5e227)


Netlist:
![image](https://github.com/user-attachments/assets/1b884218-e142-4d2d-889a-9ada81595354)

Netlist Code:

```

module opt_check3(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output y;
  wire y;
  sky130_fd_sc_hd__and3_1 _5_ (
    .A(_2_),
    .B(_3_),
    .C(_1_),
    .X(_4_)
  );
  assign _2_ = b;
  assign _3_ = c;
  assign _1_ = a;
  assign y = _4_;
endmodule

```
Example 4

code:

```
module opt_check4 (input a , input b, input c , output y);
	assign y = a?(c?b:0):0;
endmodule
```
Post optimization, the design simplifies to a 2-input XNOR gate.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog opt_check4.v
synth -top opt_check4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr opt_check4_net.v
```

![image](https://github.com/user-attachments/assets/d513fc18-8264-40a3-bb58-1b8ad0b3423c)


Netlist:
![image](https://github.com/user-attachments/assets/babd5f23-5953-42a3-9a60-745fab313b2d)


Netlist Code:

```

module opt_check4(a, b, c, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  wire _6_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output y;
  wire y;
  sky130_fd_sc_hd__xnor2_1 _7_ (
    .A(_5_),
    .B(_3_),
    .Y(_6_)
  );
  assign _5_ = c;
  assign _3_ = a;
  assign _4_ = b;
  assign y = _6_;
endmodule

```

Example 5

code:

```
module sub_module1(input a , input b , output y);
 assign y = a & b;
endmodule

module sub_module2(input a , input b , output y);
 assign y = a^b;
endmodule

module multiple_module_opt(input a , input b , input c , input d , output y);
wire n1,n2,n3;

sub_module1 U1 (.a(a) , .b(1'b1) , .y(n1));
sub_module2 U2 (.a(n1), .b(1'b0) , .y(n2));
sub_module2 U3 (.a(b), .b(d) , .y(n3));

assign y = c | (b & n1); 

endmodule
```
Post optimization, the design simplifies to a AND OR gate.

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt.v
synth -top multiple_module_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt_net.v
```
![image](https://github.com/user-attachments/assets/ce32aa11-15c7-4c9b-9278-7bd4d1f0699c)



Netlist:
![image](https://github.com/user-attachments/assets/8d55a64a-15fd-4ef9-a623-4ff380512dee)

Netlist Code:


```

module multiple_module_opt(a, b, c, d, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  wire _6_;
  wire _7_;
  wire \U1.a ;
  wire \U1.b ;
  wire \U1.y ;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  input d;
  wire d;
  wire n1;
  output y;
  wire y;
  sky130_fd_sc_hd__a21o_1 _8_ (
    .A1(_3_),
    .A2(_1_),
    .B1(_2_),
    .X(_4_)
  );
  sky130_fd_sc_hd__and2_0 _9_ (
    .A(_6_),
    .B(_5_),
    .X(_7_)
  );
  assign _3_ = n1;
  assign _1_ = b;
  assign _2_ = c;
  assign y = _4_;
  assign _6_ = \U1.b ;
  assign _5_ = \U1.a ;
  assign \U1.y  = _7_;
  assign \U1.a  = a;
  assign \U1.b  = 1'h1;
  assign n1 = \U1.y ;
endmodule
```

Example 6

code:

```
module sub_module(input a , input b , output y);
	assign y = a & b;
endmodule

module multiple_module_opt2(input a , input b , input c , input d , output y);
		wire n1,n2,n3;
	sub_module U1 (.a(a) , .b(1'b0) , .y(n1));
	sub_module U2 (.a(b), .b(c) , .y(n2));
	sub_module U3 (.a(n2), .b(d) , .y(n3));
	sub_module U4 (.a(n3), .b(n1) , .y(y));
endmodule
```
After optimizing design becomes Y=0

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module_opt2.v
synth -top multiple_module_opt2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
flatten
show
write_verilog -noattr multiple_module_opt2_net.v
```
![image](https://github.com/user-attachments/assets/a8fbd81c-f2ca-402f-8f27-96f4a2edb720)


Netlist:


![image](https://github.com/user-attachments/assets/698f9c14-831d-4fb0-8e1b-decb679796fd)

Netlist Code:

```
	
module multiple_module_opt2(a, b, c, d, y);
  wire _00_;
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  wire _05_;
  wire _06_;
  wire _07_;
  wire _08_;
  wire _09_;
  wire _10_;
  wire _11_;
  wire \U1.a ;
  wire \U1.b ;
  wire \U1.y ;
  wire \U2.a ;
  wire \U2.b ;
  wire \U2.y ;
  wire \U3.a ;
  wire \U3.b ;
  wire \U3.y ;
  wire \U4.a ;
  wire \U4.b ;
  wire \U4.y ;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  input d;
  wire d;
  wire n1;
  wire n2;
  wire n3;
  output y;
  wire y;
  sky130_fd_sc_hd__and2_0 _12_ (
    .A(_01_),
    .B(_00_),
    .X(_02_)
  );
  sky130_fd_sc_hd__and2_0 _13_ (
    .A(_04_),
    .B(_03_),
    .X(_05_)
  );
  sky130_fd_sc_hd__and2_0 _14_ (
    .A(_07_),
    .B(_06_),
    .X(_08_)
  );
  sky130_fd_sc_hd__and2_0 _15_ (
    .A(_10_),
    .B(_09_),
    .X(_11_)
  );
  assign _10_ = \U4.b ;
  assign _09_ = \U4.a ;
  assign \U4.y  = _11_;
  assign \U4.a  = n3;
  assign \U4.b  = n1;
  assign y = \U4.y ;
  assign _07_ = \U3.b ;
  assign _06_ = \U3.a ;
  assign \U3.y  = _08_;
  assign \U3.a  = n2;
  assign \U3.b  = d;
  assign n3 = \U3.y ;
  assign _04_ = \U2.b ;
  assign _03_ = \U2.a ;
  assign \U2.y  = _05_;
  assign \U2.a  = b;
  assign \U2.b  = c;
  assign n2 = \U2.y ;
  assign _01_ = \U1.b ;
  assign _00_ = \U1.a ;
  assign \U1.y  = _02_;
  assign \U1.a  = a;
  assign \U1.b  = 1'h0;
  assign n1 = \U1.y ;
endmodule

```

Sequential Logic Optimizations

Example 1

code:

```
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const1.v
synth -top dff_const1
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const1_net.v
```

![image](https://github.com/user-attachments/assets/7cbc66d9-0696-4182-a800-94076b616775)


Netlist 

![image](https://github.com/user-attachments/assets/63a60b54-897f-4cdd-818b-1b5545630c7a)

Netlist Code:

```

module dff_const1(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  input clk;
  wire clk;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _3_ (
    .A(_1_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__dfrtp_1 _4_ (
    .CLK(clk),
    .D(1'h1),
    .Q(q),
    .RESET_B(_2_)
  );
  assign _1_ = reset;
  assign _2_ = _0_;
endmodule

```
Output on gtkwave

![image](https://github.com/user-attachments/assets/649ad362-0f76-4c32-86f9-78b456408781)



Example 2

code:

```
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const2.v
synth -top dff_const2
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const2_net.v
```

![image](https://github.com/user-attachments/assets/218eefb8-44cf-4e4c-aced-591f737dd41b)


Netlist:

![image](https://github.com/user-attachments/assets/9c65e766-4cdb-4179-98ee-664fbca1c57c)

Netlist Code:

```

module dff_const2(clk, reset, q);
  input clk;
  wire clk;
  output q;
  wire q;
  input reset;
  wire reset;
  assign q = 1'h1;
endmodule

```
Output on gtkwave
![image](https://github.com/user-attachments/assets/3bd8643b-c400-4fb3-bd27-253b3aa84825)

Example 3

code:

```
module dff_const3(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b0;
	end
	else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const3.v
synth -top dff_const3
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const3_net.v
```

![image](https://github.com/user-attachments/assets/410f5a45-097f-455a-a8a7-cbc094ecf8dc)

Netlist:

![image](https://github.com/user-attachments/assets/d5e30e4f-b400-4564-8006-83831c53461c)

Netlist Code:

```

module dff_const3(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input clk;
  wire clk;
  output q;
  wire q;
  wire q1;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _5_ (
    .A(_2_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_2_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__dfstp_2 _7_ (
    .CLK(clk),
    .D(q1),
    .Q(q),
    .SET_B(_3_)
  );
  sky130_fd_sc_hd__dfrtp_1 _8_ (
    .CLK(clk),
    .D(1'h1),
    .Q(q1),
    .RESET_B(_4_)
  );
  assign _2_ = reset;
  assign _3_ = _0_;
  assign _4_ = _1_;
endmodule


```

Output on Gtkwave

![image](https://github.com/user-attachments/assets/e77edaef-6186-4d05-8155-6a507aa853c5)

Example 4

code:

```
module dff_const4(input clk, input reset, output reg q);
reg q1;

always @(posedge clk, posedge reset)
begin
	if(reset)
	begin
		q <= 1'b1;
		q1 <= 1'b1;
	end
else
	begin
		q1 <= 1'b1;
		q <= q1;
	end
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const4.v
synth -top dff_const4
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const4_net.v
```

![image](https://github.com/user-attachments/assets/d4b8891c-56ed-4e4b-9b25-ec736a0f94af)

Netlist:

![image](https://github.com/user-attachments/assets/8fb760f5-b77f-4321-9cc9-857e05046759)

Netlist Code:

```

module dff_const4(clk, reset, q);
  input clk;
  wire clk;
  output q;
  wire q;
  wire q1;
  input reset;
  wire reset;
  assign q = 1'h1;
  assign q1 = 1'h1;
endmodule

```

Output on gtkwave

![image](https://github.com/user-attachments/assets/fa287499-a1bd-44bb-aaf3-6255c4259788)

Example 5

code:

```
module dff_const5(input clk, input reset, output reg q);
reg q1;
always @(posedge clk, posedge reset)
	begin
		if(reset)
		begin
			q <= 1'b0;
			q1 <= 1'b0;
		end
	else
		begin
			q1 <= 1'b1;
			q <= q1;
		end
	end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog dff_const5.v
synth -top dff_const5
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr dff_const5_net.v
```

![image](https://github.com/user-attachments/assets/3da5876e-9de6-47ff-b82e-d0bb80a46641)


Netlist:

![image](https://github.com/user-attachments/assets/980cdae5-7968-4704-a6ef-2a28145842b9)

Netlist Code:

```

module dff_const5(input clk, input reset, output reg q);
    reg q1;
    always @(posedge clk, posedge reset) begin
        if (reset) begin
            q <= 1'b0;
            q1 <= 1'b0;
        end else begin
            q1 <= 1'b1;
            q <= q1;
        end
    end
endmodule
```

Output on gtkwave

![image](https://github.com/user-attachments/assets/6ca6ccac-c785-4155-bfd0-899a51f373d4)

Sequential logic optimizations for unused outputs

Example 1
code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = count[0];
always @(posedge clk ,posedge reset)
begin
	if(reset)
		count <= 3'b000;
	else
		count <= count + 1;
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt_net.v
```

![image](https://github.com/user-attachments/assets/916fe77e-5061-4200-8b17-642e869d4c9c)


Netlist:

![image](https://github.com/user-attachments/assets/2f823b1e-50dc-4454-9f49-b72eaaa60e14)


Netlist Code:

```

module counter_opt(clk, reset, q);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire [2:0] _4_;
  wire _5_;
  input clk;
  wire clk;
  wire [2:0] count;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_2_),
    .Y(_0_)
  );
  sky130_fd_sc_hd__clkinv_1 _7_ (
    .A(_3_),
    .Y(_1_)
  );
  sky130_fd_sc_hd__dfrtp_1 _8_ (
    .CLK(clk),
    .D(_4_[0]),
    .Q(count[0]),
    .RESET_B(_5_)
  );
  assign _4_[2:1] = count[2:1];
  assign q = count[0];
  assign _2_ = count[0];
  assign _4_[0] = _0_;
  assign _3_ = reset;
  assign _5_ = _1_;
endmodule

```


Output on gtkwave

![image](https://github.com/user-attachments/assets/c69eb90c-74ad-4e48-8a2c-daccbbf21e00)


Modified counter :

code:

```
module counter_opt (input clk , input reset , output q);
reg [2:0] count;
assign q = {count[2:0]==3'b100};
always @(posedge clk ,posedge reset)
begin
if(reset)
	count <= 3'b000;
else
	count <= count + 1;
end
endmodule
```

```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog counter_opt.v
synth -top counter_opt
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
write_verilog -noattr counter_opt_net.v
```

![image](https://github.com/user-attachments/assets/bd5a71a4-829a-4b30-be35-ec7aeb092033)

Netlist:

![image](https://github.com/user-attachments/assets/5a448bf8-5c67-4130-ad9a-dd99964ae158)

Netlist Code:

```

module counter_opt(clk, reset, q);
  wire _00_;
  wire _01_;
  wire _02_;
  wire _03_;
  wire _04_;
  wire _05_;
  wire _06_;
  wire _07_;
  wire _08_;
  wire _09_;
  wire _10_;
  wire _11_;
  wire _12_;
  wire _13_;
  wire [2:0] _14_;
  wire [2:0] _15_;
  wire _16_;
  wire _17_;
  wire _18_;
  input clk;
  wire clk;
  wire [2:0] count;
  output q;
  wire q;
  input reset;
  wire reset;
  sky130_fd_sc_hd__clkinv_1 _19_ (
    .A(_08_),
    .Y(_02_)
  );
  sky130_fd_sc_hd__clkinv_1 _20_ (
    .A(_13_),
    .Y(_05_)
  );
  sky130_fd_sc_hd__nor3b_1 _21_ (
    .A(_08_),
    .B(_09_),
    .C_N(_10_),
    .Y(_12_)
  );
  sky130_fd_sc_hd__nand2_1 _22_ (
    .A(_08_),
    .B(_09_),
    .Y(_11_)
  );
  sky130_fd_sc_hd__xor2_1 _23_ (
    .A(_08_),
    .B(_09_),
    .X(_03_)
  );
  sky130_fd_sc_hd__xnor2_1 _24_ (
    .A(_10_),
    .B(_11_),
    .Y(_04_)
  );
  sky130_fd_sc_hd__clkinv_1 _25_ (
    .A(_13_),
    .Y(_06_)
  );
  sky130_fd_sc_hd__clkinv_1 _26_ (
    .A(_13_),
    .Y(_07_)
  );
  sky130_fd_sc_hd__dfrtp_1 _27_ (
    .CLK(clk),
    .D(_14_[0]),
    .Q(count[0]),
    .RESET_B(_16_)
  );
  sky130_fd_sc_hd__dfrtp_1 _28_ (
    .CLK(clk),
    .D(_15_[1]),
    .Q(count[1]),
    .RESET_B(_17_)
  );
  sky130_fd_sc_hd__dfrtp_1 _29_ (
    .CLK(clk),
    .D(_15_[2]),
    .Q(count[2]),
    .RESET_B(_18_)
  );
  assign _15_[0] = _14_[0];
  assign _14_[2:1] = count[2:1];
  assign _08_ = count[0];
  assign _14_[0] = _02_;
  assign _09_ = count[1];
  assign _10_ = count[2];
  assign q = _12_;
  assign _15_[1] = _03_;
  assign _15_[2] = _04_;
  assign _13_ = reset;
  assign _16_ = _05_;
  assign _17_ = _06_;
  assign _18_ = _07_;
endmodule

```

Output on gtkwave

![image](https://github.com/user-attachments/assets/ac7a640b-dd34-47a7-888b-c5a93ad594b6)


## Day 4

Gate Level Simulation (GLS) is an essential part of the verification process for digital circuits. It entails simulating the synthesized netlist, which serves as a lower-level representation of the design, using a testbench to confirm its logical accuracy and timing characteristics. By comparing the simulated outputs with the expected results, GLS ensures that the synthesis process has not introduced any errors and that the design fulfills its performance criteria.


Sensitivity lists are essential for ensuring correct circuit behavior. An incomplete sensitivity list can result in unexpected latches. Additionally, blocking and non-blocking assignments in always blocks have distinct execution behaviors. Misusing blocking assignments can inadvertently lead to latches, resulting in discrepancies between synthesis and simulation. To prevent these problems, it’s important to thoroughly analyze circuit behavior and ensure that the sensitivity list and assignments match the intended functionality.

![image](https://github.com/user-attachments/assets/bd73c591-a8e9-4798-a9ea-2e7160eecdee)

GLS Simulation

Example 1

code:

```
module ternary_operator_mux (input i0 , input i1 , input sel , output y);
assign y = sel?i1:i0;
endmodule
```

Simulation:

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

Gtkwave

![image](https://github.com/user-attachments/assets/3fa87094-74e2-4b51-88a0-5db5e2b5a5eb)


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr ternary_operator_mux_net.v
```

![image](https://github.com/user-attachments/assets/3e4953df-4f90-44d1-bbf3-921142c809d8)


Netlist

![image](https://github.com/user-attachments/assets/ce656734-7f7e-4600-86d1-1b49c1a440f1)

Netlist code:

```

module ternary_operator_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule


```

Gate level simulation

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v ternary_operator_mux_net.v tb_ternary_operator_mux.v
./a.out
gtkwave tb_ternary_operator_mux.vcd
```

![image](https://github.com/user-attachments/assets/2ed1ceee-0a8d-47d8-a5e6-65727b358b4e)

Example 2

Code:

```
module bad_mux (input i0 , input i1 , input sel , output reg y);
always @ (sel)
begin
	if(sel)
		y <= i1;
	else 
		y <= i0;
end
endmodule
```

GTKwave simulation

```
iverilog bad_mux.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![image](https://github.com/user-attachments/assets/2924ee43-fde1-4bf1-a483-d9c9953fd204)


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog bad_mux.v
synth -top bad_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr bad_mux_net.v
```

![image](https://github.com/user-attachments/assets/51a6100f-ca43-490e-8332-135cafda42d5)


Netlist:

![image](https://github.com/user-attachments/assets/5c7eab33-b77a-4de7-91ec-4db536c65448)


```

module bad_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  input i0;
  wire i0;
  input i1;
  wire i1;
  input sel;
  wire sel;
  output y;
  wire y;
  sky130_fd_sc_hd__mux2_1 _4_ (
    .A0(_0_),
    .A1(_1_),
    .S(_2_),
    .X(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```

Gate level simulation:

```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v bad_mux_net.v tb_bad_mux.v
./a.out
gtkwave tb_bad_mux.vcd
```

![image](https://github.com/user-attachments/assets/0962c542-cd23-4029-a77b-4cfd9cb7848e)


In this instance, there is a mismatch between synthesis and simulation. During the synthesis process, Yosys has corrected the sensitivity list error.


### Synthesis-Simulation Mismatch for Blocking Assignments

code:

```
module blocking_caveat (input a , input b , input  c, output reg d); 
reg x;
always @ (*)
begin
d = x & c;
x = a | b;
end
endmodule
```

GTKwave Simulation:

```
iverilog blocking_caveat.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![image](https://github.com/user-attachments/assets/b6570128-3ba7-49df-bcee-fe6fd9786377)


```
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib  
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib 
show
write_verilog -noattr blocking_caveat_net.v
```
![image](https://github.com/user-attachments/assets/2becff78-51b0-40d7-9107-85c32bf1cdd6)


Netlist:

![image](https://github.com/user-attachments/assets/000fd2e1-432d-4b92-8fd4-3a43b2e95b5b)


```

module blocking_caveat(a, b, c, d);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  input a;
  wire a;
  input b;
  wire b;
  input c;
  wire c;
  output d;
  wire d;
  sky130_fd_sc_hd__o21a_1 _5_ (
    .A1(_2_),
    .A2(_1_),
    .B1(_3_),
    .X(_4_)
  );
  assign _2_ = b;
  assign _1_ = a;
  assign _3_ = c;
  assign d = _4_;
endmodule

```

Gate Level Simulation
```
iverilog ../my_lib/verilog_model/primitives.v ../my_lib/verilog_model/sky130_fd_sc_hd.v blocking_caveat_net.v tb_blocking_caveat.v
./a.out
gtkwave tb_blocking_caveat.vcd
```

![image](https://github.com/user-attachments/assets/babf0564-8707-4a59-8cb3-ea4518a2ff4c)

There is a mismatch between synthesis and simulation. During the synthesis process, Yosys has fixed the latch error.


## Task 10

The src folder from  BabySoC folder is copied inside sky130RTLDesignAndSynthesisWorkshop folder in the VLSI folder from previous lab.

Go to this path : ~/Desktop/asic/VLSI/sky130RTLDesignAndSynthesisWorkshop/src/module


Synthesis

Commands

```
yosys
read_liberty -lib ~/Desktop/asic/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog clk_gate.v
read_verilog rvmyth.v
synth -top rvmyth
abc -liberty ~/Desktop/asic/VLSI/sky130RTLDesignAndSynthesisWorkshop/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr rvmyth_net.v
vim rvmyth_net.v

```




![image](https://github.com/user-attachments/assets/0c857ce3-2690-4681-b321-f7f9756c03ba)  

![image](https://github.com/user-attachments/assets/e814e356-727c-4ac5-96d1-6d6e1ad0f526)


Netlist File

![image](https://github.com/user-attachments/assets/5620c3b2-7c9b-42ce-88d5-d85a33dc86a3)

![image](https://github.com/user-attachments/assets/6e014370-d0fb-42fd-b7a9-87f1be80c07d)


Simulation

![image](https://github.com/user-attachments/assets/52be6fed-0ae4-4d34-8962-2e14abeda04a)


Output waveform

![image](https://github.com/user-attachments/assets/dda383b5-80bb-485b-a916-fa6fce2fd5a4)

![image](https://github.com/user-attachments/assets/7bfd2f84-7443-45a7-8180-7fc01cfeaec0)


RTL Simulations

```

cd BabySoC

iverilog -o ./pre_synth_sim.out -DPRE_SYNTH_SIM src/module/testbench.v -I src/include -I src/module/

./pre_synth_sim.out

gtkwave pre_synth_sim.vcd


```

![image](https://github.com/user-attachments/assets/929c372e-fab0-461a-8cf9-5aa6983491fb)

![image](https://github.com/user-attachments/assets/19f77bdd-1970-43e1-9713-722b08125ee0)

![image](https://github.com/user-attachments/assets/3bf64c59-8304-4d02-aa65-868780d6f2aa)


## Task 11

Commands to run:
```
cd VSDBabySoc/src
sta
read_liberty -min ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -min ./lib/avsdpll.lib
read_liberty -min ./lib/avsddac.lib
read_liberty -max ./lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max ./lib/avsdpll.lib
read_liberty -max ./lib/avsddac.lib
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
report_checks -path_delay min_max -format full_clock_expanded -digits 4

```
Screenshots related to setup , hold time

![WhatsApp Image 2024-10-28 at 16 46 57](https://github.com/user-attachments/assets/c7509271-1f69-4a1f-b9b3-901e1a535f74)

![WhatsApp Image 2024-10-28 at 16 46 57(1)](https://github.com/user-attachments/assets/5eaf740c-63c8-46ad-86a3-d023a4e92a0d)

![WhatsApp Image 2024-10-28 at 16 46 57(2)](https://github.com/user-attachments/assets/6902637a-9d17-464c-93a3-4e247dc7f9cc)



sdc file :

```

set PERIOD 11.25

set_units -time ns
create_clock [get_pins {pll/CLK}] -name clk -period $PERIOD
set_clock_uncertainty -setup  [expr $PERIOD * 0.05] [get_clocks clk]
set_input_delay -min 0 [get_ports ENb_VCO] -clock [get_clocks "clk"]
set_input_delay -min 0 [get_ports ENb_CP] -clock [get_clocks "clk"]
set_input_delay -min 0 [get_ports VCO_IN] -clock [get_clocks "clk"]
set_input_delay -min 0 [get_ports VREFH] -clock [get_clocks "clk"]
set_input_delay -min 0 [get_ports REF] -clock [get_clocks "clk"]
set_clock_transition [expr $PERIOD * 0.05] [get_clocks clk]
set_clock_uncertainty -hold [expr $PERIOD * 0.08] [get_clocks clk]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_VCO]
set_input_transition [expr $PERIOD * 0.08] [get_ports ENb_CP]
set_input_transition [expr $PERIOD * 0.08] [get_ports REF]
set_input_transition [expr $PERIOD * 0.08] [get_ports VREFH]
set_input_transition [expr $PERIOD * 0.08] [get_ports VCO_IN]

```

## Task 12

We will navigate to VSDBabySoC/src and make a file named sta_across_pvt.tcl . We will create a folder timing_libs and have all the lib files in it. 
Tickle file:

```
set list_of_lib_files(1) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v95.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1"
set list_of_lib_files(8) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2"
set list_of_lib_files(9) "sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(14) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(15) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(16) "sky130_fd_sc_hd__ss_n40C_1v60.lib"
set list_of_lib_files(17) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1"
set list_of_lib_files(18) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2"
set list_of_lib_files(19) "sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3"
set list_of_lib_files(20) "sky130_fd_sc_hd__ss_n40C_1v76.lib"
set list_of_lib_files(21) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(22) "sky130_fd_sc_hd__tt_100C_1v80.lib"

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {
read_liberty ./timing_libs/$list_of_lib_files($i)
read_verilog ../output/synth/vsdbabysoc.synth.v
link_design vsdbabysoc
read_sdc ./sdc/vsdbabysoc_synthesis.sdc
check_setup -verbose
report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} > ./sta_output/min_max_$list_of_lib_files($i).txt

}

```
Commands

![image](https://github.com/user-attachments/assets/03ad66d9-3458-4faa-8155-8b2f08556d61)


Constraint File:

![WhatsApp Image 2024-11-04 at 12 17 34](https://github.com/user-attachments/assets/1ba5753d-2be8-41d5-835e-acb7b7ec19d7)



### Table

This table summarizes the Worst Negative Slack (WNS) and Worst Hold Slack (WHS) for various library files.

| Library File                                      | WNS      | WHS      |
|---------------------------------------------------|----------|----------|
| sky130_fd_sc_hd__ff_100C_1v65.lib                 | -3.1656  | -0.6095  |
| sky130_fd_sc_hd__ff_100C_1v95.lib                 | -1.3069  | -0.6696  |
| sky130_fd_sc_hd__ff_n40C_1v56.lib                 | -2.8298  | -0.5478  |
| sky130_fd_sc_hd__ff_n40C_1v65.lib                 | -1.7174  | -0.5885  |
| sky130_fd_sc_hd__ff_n40C_1v76.lib                 | -0.7958  | -0.6248  |
| sky130_fd_sc_hd__ff_n40C_1v95.lib                 | 0.1027   | -0.6686  |
| sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part1  | 0.1027   | -0.6686  |
| sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part2  | 0.1027   | -0.6686  |
| sky130_fd_sc_hd__ff_n40C_1v95_ccsnoise.lib.part3  | 0.1027   | -0.6686  |
| sky130_fd_sc_hd__ss_100C_1v40.lib                  | -25.8686 | 0.1341   |
| sky130_fd_sc_hd__ss_100C_1v60.lib                  | -15.6804 | -0.1459  |
| sky130_fd_sc_hd__ss_n40C_1v28.lib                  | -96.7482 | 1.0337   |
| sky130_fd_sc_hd__ss_n40C_1v35.lib                  | -60.8662 | 0.5698   |
| sky130_fd_sc_hd__ss_n40C_1v40.lib                  | -46.3413 | 0.3456   |
| sky130_fd_sc_hd__ss_n40C_1v44.lib                  | -38.9064 | 0.2077   |
| sky130_fd_sc_hd__ss_n40C_1v60.lib                  | -19.5937 | -0.1248  |
| sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part1   | -19.5937 | -0.1248  |
| sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part2   | -19.5937 | -0.1248  |
| sky130_fd_sc_hd__ss_n40C_1v60_ccsnoise.lib.part3   | -19.5937 | -0.1248  |
| sky130_fd_sc_hd__ss_n40C_1v76.lib                  | -11.1903 | -0.3445  |
| sky130_fd_sc_hd__tt_025C_1v80.lib                   | -4.2009  | -0.5455  |
| sky130_fd_sc_hd__tt_100C_1v80.lib                   | -5.1171  | -0.5477  |


![image](https://github.com/user-attachments/assets/300b4fe4-5709-459a-9e42-f83c11663392)


Graphs:

![image](https://github.com/user-attachments/assets/d8e80e19-45d4-46b3-b3ea-b27ede26f015)


![image](https://github.com/user-attachments/assets/58764ded-8d4b-4404-aa5b-b690eae4103a)



## Task 13 

Day 1

The QFN-48 Package is a small, leadless package featuring 48 connection pads along its edges. It offers superior thermal and electrical performance, making it well-suited for high-density applications.

### Chip

An integrated circuit (IC) built on a silicon base, incorporating various functional components such as memory, processors, and I/O interfaces, tailored for specific electronic applications.

### Pads

Small metallic contact areas on a chip or package that facilitate the connection between the internal circuitry and external components, allowing for the transmission of signals.


### Core

The primary processing unit of a chip, housing the essential logic elements designed for optimal performance and power efficiency.

### Die

The portion of a silicon wafer that contains a single integrated circuit (IC) before it undergoes packaging, containing all the active circuitry of the chip.

### IPs (Intellectual Properties)

Pre-engineered functional components, like USB controllers or memory interfaces, that are licensed for reuse in various designs to reduce development time and costs.

### Software to Hardware Execution Flow & ASIC Design Process

This repository explains the process of running an application on hardware, focusing on the flow from high-level software to hardware execution. Additionally, it provides an overview of ASIC design, including key concepts and tools involved in the design and fabrication of custom integrated circuits (ICs).

###  Software to Hardware Execution Flow

When running an application on hardware, it first passes through the system software layer, which converts it into binary form. The primary components involved are:

### Key Components:

- **OS**: The Operating System breaks down the application functions written in high-level languages (like C or Java) and passes them to the compiler.
- **Compiler**: The compiler converts high-level functions into low-level, hardware-specific instructions.
- **Assembler**: The assembler further converts these instructions into machine-readable binary code for hardware execution.

### Example Flow (Stopwatch App on RISC-V):
- **OS** generates a function in C.
- **Compiler** generates RISC-V-specific instructions.
- **Assembler** converts instructions into binary code, which is then executed by the hardware.

### Process Overview:
1. The compiler and assembler produce architecture-specific instructions and binary code.
2. The binary code is interpreted by the **Register Transfer Level (RTL)** design, written in a Hardware Description Language (HDL).
3. The RTL design is synthesized into a **netlist** of interconnected logic gates.
4. The design undergoes **physical implementation**, preparing it for chip fabrication.

###  Components of ASIC Design

Key components and tools used in designing custom Application-Specific Integrated Circuits (ASICs):

- **RTL IPs**: Verified circuit blocks (like adders, flip-flops) written in HDL, used for accelerating complex designs.
- **EDA Tools**: Software tools for automating tasks like synthesis, optimization, and timing analysis to meet design specifications.
- **PDK Data**: Foundry data files that define the manufacturing process, ensuring the design is ready for fabrication.

###  Simplified RTL to GDSII Flow

Here is a high-level overview of the steps involved in turning RTL design into a finished GDSII file, which is used in chip fabrication:

1. **RTL Design**: Describes the circuit function using HDLs such as Verilog or VHDL.
2. **RTL Synthesis**: Converts RTL code into a gate-level netlist, optimizing cells for the target technology.
3. **Floor and Power Planning**: Layout of the major components, power grid, and input/output systems.
4. **Placement**: Allocates cells in the layout to minimize wire length and reduce signal delays.
5. **Clock Tree Synthesis (CTS)**: Ensures uniform distribution of clock signals across the design.
6. **Routing**: Connects the components while meeting design rules.
7. **Sign-off**: Final verification of the design before sending it for fabrication.
8. **GDSII Generation**: Converts the physical layout into the GDSII file format for chip manufacturing.

###  OpenLane ASIC Flow Overview

 Below is an overview of the key steps:

1. **RTL Synthesis and Technology Mapping**: Uses tools like Yosys and ABC to convert RTL to a gate-level netlist and map it to target technology libraries.
2. **Static Timing Analysis**: OpenSTA performs timing analysis to ensure the design meets speed and timing requirements.
3. **Floor Planning**: Tools like init_fp, ioPlacer, pdn, and tapcell are used for floor planning and power grid design.
4. **Placement**: Managed by tools such as RePLace, Resizer, OpenPhySyn, and OpenDP.
5. **Clock Tree Synthesis**: TritonCTS optimizes the distribution of clock signals.
6. **Fill Insertion**: OpenDP adds filler cells to ensure correct layout density.
7. **Routing**: FastRoute and TritonRoute are used for global and detailed routing, respectively.
8. **SPEF Extraction**: Parasitic data is extracted using OpenRCX for better timing and power analysis.
9. **GDSII Output**: Magic and KLayout generate the final GDSII file, ready for chip fabrication.
10. **Design Rule Checks**: Magic and KLayout also perform checks to ensure the design adheres to manufacturing rules.
11. **Layout vs. Schematic Check**: Netgen is used to verify that the layout matches the schematic.
12. **Antenna Effect Checks**: Magic ensures the design avoids issues related to parasitic capacitance.

### OpenLane Directory Structure

```
├── OpenLane            # Main tool directory
│   ├── designs         # Contains all design projects
│   │   └── picorv32a   # Example project/design
├── pdks                # Files related to Process Design Kits (PDKs)
│   ├── skywater-pdk    # PDKs for the Skywater 130nm process
│   ├── open-pdks       # Scripts for compatibility with open-source tools
│   ├── sky130A         # Open-source compatible version of the Skywater PDK
│   │   ├── libs.ref    # Node-specific files 
│   │   ├── libs.tech   # Tool-specific files 

```

### Openlane synthesis

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
cd designs/picorv32a/runs/09-11_06-33/results/synthesis/
gedit picorv32a.synthesis.v

```

![image](https://github.com/user-attachments/assets/fb9bb8d6-8379-45f3-aa66-be42cf276924)

![image](https://github.com/user-attachments/assets/39abc441-b432-427a-acf2-0b031bc94f04)



Netlist

[Screenshot from 2024-11-13 14-44-27](https://github.com/user-attachments/assets/f6f0d685-9d2b-4387-aac6-8bf54c9cef99)


![Screenshot from 2024-11-13 14-44-33](https://github.com/user-attachments/assets/55c33f70-16dd-4022-aa79-73af05549ffc)


For report

```
cd ../..
cd reports/synthesis
gedit 1-yosys_4.stat.rpt
```

!

![Screenshot from 2024-11-13 14-52-15](https://github.com/user-attachments/assets/48f58ac1-f4ba-4ed4-b827-508bcb823983)


![image](https://github.com/user-attachments/assets/6d13da7e-84e9-4890-9d46-9eb87f22b6ca)

```

28. Printing statistics.

=== picorv32a ===

   Number of wires:              14596
   Number of wire bits:          14978
   Number of public wires:        1565
   Number of public wire bits:    1947
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:              14876
     sky130_fd_sc_hd__a2111o_2       1
     sky130_fd_sc_hd__a211o_2       35
     sky130_fd_sc_hd__a211oi_2      60
     sky130_fd_sc_hd__a21bo_2      149
     sky130_fd_sc_hd__a21boi_2       8
     sky130_fd_sc_hd__a21o_2        57
     sky130_fd_sc_hd__a21oi_2      244
     sky130_fd_sc_hd__a221o_2       86
     sky130_fd_sc_hd__a22o_2      1013
     sky130_fd_sc_hd__a2bb2o_2    1748
     sky130_fd_sc_hd__a2bb2oi_2     81
     sky130_fd_sc_hd__a311o_2        2
     sky130_fd_sc_hd__a31o_2        49
     sky130_fd_sc_hd__a31oi_2        7
     sky130_fd_sc_hd__a32o_2        46
     sky130_fd_sc_hd__a41o_2         1
     sky130_fd_sc_hd__and2_2       157
     sky130_fd_sc_hd__and3_2        58
     sky130_fd_sc_hd__and4_2       345
     sky130_fd_sc_hd__and4b_2        1
     sky130_fd_sc_hd__buf_1       1656
     sky130_fd_sc_hd__buf_2          8
     sky130_fd_sc_hd__conb_1        42
     sky130_fd_sc_hd__dfxtp_2     1613
     sky130_fd_sc_hd__inv_2       1615
     sky130_fd_sc_hd__mux2_1      1224
     sky130_fd_sc_hd__mux2_2         2
     sky130_fd_sc_hd__mux4_1       221
     sky130_fd_sc_hd__nand2_2       78
     sky130_fd_sc_hd__nor2_2       524
     sky130_fd_sc_hd__nor2b_2        1
     sky130_fd_sc_hd__nor3_2        42
     sky130_fd_sc_hd__nor4_2         1
     sky130_fd_sc_hd__o2111a_2       2
     sky130_fd_sc_hd__o211a_2       69
     sky130_fd_sc_hd__o211ai_2       6
     sky130_fd_sc_hd__o21a_2        54
     sky130_fd_sc_hd__o21ai_2      141
     sky130_fd_sc_hd__o21ba_2      209
     sky130_fd_sc_hd__o21bai_2       1
     sky130_fd_sc_hd__o221a_2      204
     sky130_fd_sc_hd__o221ai_2       7
     sky130_fd_sc_hd__o22a_2      1312
     sky130_fd_sc_hd__o22ai_2       59
     sky130_fd_sc_hd__o2bb2a_2     119
     sky130_fd_sc_hd__o2bb2ai_2     92
     sky130_fd_sc_hd__o311a_2        8
     sky130_fd_sc_hd__o31a_2        19
     sky130_fd_sc_hd__o31ai_2        1
     sky130_fd_sc_hd__o32a_2       109
     sky130_fd_sc_hd__o41a_2         2
     sky130_fd_sc_hd__or2_2       1088
     sky130_fd_sc_hd__or2b_2        25
     sky130_fd_sc_hd__or3_2         68
     sky130_fd_sc_hd__or3b_2         5
     sky130_fd_sc_hd__or4_2         93
     sky130_fd_sc_hd__or4b_2         6
     sky130_fd_sc_hd__or4bb_2        2

   Chip area for module '\picorv32a': 147712.918400
```

```

Flop ratio = Number of D Flip flops = 1613  = 0.1084
             ______________________   _____
             Total Number of cells    14876

```

### Day 2

Introduction to library cells , Good floorplan vs Bad floorplan

Utilization Factor and Aspect Ratio

In integrated circuit (IC) floor planning, two important parameters are the utilization factor and aspect ratio. The utilization factor is the ratio of the area occupied by the netlist to the total core area. While a utilization factor of 1 (100%) is considered ideal, most practical designs target a factor between 0.5 and 0.6 to allow for buffer zones, routing space, and room for future adjustments. The aspect ratio, calculated by dividing the height by the width, defines the chip's overall shape. A ratio of 1 results in a square shape, while other values lead to a rectangular layout. The aspect ratio is chosen based on functional, packaging, and manufacturing requirements.

```
Utilization Factor =  Area occupied by netlist
                     __________________________
                         Total area of core
                         

Aspect Ratio =  Height
               ________
                Width
```

Pre-placed Cells

Pre-placed cells are essential functional blocks—such as memory units, custom processors, and analog circuits—that are manually positioned at specific locations. These cells are crucial for maintaining the chip's performance and remain fixed during the placement and routing process to ensure their functionality and preserve layout integrity.

Decoupling Capacitors

Decoupling capacitors are placed near logic circuits to stabilize power supply voltages during transient events. Acting as local energy buffers, they help mitigate voltage fluctuations, crosstalk, and electromagnetic interference (EMI), ensuring consistent power delivery to sensitive circuits.

Power Planning

A robust power planning strategy involves creating a power and ground mesh that evenly distributes the VDD (power) and VSS (ground) across the chip. This design improves power delivery stability, minimizes voltage drops, and enhances the overall efficiency of the chip. Multiple power and ground connections further reduce the risk of instability and support the design’s power requirements.

Pin Placement

Pin placement, or I/O planning, is a critical aspect of chip design. Properly assigned pins minimize signal degradation, enhance heat dissipation, and contribute to thermal stability. Strategically positioning power and ground pins further supports signal strength and contributes to the overall manufacturability and reliability of the chip.

Floorplanning in OPENLANE:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
run_synthesis
run_floorplan


```

![image](https://github.com/user-attachments/assets/03bb5ee5-14ce-4d71-bdf6-523db786567a)


![image](https://github.com/user-attachments/assets/d4589a09-2c10-44eb-bdbc-6b13242ac508)

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_16-38/results/floorplan
gedit picorv32a.floorplan.def

```

![image](https://github.com/user-attachments/assets/b423bab5-b58c-451b-ae00-7b10ed6a361c)

By the floorplan definition

1000 Unit Distance = 1 Micron

Die width in unit distance = 660685−0 = 660685

Die height in unit distance = 671405−0 = 671405

Distance in microns = Value in Unit Distance / 1000

​Die width in microns = 660685 / 1000 = 660.685 Microns

Die height in microns = 671405 / 1000 = 671.405 Microns

Area of die in sq microns = 660.685 × 671.405 = 443587.212425 sq Microns

Floorplan in magic:

magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def &

![image](https://github.com/user-attachments/assets/d3f72d5b-ccff-4638-9f19-3140beef5223)

Decap Cells: These cells are positioned near logic cells to help manage temporary power supply fluctuations and ensure stability in the power delivery network.

Tap Cells: These cells are used to connect to the power grid and help mitigate substrate noise that could negatively impact the chip's performance.

![image](https://github.com/user-attachments/assets/1cfd7b2e-bea5-4ec2-a9cb-15a7a123c04b)

Unplaced standard cells at origin 


![image](https://github.com/user-attachments/assets/b63a416c-9d61-496c-8af9-e9b4aa287851)

Placement
```

run_placement

```


![image](https://github.com/user-attachments/assets/25a074db-a1e3-4463-a164-7ec40724b3c8)


![image](https://github.com/user-attachments/assets/4783cf28-34d4-479f-b8a6-902cc644c4d8)


![image](https://github.com/user-attachments/assets/ed44e76f-2a4d-4ed2-9d41-64031959b067)


Commands


![image](https://github.com/user-attachments/assets/fd712ea6-7300-4bcd-88c4-c272b1fc0c07)


![image](https://github.com/user-attachments/assets/ec260eab-5faf-4df4-b0f3-600e8fc54bd0)



![image](https://github.com/user-attachments/assets/22873dde-4e85-4a45-a785-29b772a1d623)



Cell Design and Characterization Flow

A library is a collection of information about different cells, each with its own size, functionality, and threshold voltages. The cell design flow typically follows a series of steps, from initial input to final output.
Inputs:

PDKs (Process Design Kits): Includes design rules (DRC & LVS), SPICE models, libraries, and user-defined specifications.

Design Steps:

Circuit Design: Create the functional design of the cell.
Layout Design: Draw the layout using techniques like Euler’s path and stick diagrams.
Parasitic Extraction: Extract the parasitic elements (like capacitance and resistance) from the layout.
Characterization: Analyze the cell for timing, noise, and power consumption.

Outputs:

CDL (Circuit Description Language): A file that describes the circuit.
LEF: Layout exchange format, describing the layout of the cell.
GDSII: The file format used for chip manufacturing layouts.
Extracted SPICE Netlist (.cir): A file containing the electrical characteristics of the circuit.
Timing, Noise, and Power .lib files: Library files with the characterization results.


Standard Cell Characterization Flow

The process for characterizing standard cells in the industry usually follows these steps:

Load Models and Technology Files: Start by reading in the models and technology files (like those from the PDK).
Load the Extracted SPICE Netlist: Read the SPICE netlist, which contains the electrical properties of the circuit.
Identify Cell Behavior: Understand how the cell behaves electrically.
Read Subcircuits: Load any subcircuits that are part of the cell.
Attach Power Sources: Add the necessary power sources for the cell.
Apply Stimuli: Set up the test conditions and inputs (stimulus) for the characterization.
Add Output Capacitance Loads: Specify the output loads for the cell during testing.
Set Simulation Commands: Provide the commands needed to run the simulations.

Once these eight steps are prepared, they are bundled together into a configuration file and passed to characterization software called GUNA. This software then generates models for timing, noise, and power. The resulting .lib files are divided into three categories:

Timing Characterization
Power Characterization
Noise Characterization

#### Timing Parameters 

| Timing Definition     | Value       |
|-----------------------|-------------|
| slew_low_rise_thr     | 20%         |
| slew_high_rise_thr    | 80%         |
| slew_low_fall_thr     | 20%         |
| slew_high_fall_thr    | 80%         |
| in_rise_thr           | 50%         |
| in_fall_thr           | 50%         |
| out_rise_thr          | 50%         |
| out_fall_thr          | 50%         |

Propagation delay 

It describes the time required for an input signal change to reach 50% of its final value, which then causes the output signal to similarly reach 50% of its final value in a digital circuit.

Rise Delay = time(out-fall-thr) − time(in-rise-thr) 

Transistion time

Transition time is the duration it takes for a signal to change between different states, typically measured from 10% to 90% or 20% to 80% of the signal’s voltage range.

Fall Transition Time = time(slew-high-fall-thr) − time(slew-low-fall-thr)
Rise Transition Time = time(slew-high-rise-thr) − time(slew-low-rise-thr) 


### Day 3

Design library cell using Magic Layout and ngspice characterization

CMOS Inverter ngspice Simulations

Creating a SPICE Deck for CMOS Inverter Simulation

Netlist Creation: Create a netlist that defines how the components in the CMOS inverter are connected. Each node should be clearly labeled for easy identification during the SPICE simulation. Common node labels include input, output, ground, and supply.

Device Sizing: Set the Width-to-Length (W/L) ratios for the PMOS and NMOS transistors. To ensure balanced drive strength, the PMOS width should generally be 2x to 3x larger than the NMOS width.

Voltage Levels: Define the gate and supply voltages, which are typically set as multiples of the transistor's length for proper operation.

Node Naming: Assign specific names to the nodes around the components (such as VDD, GND, IN, OUT) to help identify and distinguish each element in the SPICE netlist. This ensures that SPICE can accurately simulate the circuit's behavior.


Transistor description syntax

[component name] [drain] [gate] [source] [substrate] [transistor type] W=[width] L=[length]

![image](https://github.com/user-attachments/assets/15237f9f-57c6-4dbc-a50c-d37e2aef1692)


Simulation Commands

For transient analysis

```

source [filename].cir 
run 
setplot 
dc1 
plot out vs in


``` 

![image](https://github.com/user-attachments/assets/e749892f-5b96-4755-a215-744d6eab10aa)



Switching Threshold (Vm): Vm is the input voltage at which the output of the inverter transitions between logic levels. When the PMOS and NMOS sizes are equal, Vm is typically around VDD/2. Changing the sizes of the PMOS or NMOS transistors will shift Vm higher or lower.

Threshold Calculation spice command

```

Vin in 0 2.5 
.op 
.dc Vin 0 2.5 0.05 

```

![image](https://github.com/user-attachments/assets/0288886d-68d4-4421-a748-72c35b0e7525)


Transient analysis to get propagation delay 

We apply a pulse input signal 

```

Vin in 0 0 pulse 0 2.5 0 10p 10p 1n 2n 
*** Simulation Command ***
.op
.tran 10p 4n

```

![image](https://github.com/user-attachments/assets/2cb9a655-596d-4162-bee0-e25f1cbc5e93)

Cloning a custom inverter layout

```


git clone https://github.com/nickson-jose/vsdstdcelldesign
cd Desktop/work/tools/openlane_working_dir/openlane
cd vsdstdcelldesign/
cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech .
ls
magic -T sky130A.tech sky130_inv.mag &

```
![image](https://github.com/user-attachments/assets/28417f72-2f92-4c8c-97d8-419065f2b5e6)

![image](https://github.com/user-attachments/assets/fd69c32e-7198-492b-a57e-1b5f92f2b706)


Fabrication Process  for CMOS  

16 Mask Method

The CMOS fabrication process involves a series of precise steps using photolithography and chemicals to create integrated circuits on a silicon wafer. It consists of several layers, and 16 photomasks are used to define and protect specific areas at each stage. Every step is carefully controlled to ensure the chips are reliable and work correctly.

1. Substrate Preparation: Begin with a clean, polished silicon wafer as the base.
2. N-Well Formation: Create N-well regions in the p-type substrate by implanting phosphorus.
3. P-Well Formation: Create P-well regions in n-type areas by implanting boron.
4. Gate Oxide Deposition: Apply a thin silicon dioxide layer to insulate the gate from the channel.
5. Polysilicon Deposition: Add a polysilicon layer that will form the gate electrode.
6. Polysilicon Masking and Etching: Use masks and etching to shape the polysilicon into gate structures.
7. N-Well Masking and Implantation: Mask and implant phosphorus to define the N-well areas.
8. P-Well Masking and Implantation: Mask and implant boron to define the P-well areas.
9. Source/Drain Implantation: Create source and drain regions by implanting dopants (e.g., arsenic for NMOS and boron for PMOS).
10. Gate Formation: Use a mask to define and etch the gate structure.
11. Source/Drain Masking and Etching: Etch to expose the source and drain regions for further processing.
12. Contact/Via Formation: Etch holes to access the source, drain, and gate regions for connections.
13. Metal Deposition: Deposit metal (like aluminum or copper) to create the interconnections.
14. Metal Masking and Etching: Mask and etch the metal to form the interconnection patterns.
15. Passivation Layer Deposition: Apply a protective layer of silicon dioxide or nitride over the wafer.
16. Final Testing and Packaging: Test the wafer, separate the working chips, and package them for use.


![image](https://github.com/user-attachments/assets/062a9741-f975-4277-9e04-7d2c6eb7d9c0)

Identifying components

PMOS transistor

![image](https://github.com/user-attachments/assets/74da71fa-ad62-45b0-92ff-6c92a74e650e)

NMOS transistor

![image](https://github.com/user-attachments/assets/0112f736-f6ef-4bd9-9bb8-05eb933df4b5)

Output Y

![image](https://github.com/user-attachments/assets/a50d4906-66c1-4de0-bcde-7efd876cb371)

Extraction of inverter from magic into spice

```
pwd
extract all
ext2spice cthresh 0 rthresh 0
ext2spice

```


![image](https://github.com/user-attachments/assets/b6afda96-a7e0-4bfd-a844-220d0e1b633f)

Spice file

![image](https://github.com/user-attachments/assets/a4b49fbe-1b9e-4118-acd9-47220970e8ab)




Finding transient response 

Modified spice file

```
* SPICE3 file created from sky130_inv.ext - technology: sky130A

.option scale=0.01u
.include ./libs/pshort.lib
.include ./libs/nshort.lib

//.subckt sky130_inv A Y VPWR VGND
M1000 Y A VGND VGND nshort_model.0 w=35 l=23
+  ad=1.44n pd=0.152m as=1.37n ps=0.148m
M1001 Y A VPWR VPWR pshort_model.0 w=37 l=23
+  ad=1.44n pd=0.152m as=1.52n ps=0.156m

VDD VPWR 0 3.3V
VSS VGND 0 0V
Va A VGND PULSE(0V 3.3V 0 0.1ns 0.1ns 2ns 4ns)

C0 A VPWR 0.0774f
C1 VPWR Y 0.117f
C2 A Y 0.0754f
C3 Y VGND 2f
C4 A VGND 0.45f
C5 VPWR VGND 0.781f
//.ends

.tran 1n 20n
.control
run
.endc
.end

```






