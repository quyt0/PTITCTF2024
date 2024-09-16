# Write-up: FlagChecker (First Solve in Reverse Engineering)

##  1.  Analyzing the Challenge

The challenge provides a compressed file containing `FlagChecker.exe`. Running the file shows a GUI-based app that checks the flag.
![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/flag-checker/app.png)

Using IDA to gather more information
![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/flag-checker/ida.png)

In `sub_7FF731D11450`, I found the declaration of an array `a1` with 50 elements (from 0 to 49). Each element is multiplied by an integer. 
&rarr; **This is likely a system of equations with 50 unknowns. I'll figure out how to solve it.**

## 2. Solving the System of Equations

### a. Analysis
Since the system involves a large number of equations (at least 50), and each equation has 50 coefficients and 50 unknowns, I’ll use the [z3](https://z3prover.github.io/api/html/z3.html) library to solve it. 

To solve the system in Python, I need to replace some parts like *pointers* and *bit shifts* appropriately (see the example below). 
*Example: Code for one equation in the system.*
```c
    if ( 49 * a1[49]
     + 777 * a1[48]
     + 279 * a1[47]
     + 923 * a1[46]
     + 39 * a1[45]
     + 423 * a1[44]
     + 537 * a1[43]
     + 1011 * a1[42]
     + 258 * a1[41]
     + 1000 * a1[40]
     + 832 * a1[39]
     + 430 * a1[34]
     + 225 * a1[33]
     + 617 * a1[32]
     + 224 * a1[31]
     + 835 * a1[30]
     + 416 * a1[29]
     + 950 * a1[28]
     + 458 * a1[27]
     + 863 * a1[26]
     + v1 //Biến v1  
     + 601 * a1[35]
     + 20 * a1[36]
     + 216 * a1[37]
     + 288 * a1[38]
     + 301 * a1[50] == 2146536 )
```

For `v1` I replaced it with

```c
    v1 = 336 * a1[25]
     + 822 * a1[24]
     + 914 * a1[23]
     + 338 * a1[22]
     + 145 * a1[21]
     + 944 * a1[20]
     + 648 * a1[19]
     + 381 * a1[18]
     + 338 * a1[17]
     + 974 * a1[16]
     + 855 * a1[15]
     + 460 * a1[14]
     + 471 * a1[11]
     + 882 * a1[10]
     + 749 * a1[9]
     + 516 * a1[8]
     + 4 * a1[5]
     + 424 * a1[4]
     + 413 * a1[3]
     + 795 * a1[2]
     + 173 * a1[1]
     + 896 * *a1 // I replaced this with 896 * a1[0]
     + 485 * a1[6]
     + 20 * a1[7]
     + 445 * a1[12]
     + 1023 * a1[13];
```
Additionally, there’s a bit shift operation
Eg: I replaced `a1[23] << 8` with `a1[23]*2*2*2*2*2*2*2*2` (2 mũ 8 :)))

### b. Solving the System of Equations

*Install z3*

    pip install z3
    pip install z3-solver
After a while using *Find & Replace* in *Sublime Text* + combining with *Regex* to convert the `C code` from IDA into `Python code` (since I was lazy and didn’t want to manually copy ~50 equations into Python), I finally got [this file](https://github.com/quyt0/PTITCTF2024/blob/main/flag-checker/flag.py)

Running the Python code solved the system of equations. Converting the solutions from `int` to the corresponding `char` values in the `ASCII table`, I got the flag: `PTITCTF{Y0u_C4n_Br34k_Equ4tion_Ag41n_6861696e64!!!}`

![enter image description here](https://raw.githubusercontent.com/quyt0/PTITCTF2024/main/flag-checker/flag.png)