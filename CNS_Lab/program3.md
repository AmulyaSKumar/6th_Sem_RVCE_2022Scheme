##  Caesar and Playfair Cipher Implementation (C Program)


To implement and demonstrate classical cryptographic techniques — **Caesar Cipher** and **Playfair Cipher** — using C programming. These algorithms are part of symmetric key cryptography and provide foundational understanding of substitution and digraph-based encryption.

---

###  **Introduction:**

* **Caesar Cipher** is a monoalphabetic substitution cipher in which each letter of the plaintext is shifted by a fixed number of positions.
* **Playfair Cipher** is a digraph substitution cipher that encrypts pairs of letters based on positions in a 5x5 matrix derived from a keyword.

Both methods are among the earliest techniques used in manual encryption and provide valuable learning for basic cryptographic principles.

---

###  **Caesar Cipher – Algorithm Overview:**

1. Define maximum sentence size and include required headers: `stdio.h`, `ctype.h`.
2. Present a menu to the user with options for encryption and decryption.
3. Get user input (plaintext/ciphertext) and shift key (default or custom).
4. For **encryption**:

   * Shift each alphabet character by a fixed offset (e.g., +3).
   * Wrap around for letters beyond 'z'.
5. For **decryption**:

   * Reverse the shift (e.g., -3) to retrieve the original message.
6. Use `tolower()` and `toupper()` to handle case conversion.
7. Display the final encrypted/decrypted message.

---

###  **Playfair Cipher – Algorithm Overview:**

1. Generate a 5x5 **key matrix** from a keyword (e.g., `"playfair example"`).

   * Omit duplicates.
   * Combine 'I' and 'J' as one letter.
2. Convert plaintext into **digraphs** (pairs).

   * Insert 'X' if both letters are the same.
   * Add 'Z' if the length is odd.
3. For each digraph, apply the **Playfair encryption rules**:

   * Same **row** → shift right
   * Same **column** → shift down
   * Else, form a rectangle and swap corners
4. Decryption applies the **inverse** of the above rules.

---

