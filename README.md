# ASIC DESIGN  

  

<details>
<summary>## TASK 1</summary>

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

</details>

<details>
	
<summary> ## TASK 2 </summary>

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

</details>

<details>
<summary>
## TASK 3

</summary>

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

</details>

<details>

 <summary>
## TASK 4

</summary>

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

</details>

<details>

<summary> Task 6- 12</summary>
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


</details>


<details>


<summary>
## Task 13

 </summary>

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

Simulating spice netlist

```
ngspice sky130_inv.spice

```

![image](https://github.com/user-attachments/assets/3685a4f9-1f9f-495c-a0e0-b1f28ec2a5a0)




Waveform


![image](https://github.com/user-attachments/assets/2f8d6498-6c3a-446d-bdf5-f71ee4382c9d)


Characterization of  Slew Rate and Propagation Delay



Fall Transition: Time for output to fall from 80% to 20%.
Rise Transition: Time for output to rise from 20% to 80% of max value.
Cell Rise/Fall Delay: Difference in time for 50% output change compared to input transition.


Rise Transition : 2.24638 - 2.18242 =  0.06396 ns = 63.96 ps
Fall Transition : 4.0955 - 4.05536 =  0.0419 ns = 41.9 ps
Cell Rise Delay : 2.21144 - 2.15008 = 0.06136 ns = 61.36 ps
Cell Fall Delay : 4.07807 - 4.05 =0.02 ns = 20 ps


Magic tool and DRC check

```

cd 
wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz 
tar xfz drc_tests.tgz 
cd drc_tests 
gvim .magicrc 
magic -d XR &
 
```

![image](https://github.com/user-attachments/assets/d96f2d14-1376-469f-ad67-226db0b042d3)

![image](https://github.com/user-attachments/assets/df4ac61f-616b-4bdd-913a-5118982ef0ea)



load poly.mag

![image](https://github.com/user-attachments/assets/30a3922d-fa07-4bd4-9565-ca03a25b3a3c)

![image](https://github.com/user-attachments/assets/0c4207a0-0519-42c4-9780-d1b05bb48cbe)

DRC error

![image](https://github.com/user-attachments/assets/1e5bcea8-609f-4eaf-8092-cba9a613226b)

Fixing the error

![image](https://github.com/user-attachments/assets/8ce21fbe-5bd3-4249-aebf-08eaa65aba6c)

We add these commands to the file

![Screenshot from 2024-11-14 09-15-43](https://github.com/user-attachments/assets/5eef280d-ffed-4ead-862e-a513410e5bab)


![image](https://github.com/user-attachments/assets/5235be89-8da8-4e6b-87d8-bf3e4390b64a)

Commands

```
tech load sky130A.tech 
drc check 
drc why 

```


![image](https://github.com/user-attachments/assets/df10e2c9-6373-4656-945a-0daba5d0b5da)


## Day 4

Pre-layout timing analysis and importance of good clock tree

Extracting tracks.info file:

```
cd Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign
cd ../../pdks/sky130A/libs.tech/openlane/sky130_fd_sc_hd/
less tracks.info
```

![image](https://github.com/user-attachments/assets/04335003-ef69-4ad5-ab5b-1fadb1ce99ee)

Setting Grid in tkcon for Local-Interconnect Layer

In tkcon window:

```

grid 0.46um 0.34um 0.23um 0.17um

```

![image](https://github.com/user-attachments/assets/e67d7ca7-d0b4-44c3-86bd-70d571f1c395)

save with a custom name 

![image](https://github.com/user-attachments/assets/926b21f5-295c-4fb6-92b7-3ee3c022e8e9)

opening the file

```
magic -T sky130A.tech sky130_anshinv.mag &

```

![image](https://github.com/user-attachments/assets/f1908b1d-a0d6-4277-a7b8-ae077ff23074)


```
lef write

```

![image](https://github.com/user-attachments/assets/d894e551-eb37-430f-b001-7ac55c6b6c82)

![image](https://github.com/user-attachments/assets/42a31e00-777f-4a31-a72f-3d124efee7e3)

We then modify the config.tcl file

```
# Design
set ::env(DESIGN_NAME) "picorv32a"

set ::env(VERILOG_FILES) "./designs/picorv32a/src/picorv32a.v"
set ::env(SDC_FILE) "./designs/picorv32a/src/picorv32a.sdc"

set ::env(CLOCK_PERIOD) "5.000"
set ::env(CLOCK_PORT) "clk"

set ::env(CLOCK_NET) $::env(CLOCK_PORT)

set ::env(LIB_SYNTH) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"
set ::env(LIB_FASTEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib"
set ::env(LIB_SLOWEST) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib"
set ::env(LIB_TYPICAL) "$::env(OPENLANE_ROOT)/designs/picorv32a/src/sky130_fd_sc_hd__typical.lib"

set ::env(EXTRA_LEFS) [glob $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/src/*.lef]

set filename $::env(OPENLANE_ROOT)/designs/$::env(DESIGN_NAME)/$::env(PDK)_$::env(STD_CELL_LIBRARY)_config.tcl
if { [file exists $filename] == 1 } {
  source $filename
}

```

Openlane flow synthesis:

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
run_synthesis

```


![Screenshot from 2024-11-14 11-51-25](https://github.com/user-attachments/assets/a812ecee-945e-40ad-a606-089a58b594cf)


![image](https://github.com/user-attachments/assets/62272f4e-e8d3-435b-972c-f9f3e51c39a0)

![image](https://github.com/user-attachments/assets/3046964e-798d-46f4-a23b-efbc149a9c87)

![image](https://github.com/user-attachments/assets/f8268942-4991-4139-966c-6a9cb3303b52)

![image](https://github.com/user-attachments/assets/fbbf42db-1804-4ff7-b02b-d818038e6af7)


![image](https://github.com/user-attachments/assets/0ac700f4-8ecd-4e10-bce0-8ffb33df41f1)

![image](https://github.com/user-attachments/assets/eb1169b3-e683-4ff7-adfe-5b73fff72fdc)

![image](https://github.com/user-attachments/assets/0024064c-5671-40b8-9d3d-3fdc1bf13388)

Delay tables

Delay is an important factor in cell timing, influenced by input transition and output load. Even cells of the same type can experience different delays due to variations in wire length, resistance, and capacitance. To account for this, "delay tables" are used—2D arrays that map input slew and load capacitance for each buffer size, providing timing models. Algorithms use these tables to compute the delays of buffers, interpolating between data points when exact values are not available, ensuring accurate delay estimation and maintaining signal integrity across different load conditions.


![image](https://github.com/user-attachments/assets/80e184c5-bbde-4dae-9356-8cf71f1e9cdc)



Fixing Slack

```
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a -tag 24-03_10-03 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
echo $::env(SYNTH_STRATEGY)
set ::env(SYNTH_STRATEGY) "DELAY 3"
echo $::env(SYNTH_BUFFERING
echo $::env(SYNTH_SIZING)
set ::env(SYNTH_SIZING) 1
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis

```

![image](https://github.com/user-attachments/assets/733ed2ac-9f1d-4a43-8195-ea2379df34b3)



![image](https://github.com/user-attachments/assets/e710df8b-5a56-444a-9f57-2d600107afe1)



run_floorplan

![image](https://github.com/user-attachments/assets/2b53776f-5595-4e9c-a4f1-e4950f11a90e)

To avoid the unexplainable error ,we can instead use the another set of commands which are available based on information from Desktop/work/tools/openlane_working_dir/openlane/scripts/tcl_commands/floorplan.tcl and also based on Floorplan Commands section in Desktop/work/tools/openlane_working_dir/openlane/docs/source/OpenLANE_commands.md

```
init_floorplan
place_io
tap_decap_or

```


run _placement

![image](https://github.com/user-attachments/assets/af25a05f-8b17-4717-bd54-a186bbe9e16b)


![image](https://github.com/user-attachments/assets/f20c7634-be41-4d27-bf1e-679908c10d4b)

Run the below commands in a new terminal  to load placement def in magic

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/placement/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def &


```

![Screenshot from 2024-11-14 19-05-34](https://github.com/user-attachments/assets/6fac4084-57e6-45a7-8998-97435aa53e9c)

![image](https://github.com/user-attachments/assets/43926e77-b75b-4913-9d2c-78d744f5ba0c)

![image](https://github.com/user-attachments/assets/7a6835ab-184f-4532-be98-76cffe2f7f20)


Custom inverter

![image](https://github.com/user-attachments/assets/5b66ce8f-ccaf-484c-a21c-49dbc97260bc)


![image](https://github.com/user-attachments/assets/b03c5095-4495-4f00-8da7-55a5c0ad85a0)


![image](https://github.com/user-attachments/assets/4230163f-daab-4160-b1d6-a1db9f68b314)


Timing analysis with ideal clocks using openSTA


Pre-layout Static Timing Analysis (STA) will account for the impact of clock buffers and net delays caused by RC parasitics. The wire delay will be calculated based on the wire model provided in the Process Design Kit (PDK) library.

![image](https://github.com/user-attachments/assets/f56f9393-884c-4cff-8ec2-291ab76eb8da)


Since the improved timing run resulted in 0 WNS, we will perform the timing analysis on the initial synthesis run, which has numerous violations and lacks any parameters for timing improvement.

Invoking openlane flow

Commands

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis

```

![image](https://github.com/user-attachments/assets/7b241043-1f64-41fe-b6be-07468727fd41)


Create a file pre_sta.conf

Contents

```
set_cmd_units -time ns -capacitance pF -current mA -voltage V -resistance kOhm -distance um
read_liberty -max /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__slow.lib
read_liberty -min /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/sky130_fd_sc_hd__fast.lib
read_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/24-03_10-03/results/synthesis/picorv32a.synthesis.v
link_design picorv32a
read_sdc /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/src/my_base.sdc
report_checks -path_delay min_max -fields {slew trans net cap input_pin}
report_tns
report_wns

```

my_base.sdc:

```
set ::env(CLOCK_PORT) clk
set ::env(CLOCK_PERIOD) 12.000
set ::env(SYNTH_DRIVING_CELL) sky130_fd_sc_hd__inv_8
set ::env(SYNTH_DRIVING_CELL_PIN) Y
set ::env(SYNTH_CAP_LOAD) 17.65
create_clock [get_ports $::env(CLOCK_PORT)]  -name $::env(CLOCK_PORT)  -period $::env(CLOCK_PERIOD)
set IO_PCT  0.2
set input_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
set output_delay_value [expr $::env(CLOCK_PERIOD) * $IO_PCT]
puts "\[INFO\]: Setting output delay to: $output_delay_value"
puts "\[INFO\]: Setting input delay to: $input_delay_value"


set clk_indx [lsearch [all_inputs] [get_port $::env(CLOCK_PORT)]]
#set rst_indx [lsearch [all_inputs] [get_port resetn]]
set all_inputs_wo_clk [lreplace [all_inputs] $clk_indx $clk_indx]
#set all_inputs_wo_clk_rst [lreplace $all_inputs_wo_clk $rst_indx $rst_indx]
set all_inputs_wo_clk_rst $all_inputs_wo_clk


# correct reset
set_input_delay $input_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] $all_inputs_wo_clk_rst
#set_input_delay 0.0 -clock [get_clocks $::env(CLOCK_PORT)] {resetn}
set_output_delay $output_delay_value  -clock [get_clocks $::env(CLOCK_PORT)] [all_outputs]

# TODO set this as parameter
set_driving_cell -lib_cell $::env(SYNTH_DRIVING_CELL) -pin $::env(SYNTH_DRIVING_CELL_PIN) [all_inputs]
set cap_load [expr $::env(SYNTH_CAP_LOAD) / 1000.0]
puts "\[INFO\]: Setting load to: $cap_load"
set_load  $cap_load [all_outputs]


```

Running STA

```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf

```
![image](https://github.com/user-attachments/assets/8d337654-d13e-434d-b3d8-a16c7ab2eb07)


![image](https://github.com/user-attachments/assets/97351ba7-2c2c-44af-9a74-0eb91d90f0ee)

![image](https://github.com/user-attachments/assets/0de40c23-88e8-4f5a-b93a-ee323bc7f1da)


Optimizing synthesis

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 14-11_20-06 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
set ::env(SYNTH_MAX_FANOUT) 4
echo $::env(SYNTH_DRIVING_CELL)
run_synthesis

```

![image](https://github.com/user-attachments/assets/b994822a-423b-4251-a9f3-bb73d369b9a9)

Running STA

Commands

```
cd Desktop/work/tools/openlane_working_dir/openlane
sta pre_sta.conf
```
![image](https://github.com/user-attachments/assets/2f1b980c-3b16-4067-897e-aaca68009cfb)


![image](https://github.com/user-attachments/assets/5f0d23fc-5c1b-4ab8-b20e-5b109aab88f5)

Basic timing ECO 


NOR gate of drive strength 2 is driving 5 fanouts

![image](https://github.com/user-attachments/assets/7201473b-5485-41fb-83ae-4cc4f829e0f6)

Optimizing commands :

```
report_net -connections _13111_
replace_cell _16171_ sky130_fd_sc_hd__nor3_2
report_checks -fields {net cap slew input_pins} -digits 4

```

![image](https://github.com/user-attachments/assets/566653ff-7343-4411-9a18-17ff865f3926)



Clock Tree Synthesis (CTS) methods differ depending on design requirements:

Balanced Tree CTS: Utilizes a balanced binary tree to ensure equal path lengths, reducing clock skew but offering moderate power efficiency.

H-tree CTS: Adopts an "H"-shaped configuration, ideal for large designs and optimized for power efficiency.


![image](https://github.com/user-attachments/assets/96f7858b-efc9-4c5b-a3e7-4ffe5a0c24a9)


Star CTS: Distributes the clock from a central point, reducing skew, but demands additional buffers near the source.

Global-Local CTS: Merges star and tree structures, with a global tree for clock distribution across domains and local trees within those domains, providing a balance between global and local timing.

Mesh CTS: Implements a grid pattern, well-suited for structured designs, balancing ease of implementation with clock skew reduction.

Adaptive CTS: Adjusts dynamically based on timing and congestion conditions, offering greater flexibility, though at the cost of increased complexity.


Crosstalk


Crosstalk refers to interference caused by overlapping electromagnetic fields between neighboring circuits, leading to unwanted signal interactions. In VLSI, it can result in data corruption, timing problems, and increased power consumption. Strategies to mitigate crosstalk include optimized layout and routing, shielding techniques, and clock gating to reduce dynamic power and minimize interference.


![image](https://github.com/user-attachments/assets/be234746-9a94-4c38-9746-a79e4967c6b5)


Clock net shielding 

Clock Net Shielding involves protecting the clock network from glitches by isolating it with shields connected to VDD or GND, which remain static. This technique minimizes interference by separating the clock signals from other routing signals, often using dedicated layers and clock buffers. Furthermore, isolating clock domains helps prevent interference between domains, avoiding metastability and ensuring proper synchronization.


![image](https://github.com/user-attachments/assets/fb00ce48-a27f-4be6-928a-be009dd5131f)


Now to insert this updated netlist to PnR flow and we can use write_verilog and overwrite the synthesis netlist but before that we are going to make a copy of the old old netlist:

Commands:

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_18-01/results/synthesis/
ls
cp picorv32a.synthesis.v picorv32a.synthesis_old.v
ls

```
Commands for verilog 

```
write_verilog /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/13-11_18-01/results/synthesis/picorv32a.synthesis.v
exit

```
Overwritten netlist


![image](https://github.com/user-attachments/assets/19df7613-099f-4258-87de-67431e9e6cb3)


CTS and Post CTS timing analysis

```
cd Desktop/work/tools/openlane_working_dir/openlane
docker
./flow.tcl -interactive
prep -design picorv32a -tag 14-11_20-06 -overwrite
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_STRATEGY) "DELAY 3"
set ::env(SYNTH_SIZING) 1
run_synthesis
init_floorplan
place_io
tap_decap_or
run_placement
run_cts

```


![image](https://github.com/user-attachments/assets/782c6ab9-dfcd-4d49-9ae4-ce8ba8f246bb)

![image](https://github.com/user-attachments/assets/8a34cdee-f8a3-49d7-8800-fd5453afba40)

![image](https://github.com/user-attachments/assets/b6c23518-7307-4db4-84e8-98a0177be331)


![image](https://github.com/user-attachments/assets/c60f3c1b-9e2d-4061-8a01-1635f6d0a895)


![image](https://github.com/user-attachments/assets/64b559cd-879a-43f7-9a8f-aa2cd5b2e7f2)


![image](https://github.com/user-attachments/assets/d5edcb15-bd73-4dab-9af8-e85989faa7a7)

cts run 

![image](https://github.com/user-attachments/assets/bd07b86b-d073-4edb-8f4b-a81035c36c66)

![image](https://github.com/user-attachments/assets/af34aadb-6a99-4956-b446-ffc10e28a9dd)



Setup timing analysis using real clocks


Setup Timing Analysis with Real Clocks takes into account practical factors such as clock skew and clock jitter. Clock skew refers to the variation in arrival times of the clock signal at different parts of the circuit, caused by physical delays, which impacts the setup and hold timing margins. Clock jitter is the fluctuation in the clock period due to power, temperature, and noise variations, introducing uncertainty in the timing of clock edges. Both of these factors are essential for precise timing analysis, ensuring the design performs reliably under real-world conditions.


Commands for Post-CTS OpenROAD timing analysis:


```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/14-11_20-06/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/cts/picorv32a.cts.def
write_db pico_cts.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit

```

![image](https://github.com/user-attachments/assets/a844be17-9126-4246-a162-ae170a7ac0fa)

![image](https://github.com/user-attachments/assets/22992dee-ed50-40a1-baa7-d7b7c279c27e)


![image](https://github.com/user-attachments/assets/c0050599-fcd6-4bb5-98db-636d9e9f7e9e)



Commands for exploring post-CTS OpenROAD timing analysis by removing 'sky130_fd_sc_hd__clkbuf_1' cell from clock buffer list variable 'CTS_CLK_BUFFER_LIST'

```
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [lreplace $::env(CTS_CLK_BUFFER_LIST) 0 0]
echo $::env(CTS_CLK_BUFFER_LIST)
echo $::env(CURRENT_DEF)
set ::env(CURRENT_DEF) /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/placement/picorv32a.placement.def
run_cts
echo $::env(CTS_CLK_BUFFER_LIST)
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/14-11_20-06/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/cts/picorv32a.cts.def
write_db pico_cts1.db
read_db pico_cts.db
read_verilog /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/synthesis/picorv32a.synthesis_cts.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
report_checks -path_delay min_max -fields {slew transd net cap input_pins} -format full_clock_expanded -digits 4
report_clock_skew -hold
report_clock_skew -setup
exit
echo $::env(CTS_CLK_BUFFER_LIST)
set ::env(CTS_CLK_BUFFER_LIST) [linsert $::env(CTS_CLK_BUFFER_LIST) 0 sky130_fd_sc_hd__clkbuf_1]
echo $::env(CTS_CLK_BUFFER_LIST)

```


![image](https://github.com/user-attachments/assets/3a41b26e-036c-4e34-ba73-d21dfe8b5416)

![image](https://github.com/user-attachments/assets/8c1c0d08-9c2f-42e3-a48d-08c4690b5d5b)

![image](https://github.com/user-attachments/assets/65922f1c-f82e-4699-bdc6-75d6b41c9003)


![image](https://github.com/user-attachments/assets/627d93e4-9c55-45b7-ad0a-272f5f89b6b6)


![image](https://github.com/user-attachments/assets/3ffc11a4-42a4-4691-a93f-9765b6e31ce4)


### Day 5
Maze Routing and Lee's algorithm

Maze Routing involves establishing physical connections between pins, with algorithms like Lee's algorithm used to find optimal paths on a routing grid. The Lee algorithm begins at the source pin and assigns incremental labels to neighboring cells until it reaches the target pin, favoring L-shaped paths and using zigzag routes when necessary. Although it is effective for finding the shortest path between two pins, the Lee algorithm can be slow for large designs, leading to the adoption of faster alternatives for more complex routing challenges.

![image](https://github.com/user-attachments/assets/07d6ba5f-d2ef-49f0-a9f9-82d5582e15c7)

![image](https://github.com/user-attachments/assets/bfc0ac18-96d8-42df-9b7e-9a69145dff6b)

Generating the Power Distribution Network (PDN)

Command:

```
gen_pdn
```

In another terminal 

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-11_20-06/tmp/floorplan/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read 17-pdn.def &

```
Screenshots

![image](https://github.com/user-attachments/assets/0bb386cd-f02b-4be6-90da-f951616445cb)

![image](https://github.com/user-attachments/assets/e4d4daa0-8f59-4667-a7ac-9e57fa22067d)

![image](https://github.com/user-attachments/assets/440c100c-f0d5-45a9-979b-bea15b27adaf)


When the power distribution network (PDN) generation command is executed, the system constructs the PDN using the design_cts.def file as input.

The PDN includes power rings, straps, and rails:

    Power is first supplied from the VDD and VSS pads to the power rings.
    Horizontal and vertical straps are then connected to the rings to further distribute power, with these straps directing power to the rails that connect to the standard cells.
    Rails are positioned at intervals corresponding to the height of the standard cells, aligning with multiples of the track pitch (2.72 in this design) to ensure proper power delivery to all cells.

In this design:

    Straps are placed on metal layers 4 and 5, while the rails for the standard cells are on metal layer 1.
    Vias are used to link these metal layers, ensuring continuous power flow from the pads to the cells across different metal levels.

![image](https://github.com/user-attachments/assets/682a8ca8-559b-4027-aec7-816b360150f1)


Detailed routing using TritonRoute:

```
echo $::env(CURRENT_DEF)
echo $::env(ROUTING_STRATEGY)
run_routing

```


![image](https://github.com/user-attachments/assets/7e7e50b9-6ed5-4abc-9890-a0a14c5f163a)


![image](https://github.com/user-attachments/assets/a5edeefc-906a-403b-9e99-df0f9957ea94)


In another terminal

```
cd Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-11_20-06/results/routing/
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.def &

```

![image](https://github.com/user-attachments/assets/1da2563e-1180-4d71-8ecd-f69b95224f37)

![image](https://github.com/user-attachments/assets/28fb7ca8-696d-4469-8e7a-9fd6ee45813f)


![image](https://github.com/user-attachments/assets/2ac5f325-2342-46b8-9cac-55b0b61e8a56)

Fast route guide file:

![image](https://github.com/user-attachments/assets/7f98bb7c-3f46-4bab-aab6-458c24d52f45)



TritonRoute

Intra-layer Routing: Routing within a single metal layer is performed in parallel.
Inter-layer Routing: This routing occurs sequentially across multiple layers, ensuring alignment with the directions specified in the LEF (e.g., MET1 is horizontal, MET2 is vertical).
Route Guides: Global route guides provide an initial framework for routing, helping TritonRoute with detailed routing by minimizing conflicts and improving connectivity.
Inter-guide Connectivity: TritonRoute ensures continuous signal flow across adjacent route guides, minimizing gaps to improve overall design continuity.

![image](https://github.com/user-attachments/assets/9a1a1660-a6c0-4f8b-ac13-d59739346fba)

![image](https://github.com/user-attachments/assets/156b56f3-6d9a-4476-8172-483b5e821fe9)

![image](https://github.com/user-attachments/assets/7bb680fd-afc6-45a6-ae53-bf61deaa4161)

![image](https://github.com/user-attachments/assets/a91606b6-6cf6-4f70-9753-ae78b1746a74)


A routing topology algorithm determines how the connections between pins in an integrated circuit are structured and arranged. 
Its goal is to create an efficient layout that minimizes cost by finding the best paths and shapes for the connections.

![image](https://github.com/user-attachments/assets/7a56acac-71a8-4721-9eb7-b54a6d33a21e)



SPEF extraction Post-Route parasitic extraction using SPEF extractor


```
cd Desktop/work/tools/openlane_working_dir/openlane/scripts/spef_extractor
python3 main.py -l /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-11_20-06/tmp/merged.lef -d /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-11_20-06/results/routing/picorv32a.def

```

![image](https://github.com/user-attachments/assets/3fd743a1-5d96-41c4-83ac-0781b6ee838c)

Post-Route OpenSTA timing analysis with the extracted parasitics of the route

```
openroad
read_lef /openLANE_flow/designs/picorv32a/runs/14-11_20-06/tmp/merged.lef
read_def /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/routing/picorv32a.def
write_db pico_route.db
read_db pico_route.db
read_verilog /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/synthesis/picorv32a.synthesis_preroute.v
read_liberty $::env(LIB_SYNTH_COMPLETE)
link_design picorv32a
read_sdc /openLANE_flow/designs/picorv32a/src/my_base.sdc
set_propagated_clock [all_clocks]
read_spef /openLANE_flow/designs/picorv32a/runs/14-11_20-06/results/routing/picorv32a.spef
report_checks -path_delay min_max -fields {slew trans net cap input_pins} -format full_clock_expanded -digits 4
exit

```


![image](https://github.com/user-attachments/assets/b8f09f5e-83e8-438f-95d1-d27fc16da75e)


![image](https://github.com/user-attachments/assets/258005f9-1632-4cd7-8ec9-6df3c8468dde)



![image](https://github.com/user-attachments/assets/76e96371-d3d1-48e4-b3c3-dcbe1429ee86)


![image](https://github.com/user-attachments/assets/a9c25938-be47-43c2-9234-f5215a7070b4)


</details>



<details>

<summary>  
## Task 14
</summary>


# Evolution of Computing and Microprocessor Trends

## Introduction

### Bombe
The Bombe was an electro-mechanical machine used during World War II to break German Enigma-encrypted messages. It was designed by Alan Turing and Gordon Welchman at Bletchley Park in the UK. The Bombe tested various rotor settings of the Enigma machine, using known plaintext patterns, to significantly speed up the decryption process, playing a crucial role in the Allied war effort.

### ENIAC (Electronic Numerical Integrator and Computer)
ENIAC, developed during World War II by John Presper Eckert and John Mauchly at the University of Pennsylvania, was the first general-purpose electronic computer. Completed in 1945, it was designed to calculate artillery firing tables for the U.S. Army. ENIAC used vacuum tubes and was much faster than previous mechanical devices. However, it lacked the ability to store programs, meaning it had to be manually reconfigured for each new task.

### EDVAC (Electronic Discrete Variable Automatic Computer)
EDVAC, developed by Eckert and Mauchly with contributions from John von Neumann, was one of the first computers to use the stored-program concept. Completed in 1949, EDVAC used binary numbers instead of decimal and stored both data and instructions in memory. This innovation simplified programming and laid the groundwork for the modern von Neumann architecture.

## 50 Years of Microprocessor Trend Data

### Key Metrics

- **Transistors:** The number of transistors on microprocessor chips has increased exponentially, following Moore's Law, which predicts a doubling of transistors every two years. This has enabled processors to become more powerful, with billions of transistors on chips by the 2020s.
- **Single-Thread Performance:** Measured using SpecINT, this metric shows the computational power of a single processor core. Performance has steadily improved, but growth slowed after 2005 due to power and heat limitations.
- **Frequency (Clock Speed):** Clock speed (in MHz) increased steadily until the early 2000s, after which it stagnated due to heat dissipation challenges.
- **Power Consumption:** Power consumption has increased as transistor density and clock speed grew, becoming a major concern in the mid-2000s.
- **Logical Cores:** The number of processor cores increased starting in the mid-2000s, as the focus shifted to multi-core processors to enhance performance in parallel computing.

### Key Milestones

- **iPhone Release (2007):** The iPhone marked the rise of mobile computing, where power efficiency became as important as raw performance. This shift led to innovations in low-power processor designs.
- **Datacenter-Scale Computing (Post-2010):** Cloud computing and large-scale data centers emerged, with energy efficiency and parallelism becoming central design priorities.

## Path to Zetta-Scale Computing

The path to zetta-scale computing is projected to reach **10²¹ FLOPS** by around 2035, with systems performing a quintillion (10¹⁸) floating-point operations per second in the exascale era.

### Key Performance Levels

- **Gigascale (1984, 10⁹ FLOPS):** Early supercomputers capable of billions of calculations per second.
- **Terascale (1997, 10¹² FLOPS):** Reached by systems like Jaguar, achieving trillions of calculations per second.
- **Petascale (2008, 10¹⁵ FLOPS):** Achieved by systems like Titan, performing quadrillions of calculations.
- **Exascale (2021, 10¹⁸ FLOPS):** Systems like Frontier, capable of quintillions of calculations.
- **Zettascale (Projected for 2035, 10²¹ FLOPS):** The next major milestone, where systems will handle septillions of operations per second.

## CMOS Evolution and Next-Gen Candidates

The future of semiconductor technology is focused on overcoming the scaling limits of CMOS technology. Key developments include:

### Channel Material
- **Current:** Silicon is the standard material used in CMOS transistors, with **strained SiGe** used for high-performance applications.
- **Future:** **Germanium (Ge)** and **2D materials** like **MoS₂** are being explored for better electrical characteristics at smaller scales.

### Patterning
- **Current:** **Deep Ultraviolet (DUV)** lithography is used for defining transistor features.
- **Future:** **Extreme Ultraviolet (EUV)** lithography will be essential for sub-7nm nodes.

### Gate Stack Materials
- **Current:** **High-K metal gates (HKMG)** are used to reduce gate leakage and improve switching performance.
- **Future:** **Negative Capacitance FETs (NC-FETs)** and **Tunnel FETs (TFETs)** are promising for reducing power consumption.

### Interconnection Materials
- **Current:** **Copper (Cu)** is used for interconnects due to its low resistivity.
- **Future:** Materials like **ruthenium (Ru)** and **topological semi-metals** are being considered for better performance at small nodes.

### Device Structures
- **Current:** **FinFET** and **planar** transistors are widely used.
- **Future:** **3D Stacked FETs (3DS-FETs)** and **Vertical FETs (VFETs)** are emerging to improve density and power efficiency.

### Design Co-Optimization
- **DTCO (Design-Technology Co-Optimization):** Integrating new design techniques with advanced process technologies.
- **STCO (System-Technology Co-Optimization):** Optimizing both system architecture and underlying technology, such as **chiplets**.

## Transistor Evolution

The evolution of transistor technology from planar to FinFET and Gate-All-Around (GAA) transistor structures:

1. **Planar Transistor:** Early flat designs with limited performance.
2. **FinFET (2011):** A vertical structure that wraps the gate around the channel, improving performance.
3. **Gate-All-Around (GAA) Transistor (2025?):** Fully surrounds the channel, offering better control and performance at smaller scales.

## Why FinFETs and GAA Transistors?

- **Planar Transistors:** Limited control over the channel, causing leakage and inefficiency.
- **FinFET Transistors:** Gate wraps around the channel, improving control and reducing leakage.
- **GAA Transistors:** The gate fully surrounds the channel, providing superior performance and efficiency as transistor sizes shrink.


# FEOL Innovations and Semiconductor Technology Evolution

## FEOL Overview
FEOL (Front-End-Of-Line) refers to the first stages of semiconductor manufacturing, where active components like transistors are built on the silicon wafer. This stage involves creating devices such as transistors, capacitors, and isolation structures before adding metal interconnections. FEOL innovations are key to improving transistor performance and helping continue the progress of Moore's Law, which states that the number of transistors on a chip doubles approximately every two years, making devices smaller, faster, and more efficient.

## CMOS Technology Inflection Points

1. **Dennard Scaling**
   - This principle says that as transistors shrink, power density stays constant.
   - It enabled voltage scaling when gate lengths became smaller, as seen in the early years.

2. **Key Technology Nodes and Innovations**
   - **~1 µm**: Start of CMOS miniaturization.
   - **180 nm**: First reduction in drive voltage.
   - **130 nm**: Copper interconnects improve conductivity.
   - **90 nm**: Strained silicon improves electron mobility in NMOS transistors.
   - **65 nm**: Embedded SiGe improves PMOS transistor performance.
   - **45 nm**: High-k dielectrics and metal gates reduce leakage.
   - **32 nm**: Advanced gate designs with raised source/drain regions improve performance.

3. **Key Technology Nodes**
   - **22 nm**: FinFET (Tri-Gate) transistors introduced to reduce leakage and improve control.
   - **14 nm**: Unidirectional metal routing increases density.
   - **10 nm**: Advanced patterning techniques are used for finer geometries.
   - **7 nm**: EUV (Extreme Ultraviolet Lithography) simplifies the patterning process.
   - **5 nm**: SiGe channels improve PMOS performance.
   - **3 nm / 2 nm / 1.4 nm**: Gate-All-Around (GAA) transistors are introduced for better control.
   - **Sub-1 nm**: CFET (Complementary FET) technology stacks NMOS and PMOS transistors to save space.

## Innovations in Transistor Design

- **Fin Depopulation**: Used by Samsung to reduce the number of fins in FinFETs while keeping the fin width the same. This reduces the size of transistors without losing performance.

- **Double Diffusion Break (DDB)**: Adds a gap between the source and drain regions of a transistor to reduce the effective width, allowing for smaller cell sizes and better scalability.
  
- **Contact Over Field Gate (COFG)**: The gate contact is placed over the field oxide region to reduce space between transistors and enable smaller cells.

- **Back-Side Power Delivery Network (BS-PDN)**: Power rails are routed to the backside of the chip, improving power delivery efficiency and allowing more space for transistors.

## Parasitic Resistance Challenges

- **Planar MOSFETs**: Traditional planar transistors have low parasitic resistance due to their simple structure.
  
- **FinFETs**: Parasitic resistance increases as the contact width decreases relative to the gate width.

- **Gate-All-Around (GAA) FETs**: This design further reduces contact width, which increases parasitic resistance and impacts performance.

- **Complementary FETs (CFETs)**: These vertically stacked transistors offer space efficiency but face similar parasitic resistance challenges as GAA FETs.

## Parasitic Resistance Breakdown

- Parasitic resistance is broken down into several components: contact resistance, resistance from the Back-End-Of-Line (BEOL) region, and resistance in the Front-End-Of-Line (FEOL) structure.
- Efforts to reduce parasitic resistance focus on improving the metal-semiconductor interface and enhancing doping concentration.

## Parasitic Capacitance and Scaling

- **Capacitance Breakdown**: As transistor nodes shrink, parasitic capacitances become more significant. For example, at 22nm, the capacitance from the gate and contact areas dominate. As technology moves to smaller nodes, techniques like air spacers are used to reduce parasitic capacitance.

- **Spacer Materials**: The use of advanced materials like SiBCN and SiCO at smaller nodes helps to reduce parasitic capacitance, improving device performance.

## 2D Materials for Advanced Transistors

- **Properties of 2D Materials (e.g., MoS₂)**: These materials have a uniform atomic scale thickness and can be ideal for scaling. They have a higher effective mass than silicon, which helps prevent direct source-to-drain tunneling at very small gate lengths.

## Scaling Challenges

- **Direct Tunneling**: As transistors shrink, electrons may tunnel directly from the source to the drain, causing leakage. To address this, materials with a higher effective mass are needed to control electron flow.
  
- **Surface Roughness and Variability**: At atomic scales, variations in surface roughness can impact performance. Using uniform, atomically thin materials like 2D materials helps minimize this issue.

- **Capacitance Ratios**: The gate-to-depletion capacitance ratio must be controlled to improve gate control. Materials with low dielectric constants are used to achieve this.

## Key Takeaways

- To maintain performance at smaller nodes, new materials and advanced designs are required. Transistor scaling is increasingly dependent on innovations in materials, contact technologies, and power delivery networks to overcome challenges like parasitic resistance, capacitance, and tunneling.


# Direct Source-to-Drain Tunneling and Advanced MOSFET Design

## Overview
This project focuses on advanced MOSFET designs using MoS₂ and other 2D materials to reduce leakage currents, improve power efficiency, and scale down transistor sizes. We explore the structure, fabrication, and performance of All-2D MOSFETs, as well as the challenges and solutions in advanced semiconductor technologies.

## Key Concepts
- **Direct Source-to-Drain Tunneling**: Smaller gate lengths lead to increased tunneling leakage. Materials like MoS₂ help reduce leakage due to their higher effective mass and larger bandgap.
- **MoS₂ Transistor with Ultra-Small Gate Length**: MoS₂ transistors with 1 nm gate lengths offer breakthroughs in miniaturization and energy efficiency.
- **All-2D MOSFET Design**: A completely 2D MOSFET with components made from graphene, MoS₂, and h-BN shows excellent performance and scalability.

## Advanced Fabrication Techniques
- **Monolithic 3D CMOS**: Stacking NMOS and PMOS transistors in separate layers reduces footprint and improves performance.
- **Copper Interconnects**: Techniques like Dual and Single Damascene Cu, along with selective barriers like TaN, improve interconnect reliability.
- **Back-Side Power Delivery Network (BS-PDN)**: Reduces IR-drop for better power efficiency and performance.

## Future Outlook
The use of 2D materials, innovative gate designs, and advanced interconnect technologies promise significant advancements in transistor scaling, energy efficiency, and overall performance.



Installation:

```
git clone --recursive https://github.com/The-OpenROAD-Project/OpenROAD-flow-scripts
cd OpenROAD-flow-scripts
sudo ./setup.sh
./build_openroad.sh --local
```

![Screenshot from 2024-11-24 22-33-17](https://github.com/user-attachments/assets/543d8816-1bc3-4099-b6e8-4a14a75ad5be)

Verifying installation:

```
source ./env.sh
yosys -help
openroad -help
cd flow
make


```

![Screenshot from 2024-11-24 22-38-32](https://github.com/user-attachments/assets/03bdb60b-f36f-44d3-a2c3-6d121d5e95d8)

![Screenshot from 2024-11-24 22-39-05](https://github.com/user-attachments/assets/ce6f948d-1ecf-4e52-a803-94a8d645c4c7)

![Screenshot from 2024-11-24 22-40-13](https://github.com/user-attachments/assets/1ad0459b-b705-437a-a79c-71d83681cce1)


![Screenshot from 2024-11-24 22-40-46](https://github.com/user-attachments/assets/92857565-4650-43e7-9ebf-dad4af073760)


ORFS directory and file formats
![image](https://github.com/user-attachments/assets/0bdbfd73-35bf-411e-b88d-4ce361c77399)

```
├── OpenROAD-flow-scripts             
│   ├── docker           -> It has Docker based installation, run scripts and all saved here
│   ├── docs             -> Documentation for OpenROAD or its flow scripts.  
│   ├── flow             -> Files related to run RTL to GDS flow  
|   ├── jenkins          -> It contains the regression test designed for each build update
│   ├── tools            -> It contains all the required tools to run RTL to GDS flow
│   ├── etc              -> Has the dependency installer script and other things
│   ├── setup_env.sh     -> Its the source file to source all our OpenROAD rules to run the RTL to GDS flow

```

Flow directory:

![image](https://github.com/user-attachments/assets/4bf4d4a1-0015-4e3e-83fb-493a0bb162ab)

```
├── flow           
│   ├── design           -> It has built-in examples from RTL to GDS flow across different technology nodes
│   ├── makefile         -> The automated flow runs through makefile setup
│   ├── platform         -> It has different technology note libraries, lef files, GDS etc 
|   ├── tutorials        
│   ├── util            
│   ├── scripts             

```

Automated RTL2GDS Flow for VSDBabySoC:
Create a Directory:

Create a directory named vsdbabysoc within the path OpenROAD-flow-scripts/flow/designs/sky130hd.

Copy Files from the VSDBabySoC Folder:

Copy the following folders from your local VSDBabySoC folder to the newly created vsdbabysoc directory:
gds folder: Contains the files avsddac.gds and avsdpll.gds.
include folder: Contains the files sandpiper.vh, sandpiper_gen.vh, sp_default.vh, and sp_verilog.vh.
lef folder: Contains the files avsddac.lef and avsdpll.lef.
lib folder: Contains the files avsddac.lib and avsdpll.lib.

Copy the Constraints File:

Copy the constraints file vsdbabysoc_synthesis.sdc from your local VSDBabySoC folder to the vsdbabysoc directory.

Copy Additional Configuration Files:

Copy the files macro.cfg and pin_order.cfg from your local VSDBabySoC folder into the vsdbabysoc directory.

Create a New Macro Configuration File:

Create a new macro.cfg file in the vsdbabysoc directory



Execution

![image](https://github.com/user-attachments/assets/fe18b791-5dad-4ca8-9aed-93b409d9fa0e)


![image](https://github.com/user-attachments/assets/496356de-917a-4d83-9bad-d0a28177711a)



Synthesis netlist 
![image](https://github.com/user-attachments/assets/a68b7658-0392-4983-9ee6-9a1c06a2391a)


Synthesis logs

![image](https://github.com/user-attachments/assets/6996dc60-543c-4676-b5c4-08d72c0f147c)


Synth check:

![image](https://github.com/user-attachments/assets/e4eb11c2-505c-4e3b-abdb-ed9830d77f97)


Synth stats

![image](https://github.com/user-attachments/assets/31a8a9fc-73ea-4081-9926-6ca4bcec9207)

![image](https://github.com/user-attachments/assets/c63e2300-6e6f-4c44-bea3-27cfefecc0bb)


![image](https://github.com/user-attachments/assets/1f83d73b-e588-4d14-8653-145abd084bf8)


Floorplan:

![image](https://github.com/user-attachments/assets/f5593366-e839-4355-900b-e0f458dcbdf7)


![image](https://github.com/user-attachments/assets/9eaa8835-8dc7-4223-bf33-b6af072011a6)





```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk place

```

![image](https://github.com/user-attachments/assets/97b33de3-79b9-4691-a5dd-cbbbf092ec2a)

![image](https://github.com/user-attachments/assets/85c8344e-86ef-449c-88dd-b53f64870399)

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_place

```

![image](https://github.com/user-attachments/assets/0c775eb5-f46a-4cfb-8cdf-c37255df0be9)

![image](https://github.com/user-attachments/assets/10598941-a126-41d4-af32-ad2cc76fa0b4)

![image](https://github.com/user-attachments/assets/d6a8a17a-c4e3-4503-bb3f-dd84c054b65d)


CTS



```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk cts

```

![image](https://github.com/user-attachments/assets/75cfff37-65c1-4e3f-9d86-0da71dba67f3)


![image](https://github.com/user-attachments/assets/c2410ce0-55b4-4585-b9be-68efb401bc86)

```
make DESIGN_CONFIG=./designs/sky130hd/vsdbabysoc/config.mk gui_cts

```


![image](https://github.com/user-attachments/assets/46f55e99-eee1-4270-bc98-9631cbec36cd)

![image](https://github.com/user-attachments/assets/6c18f34f-286b-415d-9616-31ccd59fe318)


![image](https://github.com/user-attachments/assets/72fe18d2-7090-46a3-aa18-595088a5d869)

![image](https://github.com/user-attachments/assets/de30039c-5ddc-483b-b65e-16f0b91d9a1c)

![image](https://github.com/user-attachments/assets/9a46d8f9-4845-4d72-b280-de4d5ce2d151)




Commmands route

![image](https://github.com/user-attachments/assets/5902704e-5d58-4574-bcd2-c48839702599)


![image](https://github.com/user-attachments/assets/20227a1e-8cc2-4b17-a1a4-534f78eb70c5)



cts report

![image](https://github.com/user-attachments/assets/a4d57217-943f-4d9d-8d49-f6d2322b10a1)

```

==========================================================================
cts final report_tns
--------------------------------------------------------------------------
tns 0.00

==========================================================================
cts final report_wns
--------------------------------------------------------------------------
wns 0.00

==========================================================================
cts final report_worst_slack
--------------------------------------------------------------------------
worst slack 5.88

==========================================================================
cts final report_clock_skew
--------------------------------------------------------------------------
Clock clk
   0.82 source latency core.CPU_src1_value_a3[20]$_DFF_P_/CLK ^
  -0.72 target latency core.CPU_valid_taken_br_a4$_DFF_P_/CLK ^
   0.56 clock uncertainty
   0.00 CRPR
--------------
   0.67 setup skew


==========================================================================
cts final report_checks -path_delay min
--------------------------------------------------------------------------
Startpoint: core.CPU_rs1_a2[3]$_SDFF_PN0_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src1_value_a3[2]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.16    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.26 ^ clkbuf_3_1__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.14    0.15    0.28    0.54 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1__leaf_CLK (net)
                  0.15    0.00    0.54 ^ clkbuf_leaf_63_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    11    0.04    0.06    0.18    0.72 ^ clkbuf_leaf_63_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_63_CLK (net)
                  0.06    0.00    0.72 ^ core.CPU_rs1_a2[3]$_SDFF_PN0_/CLK (sky130_fd_sc_hd__dfxtp_4)
     4    0.04    0.12    0.41    1.12 ^ core.CPU_rs1_a2[3]$_SDFF_PN0_/Q (sky130_fd_sc_hd__dfxtp_4)
                                         core.CPU_rf_rd_index1_a2[3] (net)
                  0.12    0.00    1.12 ^ _7706_/B (sky130_fd_sc_hd__nor2_1)
     2    0.01    0.07    0.09    1.22 v _7706_/Y (sky130_fd_sc_hd__nor2_1)
                                         _3281_ (net)
                  0.07    0.00    1.22 v _7709_/A2 (sky130_fd_sc_hd__a21o_1)
     3    0.01    0.07    0.22    1.44 v _7709_/X (sky130_fd_sc_hd__a21o_1)
                                         _3284_ (net)
                  0.07    0.00    1.44 v _7908_/B1 (sky130_fd_sc_hd__o22ai_1)
     1    0.00    0.13    0.13    1.57 ^ _7908_/Y (sky130_fd_sc_hd__o22ai_1)
                                         core.CPU_src1_value_a2[2] (net)
                  0.13    0.00    1.57 ^ core.CPU_src1_value_a3[2]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  1.57   data arrival time

                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.16    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.26 ^ clkbuf_3_1__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.14    0.15    0.28    0.54 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_1__leaf_CLK (net)
                  0.15    0.00    0.54 ^ clkbuf_leaf_8_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.04    0.06    0.18    0.72 ^ clkbuf_leaf_8_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_8_CLK (net)
                  0.06    0.00    0.72 ^ core.CPU_src1_value_a3[2]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                          0.90    1.62   clock uncertainty
                          0.00    1.62   clock reconvergence pessimism
                         -0.05    1.57   library hold time
                                  1.57   data required time
-----------------------------------------------------------------------------
                                  1.57   data required time
                                 -1.57   data arrival time
-----------------------------------------------------------------------------
                                  0.00   slack (MET)



==========================================================================
cts final report_checks -path_delay max
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.16    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.26 ^ clkbuf_3_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.24    0.24    0.34    0.60 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4__leaf_CLK (net)
                  0.24    0.00    0.60 ^ clkbuf_leaf_5_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.07    0.21    0.81 ^ clkbuf_leaf_5_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_5_CLK (net)
                  0.07    0.00    0.81 ^ core.CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.08    0.34    1.15 ^ core.CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_dmem_rd_en_a4 (net)
                  0.08    0.00    1.15 ^ _4358_/C (sky130_fd_sc_hd__or4_4)
    55    0.39    1.09    0.86    2.01 ^ _4358_/X (sky130_fd_sc_hd__or4_4)
                                         _0778_ (net)
                  1.09    0.00    2.01 ^ _4360_/A (sky130_fd_sc_hd__clkinv_16)
    33    0.27    0.34    0.38    2.40 v _4360_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _0780_ (net)
                  0.34    0.00    2.40 v _6320_/C (sky130_fd_sc_hd__or3_1)
     1    0.01    0.10    0.47    2.87 v _6320_/X (sky130_fd_sc_hd__or3_1)
                                         _2179_ (net)
                  0.10    0.00    2.87 v _6322_/A2 (sky130_fd_sc_hd__a21oi_4)
     8    0.09    0.64    0.56    3.42 ^ _6322_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _2181_ (net)
                  0.64    0.00    3.43 ^ _7562_/A (sky130_fd_sc_hd__and2_4)
    39    0.18    0.50    0.61    4.03 ^ _7562_/X (sky130_fd_sc_hd__and2_4)
                                         _3194_ (net)
                  0.50    0.01    4.04 ^ _7610_/A2 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.09    0.12    4.17 v _7610_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _3227_ (net)
                  0.09    0.00    4.17 v hold1462/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.57    4.74 v hold1462/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1536 (net)
                  0.06    0.00    4.74 v _7611_/B1 (sky130_fd_sc_hd__a311oi_1)
     1    0.00    0.24    0.24    4.97 ^ _7611_/Y (sky130_fd_sc_hd__a311oi_1)
                                         _0752_ (net)
                  0.24    0.00    4.97 ^ hold1463/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.59    5.56 ^ hold1463/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1537 (net)
                  0.06    0.00    5.56 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.56   data arrival time

                         11.25   11.25   clock clk (rise edge)
                          0.00   11.25   clock source latency
     1    0.16    0.00    0.00   11.25 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01   11.26 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25   11.51 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00   11.51 ^ clkbuf_3_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.24    0.24    0.34   11.85 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4__leaf_CLK (net)
                  0.24    0.00   11.85 ^ clkbuf_leaf_69_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.21   12.06 ^ clkbuf_leaf_69_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_69_CLK (net)
                  0.07    0.00   12.06 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.56   11.50   clock uncertainty
                          0.00   11.50   clock reconvergence pessimism
                         -0.05   11.45   library setup time
                                 11.45   data required time
-----------------------------------------------------------------------------
                                 11.45   data required time
                                 -5.56   data arrival time
-----------------------------------------------------------------------------
                                  5.88   slack (MET)



==========================================================================
cts final report_checks -unconstrained
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

Fanout     Cap    Slew   Delay    Time   Description
-----------------------------------------------------------------------------
                          0.00    0.00   clock clk (rise edge)
                          0.00    0.00   clock source latency
     1    0.16    0.00    0.00    0.00 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01    0.01 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00    0.26 ^ clkbuf_3_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.24    0.24    0.34    0.60 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4__leaf_CLK (net)
                  0.24    0.00    0.60 ^ clkbuf_leaf_5_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    15    0.04    0.07    0.21    0.81 ^ clkbuf_leaf_5_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_5_CLK (net)
                  0.07    0.00    0.81 ^ core.CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
     2    0.01    0.08    0.34    1.15 ^ core.CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
                                         core.CPU_dmem_rd_en_a4 (net)
                  0.08    0.00    1.15 ^ _4358_/C (sky130_fd_sc_hd__or4_4)
    55    0.39    1.09    0.86    2.01 ^ _4358_/X (sky130_fd_sc_hd__or4_4)
                                         _0778_ (net)
                  1.09    0.00    2.01 ^ _4360_/A (sky130_fd_sc_hd__clkinv_16)
    33    0.27    0.34    0.38    2.40 v _4360_/Y (sky130_fd_sc_hd__clkinv_16)
                                         _0780_ (net)
                  0.34    0.00    2.40 v _6320_/C (sky130_fd_sc_hd__or3_1)
     1    0.01    0.10    0.47    2.87 v _6320_/X (sky130_fd_sc_hd__or3_1)
                                         _2179_ (net)
                  0.10    0.00    2.87 v _6322_/A2 (sky130_fd_sc_hd__a21oi_4)
     8    0.09    0.64    0.56    3.42 ^ _6322_/Y (sky130_fd_sc_hd__a21oi_4)
                                         _2181_ (net)
                  0.64    0.00    3.43 ^ _7562_/A (sky130_fd_sc_hd__and2_4)
    39    0.18    0.50    0.61    4.03 ^ _7562_/X (sky130_fd_sc_hd__and2_4)
                                         _3194_ (net)
                  0.50    0.01    4.04 ^ _7610_/A2 (sky130_fd_sc_hd__o21ai_0)
     1    0.00    0.09    0.12    4.17 v _7610_/Y (sky130_fd_sc_hd__o21ai_0)
                                         _3227_ (net)
                  0.09    0.00    4.17 v hold1462/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.57    4.74 v hold1462/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1536 (net)
                  0.06    0.00    4.74 v _7611_/B1 (sky130_fd_sc_hd__a311oi_1)
     1    0.00    0.24    0.24    4.97 ^ _7611_/Y (sky130_fd_sc_hd__a311oi_1)
                                         _0752_ (net)
                  0.24    0.00    4.97 ^ hold1463/A (sky130_fd_sc_hd__dlygate4sd3_1)
     1    0.00    0.06    0.59    5.56 ^ hold1463/X (sky130_fd_sc_hd__dlygate4sd3_1)
                                         net1537 (net)
                  0.06    0.00    5.56 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
                                  5.56   data arrival time

                         11.25   11.25   clock clk (rise edge)
                          0.00   11.25   clock source latency
     1    0.16    0.00    0.00   11.25 ^ pll/CLK (avsdpll)
                                         CLK (net)
                  0.01    0.01   11.26 ^ clkbuf_0_CLK/A (sky130_fd_sc_hd__clkbuf_16)
     8    0.22    0.23    0.25   11.51 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_0_CLK (net)
                  0.23    0.00   11.51 ^ clkbuf_3_4__f_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    16    0.24    0.24    0.34   11.85 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_3_4__leaf_CLK (net)
                  0.24    0.00   11.85 ^ clkbuf_leaf_69_CLK/A (sky130_fd_sc_hd__clkbuf_16)
    12    0.05    0.07    0.21   12.06 ^ clkbuf_leaf_69_CLK/X (sky130_fd_sc_hd__clkbuf_16)
                                         clknet_leaf_69_CLK (net)
                  0.07    0.00   12.06 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
                         -0.56   11.50   clock uncertainty
                          0.00   11.50   clock reconvergence pessimism
                         -0.05   11.45   library setup time
                                 11.45   data required time
-----------------------------------------------------------------------------
                                 11.45   data required time
                                 -5.56   data arrival time
-----------------------------------------------------------------------------
                                  5.88   slack (MET)



==========================================================================
cts final report_check_types -max_slew -max_cap -max_fanout -violators
--------------------------------------------------------------------------

==========================================================================
cts final max_slew_check_slack
--------------------------------------------------------------------------
0.41091519594192505

==========================================================================
cts final max_slew_check_limit
--------------------------------------------------------------------------
1.5

==========================================================================
cts final max_slew_check_slack_limit
--------------------------------------------------------------------------
0.2739

==========================================================================
cts final max_fanout_check_slack
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_fanout_check_limit
--------------------------------------------------------------------------
1.0000000150474662e+30

==========================================================================
cts final max_capacitance_check_slack
--------------------------------------------------------------------------
0.025694845244288445

==========================================================================
cts final max_capacitance_check_limit
--------------------------------------------------------------------------
0.02930700220167637

==========================================================================
cts final max_capacitance_check_slack_limit
--------------------------------------------------------------------------
0.8767

==========================================================================
cts final max_slew_violation_count
--------------------------------------------------------------------------
max slew violation count 0

==========================================================================
cts final max_fanout_violation_count
--------------------------------------------------------------------------
max fanout violation count 0

==========================================================================
cts final max_cap_violation_count
--------------------------------------------------------------------------
max cap violation count 0

==========================================================================
cts final setup_violation_count
--------------------------------------------------------------------------
setup violation count 0

==========================================================================
cts final hold_violation_count
--------------------------------------------------------------------------
hold violation count 0

==========================================================================
cts final report_checks -path_delay max reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_valid_load_a4$_DFF_P_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: max

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34    0.60 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21    0.81 ^ clkbuf_leaf_5_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.81 ^ core.CPU_valid_load_a4$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.34    1.15 ^ core.CPU_valid_load_a4$_DFF_P_/Q (sky130_fd_sc_hd__dfxtp_1)
   0.86    2.01 ^ _4358_/X (sky130_fd_sc_hd__or4_4)
   0.39    2.40 v _4360_/Y (sky130_fd_sc_hd__clkinv_16)
   0.47    2.87 v _6320_/X (sky130_fd_sc_hd__or3_1)
   0.56    3.42 ^ _6322_/Y (sky130_fd_sc_hd__a21oi_4)
   0.61    4.03 ^ _7562_/X (sky130_fd_sc_hd__and2_4)
   0.13    4.17 v _7610_/Y (sky130_fd_sc_hd__o21ai_0)
   0.57    4.74 v hold1462/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.24    4.97 ^ _7611_/Y (sky130_fd_sc_hd__a311oi_1)
   0.59    5.56 ^ hold1463/X (sky130_fd_sc_hd__dlygate4sd3_1)
   0.00    5.56 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/D (sky130_fd_sc_hd__dfxtp_1)
           5.56   data arrival time

  11.25   11.25   clock clk (rise edge)
   0.00   11.25   clock source latency
   0.00   11.25 ^ pll/CLK (avsdpll)
   0.26   11.51 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.34   11.85 ^ clkbuf_3_4__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.21   12.06 ^ clkbuf_leaf_69_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00   12.06 ^ core.CPU_Xreg_value_a4[9][24]$_SDFFE_PP0P_/CLK (sky130_fd_sc_hd__dfxtp_1)
  -0.56   11.50   clock uncertainty
   0.00   11.50   clock reconvergence pessimism
  -0.05   11.45   library setup time
          11.45   data required time
---------------------------------------------------------
          11.45   data required time
          -5.56   data arrival time
---------------------------------------------------------
           5.88   slack (MET)



==========================================================================
cts final report_checks -path_delay min reg to reg
--------------------------------------------------------------------------
Startpoint: core.CPU_rs1_a2[3]$_SDFF_PN0_
            (rising edge-triggered flip-flop clocked by clk)
Endpoint: core.CPU_src1_value_a3[2]$_DFF_P_
          (rising edge-triggered flip-flop clocked by clk)
Path Group: clk
Path Type: min

  Delay    Time   Description
---------------------------------------------------------
   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.28    0.54 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.72 ^ clkbuf_leaf_63_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.72 ^ core.CPU_rs1_a2[3]$_SDFF_PN0_/CLK (sky130_fd_sc_hd__dfxtp_4)
   0.41    1.12 ^ core.CPU_rs1_a2[3]$_SDFF_PN0_/Q (sky130_fd_sc_hd__dfxtp_4)
   0.10    1.22 v _7706_/Y (sky130_fd_sc_hd__nor2_1)
   0.22    1.44 v _7709_/X (sky130_fd_sc_hd__a21o_1)
   0.13    1.57 ^ _7908_/Y (sky130_fd_sc_hd__o22ai_1)
   0.00    1.57 ^ core.CPU_src1_value_a3[2]$_DFF_P_/D (sky130_fd_sc_hd__dfxtp_1)
           1.57   data arrival time

   0.00    0.00   clock clk (rise edge)
   0.00    0.00   clock source latency
   0.00    0.00 ^ pll/CLK (avsdpll)
   0.26    0.26 ^ clkbuf_0_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.28    0.54 ^ clkbuf_3_1__f_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.18    0.72 ^ clkbuf_leaf_8_CLK/X (sky130_fd_sc_hd__clkbuf_16)
   0.00    0.72 ^ core.CPU_src1_value_a3[2]$_DFF_P_/CLK (sky130_fd_sc_hd__dfxtp_1)
   0.90    1.62   clock uncertainty
   0.00    1.62   clock reconvergence pessimism
  -0.05    1.57   library hold time
           1.57   data required time
---------------------------------------------------------
           1.57   data required time
          -1.57   data arrival time
---------------------------------------------------------
           0.00   slack (MET)



==========================================================================
cts final critical path target clock latency max path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path target clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path source clock latency min path
--------------------------------------------------------------------------
0

==========================================================================
cts final critical path delay
--------------------------------------------------------------------------
5.5626

==========================================================================
cts final critical path slack
--------------------------------------------------------------------------
5.8841

==========================================================================
cts final slack div critical path delay
--------------------------------------------------------------------------
105.779671

==========================================================================
cts final report_power
--------------------------------------------------------------------------
Group                  Internal  Switching    Leakage      Total
                          Power      Power      Power      Power (Watts)
----------------------------------------------------------------
Sequential             3.77e-03   2.87e-04   8.17e-09   4.05e-03  40.1%
Combinational          8.32e-04   1.32e-03   1.86e-08   2.15e-03  21.3%
Clock                  2.27e-03   1.64e-03   1.85e-09   3.90e-03  38.6%
Macro                  0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
Pad                    0.00e+00   0.00e+00   0.00e+00   0.00e+00   0.0%
----------------------------------------------------------------
Total                  6.86e-03   3.24e-03   2.86e-08   1.01e-02 100.0%
                          67.9%      32.1%       0.0%

```



</details>
