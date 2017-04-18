**Liu Ziyang**<br>
**A0148037E**

### Description
---
Use top down dynamic programming with recursion to solve this problem. In the meantime, save the result of overlapping sub problems. Here is the pseudo-code:
```Java
//start is the galaxy to be put in the ring;
//leftS, leftE, leftL is the number of left S, E, L respectively.
ALGORITHM computeRingArrangement(start, leftS, leftE, leftL)
if it is impossible to make a ring with given arguments //Max(leftS, leftE, leftL) > the sum of remaining two + 1;
    return 0;
if start is S
    if leftS is 0 //there should at least be one S
        return 0;
    if s[leftS][leftE][leftL] has been memorized //s[leftS][leftE][leftL] means the total ways  
                                                 //of arrangement given S as the first galaxy, 
                                                 //leftS number of S, leftE number of E and leftL number of L;
        return the memorized result;
    if leftE and leftL are both 0
        startSEndS[leftS][leftE][leftL] = 1; //startSEndS[leftS][leftE][leftL] means the total ways 
                                             //of arrangement given S as both the first and the last galaxy, 
                                             //leftS number of S, leftE number of E and leftL number of L;
        return 1;

    set newS as leftS - 1
    
    //recursion to solve the total ways of arrangements allowing the start and end to be the same
    result = computeRingArrangement("E", newS, leftE, leftL) + computeRingArrangement("L", newS, leftE, leftL);
    s[leftS][leftE][leftL] = result;

    //keep track of ways of arrangement ending with S, E and L
    startSEndS[leftS][leftE][leftL] = startEEndS[newS][leftE][leftL] + startLEndS[newS][leftE][leftL];
    startSEndE[leftS][leftE][leftL] = startEEndE[newS][leftE][leftL] + startLEndE[newS][leftE][leftL];
    startSEndL[leftS][leftE][leftL] = startEEndL[newS][leftE][leftL], startLEndL[newS][leftE][leftL];

    //in fact the result has already been memorized in the 3d array s[leftS][leftE][leftL].
    //s[leftS][leftL][leftE] should have the same result as s[leftS][leftE][leftL],
    //because if we treat E as L and treat L as E, these two subproblems become exactly the same.
    //therefore, we need to memorize all the interchangeable ones:
    //s[leftS][leftL][leftE] = s[leftS][leftE][leftL];    
    //startSEndL[leftS][leftL][leftE] = startSEndE[leftS][leftE][leftL]; 
    //startSEndE[leftS][leftL][leftE] = startSEndL[leftS][leftE][leftL].
    //similarly, e[leftE][leftS][leftL], e[leftL][leftS][leftE], l[leftL][leftE][leftS], 
    //and l[leftE][leftL][leftS] should also be memorized
    memorization("S", leftS, leftE, leftL);

else if start is E
    if leftE is 0
        return 0;
    if e[leftS][leftE][leftL] has been memorized
        return the memorized result;
    if leftS and leftL are both 0
        startEEndE[leftS][leftE][leftL] = 1;
        return 1;
        set newE as leftE - 1
    result = computeRingArrangement("S", leftS, newE, leftL) + computeRingArrangement("L", leftS, newE, leftL);
    e[leftS][leftE][leftL] = result;
    startEEndS[leftS][leftE][leftL] = startSEndS[leftS][newE][leftL] + startLEndS[leftS][newE][leftL];
    startEEndE[leftS][leftE][leftL] = startSEndE[leftS][newE][leftL] + startLEndE[leftS][newE][leftL];
    startEEndL[leftS][leftE][leftL] = startSEndL[leftS][newE][leftL] + startLEndL[leftS][newE][leftL];
    memorization("E", leftS, leftE, leftL);

else if start is L
    if leftL is 0
        return 0;
    if l[leftS][leftE][leftL] has been memorized
        return the memorized result;
    if leftS and leftE are both 0
        startLEndL[leftS][leftE][leftL] = 1;
        return 1;
    set newL as leftL - 1
    result = computeRingArrangement("S", leftS, leftE, newL) + computeRingArrangement("E", leftS, leftE, newL);
    l[leftS][leftE][leftL] = result;
    startLEndS[leftS][leftE][leftL] = startSEndS[leftS][leftE][newL] + startLEndS[leftS][leftE][newL];
    startLEndE[leftS][leftE][leftL] = startSEndE[leftS][leftE][newL] + startLEndE[leftS][leftE][newL];
    startLEndL[leftS][leftE][leftL] = startSEndL[leftS][leftE][newL] + startLEndL[leftS][leftE][newL];
    memorization("L", leftS, leftE, leftL);
else do nothing.

return result;
```

