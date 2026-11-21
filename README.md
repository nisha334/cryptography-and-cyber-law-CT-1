
# Number Theory, Cryptography & Prime Number

**Name:** Fatema Islam Nisha 
**Student ID:** IT22033

---

## Table of Contents

1. Introduction to Number Theory
2. Divisibility, Factors and Multiples
3. Greatest Common Divisor (GCD)
4. Least Common Multiple (LCM)
5. Diophantine Equations
6. Modular Arithmetic and Remainders
7. Modular Multiplicative Inverse and Division
8. Chinese Remainder Theorem (CRT)
9. Fast Modular Exponentiation
10. Fermat's Little Theorem
11. Euler's Totient Function
12. Euler's Theorem
13. Cryptography and RSA
14. Prime Numbers
15. Primality Testing
16. Further Properties of Prime Numbers
17. Divisor Functions and Antiprimes
18. Fundamental Theorem of Arithmetic
19. Python Practical Implementations
20. Practice Problems
21. References and Conclusion

---

# 1. Introduction to Number Theory

Number Theory is a branch of mathematics that studies integers and their mathematical properties.

It focuses on divisibility, prime numbers, factors, multiples, remainders, modular arithmetic, and integer equations.

Number Theory plays an important role in computer science, algorithm design, and modern cryptography.

## Main Types of Numbers


**Key Learning:** Understanding the properties of integers is the foundation of Number Theory and Cryptography.

---

# 2. Divisibility, Factors and Multiples

Divisibility describes whether one integer can divide another integer without leaving a remainder.

For integers a and b, where a is not equal to zero, we say that a divides b if there exists an integer k such that:

$$
b=ak
$$

The notation is:

$$
a\mid b
$$

If a does not divide b, we write:

$$
a\nmid b
$$

## Important Concepts

- **Factor:** A number that divides another number exactly.
- **Multiple:** A number obtained by multiplying an integer by another number.
- **Divisor:** Another name for a factor.

### Example

$$
4\mid20
$$

Because:

$$
20=4\times5
$$

However:

$$
6\nmid20
$$

Because 20 is not divisible by 6 without a remainder.

## Python Implementation

```python
a = 4
b = 20

if b % a == 0:
    print(a, "divides", b)
else:
    print(a, "does not divide", b)
```

### Output

```text
4 divides 20
```

**Explanation:** The modulo operator (%) calculates the remainder. If the remainder is zero, the number is divisible.

**Key Learning:** Divisibility is one of the most fundamental concepts used in GCD, prime factorization, and modular arithmetic.

---

# 3. Greatest Common Divisor (GCD)

The Greatest Common Divisor (GCD) of two integers is the largest positive integer that divides both numbers exactly.

It is represented by:

$$
\gcd(a,b)
$$

## Examples

$$
\gcd(24,16)=8
$$

$$
\gcd(9,17)=1
$$

$$
\gcd(239,0)=239
$$

Two integers are called coprime or relatively prime if their GCD is 1.

## 3.1 Naive GCD Algorithm

The Naive GCD Algorithm finds the greatest common divisor by checking possible divisors one by one, starting from the smaller input.

### Python Code

```python
def naive_gcd(a, b):
    a = abs(a)
    b = abs(b)

    if a == 0 and b == 0:
        raise ValueError("GCD is undefined")

    for i in range(min(a, b), 0, -1):
        if a % i == 0 and b % i == 0:
            return i

print("GCD:", naive_gcd(24, 16))
```

### Output

```text
GCD: 8
```

**Explanation:** The algorithm checks possible divisors from the smaller number downwards. The first common divisor is the GCD.

## 3.2 Euclid's Lemma and Subtraction Algorithm

Euclid's Lemma provides an important mathematical property of divisibility.

If an integer divides two numbers, it also divides their difference.

Mathematical identity:

$$
\gcd(a,b)=\gcd(a-b,b)
$$

where a is greater than or equal to b.

### Example

$$
\gcd(10,6)
$$

Steps:

$$
10-6=4
$$

$$
6-4=2
$$

$$
4-2=2
$$

Therefore:

$$
\gcd(10,6)=2
$$

### Python Code

```python
def subtraction_gcd(a, b):
    a = abs(a)
    b = abs(b)

    if a == 0 and b == 0:
        raise ValueError("GCD is undefined")

    while a != 0 and b != 0:
        if a > b:
            a -= b
        else:
            b -= a

    return a if a else b

print("GCD:", subtraction_gcd(10, 6))
```

### Output

```text
GCD: 2
```

**Key Learning:** Repeated subtraction provides the mathematical foundation for Euclid's more efficient remainder-based algorithm.

## 3.3 Euclid's Algorithm

Euclid's Algorithm is an efficient method for finding the GCD of two integers using repeated division and remainders.

Mathematical identity:

$$
\gcd(a,b)=\gcd(b,a\bmod b)
$$

The process continues until the remainder becomes zero. The last non-zero remainder is the GCD.

### Example

$$
24=1\times16+8
$$

$$
16=2\times8+0
$$

Therefore:

$$
\gcd(24,16)=8
$$

### Python Code

```python
def euclidean_gcd(a, b):
    a = abs(a)
    b = abs(b)

    while b != 0:
        a, b = b, a % b

    return a

print("GCD:", euclidean_gcd(252, 105))
```

### Output

```text
GCD: 21
```

**Time Complexity:**

$$
O(\log(\min(a,b)))
$$

**Key Learning:** Euclid's Algorithm is widely used in modular arithmetic, cryptography, and integer computations.

## 3.4 Extended Euclidean Algorithm and Bézout's Identity

The Extended Euclidean Algorithm is an extension of Euclid's Algorithm.

It calculates the GCD and also finds two integers x and y satisfying:

$$
ax+by=\gcd(a,b)
$$

This equation is known as Bézout's Identity.

### Example

$$
\gcd(10,6)=2
$$

One possible representation is:

$$
10(-1)+6(2)=2
$$

Therefore:

$$
x=-1,\quad y=2
$$

### Python Code

```python
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0

    g, x1, y1 = extended_gcd(b, a % b)

    x = y1
    y = x1 - (a // b) * y1

    return g, x, y

g, x, y = extended_gcd(10, 6)

print("GCD:", g)
print("x:", x)
print("y:", y)
print("Verification:", 10*x + 6*y)
```

### Output

```text
GCD: 2
x: -1
y: 2
Verification: 2
```

**Explanation:** The algorithm finds coefficients that satisfy Bézout's Identity.

**Key Learning:** The Extended Euclidean Algorithm is especially important for finding modular inverses and solving Diophantine equations.

---
# 4. Least Common Multiple (LCM)

The Least Common Multiple (LCM) is the smallest positive integer that is divisible by two given positive integers.
## Python Implementation

```python
import math

def calculate_lcm(a, b):
    if a == 0 or b == 0:
        return 0

    return abs(a * b) // math.gcd(a, b)

print("LCM:", calculate_lcm(24, 16))
print("LCM:", calculate_lcm(18, 30))
```

### Output

```text
LCM: 48
LCM: 90
```

**Explanation:** The program uses the relationship between GCD and LCM to calculate the smallest common multiple.

**Key Learning:** GCD and LCM are closely related and are useful in divisibility problems, fractions, and number-theoretic computations.

---

# 5. Diophantine Equations

A Diophantine Equation is an algebraic equation where the solutions are required to be integers.

A common linear Diophantine equation has the form:

$$
ax+by=c
$$

Here, a, b, and c are known integers, while x and y are unknown integer variables.

## Example

Consider:

$$
187x+55y=121
$$

One solution is:

$$
x=3,\quad y=-8
$$

Verification:

$$
187(3)+55(-8)=121
$$

