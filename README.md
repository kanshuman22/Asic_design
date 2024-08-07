# ASIC DESIGN  

## TASK 1

### C Code 
A code to print sum of numbers from 1 to n(100)

Code:


### GCC compilation

Compilation and output of the code:


### RISC-V compilation

Compilation with flag set as O1

1) Object file creation shown below:


2) Execution of the program and corresponding output:

3) We will now focus on the main section . We want to calculate the total no. of instructions. To do this we will subtract the address of the first instruction in the main section from the first instruction in the next section. As it is a byte addressable memory so we will divide by 4 to get the no. of instructions.

