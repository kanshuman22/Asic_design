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




