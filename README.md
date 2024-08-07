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

Compilation with flag set as O1:

![Screenshot from 2024-08-07 19-00-55](https://github.com/user-attachments/assets/d86ad928-17d3-45d1-b9a8-848426dc538f)


1) Object file creation shown below:


2) Execution of the program and corresponding output:

![image](https://github.com/user-attachments/assets/17392b91-bea8-40a7-aae3-b8e107f6e933)

3) We will now focus on the main section . We want to calculate the total no. of instructions. To do this we will subtract the address of the first instruction in the main section from the first instruction in the next section. As it is a byte addressable memory so we will divide by 4 to get the no. of instructions.