$$
561-440=121
$$

Therefore, the solution is correct.

## 5.1 Existence of Integer Solutions

The linear Diophantine equation:

$$
ax+by=c
$$

has an integer solution if and only if:

$$
\gcd(a,b)\mid c
$$

This means that the GCD of the coefficients must divide the constant term.

If this condition is not satisfied, there is no integer solution.

### Python Code

```python
import math

def has_integer_solution(a, b, c):
    if a == 0 and b == 0:
        return c == 0

    return c % math.gcd(abs(a), abs(b)) == 0

print(has_integer_solution(187, 55, 121))
print(has_integer_solution(12, 18, 7))
```

### Output

```text
True
False
```

**Explanation:**

- The equation 187x + 55y = 121 has an integer solution.
- The equation 12x + 18y = 7 has no integer solution because gcd(12,18) = 6 and 6 does not divide 7.

## 5.2 Finding All Solutions

If one solution (x₀, y₀) exists and:

$$
d=\gcd(a,b)
$$

then all integer solutions can be expressed as:

$$
x=x_0+\frac{b}{d}t
$$

$$
y=y_0-\frac{a}{d}t
$$

where:

$$
t\in\mathbb{Z}
$$

### Example

Consider:

$$
3x+5y=22
$$

One solution is:

$$
x=9,\quad y=-1
$$

Therefore, all integer solutions are:

$$
x=9+5t
$$

$$
y=-1-3t
$$

### Python Code

```python
def all_solutions(t):
    x = 9 + 5 * t
    y = -1 - 3 * t
    return x, y

for t in range(-2, 3):
    x, y = all_solutions(t)
    print("t =", t, "x =", x, "y =", y)
    print("Verification:", 3*x + 5*y)
```

### Output

```text
t = -2 x = -1 y = 5
Verification: 22

t = -1 x = 4 y = 2
Verification: 22

t = 0 x = 9 y = -1
Verification: 22

t = 1 x = 14 y = -4
Verification: 22

t = 2 x = 19 y = -7
Verification: 22
```

**Key Learning:** A single solution can be extended to an infinite family of integer solutions.

---

# 6. Modular Arithmetic and Remainders

Modular Arithmetic is a mathematical system that deals with remainders after division by a positive integer.

The remainder of a divided by n is written as:

$$
a\bmod n
$$

Two integers a and b are congruent modulo n if they have the same remainder.

The notation is:

$$
a\equiv b\pmod n
$$

This is equivalent to:

$$
n\mid(a-b)
$$

## Example

$$
17\equiv5\pmod{12}
$$

Because:

$$
17\bmod12=5
$$

## Python Implementation

```python
a = 17
n = 12

remainder = a % n

print("Remainder:", remainder)

if a % n == 5:
    print("17 is congruent to 5 modulo 12")
```

### Output

```text
Remainder: 5
17 is congruent to 5 modulo 12
```

## 6.1 Congruence Properties

If:

$$
a\equiv b\pmod n
$$

and:

$$
c\equiv d\pmod n
$$

then addition and multiplication preserve congruence:

### Addition

$$
a+c\equiv b+d\pmod n
$$

### Multiplication

$$
ac\equiv bd\pmod n
$$

### Powers

For non-negative integer k:

$$
a^k\equiv b^k\pmod n
$$

These properties allow large calculations to be simplified by reducing numbers modulo n.

### Python Code

```python
a = 17
b = 5
n = 12

print("Addition:")
print((a + 7) % n)
print((b + 7) % n)

print("\nMultiplication:")
print((a * 3) % n)
print((b * 3) % n)

print("\nPower:")
print(pow(a, 3, n))
print(pow(b, 3, n))
```

### Output

```text
Addition:
0
0

Multiplication:
3
3

Power:
5
5
```

**Explanation:** Since 17 and 5 are congruent modulo 12, applying the same addition, multiplication, or non-negative power preserves their congruence.

## 6.2 Divisibility Criteria

Divisibility criteria are simple mathematical rules used to determine whether an integer is divisible by another integer without performing complete division.

Common decimal divisibility rules include:

- **Divisible by 2:** The last digit is even.
- **Divisible by 3:** The sum of the digits is divisible by 3.
- **Divisible by 5:** The last digit is 0 or 5.
- **Divisible by 9:** The sum of the digits is divisible by 9.
- **Divisible by 10:** The last digit is 0.

These rules are connected to modular arithmetic.

For example:

$$
10\equiv1\pmod9
$$

Therefore, a number has the same remainder modulo 9 as the sum of its decimal digits.

### Python Code

```python
def divisibility_check(n):
    print("Number:", n)

    print("Divisible by 2:", n % 2 == 0)
    print("Divisible by 3:", n % 3 == 0)
    print("Divisible by 5:", n % 5 == 0)
    print("Divisible by 9:", n % 9 == 0)
    print("Divisible by 10:", n % 10 == 0)

divisibility_check(90)
```

### Output

```text
Number: 90
Divisible by 2: True
Divisible by 3: True
Divisible by 5: True
Divisible by 9: True
Divisible by 10: True
```

**Key Learning:** Modular arithmetic explains many familiar divisibility rules and is essential for cryptographic algorithms.

---

# 7. Modular Multiplicative Inverse and Division

The Modular Multiplicative Inverse of an integer a modulo n is an integer x satisfying:

$$
ax\equiv1\pmod n
$$

The inverse exists if and only if:

$$
\gcd(a,n)=1
$$

The Extended Euclidean Algorithm is commonly used to calculate modular inverses.

## Example

Find the inverse of 2 modulo 9.

$$
2\times5=10
$$

Since:

$$
10\equiv1\pmod9
$$

Therefore:

$$
2^{-1}\equiv5\pmod9
$$

## Python Implementation

```python
def modular_inverse(a, n):
    try:
        return pow(a, -1, n)
    except ValueError:
        return None

a = 2
n = 9

inverse = modular_inverse(a, n)

print("Modular Inverse:", inverse)

if inverse is not None:
    print("Verification:", (a * inverse) % n)
```

### Output

```text
Modular Inverse: 5
Verification: 1
```

**Explanation:** Python's pow(a, -1, n) calculates the modular inverse when it exists.

## Modular Division

Modular division is performed by multiplying with the modular inverse.

If the inverse of a modulo n exists:

$$
\frac{b}{a}\pmod n
\equiv ba^{-1}\pmod n
$$

### Example

Calculate 6 divided by 2 modulo 9.

The inverse of 2 modulo 9 is 5.

Therefore:

$$
6\times5=30
$$

$$
30\bmod9=3
$$

### Python Code

```python
a = 2
b = 6
n = 9

inverse = pow(a, -1, n)
result = (b * inverse) % n

print("Modular Division Result:", result)
```

### Output

```text
Modular Division Result: 3
```

**Key Learning:** Modular inverses are essential in solving congruences and implementing public-key cryptography.

---

# 8. Chinese Remainder Theorem (CRT)

The Chinese Remainder Theorem (CRT) is an important result in Number Theory that helps solve systems of simultaneous congruences.

If two moduli m and n are coprime, then the system:

$$
x\equiv a\pmod m
$$

$$
x\equiv b\pmod n
$$

has a unique solution modulo mn.

## Example

Solve the following congruences:

$$
x\equiv2\pmod3
$$

$$
x\equiv3\pmod5
$$

The solution is:

$$
x\equiv8\pmod{15}
$$

Because:

$$
8\bmod3=2
$$

$$
8\bmod5=3
$$

Therefore:

$$
\boxed{x\equiv8\pmod{15}}
$$

## Python Implementation

