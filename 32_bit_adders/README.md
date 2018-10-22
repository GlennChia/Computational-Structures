# 1. Brent-Kung Architecture Links
1. https://www.slideshare.net/VINAYAKKINI3/custom-design-and-layout-implementation-of-16-bit-brent-kung-adder-in-45nm-cmos-technology
2. http://article.sciencepublishinggroup.com/html/10.11648.j.jeee.20150306.11.html

# 2. jsim
1. 0 is treated as the gnd and is not a node 
2. 1 is treated as a node. Hence for the selector, if we want it to be take the carry in as 1 we have to use the following line to force the node 1 to be treated as binary 1
```
Xforce 1 constant1
```

# 3. Coding a .bc file
## 3.1 .bc syntax
1. Link: http://users.ics.aalto.fi/tjunttil/circuits/
2. Note that header files must have BC at the front and a float that follows. Alphabetical names somehow don't work
```
BC1.1
```
3. There isn't a NAND gate in the .bc so use the following to create one. Note that nands don't show up as a new node in the generated .cnf file but its effect will be there. This means that the cnf will show nr1a and nr1b as the nodes but the effecct of the nand will be in their iterators
```
nr1a := AND(op0, xr1);
notnr1a := !nr1a;
nr1b := AND(xb1, A1);
notnr1b := NOT(nr1b);
```

## 3.2 Comparing 2 circuits 
1. Choose an appropriate bit for comparison 
2. S[0] is not appropriate because it is essentially similar in logic
3. Choose something like S[3] because in Brent-Kung, it does not wait for the carry out of the previous one to proceed. It uses C[0] and the P and G generated from the A and B inputs
4. At the end of the .bc file, it is necessary to include xor by the following line. Note that s3 refers to the Brent-Kung's s3 while srip refers to the ripple carry s[3]
```
ans := ODD(s3,srip);
```
# 4. Generating a .cnf file 
## 4.1 .cnf syntax
1. Link to generator: http://10.1.3.26:8000/bc2cnf/
2. Link to syntax: https://app.box.com/s/opgbiv7310szl97nzv5devu3gi7qzbe3
3. The top part of the code has comments that start with c. The code below means that the xr2 node is the iterator with name '12'
```
c xr2 <-> 12
```
4. In the cnf, if we see the node '12' with -12, it has a binary value of 0. If it has a value 12 then it has a binary value of 1
5. To make sure the verification step works later on, find the node of the ans (This is the xor comparison done earlier in the .bc file). In the example below, it is node 1. Hence we add the following line to the end of the .cnf code. This ensures that ans must be true for the whole cnf to be true since all the clauses must be true.
```
c ans <-> 1
```

# 5. Verifying the .cnf
## 5.1 Pre-requisites
1. Download findsolssat.jar
Link: https://app.box.com/s/rqwzxn0boi7erra2asqn0g7enyecdh95
2. Save this .jar file in the same file as your .cnf file
3. Find the directory from the command line. There are 2 methods to do this
  * In the file explorer, click the file path and copy and paste it into the command prompt
  * Use dir to list directories and cd to change directory into the desired one (For windows)
## 5.2 Running the file
1. Run the file with 
```
java -jar findsolssat.jar name_of_cnf_file.cnf
```
2. If it works it will say "Unsatisfiable: True"
## 5.3 Debugging strategies 
1. If it fails, there will be many solutions printed on the screen
2. Choose one input and from the nodes corresponding to A, B and C, verify the logic of the architectures 
**TIP: start from the ripple carry adder because it is easier and we can do it manually**
3. From the wrong adder, check the .bc file and check if the code is done properly 
4. If the code is flawless, check the path and the logic gates drawn (for complex architectures, it is common to make mistakes in the drawing of gates). This means that the .bc is correct but the architecture that it codes for is wrong. Adjust the drawing and code the .bc again 
5. **TIP: it is recommended to try it for S[0] to get the idea of how it works before doing something crazy like S[16]**
