# Warings
Waring's solutions to \$n = x^2 + y^2 + z^3 + w^3$

## Why
It is a well know result proven by <a href="https://en.wikipedia.org/wiki/Lagrange%27s_four-square_theorem">Lagrange</a> in 1770 that \$n = x^2 + y^2 + z^2 + w^2$ always has integer solutions for all positive n.
It is also true that \$n = x^2 + y^2 + z^2 + w^3$ also always has integer solutions.
But what about \$n = x^2 + y^2 + z^3 + w^3$ ?
If the cubes are positive then there is a conjecture that they can be any natural number except 23. I have checked this conjecture up to a trillion and not found any counter examples.

Professor Trevor Wooley of Bristol university replied to my email on the subject on 11th April 2011:-

>“Your conjecture seems quite plausible, although we do not know how to prove it at this point! It is known that all large enough integers are the sum of two squares and three cubes of natural numbers ( see Hooley, Christopher on Waring’s problem for two squares and three cubes. J. Reine Angew. Math. 328 (1981), 161–207. ) for more on this.

>“One can also prove, subject to the proof of the Generalised Riemann hypothesis, that all large enough integers are the sum of two squares, two cubes and 2 sixth powers of natural numbers.( that’s an unpublished result of mine with Elena Golubeva). But two squares and two cubes is just too far beyond our current methods. The best approach we have is to replace the two cubes by something of the shape \$(h+z)^3+(h−z)^3$  , which has a \$6hz^2$ term, so that the problem becomes ternary quadratic form problem with a large coefficient. But we know too little about the associated cusp form coefficients to prove anything from here.

>“So ….your conjecture is likely true, but a proof will need ideas beyond what are currently available.”

The attached Python program attempts to give a lower limit on n up to which this conjecture is true.

## How to run
1. Download the file SumOf2SquaresAnd2Cubesv3.py into a windows file.
2. Create a parameter file in notepad with two rows:-
   
   2.1. First square root of n to be calculated.
   
   2.2. Last square root of n to be calculated.
   
3. In command prompt
   py SumOf2SquaresAnd2Cubesv3.py parameterfile.txt

There is a jit compiler called [pypy](https://www.pypy.org/download.html)  which on my machine runs 7 times faster than standard windows Python.

## How does it work

The program splits into three main parts, the first two set up arrays which are used to optimise the third part which loops through values of n and finds candidate values of x,y,z,and w.

### Calculate all values which can not be written as \$y^2 + z^3 +w^3$
See [OEIS A022557](https://oeis.org/A022557)

### Calculate all possible mod 63 values for which \$m = z^3 + w^3$

### Loop through vaLues of n finding values of x,y,z,w such that \$n = x^2 + y^2 + z^3 + w^3$
This section has 5 nested loops:-
1. Loop through \$floor(\sqrt{n})$
2. Loop through n
3. Loop through x ( Highest to lowest )
4. Loop though y ( Highest to lowest )
5. Loop through z ( lowest to highest ). Only values of z mod 63 are used for which \$n - x^2 - y^2 = z^3 + w^3$ is possible.

## Proposed improvements

The current code does not use the symmetry of the equation - the fact that without loss of generality if \$n = x^2 + y^2 + z^3 + w^3$ then \$n = x^2 + y^2 + w^3 + z^3$ so you only need to check z >= w . 

This slows down finding candidates as for example if \$1000 = n - x^2 - y^2$ then you only need to check z = 8 , 9 , 10 and checking z = 0, .. , 7 is unnecessary.
Looping through is complicated becasue we are only checking certain values of z mod 63 depending on n,x, and y.

Lets call this cubic sum residual r so we can say \$r = n - x^2 - y^2$

We need to quickly calculate the values of m and i which give us candidate values for z:-

\$z = 63 m + f(r,i)$

Maximum of z cubed is r and minimum of z cubed is r/2.

\$\sqrt[3]{r} \ge z \ge \sqrt[3]{\frac{r}{2}} $

\$\sqrt[3]{r} \ge 63 m + f(r,i) \ge \sqrt[3]{\frac{r}{2}} $

The maximum and minimum values of m are quick to work out but maximum and minimum values of i rely on an inverse function of f to be calculated in advance.

## Results
   
| Start Date(UTC)  | End Date(UTC) | Maximum n | Posted by | System | Software |
| ------------- | ------------- | ------------- | ------------- | ------------- | ------------- |
| 2024-03-26 19:32  | 2024-03-29 10:00  | 1,226,540,484 | Mark Pinsker | i7-6700 CPU@3.40GHz | Python pypy3.10-v7.3.15-win64 |