```python
from sympy.ntheory.modular import crt

moduli = [3, 5]
remainders = [2, 3]

result, modulus = crt(moduli, remainders)

print("Solution:", result)
print("Combined Modulus:", modulus)

for m, r in zip(moduli, remainders):
    print(f"Verification: {result} mod {m} =", int(result) % m)
```

### Output

```text
Solution: 8
Combined Modulus: 15
Verification: 8 mod 3 = 2
Verification: 8 mod 5 = 3
```

## CRT Using a Manual Algorithm

The Chinese Remainder Theorem can also be implemented using modular inverses.

```python
def chinese_remainder(remainders, moduli):
    N = 1

    for m in moduli:
        N *= m

    result = 0

    for a, m in zip(remainders, moduli):
        Ni = N // m
        inverse = pow(Ni, -1, m)
        result += a * Ni * inverse

    return result % N

remainders = [2, 3]
moduli = [3, 5]

print("CRT Solution:", chinese_remainder(remainders, moduli))
```

### Output

```text
CRT Solution: 8
```

**Key Learning:** CRT is useful for simplifying large modular calculations and is applied in cryptographic systems.

---

# 9. Fast Modular Exponentiation

Fast Modular Exponentiation is an efficient algorithm for calculating large powers modulo an integer.

The mathematical operation is:

$$
a^b\bmod n
$$

Directly calculating a^b may produce extremely large numbers.

Repeated squaring avoids this problem by reducing intermediate results modulo n.

The main mathematical properties are:

$$
a^{2k}=(a^k)^2
$$

$$
a^{2k+1}=a(a^k)^2
$$

This reduces the number of required multiplication steps.

## Example

Calculate:

$$
3^{13}\bmod7
$$

We can calculate:

$$
3^{13}=1594323
$$

Therefore:

$$
3^{13}\bmod7=3
$$

## Python Implementation Using pow()

Python provides a built-in function for fast modular exponentiation.

```python
a = 3
b = 13
n = 7

result = pow(a, b, n)

print("Result:", result)
```

### Output

```text
Result: 3
```

## Manual Fast Modular Exponentiation Algorithm

```python
def fast_mod_exp(base, exponent, modulus):
    if modulus <= 0 or exponent < 0:
        raise ValueError("Invalid input")

    result = 1
    base = base % modulus

    while exponent > 0:
        if exponent % 2 == 1:
            result = (result * base) % modulus

        base = (base * base) % modulus
        exponent //= 2

    return result

print("Result:", fast_mod_exp(3, 13, 7))
print("Result:", fast_mod_exp(2, 100, 13))
```

### Output

```text
Result: 3
Result: 3
```

**Time Complexity:**

$$
O(\log b)
$$

**Key Learning:** Fast modular exponentiation is an essential operation in RSA encryption, primality testing, and other cryptographic algorithms.

---

# 10. Fermat's Little Theorem

Fermat's Little Theorem is an important theorem in Number Theory involving prime numbers and modular arithmetic.

If p is a prime number and p does not divide a, then:

$$
a^{p-1}\equiv1\pmod p
$$

An equivalent form is:

$$
a^p\equiv a\pmod p
$$

for every integer a.

## Example

Let:

$$
a=3,\quad p=7
$$

Then:

$$
3^{6}\equiv1\pmod7
$$

Because:

$$
729\bmod7=1
$$

Therefore, Fermat's Little Theorem is verified.

## Python Implementation

```python
a = 3
p = 7

result = pow(a, p - 1, p)

print("Result:", result)

if result == 1:
    print("Fermat's Little Theorem Verified")
else:
    print("Condition Failed")
```

### Output

```text
Result: 1
Fermat's Little Theorem Verified
```

## Important Note

Passing a Fermat primality test does not guarantee that a number is prime because some composite numbers can also satisfy the congruence.

For example, 341 is a pseudoprime to base 2.

**Key Learning:** Fermat's Little Theorem is used in modular arithmetic, cryptography, and Fermat-based primality testing.

---

# 11. Euler's Totient Function

Euler's Totient Function counts the number of positive integers from 1 to n that are relatively prime to n.

It is represented by:

$$
\phi(n)
$$

For a prime number p:

$$
\phi(p)=p-1
$$

For two distinct prime numbers p and q:

$$
\phi(pq)=(p-1)(q-1)
$$

The general formula is:

$$
\phi(n)=n\prod_{p\mid n}
\left(1-\frac{1}{p}\right)
$$

where the product is taken over the distinct prime divisors of n.

## Example

For:

$$
n=10
$$

The integers relatively prime to 10 are:

$$
1,3,7,9
$$

Therefore:

$$
\phi(10)=4
$$

## Python Implementation

```python
from sympy import totient

numbers = [10, 12, 15, 36]

for n in numbers:
    print(f"phi({n}) =", totient(n))
```

### Output

```text
phi(10) = 4
phi(12) = 4
phi(15) = 8
phi(36) = 12
```

## Manual Euler Totient Implementation

```python
def euler_totient(n):
    if n < 1:
        raise ValueError("n must be positive")

    count = 0

    for i in range(1, n + 1):
        if math.gcd(i, n) == 1:
            count += 1

    return count

import math

print("phi(10) =", euler_totient(10))
print("phi(36) =", euler_totient(36))
```

### Output

```text
phi(10) = 4
phi(36) = 12
```

**Explanation:** The program counts all positive integers up to n that have GCD 1 with n.

**Key Learning:** Euler's Totient Function plays an important role in RSA and Euler's theorem.

---

# 12. Euler's Theorem

Euler's Theorem is a generalization of Fermat's Little Theorem.

If two integers a and n are coprime, then:

$$
a^{\phi(n)}\equiv1\pmod n
$$

The condition is:

$$
\gcd(a,n)=1
$$

For a prime number p:

$$
\phi(p)=p-1
$$

Therefore, Euler's theorem becomes Fermat's Little Theorem.

## Example

Let:

$$
a=3,\quad n=10
$$

We know:

$$
\phi(10)=4
$$

According to Euler's theorem:

$$
3^4\equiv1\pmod{10}
$$

Because:

$$
81\bmod10=1
$$

Therefore, Euler's theorem is verified.

## Python Implementation

```python
import math

a = 3
n = 10

phi_n = 4

if math.gcd(a, n) == 1:
    result = pow(a, phi_n, n)

    print("Euler Totient:", phi_n)
    print("Result:", result)

    if result == 1:
        print("Euler's Theorem Verified")
else:
    print("a and n are not coprime")
```

### Output

```text
Euler Totient: 4
Result: 1
Euler's Theorem Verified
```

**Key Learning:** Euler's theorem provides the mathematical foundation for important operations in RSA public-key cryptography.

---

# 13. Cryptography and Public-Key Idea

Cryptography is the science of protecting information using mathematical techniques and algorithms.

It helps maintain the confidentiality, integrity, and authenticity of digital information.

## Main Concepts of Cryptography

| Concept | Description |
|---|---|
| Plaintext | Original readable message |
| Ciphertext | Encrypted message |
| Encryption | Converting plaintext into ciphertext |
| Decryption | Recovering the original message |
| Public Key | A key that can be shared publicly |
| Private Key | A secret key kept by its owner |

## Types of Cryptography

### 1. Symmetric-Key Cryptography

The same secret key is used for both encryption and decryption.

### 2. Asymmetric-Key Cryptography

Two mathematically related keys are used: a public key and a private key.

## RSA Cryptography

RSA is a popular public-key cryptographic algorithm based on the mathematical properties of prime numbers.

Its basic mathematical process includes:

1. Selecting two large prime numbers.
2. Calculating their product.
3. Computing Euler's Totient Function.
4. Selecting a public exponent.
5. Calculating the private exponent using a modular inverse.
6. Performing encryption and decryption using modular exponentiation.

