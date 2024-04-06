# RSA Factoring Challenge

**AlgorithmScripting by Julien Barbier**

This project aims to tackle the RSA Factoring Challenge, a fascinating endeavor in algorithmic problem-solving. The challenge involves factorizing numbers into products of two smaller numbers, with a twist in the advanced version where the factors must be prime.

## Project Details

* **Weight:** 1
* **Project Start:** Nov 13, 2023, 6:00 AM
* **Project End:** Nov 27, 2023, 6:00 AM
* **Checker Release:** Nov 16, 2023, 6:00 PM
* **Auto Review:** Will be launched at the deadline

## Tasks

### Task 0: Factorize all the things! #advanced

* Factorize as many numbers as possible into a product of two smaller numbers: the task requires you to write a python script called `factors` that reads numbers from a file and factors them into a product of two smaller numbers. The script should follow certain requirements, such as reading numbers from a file, outputting the factorization in the format `n=p*q`, handling numbers greater than 1, running without any dependencies, and completing within a time limit of 5 seconds.

```python
#!/usr/bin/python3
"""
Let's start by importing the necessary modules: `sys` for command-line
argument and `sqrt` from `math` for square root calculations
"""

from sys import argv
from math import sqrt


# We the define a function called `factor` that will perform the factorization
def factor():
    """
    We are now going to use the `with` statement to open the
    file specified in the command-line arguments (`argv[1]`)
    and loop over each line in the file
    """
    with open(argv[1]) as f:
        for line in f:
            """
            Now, let's convert the line to an integer and assign
            it to the variable `num`
            """
            num = int(line)
            """
            We are now going to print the number followed by an
            equal sign (=) using the `print` function and the end
            parameter set to an empty string to prevent a newline
            """
            print("{:d}=".format(num), end="")
            """
            Next, we are going to check if the number is divisible by 2
            by using the modulo operator (%). If it is, print the factorization
            as `num/2` and continue to the next iteration of the loop.
            REASON: to avoid unnecessary calculations for numbers that are
            divisible by 2.
            """
            if num % 2 == 0:
                print("{}*2".format(num//2))
                continue
            """
            We are now going to calculate square root of the number and assign
            it to the variable `sqn`. Convert it to an integer using `int` function
            REASON: to ensure we find the maximum possible factor of the number
            """
            sqn = int(sqrt(num))
            """
            Next, we check if the square root is divisible by 2. If it is, we increment
            it by 1 to ensure it is an odd number for subsequent calculations
            """
            if sqn % 2 == 0:
                sqn += 1
                """
                We would now use a for loop to iterate from 3 to the square root
                (inclusive) in a steps of 2 in order to check for divisors of the
                number being evaluated.
                REASON: By incrementing the loop variable by 2 in each iteration,
                we only consider odd numbers as potential divisors, which helps
                optimize the process of finding prime numbers.
                """
            for i in range(3, sqn + 1, 2):
                """
                We are now going to check if the number is divisible by the current
                value of i. If it is, we print the factorization as i * (num//i) and
                break out of the loop.
                REASON: to avoid further unnnecessry iterations.
                """
                if num % i == 0:
                    print("{}*{}".format(i, num//i))
                    break
            """
            After the loop, check if the number is not divisible by any value
            of i. If it is not, print the factorization as num = num * 1
            REASON: We print the factoriztion as num = num * 1 to indicate
            that the number is a prime number.
            """
            if num % i != 0:
                print("{}={}*1".format(num, num))


"""
Outside the loop, we call the `factor` function to start the
factoriztion process and execute the code within the function,
which performs the actual factorization of the given number.
"""
factor()
```

**Usage:**
bash
factors <file>
    <file> is a file containing natural numbers to factor.
    One number per line.
    All lines will be valid natural numbers greater than 1.
    No empty lines or spaces before/after the valid number.
    The file will always end with a new line.

Output format: n=p*q
    One factorization per line.
    p and q don’t have to be prime numbers.

Additional Guidelines:

    Work on the numbers in the file in the order of your choice.
    Your program should run without any dependency.
    Time limit: 5 seconds.

Example:

```bash
$ cat tests/test00
4
12
34
...
$ time ./factors tests/test00
4=2*2
12=6*2
34=17*2
...
real    0m0.009s
user    0m0.008s
sys     0m0.001s
```

### Task 1: RSA Factoring Challenge #advanced

RSA Laboratories states that for each RSA number n, there exist prime numbers p and q such that n = p × q. The task is to find these two primes, given only n.

This task is an extension of Task 0 with the following differences:

    p and q are always prime numbers.
    There is only one number in the files.

How far can you go in less than 5 seconds?

Explanation of the code is as follows:

```python
#!/usr/bin/python3

# We import the necessary modules
from fractions import gcd
from sys import argv

"""
The fraction module provides support for rational numbers, which are used to perform the gcd calculation. The sys module is used to access command-line argument.
Next, define a function to perform the Pollard's Rho algorithm. Pollard's Rho algorithm is a powerful tool for factoring large composite numbers, especially when dealing with numbers that have small prime factors. It is widely used in practice and forms an integral part of many modern factorization algorithms.
"""
def pollards_rho(n):
    x = 2
    y = 2
    d = 1
    f = lambda x: (x**2 + 1) % n
    while d == 1:
        x = f(x)
        y = f(f(y))
        d = gcd(abs(x - y), n)
    if d != n:
        return d
"""
The Pollard's Rho algorithm for finding a factor of a number. It works repeatedly applying the function f(x) to a starting value x until a cycle is detected. The cycle then contains a factor of n, which is returned.

In the code above, the pollards_rho function takes a number n as input and returns a factor of n if one exists. It initializes the variables x, y, and d to 2, 2, and 1, respectively. It then defines the function f to be the sucessor function of the quadratic polynomial x^2 + 1(mode n). Finally, it enters a loop that repeatedly applies f to x and y, and calculates the gcd of the absolute difference between x and y and n. If the gcd is not 1, it means that a factor of n has been found, and the function returns it.
"""

# Define a function to read the input file and perform the RSA factorization
def read_file():
    with open(argv[1]) as f:
        for number in f:
            num = int(number)
            if num % 2 == 0:
                print("{:d}={}*{}".format(num, num // 2, 2))
            else:
                p = pollards_rho(num)
                f1 = p
                f2 = int(num / p)
                print("{}={}*{}".format(num, f1, f2))
"""
The read_file function takes no input and reads the input file specified by the command-line argument. It then iterates over each number in the file and performs the RSA factorization using the pollards_rho function. If the number is even, it is factored as 2*k, where k is an integer. Otherwise, it is factored as p * q, wher p and q are prime numbers. The results are printed to the console.
"""

# Check if the script is being run as the main program
if __name__ == "__main__":
    read_file()

"""
This code is used to ensure that the read_file function is called if the script is being run as the main program. This is necessary because the read_file function reads the command-line argument, which is only available when the script is run as the main program
"""
```

Example:

```bash
$ cat tests/rsa-1
6
$ ./rsa tests/rsa-1
6=3*2
...
$ cat tests/rsa-16
2497885147362973
$ ./rsa tests/rsa-16
2497885147362973=49979141*49978553
```

## GitHub Repository

* Repository Name: RSA-Factoring-Challenge
* Files: factors, rsa
