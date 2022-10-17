#Bugs

##Crash1:
In this testcase, I used the flaw on the line 186 of the original program; The input from the user is not sanitized in this case. Rather, it is used directly to dynamically allocate memory through the variable ret_val->num_bytes. If the user passes a negative value to the program, it will cause an exception in the malloc function and makes the program crash. I put -1 in this variable and I guess it gets wrapped around to a very large positive size which is not available in the memory to allocate to the program.
I fixed it by putting an if clause before calling malloc so that the program is safely terminated in case the size is negative.

##Crash2:
When the animate function is called with opcode 0x01, the message is assigned to the arg1 element of the reg array. The problem is we only have 16 registers meaning that if arg1 represents a number greater than 15, the buffer will overflow and cause the program to crash! If the message is a valid shellcode, it may even get executed as a result of a bufferoverflow attack.
To fix this problem, I check that arg1 is a non-negative value less than 16 so that it won't overflow the registers.

##Hang
When the animate function is called with opcode 0x09, the pc value is incremented by the char representation of arg1. The problem is, in the giftcardwriter program, an UNSIGNED char value is written into memory but here we explicitly cast it to char. That's why the most significant digit translates into negative (in case of 1) or positive (in case of zero). Since the pc is incremented by three after each loop, and we want the loop to run forever, we have to somehow decrement pc by 3. That's why we write 130 into arg1 value, which when casted to char gets warpped around and equals -3. This causes pc value to stay the same forever [pc+(-3)+3 = pc-3+3 = pc]!
To fix this problem, I changed the char into unsigned char so the destination casting is in accordance to the origin.
