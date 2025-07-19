# Number Theory – Week 8 DSA Bootcamp

This week covers essential number theory techniques used in competitive programming. We'll focus on two foundational algorithms and briefly touch on supporting concepts, based on the Competitive Programmer’s Handbook.

## Core Algorithms

### 1. Sieve of Eratosthenes with Smallest Prime Factor (SPF)

The Sieve of Eratosthenes is a preprocessing algorithm used to efficiently determine prime numbers up to a given number `n`. A modified version of the sieve also stores the smallest prime factor (SPF) for each number, which allows quick prime factorization.

#### Purpose
- Check if a number is prime in constant time after preprocessing.
- Factorize numbers ≤ `n` in logarithmic time using stored smallest prime factors.

#### Time Complexity
- Preprocessing: O(n log log n)
- Prime check: O(1)
- Factorization: O(log n)

#### Code (C++)
```cpp
const int N = 1e6 + 5;
int sieve[N];

void build_sieve(int n) {
    for (int x = 2; x <= n; x++) {
        if (sieve[x] != 0) continue;
        for (int u = 2 * x; u <= n; u += x) {
            if (sieve[u] == 0)
                sieve[u] = x;
        }
    }
}
Example (n = 20)
less
Copy
Edit
Index (k):      2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20
sieve[k]:       0  0  2  0  2  0  2  3  2  0  2  0  2  3  2  0  2  0  2
Here, sieve[k] == 0 indicates that k is prime. Otherwise, sieve[k] gives the smallest prime factor of k.

2. Modular Exponentiation (Binary Exponentiation)
Modular exponentiation is used to compute large powers modulo a number, typically to avoid overflow and maintain performance. It is especially useful in modular inverse calculations.

Purpose
Efficiently compute (a^b) % m for large b.

Time Complexity
O(log b)

Code (C++)
cpp
Copy
Edit
long long modpow(long long a, long long b, long long m) {
    long long res = 1;
    a %= m;
    while (b > 0) {
        if (b & 1)
            res = (res * a) % m;
        a = (a * a) % m;
        b >>= 1;
    }
    return res;
}
Example
cpp
Copy
Edit
modpow(2, 10, 1000) // returns 24
Supporting Concepts
These concepts are often used alongside the above algorithms in competitive programming problems.

Modular Arithmetic Rules
(a + b) % m = ((a % m) + (b % m)) % m

(a * b) % m = ((a % m) * (b % m)) % m

(a - b + m) % m ensures non-negative results

Prime Factorization using SPF Array
Once the smallest prime factors are stored with the sieve, we can factorize any number using:

cpp
Copy
Edit
vector<int> factorize(int x) {
    vector<int> res;
    while (sieve[x]) {
        res.push_back(sieve[x]);
        x /= sieve[x];
    }
    if (x > 1) res.push_back(x);
    return res;
}
Modular Inverse
When m is prime, the modular inverse of a can be calculated using Fermat's Little Theorem:

css
Copy
Edit
a^(-1) ≡ a^(m-2) mod m
This is frequently used in modular division.

GCD and the Euclidean Algorithm
Used for simplifying ratios, checking coprimality, or solving Diophantine equations.

cpp
Copy
Edit
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
Summary
For this week, focus on mastering the following two algorithms:

Sieve of Eratosthenes with SPF – for efficient prime generation and fast factorization.

Modular Exponentiation – for computing large powers under modulo efficiently.

These two techniques form the foundation of number theory problems in competitive programming. A clear understanding of modular arithmetic, GCD, and modular inverses will significantly improve your ability to solve a wide range of problems.

yaml
Copy
Edit

---
