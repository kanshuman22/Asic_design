# ASIC DESIGN  

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