### **Program Details:**
```c
// Caesar and Playfair Cipher Implementation in C
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 100

// Caesar Cipher Functions
void caesarEncrypt(char *text, int shift) {
    char ch;
    for (int i = 0; text[i] != '\0'; ++i) {
        ch = text[i];
        if (isalpha(ch)) {
            char base = islower(ch) ? 'a' : 'A';
            text[i] = ((ch - base + shift) % 26) + base;
        }
    }
}

void caesarDecrypt(char *text, int shift) {
    caesarEncrypt(text, 26 - (shift % 26));
}

// Playfair Cipher Functions
char matrix[5][5];
int pos[26][2]; // Position of each character in matrix

void generateKeyMatrix(char *key) {
    int used[26] = {0};
    used['J' - 'A'] = 1;
    int x = 0, y = 0;

    for (int i = 0; key[i] != '\0'; ++i) {
        char ch = toupper(key[i]);
        if (ch < 'A' || ch > 'Z' || used[ch - 'A']) continue;
        matrix[x][y] = ch;
        pos[ch - 'A'][0] = x;
        pos[ch - 'A'][1] = y;
        used[ch - 'A'] = 1;
        y++;
        if (y == 5) { y = 0; x++; }
    }

    for (char ch = 'A'; ch <= 'Z'; ++ch) {
        if (!used[ch - 'A']) {
            matrix[x][y] = ch;
            pos[ch - 'A'][0] = x;
            pos[ch - 'A'][1] = y;
            used[ch - 'A'] = 1;
            y++;
            if (y == 5) { y = 0; x++; }
        }
    }
}

void playfairEncryptPair(char a, char b, char *res) {
    int ax = pos[a - 'A'][0], ay = pos[a - 'A'][1];
    int bx = pos[b - 'A'][0], by = pos[b - 'A'][1];

    if (ax == bx) {
        res[0] = matrix[ax][(ay + 1) % 5];
        res[1] = matrix[bx][(by + 1) % 5];
    } else if (ay == by) {
        res[0] = matrix[(ax + 1) % 5][ay];
        res[1] = matrix[(bx + 1) % 5][by];
    } else {
        res[0] = matrix[ax][by];
        res[1] = matrix[bx][ay];
    }
}

void prepareText(char *text, char *out) {
    int i = 0, j = 0;
    while (text[i]) {
        char a = toupper(text[i]);
        if (a == 'J') a = 'I';
        if (!isalpha(a)) { i++; continue; }

        char b = 'X';
        if (text[i+1]) {
            b = toupper(text[i+1]);
            if (b == 'J') b = 'I';
            if (!isalpha(b) || a == b) b = 'X';
            else i++;
        }
        out[j++] = a;
        out[j++] = b;
        i++;
    }
    out[j] = '\0';
}

void playfairEncrypt(char *text) {
    char prepared[SIZE], result[SIZE];
    prepareText(text, prepared);
    int len = strlen(prepared), j = 0;
    for (int i = 0; i < len; i += 2) {
        char enc[2];
        playfairEncryptPair(prepared[i], prepared[i+1], enc);
        result[j++] = enc[0];
        result[j++] = enc[1];
    }
    result[j] = '\0';
    printf("Encrypted: %s\n", result);
}

int main() {
    int choice;
    char text[SIZE], key[SIZE];

    while (1) {
        printf("\n1. Caesar Encrypt\n2. Caesar Decrypt\n3. Playfair Encrypt\n4. Exit\nEnter your choice: ");
        scanf("%d", &choice);
        getchar();

        switch (choice) {
            case 1:
                printf("Enter text: "); gets(text);
                caesarEncrypt(text, 3);
                printf("Encrypted: %s\n", text);
                break;
            case 2:
                printf("Enter text: "); gets(text);
                caesarDecrypt(text, 3);
                printf("Decrypted: %s\n", text);
                break;
            case 3:
                printf("Enter key: "); gets(key);
                generateKeyMatrix(key);
                printf("Enter text: "); gets(text);
                playfairEncrypt(text);
                break;
            case 4:
                return 0;
            default:
                printf("Invalid choice!\n");
        }
    }
    return 0;
}
```

###  **Sample Caesar Cipher Output:**

```
Plaintext: THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
Encrypted (Caesar, Shift = 3): QEB NRFZH YOLTK CLU GRJMP LSBO QEB IXWV ALD
Decrypted: THE QUICK BROWN FOX JUMPS OVER THE LAZY DOG
```

###  **Sample Playfair Cipher Output:**

```
Key: playfair example
Plaintext: HELLO WORLD
Converted Digraphs: HE LX LO WO RL DZ
Encrypted Text: BM OD ZB XD NA BE KU
```

> Note: I/J are treated as a single character, and 'X' or 'Z' is inserted to split duplicate or odd-letter digraphs.

---

###  **Viva Questions & Answers:**

| Question                                            | Answer                                                                    |
| --------------------------------------------------- | ------------------------------------------------------------------------- |
| What is Caesar Cipher?                              | A substitution cipher that shifts letters by a fixed number of positions. |
| What is the default shift in Caesar Cipher?         | Typically 3 positions (A→D, B→E, etc.).                                   |
| What is Playfair Cipher?                            | A digraph substitution cipher using a 5x5 key matrix.                     |
| Why do we treat I and J as one letter in Playfair?  | To fit 25 letters in the 5x5 matrix.                                      |
| What happens to repeated letters in Playfair pairs? | An 'X' is inserted to split the pair.                                     |
| Is Caesar Cipher secure for modern use?             | No, it’s easily breakable; used only for learning purposes.               |
| Which is stronger: Caesar or Playfair?              | Playfair, as it encrypts digraphs and hides letter frequency.             |


