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

### Instruction Types

### R-Type Instructions
R-Type instructions perform operations between registers.

- **ADD** r4, r4, r4  
  - **Binary Pattern**: `0000000 00100 00100 000 00100 0110011`

- **SUB** r4, r4, r4  
  - **Binary Pattern**: `0100000 00100 00100 000 00100 0110011`

- **AND** r4, r4, r4  
  - **Binary Pattern**: `0000000 00100 00100 111 00100 0110011`

- **OR** r8, r4, r5  
  - **Binary Pattern**: `0000000 00101 00100 110 01000 0110011`

- **XOR** r8, r4, r4  
  - **Binary Pattern**: `0000000 00100 00100 100 01000 0110011`

- **SLT** r00, r1, r4  
  - **Binary Pattern**: `0000000 00100 00001 010 00000 0110011`

- **SLL** r05, r01, r1  
  - **Binary Pattern**: `0000000 00001 00001 001 00101 0110011`

### I-Type Instructions
I-Type instructions involve an immediate value.

- **ADDI** r02, r2, 5  
  - **Binary Pattern**: `000000000101 00010 000 00010 0010011`

- **SRL** r06, r01, r1  
  - **Binary Pattern**: `0000000 00001 00001 101 00110 0010011`

- **LW** r03, r01, 2  
  - **Binary Pattern**: `000000000010 00001 010 00011 0000011`

### S-Type Instructions
S-Type instructions are used for store operations.

- **SW** r2, r0, 4  
  - **Binary Pattern**: `000000000100 00000 010 00010 0100011`

### B-Type Instructions
B-Type instructions are used for conditional branches.

- **BNE** r0, r0, 20  
  - **Binary Pattern**: `00000000010100 00000 001 00000 1100011`

- **BEQ** r0, r0, 15  
  - **Binary Pattern**: `00000000001111 00000 000 00000 1100011`