```java
result = computeRingArrangement("S", numberS, numberE, numberL) +
        computeRingArrangement("E", numberS, numberE, numberL) +
        computeRingArrangement("L", numberS, numberE, numberL);//allowing the start and end to be the same

//since we don't allow the start and end to be the same
if the size of input is more than 1
    result = result - startSEndS(numberS, numberE, numberL) 
        - startEEndE(numberS, numberE, numberL) 
        - startLEndL(numberS, numberE, numberL);

print the result modulo 10^9 with a newline;
```
### Correctness
---
The algorithm described above firstly computed the ways of arrangement **M**, (*assuming galaxies are put in a list first*), then find the number of ways **N** with start and end being the same. Clearly, **M** - **N** is the final result.


**Proof by introduction:**
```
Base: if numberS = numberL = numberE = 0, it is easy to check that the algorithm terminates and returns 0;
if one of these numbers is 1 and the other two are 0, obviously, the algorithm terminates and returns 1;

Induction:
if for n > 1 galaxies, the algorithm computes the result correctly:
    for n + 1 galaxies, assuming numberS, numberL, numberE:
        M = computeRingArrangement("S", numberS, numberE, numberL) +
            computeRingArrangement("E", numberS, numberE, numberL) +
            computeRingArrangement("L", numberS, numberE, numberL);

        ∵ computeRingArrangement("S", numberS, numberE, numberL) = 
            computeRingArrangement("E", numberS - 1, numberE, numberL)
            + computeRingArrangement("L", numberS - 1, numberE, numberL)
        ∵ numberS + numberE + numberL - 1 = n,

        ∴ computeRingArrangement("S", numberS, numberE, numberL) gives the correct result;
        ∴ Similarly, computeRingArrangement("E", numberS, numberE, numberL) and 
        computeRingArrangement("L", numberS, numberE, numberL) also give the right result.

        Hence, M is computed correctly and the algorithm will terminate.

        ∵ N = startSEndS[numberS][numberE][numberL] 
            + startEEndE[numberS][numberE][numberL] 
            + startLEndL[numberS][numberE][numberL];
        ∵ for startSEndS[numberS][numberE][numberL], 
        it is computed inside computeRingArrangement("S", numberS, numberE, numberL),
        with startSEndS[numberS][numberE][numberL] = 
            startEEndS[numberS - 1][numberE][numberL] 
            + startLEndS[numberS - 1][numberE][numberL]
        ∵ numberS + numberE + numberL - 1 = n
        
        ∴ the startSEndS[numberS][numberE][numberL] is computed correctly.
        ∴ similarly, startEEndE[numberS][numberE][numberL] 
        and startLEndL[numberS][numberE][numberL] are also correct.

        Hence, N is correctly computed.

        Hence, for n + 1 galaxies, the algorithm also terminates and computes the correct result.
Hence, the algorithm is correct.
```
### Complexity
---
+ Time

there are *numberS* S, *numberE* E and *numberL* L, so, in total, there are *numberS * numberE * numberL* sub problems to solve. With the help of memorization, each sub problem needs only computing once with constant number of operations. Hence the time complexity is **O(numberS * numberE * numberL)**.

+ Space

3d arrays are used to store the computed results of sub problems. Hence, space complexity is **O(numberS * numberE * numberL)**
