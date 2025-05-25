###  Introduction

RSA (Rivest–Shamir–Adleman) is a **public-key cryptosystem** used for secure data transmission. It is based on the **mathematical difficulty of factoring large prime numbers**. RSA involves a pair of keys: a **public key** used for encryption and a **private key** used for decryption.

---

###  RSA Algorithm Overview

**Key Generation:**

1. Choose two large distinct prime numbers, `p` and `q`.
2. Compute `n = p * q` (used as the modulus).
3. Calculate Euler’s Totient Function: `φ(n) = (p - 1)(q - 1)`.
4. Choose `e` such that `1 < e < φ(n)` and `gcd(e, φ(n)) = 1`.
5. Compute `d` as the modular inverse of `e` modulo `φ(n)` (i.e., `d ≡ e⁻¹ mod φ(n)`).

**Encryption:**

* Cipher `c = (m^e) mod n`, where `m` is the message.

**Decryption:**

* Message `m = (c^d) mod n`.

---

##  RSA Algorithm in C

```c
#include <stdio.h>
#include <stdlib.h>
#include <math.h>

// Function to compute GCD
int gcd(int a, int b) {
    while (b != 0) {
        int t = b;
        b = a % b;
        a = t;
    }
    return a;
}

// Function to compute modular inverse using Extended Euclidean Algorithm
int modInverse(int e, int phi) {
    int t = 0, new_t = 1;
    int r = phi, new_r = e;
    while (new_r != 0) {
        int quotient = r / new_r;
        int temp = new_t;
        new_t = t - quotient * new_t;
        t = temp;

        temp = new_r;
        new_r = r - quotient * new_r;
        r = temp;
    }
    if (r > 1) return -1; // Not invertible
    if (t < 0) t += phi;
    return t;
}

// Function to compute (base^exp) % mod
int modExp(int base, int exp, int mod) {
    int result = 1;
    base %= mod;
    while (exp > 0) {
        if (exp % 2 == 1)
            result = (result * base) % mod;
        exp /= 2;
        base = (base * base) % mod;
    }
    return result;
}

int main() {
    int p, q, n, phi, e, d, m, c;

    // Step 1: Choose two prime numbers
    printf("Enter two prime numbers p and q: ");
    scanf("%d%d", &p, &q);

    // Step 2: Compute n and phi
    n = p * q;
    phi = (p - 1) * (q - 1);

    // Step 3: Choose e
    printf("Enter a value for e such that 1 < e < %d and gcd(e, %d) = 1: ", phi, phi);
    scanf("%d", &e);

    if (gcd(e, phi) != 1) {
        printf("e and phi(n) are not coprime. Aborting.\n");
        return 1;
    }

    // Step 4: Compute d
    d = modInverse(e, phi);
    if (d == -1) {
        printf("Modular inverse of e doesn't exist. Aborting.\n");
        return 1;
    }

    // Show keys
    printf("Public Key: (%d, %d)\n", e, n);
    printf("Private Key: (%d, %d)\n", d, n);

    // Step 5: Encrypt
    printf("Enter message (m < %d) to encrypt: ", n);
    scanf("%d", &m);
    c = modExp(m, e, n);
    printf("Encrypted message: %d\n", c);

    // Step 6: Decrypt
    int decrypted = modExp(c, d, n);
    printf("Decrypted message: %d\n", decrypted);

    return 0;
}
```

---

##  Code Explanation

| Step | Code Logic                                       | Purpose                                              |
| ---- | ------------------------------------------------ | ---------------------------------------------------- |
| 1    | `gcd(a, b)`                                      | To check if e is co-prime with φ(n)                  |
| 2    | `modInverse(e, phi)`                             | Finds the private key `d`                            |
| 3    | `modExp(base, exp, mod)`                         | Modular exponentiation for encryption and decryption |
| 4    | `Public Key = (e, n)` and `Private Key = (d, n)` | Used to encrypt and decrypt respectively             |
| 5    | `c = (m^e) mod n`                                | Encrypts message                                     |
| 6    | `m = (c^d) mod n`                                | Decrypts message                                     |

---

## Sample Output

### Example 1

```
Enter two prime numbers p and q: 3 11
Enter a value for e such that 1 < e < 20 and gcd(e, 20) = 1: 7
Public Key: (7, 33)
Private Key: (3, 33)
Enter message (m < 33) to encrypt: 2
Encrypted message: 29
Decrypted message: 2
```

### Example 2

```
Enter two prime numbers p and q: 17 23
Enter a value for e such that 1 < e < 352 and gcd(e, 352) = 1: 3
Public Key: (3, 391)
Private Key: (235, 391)
Enter message (m < 391) to encrypt: 100
Encrypted message: 1000
Decrypted message: 100
```

---

##  Viva Questions & Answers

| Question                              | Answer                                                                |
| ------------------------------------- | --------------------------------------------------------------------- |
| What is RSA?                          | A public-key cryptographic algorithm based on factoring large primes. |
| What is the role of `e` and `d`?      | `e` is public exponent; `d` is the private decryption exponent.       |
| Why must `e` and φ(n) be coprime?     | To ensure the existence of a unique modular inverse `d`.              |
| What is `φ(n)`?                       | Euler’s totient function, φ(n) = (p−1)(q−1) for primes p and q.       |
| How do we encrypt data?               | `cipher = (message^e) mod n`                                          |
| How is the message decrypted?         | `message = (cipher^d) mod n`                                          |
| What makes RSA secure?                | The difficulty of factoring the large number `n = p * q`.             |
| What are the public and private keys? | Public: (e, n), Private: (d, n)                                       |
| Is RSA symmetric or asymmetric?       | Asymmetric – uses two separate keys.                                  |
| Real-world uses of RSA?               | SSL/TLS certificates, digital signatures, encrypted emails.           |

---