### RSA Encryption

$$
C=M^e\bmod n
$$

### RSA Decryption

$$
M=C^d\bmod n
$$

Here:

- M = Original message
- C = Ciphertext
- e = Public exponent
- d = Private exponent
- n = Product of two prime numbers

## Educational RSA Example

For educational purposes, consider two small prime numbers:

$$
p=3,\quad q=11
$$

Calculate:

$$
n=pq=3\times11=33
$$

Calculate Euler's Totient:

$$
\phi(n)=(3-1)(11-1)
$$

$$
\phi(n)=20
$$

Choose a public exponent:

$$
e=3
$$

Calculate the private exponent d:

$$
ed\equiv1\pmod{20}
$$

$$
3d\equiv1\pmod{20}
$$

Therefore:

$$
d=7
$$

The public key is:

$$
(e,n)=(3,33)
$$

The private key is:

$$
(d,n)=(7,33)
$$

### RSA Python Implementation

```python
p = 3
q = 11

n = p * q
phi = (p - 1) * (q - 1)

e = 3
d = pow(e, -1, phi)

message = 4

ciphertext = pow(message, e, n)

decrypted = pow(ciphertext, d, n)

print("Public Key:", (e, n))
print("Private Key:", (d, n))
print("Original Message:", message)
print("Encrypted Message:", ciphertext)
print("Decrypted Message:", decrypted)
```

### Output

```text
Public Key: (3, 33)
Private Key: (7, 33)
Original Message: 4
Encrypted Message: 31
Decrypted Message: 4
```

**Explanation:**

- The original message is 4.
- Encryption produces ciphertext 31.
- Decryption recovers the original message 4.

**Important Security Note:** This RSA example uses very small prime numbers and is only an educational demonstration. It is not suitable for real-world security. Real RSA requires secure key generation, sufficiently large keys, and appropriate padding schemes.

**Key Learning:** Public-key cryptography demonstrates how mathematical concepts from Number Theory can be applied to secure digital communication.

---

# 14. Prime Numbers

A Prime Number is a natural number greater than 1 that has exactly two positive divisors: 1 and itself.

Examples:

$$
2,3,5,7,11,13,17,19
$$

A Composite Number is a natural number greater than 1 that has more than two positive divisors.

The number 1 is neither prime nor composite.

The number 2 is the only even prime number.

## Python Implementation

```python
def is_prime(n):
    if n < 2:
        return False

    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False

    return True

numbers = [2, 3, 4, 5, 9, 11, 15, 17]

for number in numbers:
    if is_prime(number):
        print(number, "is Prime")
    else:
        print(number, "is Composite")
```

### Output

```text
2 is Prime
3 is Prime
4 is Composite
5 is Prime
9 is Composite
11 is Prime
15 is Composite
17 is Prime
```

**Key Learning:** Prime numbers are the fundamental building blocks of positive integers.

---

## 14.1 Prime Distribution and Infinitude

Prime numbers are distributed throughout the positive integers, but their frequency generally decreases as numbers become larger.

Euclid's Theorem establishes that there are infinitely many prime numbers.

### Euclid's Proof

A famous argument assumes that there are finitely many primes and constructs a number by multiplying all known primes and adding 1:

$$
N=p_1p_2p_3\cdots p_k+1
$$

None of the listed primes divides this new number exactly.

Therefore, either N is prime itself or it has a prime factor that is not included in the original list.

Hence, there are infinitely many prime numbers.

## Python Demonstration

```python
import math

primes = [2, 3, 5, 7]

N = math.prod(primes) + 1

print("Known Primes:", primes)
print("Constructed Number:", N)

for p in primes:
    print(f"{N} % {p} =", N % p)
```

### Output

```text
Known Primes: [2, 3, 5, 7]
Constructed Number: 211
211 % 2 = 1
211 % 3 = 1
211 % 5 = 1
211 % 7 = 1
```

**Key Learning:** Euclid's Theorem proves that there are infinitely many prime numbers.

---

## 14.2 Dirichlet's Theorem and Prime Number Theorem

### Dirichlet's Theorem

Dirichlet's Theorem states that if two positive integers a and b are coprime, then the arithmetic progression:

$$
a,\ a+b,\ a+2b,\ldots
$$

contains infinitely many prime numbers.

The condition is:

$$
\gcd(a,b)=1
$$

### Example

Consider the arithmetic progression:

$$
1,5,9,13,17,21,25,29,\ldots
$$

Here:

$$
a=1,\quad b=4
$$

Since:

$$
\gcd(1,4)=1
$$

There are infinitely many primes in this progression.

Some prime numbers are:

$$
5,13,17,29,37,41,53,\ldots
$$

### Python Implementation

```python
def is_prime(n):
    if n < 2:
        return False

    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False

    return True

a = 1
b = 4

primes = []

for k in range(100):
    number = a + k * b

    if is_prime(number):
        primes.append(number)

print("Primes in the progression:")
print(primes[:15])
```

### Output

```text
Primes in the progression:
[5, 13, 17, 29, 37, 41, 53, 61, 73, 89, 97, 101, 109, 113, 137]
```

### Prime Number Theorem

The Prime Number Theorem describes the approximate distribution of prime numbers.

If π(x) represents the number of primes not exceeding x, then:

$$
\pi(x)\sim\frac{x}{\ln x}
$$

This means that the number of primes up to x is approximately:

$$
\frac{x}{\ln x}
$$

### Python Implementation

```python
import sympy as sp
import math

x = 100

actual = int(sp.primepi(x))
approximation = x / math.log(x)

print("Number of Primes:", actual)
print("Approximation:", round(approximation, 2))
```

### Output

```text
Number of Primes: 25
Approximation: 21.71
```

**Key Learning:** Dirichlet's Theorem and the Prime Number Theorem explain important patterns in the distribution of prime numbers.

---

## 14.3 Prime Conjectures

A mathematical conjecture is a statement believed to be true but not yet proven.

Important prime-number conjectures include:

### 1. Goldbach Conjecture

Every even integer greater than 2 can be represented as the sum of two prime numbers.

Example:

$$
50=19+31
$$

### 2. Twin Prime Conjecture

There are infinitely many pairs of prime numbers that differ by 2.

Examples:

$$
(3,5),(5,7),(11,13),(17,19)
$$

### 3. Prime-Generating Questions

Mathematical research investigates which polynomial expressions produce prime values for different integer inputs.

### 4. Prime Distribution Conjectures

Several open problems concern the patterns and gaps between consecutive prime numbers.

### Python Implementation: Goldbach Representation

```python
def is_prime(n):
    if n < 2:
        return False

    for i in range(2, int(n**0.5) + 1):
        if n % i == 0:
            return False

    return True

number = 50

for i in range(2, number):
    j = number - i

    if is_prime(i) and is_prime(j):
        print(number, "=", i, "+", j)
        break
```

### Output

```text
50 = 3 + 47
```

**Key Learning:** Many important questions about prime numbers remain active topics of mathematical research. Computational examples cannot prove conjectures involving infinitely many integers.

---

## 14.4 Generating Primes and Polynomial Examples

Certain polynomial expressions generate prime numbers for several consecutive integer inputs.

A famous example is Euler's polynomial:

$$
f(n)=n^2+n+41
$$

For many small non-negative integer values of n, this polynomial produces prime numbers.

However, it does not always generate primes.

### Example

For n = 40:

$$
f(40)=40^2+40+41
$$

$$
f(40)=1681
$$

Since:

$$
1681=41^2
$$

The result is composite.

### Python Implementation

