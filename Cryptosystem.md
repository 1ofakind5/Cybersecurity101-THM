# 🔐 Cryptography: RSA Close Prime Factorization (`q = primo(p)`)

A technical reference guide and post-mortem analysis of an insecure RSA key generation algorithm where prime factors are chosen sequentially, exposing the cryptosystem to instant factorization via Fermat's Factorization Algorithm.

---

## 1. Vulnerability Mechanics & Implementation Flaw

In robust public-key cryptography, RSA requires two large, independent, and cryptographically random prime numbers ($p$ and $q$) to construct the modulus ($N = p \times q$). 

### The Flaw
In the target implementation, $q$ was derived directly from $p$ using a sequential prime generator:

```python
def primo(n):
    n += 2 if n & 1 else 1
    while not isPrime(n):
        n += 2
    return n

p = getPrime(1024)
q = primo(p)  # Flaw: q is the immediate prime successor of p
```

### Mathematical Impact
Because $q$ is the very next prime following $p$, the absolute difference $|p - q|$ is extremely small (often less than a few hundred). 

1. **Proximity to Square Root:** When $p \approx q$, both primes lie virtually adjacent to $\sqrt{N}$.
2. **Fermat Factorization Collapse:** Fermat's method represents $N$ as the difference of two squares:

$$N = a^2 - b^2 = (a - b)(a + b)$$

Where:
* $a = \lceil\sqrt{N}\rceil + k$
* $b = \sqrt{a^2 - N}$

When $|p - q|$ is minimal, $a$ starts directly next to $\sqrt{N}$, allowing $b^2 = a^2 - N$ to yield a perfect square almost instantaneously ($k \approx 0$). Once $a$ and $b$ are computed, the prime factors are recovered via $p = a - b$ and $q = a + b$.

---

## 2. Exploitation & Recovery Script

The following script implements Fermat's algorithm to factor $N$, calculate Euler's totient $\phi(N)$, compute the private exponent $d$, and recover the original plaintext.

```python
from math import isqrt
from Crypto.Util.number import inverse, long_to_bytes

# Public RSA Parameters
n = 15956250162063169819282947443743274370048643274416742655348817823973383829364700573954709256391245826513107784713930378963551647706777479778285473302665664446406061485616884195924631582130633137574953293367927991283669562895956699807156958071540818023122362163066253240925121801013767660074748021238790391454429710804497432783852601549399523002968004989537717283440868312648042676103745061431799927120153523260328285953425136675794192604406865878795209326998767174918642599709728617452705492122243853548109914399185369813289827342294084203933615645390728890698153490318636544474714700796569746488209438597446475170891
c = 3591116664311986976882299385598135447435246460706500887241769555088416359682787844532414943573794993699976035504884662834956846849863199643104254423886040489307177240200877443325036469020737734735252009890203860703565467027494906178455257487560902599823364571072627673274663460167258994444999732164163413069705603918912918029341906731249618390560631294516460072060282096338188363218018310558256333502075481132593474784272529318141983016684762611853350058135420177436511646593703541994904632405891675848987355444490338162636360806437862679321612136147437578799696630631933277767263530526354532898655937702383789647510
e = 0x10001

def fermat_factor(n):
    """
    Factors modulus N into p and q when |p - q| is small.
    """
    a = isqrt(n)
    if a * a < n:
        a += 1

    while True:
        b2 = a * a - n
        b = isqrt(b2)
        if b * b == b2:
            return a - b, a + b
        a += 1

# 1. Recover prime factors
p, q = fermat_factor(n)

# 2. Compute totient φ(N) and private exponent d
phi = (p - 1) * (q - 1)
d = inverse(e, phi)

# 3. Decrypt ciphertext: M = C^d mod N
m = pow(c, d, n)

# 4. Decode plaintext
plaintext = long_to_bytes(m)
print(f"[+] Decrypted Message: {plaintext.decode()}")
```

---

## 3. Defensive Engineering Standards

To secure RSA key generation against prime proximity vulnerabilities:

* **Independent Random Prime Generation:** Primes $p$ and $q$ must be generated using cryptographically secure pseudorandom number generators (CSPRNGs) with no functional dependency between them.
* **Prime Difference Constraints:** Conform to **NIST SP 800-56B Rev. 2**, ensuring $|p - q| > 2^{n/2 - 100}$ (where $n$ is the total key length in bits).
* **Algorithm Modernization:** Prefer Elliptic Curve Cryptography (ECC / Ed25519) or standard RSA key sizes $\ge 2048$ bits generated via validated cryptographic libraries (e.g., OpenSSL, PyCryptodome standard functions).
