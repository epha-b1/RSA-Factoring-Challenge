#!/usr/bin/python3
import math
import sys

def factorize(n):
    '''
    factorize a natural number - n into a product of two numbers.
    '''
    for i in range(2, int(math.sqrt(n))+1):
        if n % i == 0:
            return f'{n}={i}*{n//i}'
    return f'{n}={n}*1'

if __name__ == '__main__':
    if len(sys.argv) != 2:
        print(f'Usage: {sys.argv[0]} <file>')
        sys.exit(1)
    filename = sys.argv[1]
    with open(filename) as f:
        for line in f:
            n = int(line.strip())
            print(factorize(n))