```python
import sympy as sp

def euler_polynomial(n):
    return n**2 + n + 41

for n in range(10):
    value = euler_polynomial(n)

    print(
        f"f({n}) = {value}, "
        f"Prime = {sp.isprime(value)}"
    )

print("f(40) =", euler_polynomial(40))
print("Is Prime:", sp.isprime(euler_polynomial(40)))
```

### Output

```text
f(0) = 41, Prime = True
f(1) = 43, Prime = True
f(2) = 47, Prime = True
f(3) = 53, Prime = True
f(4) = 61, Prime = True
f(5) = 71, Prime = True
f(6) = 83, Prime = True
f(7) = 97, Prime = True
f(8) = 113, Prime = True
f(9) = 131, Prime = True
f(40) = 1681
Is Prime: False
```

**Key Learning:** Polynomial expressions can reveal interesting patterns in prime numbers, but experimental results do not establish that a polynomial always generates primes.

---

# 15. Primality Testing

Primality Testing is the process of determining whether a given integer is prime or composite.

It is an important area of Number Theory and Cryptography because secure cryptographic systems often require large prime numbers.

Different primality-testing methods have different levels of efficiency and mathematical certainty.

## 15.1 Trial Division

Trial Division is a basic deterministic primality-testing algorithm.

It checks whether a number is divisible by any integer greater than 1 and not exceeding its square root.

If no divisor is found, the number is prime.

The mathematical principle is:

$$
d\leq\sqrt n
$$

There is no need to check divisors greater than the square root because factors occur in pairs.

### Example

Check whether 97 is prime.

$$
\sqrt{97}\approx9.85
$$

We only need to check divisibility by:

$$
2,3,4,5,6,7,8,9
$$

Since none of these numbers divides 97 exactly, 97 is prime.

### Python Implementation

```python id="8usq9z"
def trial_division(n):
    if n < 2:
        return False

    if n == 2:
        return True

    if n % 2 == 0:
        return False

    for d in range(3, int(n**0.5) + 1, 2):
        if n % d == 0:
            return False

    return True

number = 97

if trial_division(number):
    print(number, "is Prime")
else:
    print(number, "is Composite")
```

### Output

```text id="v65slc"
97 is Prime
```

### Time Complexity

$$
O(\sqrt n)
$$

**Explanation:** Trial Division checks possible divisors only up to the square root of the given number.

**Key Learning:** Trial Division is a simple deterministic method for checking whether a number is prime.

---

## 15.2 Sieve of Eratosthenes

The Sieve of Eratosthenes is an efficient algorithm for generating all prime numbers up to a specified limit.

The algorithm works by:

1. Listing all integers from 2 to the given limit.
2. Selecting the smallest unmarked number.
3. Marking its multiples as composite.
4. Repeating the process until the square root of the limit is reached.
5. Collecting all unmarked numbers as primes.

### Example

Generate all prime numbers up to 30.

The prime numbers are:

$$
2,3,5,7,11,13,17,19,23,29
$$

### Python Implementation

```python
def sieve_of_eratosthenes(n):
    is_prime = [True] * (n + 1)

    is_prime[0] = False
    is_prime[1] = False

    for p in range(2, int(n**0.5) + 1):
        if is_prime[p]:
            for multiple in range(p*p, n + 1, p):
                is_prime[multiple] = False

    return [
        number
        for number in range(2, n + 1)
        if is_prime[number]
    ]

limit = 30

primes = sieve_of_eratosthenes(limit)

print("Prime numbers up to", limit)
print(primes)
```

### Output

```text
Prime numbers up to 30
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

### Time Complexity

$$
O(n\log\log n)
$$

### Key Learning

The Sieve of Eratosthenes is particularly useful when many prime numbers are needed within a fixed range.

---

## 15.3 Wheel Factorization

Wheel Factorization is an optimization technique that reduces the number of candidate integers checked during prime generation.

It uses small prime numbers to eliminate numbers that are automatically composite.

For example, using the primes 2, 3, and 5 gives a wheel of 30.

The possible residues are:

$$
1,7,11,13,17,19,23,29
$$

These are the integers between 0 and 29 that are coprime to 30.

Every prime greater than 5 must belong to one of these residue classes.

### Python Implementation

```python
import math

