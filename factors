#!/usr/bin/env python3

import sys
import math

def quadratic_sieve(n):
    if n % 2 == 0:
        return 2

    limit = int(math.sqrt(n))
    sieve = [0] * (limit + 1)
    primes = []

    for i in range(3, limit + 1, 2):
        if sieve[i] == 0:
            primes.append(i)
            for j in range(i * i, limit + 1, i * 2):
                sieve[j] = 1

    for p in primes:
        q = p
        while q <= limit * limit // p:
            r = n
            for i in range(q, limit + 1, p):
                r -= r // (i * i)
            q *= p
            if r % p == 0:
                return p

    return n

def factor(n):
    if n < 2:
        return []

    factors = []
    while n % 2 == 0:
        factors.append(2)
        n //= 2

    p = 3
    while p <= math.isqrt(n):
        if n % p == 0:
            factors.append(p)
            n //= p
        else:
            p += 2

    if n > 1:
        q = quadratic_sieve(n)
        if q < n:
            factors.extend(factor(q))
            factors.extend(factor(n // q))
        else:
            factors.append(n)

    return factors

if __name__ == "__main__":
    if len(sys.argv) != 2:
        print(f"Usage: {sys.argv[0]} <file>")
        sys.exit(1)

    try:
        with open(sys.argv[1], "r") as file:
            numbers = [int(line.strip()) for line in file.readlines() if line.strip()]
    except (FileNotFoundError, ValueError):
        print(f"Error: Invalid file or contents in {sys.argv[1]}")
        sys.exit(1)

    for n in numbers:
        factors = factor(n)
        p = q = 1
        for f in factors:
            if p == 1:
                p = f
            else:
                q *= f
        print(f"{n}={p}*{q}")