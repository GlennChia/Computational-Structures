# 1. Overall idea

1. Follow the C Code closely
2. Try not to branch too much and we do this by following a sequential order. 
3. Sequential means going into the for loop first and then doing a CMPLEQ to check if the condition is fulfilled. 
4. Can consider adding to the index straight away in case we forget 

# 2. Common Mistakes 

1. Did not initialise the for loop values 
Use CMOVE(0,reg)
2. Did not pop the registers at the end
The caller may need those registers 
3. For constants, use the C behind the commands 
ADDC, CMPLEQ
4. The lab sheet says use SHRA instead of SHRC

# 3. Tips

1. Easier to have one register to store each value even for the compare, this makes things less confusing 
2. Comment every line and follow the C code closely 