def wheel_candidates(limit):
    residues = [1, 7, 11, 13, 17, 19, 23, 29]
    candidates = []

    for block in range(limit // 30 + 1):
        for r in residues:
            number = block * 30 + r

            if 7 <= number <= limit:
                candidates.append(number)

    return candidates

print(wheel_candidates(50))
```

### Output

```text
[7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 49]
```

**Important Note:** Wheel Factorization eliminates multiples of 2, 3, and 5, but it does not guarantee that every remaining candidate is prime. For example, 49 is composite.

### Key Learning

Wheel Factorization reduces unnecessary divisibility checks and improves trial-based prime generation.

---

## 15.4 Pseudoprimes and Fermat Primality Test

A Pseudoprime is a composite number that satisfies a particular primality-test condition for one or more selected bases.

Fermat's Primality Test uses:

$$
a^{n-1}\equiv1\pmod n
$$

when:

$$
\gcd(a,n)=1
$$

If the congruence fails, n is definitely composite.

However, if it passes, n may still be composite.

Examples of pseudoprimes include:

- 341 for base 2.
- 91 for base 3.

### Python Implementation

```python
import math

def fermat_test(n, a):
    if n < 2:
        return False

    if math.gcd(a, n) != 1:
        return False

    return pow(a, n - 1, n) == 1

print("341, base 2:", fermat_test(341, 2))
print("91, base 3:", fermat_test(91, 3))
print("97, base 2:", fermat_test(97, 2))
```

### Output

```text
341, base 2: True
91, base 3: False
97, base 2: True
```

### Explanation

- 341 passes the Fermat test for base 2, even though it is composite.
- 91 fails the test for base 3.
- 97 passes the test for base 2 and is actually prime.

### Key Learning

Fermat's test is useful for identifying composite numbers, but passing the test is not a mathematical proof of primality.

---

## 15.5 Carmichael Numbers

Carmichael Numbers are special composite integers that satisfy Fermat's congruence for every integer base relatively prime to them.

The defining property is:

$$
a^{n-1}\equiv1\pmod n
$$

for every a satisfying:

$$
\gcd(a,n)=1
$$

The first Carmichael number is:

$$
561=3\times11\times17
$$

Other examples include:

$$
1105,\ 1729,\ 2465,\ 2821
$$

### Korselt's Criterion

Korselt's Criterion states that a composite integer n is a Carmichael number if and only if:

1. n is square-free.
2. For every prime divisor p of n:

$$
p-1\mid n-1
$$

### Python Implementation

```python
import sympy as sp

def is_carmichael(n):
    if n < 2 or sp.isprime(n):
        return False

    factors = sp.factorint(n)

    square_free = all(
        exponent == 1
        for exponent in factors.values()
    )

    condition = all(
        (n - 1) % (p - 1) == 0
        for p in factors
    )

    return square_free and condition

numbers = [561, 1105, 1729, 2465, 2821]

for n in numbers:
    print(n, "Carmichael:", is_carmichael(n))
```

### Output

```text
561 Carmichael: True
1105 Carmichael: True
1729 Carmichael: True
2465 Carmichael: True
2821 Carmichael: True
```

### Key Learning

Carmichael numbers demonstrate why Fermat's primality test alone cannot reliably prove that a number is prime.

---

## 15.6 Lucas' Primality Test and the n-1 Test

Lucas' Primality Test is a deterministic method based on modular exponentiation and the prime factorization of n-1.

A useful form of Lucas' theorem states that if, for every distinct prime factor q of n-1, there exists an integer a satisfying:

$$
a^{n-1}\equiv1\pmod n
$$

and:

$$
a^{(n-1)/q}\not\equiv1\pmod n
$$

then n is prime.

### Example

Consider:

$$
n=7
$$

Then:

$$
n-1=6=2\times3
$$

Choose:

$$
a=3
$$

Check:

$$
3^6\equiv1\pmod7
$$

For q = 2:

$$
3^{6/2}=3^3\equiv6\pmod7
$$

For q = 3:

$$
3^{6/3}=3^2\equiv2\pmod7
$$

Neither result is 1 modulo 7.

Therefore, the Lucas criterion proves that 7 is prime.

### Python Implementation

```python
import sympy as sp
import math

def lucas_primality_test(n, a):
    if n < 2:
        return False

    if math.gcd(a, n) != 1:
        return False

    factors = sp.factorint(n - 1)

    if pow(a, n - 1, n) != 1:
        return False

    for q in factors:
        if pow(a, (n - 1) // q, n) == 1:
            return False

    return True

print("7 is prime:", lucas_primality_test(7, 3))
```

### Output

```text
7 is prime: True
```

### The n-1 Test

The n-1 test uses partial factorization of n-1 to construct primality certificates under suitable conditions.

It is related to Lucas-based primality testing.

### Key Learning

Lucas-based methods provide mathematical certificates of primality rather than relying only on probabilistic evidence.

---
## 15.7 Lucas-Lehmer Test and Mersenne Primes

A Mersenne Number is a number of the form:

$$
M_p=2^p-1
$$

When M_p is prime, it is called a Mersenne Prime.

For example:

$$
2^3-1=7
$$

The Lucas-Lehmer Test is a deterministic primality test specifically designed for Mersenne numbers with prime exponent p greater than 2.

### Mathematical Formula

It uses the recurrence:

$$
s_0=4
$$

$$
s_{k+1}=s_k^2-2\pmod{M_p}
$$

The Mersenne number M_p is prime if and only if:

$$
s_{p-2}\equiv0\pmod{M_p}
$$

### Example: M7

$$
M_7=2^7-1=127
$$

The final Lucas-Lehmer result is:

$$
s_5\equiv0\pmod{127}
$$

Therefore, 127 is prime.

### Python Implementation

```python
import sympy as sp

def lucas_lehmer(p):
    if p == 2:
        return True

    if p < 2 or not sp.isprime(p):
        return False

    M = 2**p - 1
    s = 4

    for _ in range(p - 2):
        s = (s * s - 2) % M

    return s == 0

print("M7 is prime:", lucas_lehmer(7))
print("M11 is prime:", lucas_lehmer(11))
print("M13 is prime:", lucas_lehmer(13))
```

### Output

```text
M7 is prime: True
M11 is prime: False
M13 is prime: True
```

**Key Learning:** The Lucas-Lehmer Test is an important specialized algorithm for identifying large Mersenne primes.

---

# 16. Further Properties of Prime Numbers

Prime numbers have several additional mathematical properties involving modular arithmetic, multiplicative groups, and algebraic structures.

## 16.1 Primitive Roots and Multiplicative Order

The Multiplicative Order of an integer a modulo n is the smallest positive integer k satisfying:

$$
a^k\equiv1\pmod n
$$

This order exists when:

$$
\gcd(a,n)=1
$$

A Primitive Root modulo a prime p is an integer whose multiplicative order is:

$$
p-1
$$

In other words, its powers generate every non-zero residue modulo p.

### Example

The powers of 3 modulo 7 are:

$$
3^1\equiv3\pmod7
$$

$$
3^2\equiv2\pmod7
$$

$$
3^3\equiv6\pmod7
$$

$$
3^4\equiv4\pmod7
$$

$$
3^5\equiv5\pmod7
$$

$$
3^6\equiv1\pmod7
$$

Therefore, 3 is a primitive root modulo 7.

### Python Implementation

```python
import math

def multiplicative_order(a, n):
    if math.gcd(a, n) != 1:
        return None

    value = 1

    for k in range(1, n + 1):
        value = (value * a) % n

        if value == 1:
            return k

    return None

a = 3
n = 7

print("Multiplicative Order:",
      multiplicative_order(a, n))
```

### Output

```text
Multiplicative Order: 6
```

### Finding Primitive Roots

```python
def is_primitive_root(a, p):
    return multiplicative_order(a, p) == p - 1

p = 7

roots = []

for a in range(1, p):
    if is_primitive_root(a, p):
        roots.append(a)

print("Primitive Roots modulo", p)
print(roots)
```

### Output

```text
Primitive Roots modulo 7
[3, 5]
```

**Key Learning:** Primitive roots and multiplicative order are important in modular group theory and cryptographic mathematics.

---


## 16.2 Prime Characterizations

Prime numbers can be characterized through several equivalent mathematical properties.

For an integer p greater than 1:

### Property 1: Multiplicative Inverses

p is prime if and only if every non-zero residue modulo p has a multiplicative inverse.

### Property 2: Euler's Totient Function

p is prime if and only if:

$$
\phi(p)=p-1
$$

### Property 3: Square Roots of Unity

If p is an odd prime, the solutions of:

$$
x^2\equiv1\pmod p
$$

are:

$$
x\equiv1\pmod p
$$

or:

$$
x\equiv-1\pmod p
$$

### Python Implementation

```python
import sympy as sp

p = 7

print("Is Prime:", sp.isprime(p))
print("Euler Totient:", sp.totient(p))

solutions = [
    x for x in range(p)
    if (x*x) % p == 1
]

print("Solutions:", solutions)
```

### Output

```text
Is Prime: True
Euler Totient: 6
Solutions: [1, 6]
```

**Key Learning:** These properties provide alternative ways to understand and characterize prime numbers.

---


# 17. Divisor Functions and Antiprimes

Divisor Functions describe important properties of the positive divisors of an integer.

Suppose the prime factorization of n is:

$$
n=p_1^{a_1}p_2^{a_2}\cdots p_k^{a_k}
$$

The number of positive divisors is given by:

$$
d(n)=(a_1+1)(a_2+1)\cdots(a_k+1)
$$

### Example

Consider:

$$
12=2^2\times3^1
$$

Therefore:

$$
d(12)=(2+1)(1+1)
$$

$$
d(12)=6
$$

The positive divisors are:

$$
1,2,3,4,6,12
$$

### Python Implementation

```python
import sympy as sp

number = 12

print("Number:", number)
print("Prime Factorization:", sp.factorint(number))
print("Number of Divisors:", sp.divisor_count(number))
print("Divisors:", sp.divisors(number))
```

### Output

```text
Number: 12
Prime Factorization: {2: 2, 3: 1}
Number of Divisors: 6
Divisors: [1, 2, 3, 4, 6, 12]
```

## Sum-of-Divisors Function

The Sum-of-Divisors Function adds all positive divisors of an integer.

It is represented by:

$$
\sigma(n)=\sum_{d\mid n}d
$$

For example:

$$
\sigma(12)=1+2+3+4+6+12
$$

$$
\sigma(12)=28
$$

### Python Implementation

```python
import sympy as sp

def sum_of_divisors(n):
    return sum(sp.divisors(n))

print("Sum of Divisors:", sum_of_divisors(12))
```

### Output

```text
Sum of Divisors: 28
```

**Key Learning:** Divisor functions connect prime factorization with the structure and distribution of integer factors.

---


markdown

## Highly Composite Numbers (Antiprimes)

A Highly Composite Number has more positive divisors than every smaller positive integer.

These numbers are also known as Antiprimes.

Examples include:

$$
1,2,4,6,12,24,36,48,60,120,360
$$

### Python Implementation

```python
import sympy as sp

def highly_composite_numbers(limit):
    result = []
    max_divisors = 0

    for n in range(1, limit + 1):
        count = sp.divisor_count(n)

        if count > max_divisors:
            result.append(n)
            max_divisors = count

    return result

print(highly_composite_numbers(100))
```

### Output

```text
[1, 2, 4, 6, 12, 24, 36, 48, 60]
```

**Key Learning:** Highly Composite Numbers have an unusually large number of divisors compared with smaller integers.

---
# 18. Fundamental Theorem of Arithmetic

The Fundamental Theorem of Arithmetic states that every integer greater than 1 can be expressed as a product of prime numbers in exactly one way, apart from the order of the factors.

This is also known as the Unique Prime Factorization Theorem.

### Example 1

$$
60=2^2\times3\times5
$$

### Example 2

$$
2024=2^3\times11\times23
$$

Prime factorization is one of the central concepts of Number Theory.

It is also important in cryptography because some public-key cryptographic systems rely on the computational difficulty of factoring sufficiently large integers into their prime factors.

### Python Implementation

```python
import sympy as sp

numbers = [60, 2024, 360, 100]

for n in numbers:
    print("Number:", n)
    print("Prime Factorization:", sp.factorint(n))
    print()
```

### Output

```text
Number: 60
Prime Factorization: {2: 2, 3: 1, 5: 1}

Number: 2024
Prime Factorization: {2: 3, 11: 1, 23: 1}

Number: 360
Prime Factorization: {2: 3, 3: 2, 5: 1}

Number: 100
Prime Factorization: {2: 2, 5: 2}
```

### Key Learning

Every integer greater than 1 has a unique prime factorization, making prime numbers the fundamental building blocks of integers.

---

# 19. Practice Problems

The following problems are designed to strengthen understanding of Number Theory, modular arithmetic, prime numbers, and cryptography.

## 19.1 Basic Number Theory

### Problem 1: Find the GCD of 252 and 105

```python
import math

a = 252
b = 105

print("GCD:", math.gcd(a, b))
```

### Output

```text
GCD: 21
```

### Problem 2: Calculate the LCM of 18 and 30

```python
import math

a = 18
b = 30

lcm = abs(a * b) // math.gcd(a, b)

print("LCM:", lcm)
```

### Output

```text
LCM: 90
```

### Problem 3: Find All Positive Divisors of 60

```python
import sympy as sp

number = 60

print("Divisors:", sp.divisors(number))
```

### Output

```text
Divisors: [1, 2, 3, 4, 5, 6, 10, 12, 15, 20, 30, 60]
```

### Problem 4: Determine Whether 97 Is Prime

```python
import sympy as sp

number = 97

if sp.isprime(number):
    print(number, "is Prime")
else:
    print(number, "is Composite")
```

### Output

```text
97 is Prime
```

### Problem 5: Find the Prime Factorization of 2024

```python
import sympy as sp

number = 2024

print("Prime Factorization:", sp.factorint(number))
```

### Output

```text
Prime Factorization: {2: 3, 11: 1, 23: 1}
```

---


## 19.2 Extended Euclidean Algorithm

### Problem 1: Find Integers x and y

Find integers x and y satisfying:

$$
35x+15y=5
$$

### Python Implementation

```python
from sympy import gcdex

x, y, g = gcdex(35, 15)

print("x =", x)
print("y =", y)
print("GCD =", g)

print("Verification:", 35*x + 15*y)
```

### Output

```text
x = 1
y = -2
GCD = 5
Verification: 5
```

### Problem 2: Find the Modular Inverse of 7 Modulo 26

We need to find x such that:

$$
7x\equiv1\pmod{26}
$$

### Python Implementation

```python
from sympy import mod_inverse

a = 7
n = 26

inverse = mod_inverse(a, n)

print("Modular Inverse:", inverse)
print("Verification:", (a * inverse) % n)
```

### Output

```text
Modular Inverse: 15
Verification: 1
```

### Problem 3: Check Whether an Integer Solution Exists

Determine whether:

$$
12x+18y=7
$$

has an integer solution.

### Python Implementation

```python
import math

a = 12
b = 18
c = 7

g = math.gcd(a, b)

if c % g == 0:
    print("Integer solution exists")
else:
    print("No integer solution exists")
```

### Output

```text
No integer solution exists
```

### Explanation

Since:

$$
\gcd(12,18)=6
$$

But:

$$
6\nmid7
$$

Therefore, no integer solution exists.

---

## 19.3 Modular Arithmetic

### Problem 1: Calculate 7^128 mod 13

### Python Implementation

```python
a = 7
b = 128
n = 13

result = pow(a, b, n)

print("Result:", result)
```

### Output

```text
Result: 3
```

### Problem 2: Solve the Chinese Remainder Theorem

Solve:

$$
x\equiv2\pmod3
$$

$$
x\equiv3\pmod5
$$

### Python Implementation

```python
from sympy.ntheory.modular import solve_congruence

result = solve_congruence(
    (2, 3),
    (3, 5)
)

print("Solution:", result)
```

### Output

```text
Solution: (8, 15)
```

Therefore:

$$
x\equiv8\pmod{15}
$$

### Problem 3: Calculate Euler's Totient Function

Find:

$$
\phi(36)
$$

### Python Implementation

```python
import sympy as sp

print("Euler Totient:", sp.totient(36))
```

### Output

```text
Euler Totient: 12
```

### Problem 4: Verify Fermat's Little Theorem

Let:

$$
a=3,\quad p=7
$$

According to Fermat's Little Theorem:

$$
3^6\equiv1\pmod7
$$

### Python Implementation

```python
a = 3
p = 7

result = pow(a, p - 1, p)

print("Result:", result)

if result == 1:
    print("Fermat's Little Theorem Verified")
```

### Output

```text
Result: 1
Fermat's Little Theorem Verified
```

---
## 19.4 Prime Numbers and Primality Testing

### Problem 1: Generate All Prime Numbers Up to 100

### Python Implementation

```python
def sieve(n):
    prime = [True] * (n + 1)
    prime[0] = prime[1] = False

    for p in range(2, int(n**0.5) + 1):
        if prime[p]:
            for j in range(p*p, n + 1, p):
                prime[j] = False

    return [i for i in range(2, n + 1) if prime[i]]

print("Prime Numbers up to 100:")
print(sieve(100))
```

### Output

```text
Prime Numbers up to 100:
[2, 3, 5, 7, 11, 13, 17, 19, 23, 29,
31, 37, 41, 43, 47, 53, 59, 61, 67,
71, 73, 79, 83, 89, 97]
```

### Problem 2: Check Whether 341 Passes the Fermat Test for Base 2

```python
import math

n = 341
a = 2

result = pow(a, n - 1, n)

print("Modular Result:", result)

if math.gcd(a, n) == 1 and result == 1:
    print("341 passes Fermat test for base 2")
else:
    print("341 fails Fermat test")
```

### Output

```text
Modular Result: 1
341 passes Fermat test for base 2
```

**Explanation:** 341 is composite because:

$$
341=11\times31
$$

This demonstrates that passing the Fermat test does not guarantee primality.

### Problem 3: Explain Why 561 Is a Carmichael Number

```python
import sympy as sp

n = 561

print("Prime Factorization:", sp.factorint(n))
print("Is Prime:", sp.isprime(n))

for a in [2, 5, 10, 50]:
    if math.gcd(a, n) == 1:
        print(f"Base {a}: {pow(a, n-1, n)}")
```

### Output

```text
Prime Factorization: {3: 1, 11: 1, 17: 1}
Is Prime: False
Base 2: 1
Base 5: 1
Base 10: 1
Base 50: 1
```

### Problem 4: Apply the Lucas-Lehmer Test to M7

```python
def lucas_lehmer(p):
    if p == 2:
        return True

    if p < 2 or not sp.isprime(p):
        return False

    M = 2**p - 1
    s = 4

    for _ in range(p - 2):
        s = (s*s - 2) % M

    return s == 0

print("M7 is prime:", lucas_lehmer(7))
```

### Output

```text
M7 is prime: True
```

### Problem 5: Find a Goldbach Representation of 50

```python
import sympy as sp

n = 50

for p in range(2, n):
    q = n - p

    if sp.isprime(p) and sp.isprime(q):
        print(f"{n} = {p} + {q}")
        break
```

### Output

```text
50 = 3 + 47
```

---
Part 4 – Small Section 20: Cryptography and RSA

markdown

## 19.5 Cryptography

Cryptography uses mathematical techniques to protect information and secure digital communication.

### Problem 1: Explain the Mathematical Steps of RSA Key Generation

RSA is a public-key cryptographic algorithm based on prime numbers and modular arithmetic.

The main mathematical steps are:

1. Select two distinct prime numbers p and q.
2. Calculate n = p × q.
3. Calculate Euler's Totient Function.
4. Select a public exponent e.
5. Calculate the private exponent d using a modular inverse.

The mathematical formulas are:

$$
n=pq
$$

$$
\phi(n)=(p-1)(q-1)
$$

$$
ed\equiv1\pmod{\phi(n)}
$$

The public key is:

$$
(e,n)
$$

The private key is:

$$
(d,n)
$$

### Educational Example

Let:

$$
p=3,\quad q=11
$$

Then:

$$
n=3\times11=33
$$

$$
\phi(n)=2\times10=20
$$

Choose:

$$
e=3
$$

The modular inverse is:

$$
d=7
$$

Because:

$$
3\times7=21\equiv1\pmod{20}
$$

Therefore:

Public Key:

$$
(3,33)
$$

Private Key:

$$
(7,33)
$$

### Python Implementation

```python
import math

p = 3
q = 11

n = p * q
phi = (p - 1) * (q - 1)

e = 3
d = pow(e, -1, phi)

print("Public Key:", (e, n))
print("Private Key:", (d, n))
print("Euler Totient:", phi)
```

### Output

```text
Public Key: (3, 33)
Private Key: (7, 33)
Euler Totient: 20
```

### Problem 2: Calculate a Modular Inverse Using Extended Euclidean Algorithm

Find the modular inverse of 7 modulo 26.

$$
7x\equiv1\pmod{26}
$$

### Python Implementation

```python
from sympy import mod_inverse

a = 7
n = 26

inverse = mod_inverse(a, n)

print("Modular Inverse:", inverse)
print("Verification:", (a * inverse) % n)
```

### Output

```text
Modular Inverse: 15
Verification: 1
```

### Problem 3: Demonstrate RSA Encryption and Decryption

Use small educational prime numbers to demonstrate RSA encryption and decryption.

Given:

$$
p=3,\quad q=11
$$

$$
n=33,\quad e=3,\quad d=7
$$

Let the message be:

$$
M=4
$$

RSA Encryption:

$$
C=M^e\mod n
$$

RSA Decryption:

$$
M=C^d\mod n
$$

### Python Implementation

```python
p = 3
q = 11

n = p * q
phi = (p - 1) * (q - 1)

e = 3
d = pow(e, -1, phi)

message = 4

# Encryption
ciphertext = pow(message, e, n)

# Decryption
decrypted = pow(ciphertext, d, n)

print("Original Message:", message)
print("Encrypted Message:", ciphertext)
print("Decrypted Message:", decrypted)
```

### Output

```text
Original Message: 4
Encrypted Message: 31
Decrypted Message: 4
```

### Explanation

The original message is 4.

After encryption, the ciphertext becomes 31.

After decryption, the original message 4 is recovered.

**Important Note:** Small RSA numbers are educational examples only. They are not suitable for real-world security. Practical RSA requires secure key sizes, padding schemes, and vetted cryptographic libraries.

---

# 20. References and Scope Notes

## Official Workshop Resources


## Study Materials

### 1. Number_therory2.pdf

Number Theory and Cryptography workshop material covering:

- GCD and Euclid's Algorithm
- Extended Euclidean Algorithm
- LCM
- Diophantine Equations
- Modular Arithmetic
- Chinese Remainder Theorem
- Fermat's Little Theorem
- Euler's Totient Function
- Euler's Theorem

### 2. Number_theory3.pdf

Prime Numbers presentation by the Maths and Physics Club, IIT Bombay (December 24, 2020).

Topics include:

- Prime Distribution
- Infinitude of Primes
- Dirichlet's Theorem
- Prime Number Theorem
- Prime Conjectures
- Primality Testing
- Pseudoprimes
- Carmichael Numbers
- Lucas Tests
- Mersenne Primes
- Divisor Functions

## Additional Learning Resources

### Python Documentation

https://docs.python.org/3/

### Google Colab

https://colab.research.google.com/

### Jupyter Notebook

https://jupyter.org/

### Mathematics Reference

https://www.math.iitb.ac.in/

## Scope Notes

This README provides a structured theoretical summary of the Number Theory and Cryptography workshop topics.

- Mathematical equations and examples are included to explain the fundamental concepts.
- Python implementations and practical exercises are maintained separately in Google Colab.
- All practical implementations are intended to run using Python 3.
- Some advanced topics are introduced at a conceptual level rather than through complete mathematical proofs.
- RSA examples using small prime numbers are educational demonstrations only and are not suitable for real-world security.
- Experimental results cannot prove mathematical conjectures involving infinitely many integers.

---


## Practical Topics Covered

The following practical implementations are included:

1. GCD and LCM Algorithms
2. Euclidean Algorithm
3. Extended Euclidean Algorithm
4. Diophantine Equations
5. Modular Arithmetic
6. Modular Multiplicative Inverse
7. Chinese Remainder Theorem
8. Fast Modular Exponentiation
9. Fermat's Little Theorem
10. Euler's Totient Function
11. Euler's Theorem
12. RSA Encryption and Decryption
13. Prime Number Generation
14. Trial Division
15. Sieve of Eratosthenes
16. Wheel Factorization
17. Fermat Primality Test
18. Carmichael Numbers
19. Lucas Primality Test
20. Lucas-Lehmer Test
21. Primitive Roots
22. Divisor Functions
23. Prime Factorization

## Development Environment

The practical implementations use:

- Programming Language: Python 3
- Platform: Google Colab
- Library: SymPy
- Notebook Environment: Jupyter Notebook

### Install Required Library

```python
!pip install sympy
```

### Output

```text
Successfully installed sympy
```

**Note:** The installation output may differ depending on the existing Colab environment.

---

# Conclusion

Through this Number Theory and Cryptography workshop, I explored the mathematical foundations of integers, divisibility, prime numbers, modular arithmetic, and cryptographic algorithms.

I learned how different mathematical concepts are connected through algorithms such as Euclid's Algorithm, Extended Euclidean Algorithm, Fast Modular Exponentiation, and the Sieve of Eratosthenes.

I also studied important theorems including Fermat's Little Theorem, Euler's Theorem, the Chinese Remainder Theorem, and the Fundamental Theorem of Arithmetic.

Furthermore, I explored prime-number properties, primality testing techniques, Carmichael numbers, primitive roots, divisor functions, and RSA public-key cryptography.

The practical Python implementations helped me understand how theoretical mathematical concepts can be applied to programming and computational problem-solving.

Overall, this workshop helped me connect theoretical mathematics with practical programming and understand how Number Theory contributes to modern cryptography and computer security.

---



**Workshop:** Number Theory, Cryptography & Prime Numbers

**Practical Platform:** Google Colab

---
