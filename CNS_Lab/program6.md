## Diffie-Hellman Key Exchange Protocol in C

###  Introduction

The **Diffie-Hellman Key Exchange** is a cryptographic protocol that allows two parties to generate a **shared secret key** over an insecure communication channel without exchanging the key itself. This shared key can then be used for symmetric encryption of messages.

It is based on the difficulty of the **discrete logarithm problem**, which makes it secure even if an attacker can observe all exchanged messages.

---

###  Algorithm Steps

1. Alice and Bob agree on a **large prime number `p`** and a **primitive root `g`** of `p`. These values are **public**.
2. Alice chooses a **private key `a`** and computes `A = g^a mod p`. She sends `A` to Bob.
3. Bob chooses a **private key `b`** and computes `B = g^b mod p`. He sends `B` to Alice.
4. Alice computes the shared secret `s = B^a mod p`.
5. Bob computes the shared secret `s = A^b mod p`.
6. Both Alice and Bob now share the **same secret key** `s`.

---

###  Diffie-Hellman Protocol in C

```c
#include <stdio.h>
#include <math.h>

// Function to compute (base^exp) % mod using modular exponentiation
int modExp(int base, int exp, int mod) {
    long long result = 1;
    base %= mod;

    while (exp > 0) {
        if (exp % 2 == 1)
            result = (result * base) % mod;

        base = (base * base) % mod;
        exp /= 2;
    }

    return result;
}

int main() {
    int p, g;
    int a, b, A, B, sharedKeyAlice, sharedKeyBob;

    // Step 1: Agree on public values p and g
    printf("Enter a prime number p: ");
    scanf("%d", &p);
    printf("Enter a primitive root modulo %d (g): ", p);
    scanf("%d", &g);

    // Step 2: Alice's private key
    printf("Alice, enter your private key (a): ");
    scanf("%d", &a);
    A = modExp(g, a, p);
    printf("Alice sends A = %d to Bob.\n", A);

    // Step 3: Bob's private key
    printf("Bob, enter your private key (b): ");
    scanf("%d", &b);
    B = modExp(g, b, p);
    printf("Bob sends B = %d to Alice.\n", B);

    // Step 4: Compute shared secrets
    sharedKeyAlice = modExp(B, a, p);
    sharedKeyBob = modExp(A, b, p);

    // Step 5: Print shared key
    printf("Shared secret computed by Alice: %d\n", sharedKeyAlice);
    printf("Shared secret computed by Bob: %d\n", sharedKeyBob);

    if (sharedKeyAlice == sharedKeyBob)
        printf("Success! Shared secret key is %d\n", sharedKeyAlice);
    else
        printf("Error! Shared keys do not match.\n");

    return 0;
}
```

---

###  Code Explanation

| Part                                               | Functionality                                |
| -------------------------------------------------- | -------------------------------------------- |
| `modExp(base, exp, mod)`                           | Performs modular exponentiation efficiently. |
| `p, g`                                             | Publicly agreed prime and primitive root.    |
| `a, b`                                             | Alice and Bob’s private keys.                |
| `A = g^a mod p`, `B = g^b mod p`                   | Exchanged public keys.                       |
| `sharedKey = (other_public_key)^private_key mod p` | Final shared secret key.                     |

---

###  Sample Output

```
Enter a prime number p: 23
Enter a primitive root modulo 23 (g): 5
Alice, enter your private key (a): 6
Alice sends A = 8 to Bob.
Bob, enter your private key (b): 15
Bob sends B = 19 to Alice.
Shared secret computed by Alice: 2
Shared secret computed by Bob: 2
Success! Shared secret key is 2
```

---

###  Viva Questions & Answers

| Question                                           | Answer                                                              |
| -------------------------------------------------- | ------------------------------------------------------------------- |
| What is the purpose of Diffie-Hellman?             | Securely exchange a shared secret key over an insecure channel.     |
| What are public values in DH?                      | A prime `p` and primitive root `g`.                                 |
| What makes DH secure?                              | The difficulty of solving the **discrete logarithm problem**.       |
| Can an attacker see values `p`, `g`, `A`, and `B`? | Yes, these are public, but still cannot derive the shared key.      |
| What is the role of modular exponentiation?        | To keep values within manageable range and for security.            |
| What is the shared key used for?                   | For symmetric encryption after exchange.                            |
| Is Diffie-Hellman symmetric or asymmetric?         | It's used to establish a symmetric key using asymmetric techniques. |
| Can the same `p` and `g` be reused?                | Yes, but it’s more secure to use fresh parameters for each session. |
| What if Alice and Bob pick same private key?       | It will still work, though it reduces randomness.                   |
| Why do we need large `p` in practice?              | Larger primes make brute-force or discrete log attacks infeasible.  |

---

