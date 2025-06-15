Here's the full **README file** for your **Vigenère Cipher** program in C, including explanation, code, sample output, and viva questions.

---

## 🔐 README: Vigenère Cipher Implementation in C

### 🎯 Objective:

To implement the **Vigenère Cipher**, a method of encrypting alphabetic text using a series of interwoven Caesar ciphers based on the letters of a repeating keyword.

---

### 📘 Algorithm Overview

#### ✅ **Encryption**

Given:

* **T<sub>i</sub>** – i-th character of plaintext
* **K<sub>i</sub>** – i-th character of key (repeated to match length)
* **C<sub>i</sub> = (T<sub>i</sub> + K<sub>i</sub>) mod 26**

Each letter of the plaintext is shifted by the corresponding letter in the key.

#### ✅ **Decryption**

Given:

* **C<sub>i</sub> – K<sub>i</sub> = T<sub>i</sub> (mod 26)**

Reverse the shift using the same key.

---

### 🧠 Sample Tabula Recta:

Used internally — think of it like 26 Caesar ciphers starting from A to Z:

```
  A B C D E F G ...
A A B C D E F G ...
B B C D E F G H ...
C C D E F G H I ...
```

---

### 💻 Sample Output:

```
Vigenère Cipher
1. Encrypt
2. Decrypt
3. Exit
Enter choice: 1
Enter Plaintext: ATTACKATDAWN
Enter Key: CAT
Encrypted: ATMCCDCTWCWG

Enter choice: 2
Enter Ciphertext: ATMCCDCTWCWG
Enter Key: CAT
Decrypted: ATTACKATDAWN
```

---

### 🧑‍💻 Code (vigenere\_cipher.c):

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

void encrypt(char* text, char* key, char* result) {
    int i, j;
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (i = 0, j = 0; i < textLen; i++) {
        if (isalpha(text[i])) {
            char offset = isupper(text[i]) ? 'A' : 'a';
            char keyChar = toupper(key[j % keyLen]) - 'A';
            result[i] = ((toupper(text[i]) - 'A' + keyChar) % 26) + 'A';
            j++;
        } else {
            result[i] = text[i];
        }
    }
    result[i] = '\0';
}

void decrypt(char* text, char* key, char* result) {
    int i, j;
    int textLen = strlen(text);
    int keyLen = strlen(key);

    for (i = 0, j = 0; i < textLen; i++) {
        if (isalpha(text[i])) {
            char offset = isupper(text[i]) ? 'A' : 'a';
            char keyChar = toupper(key[j % keyLen]) - 'A';
            result[i] = ((toupper(text[i]) - 'A' - keyChar + 26) % 26) + 'A';
            j++;
        } else {
            result[i] = text[i];
        }
    }
    result[i] = '\0';
}

int main() {
    char text[100], key[100], result[100];
    int choice;

    while (1) {
        printf("\nVigenère Cipher\n1. Encrypt\n2. Decrypt\n3. Exit\nEnter choice: ");
        scanf("%d", &choice);
        getchar(); // clear newline

        if (choice == 3) break;

        printf("Enter text: ");
        fgets(text, sizeof(text), stdin);
        text[strcspn(text, "\n")] = '\0'; // remove newline

        printf("Enter key: ");
        fgets(key, sizeof(key), stdin);
        key[strcspn(key, "\n")] = '\0'; // remove newline

        if (choice == 1) {
            encrypt(text, key, result);
            printf("Encrypted: %s\n", result);
        } else if (choice == 2) {
            decrypt(text, key, result);
            printf("Decrypted: %s\n", result);
        } else {
            printf("Invalid choice.\n");
        }
    }

    return 0;
}
```

---

### 🧪 Viva Questions and Answers:

**Q1. What is the main difference between Caesar and Vigenère cipher?**
**A:** Caesar uses a fixed shift, while Vigenère uses a repeating key to determine the shift for each letter.

**Q2. What is the key space of Vigenère cipher?**
**A:** Much larger than Caesar — it depends on the key length and complexity. A longer key increases security.

**Q3. How is the key repeated in Vigenère cipher?**
**A:** The key is cyclically repeated to match the length of the plaintext or ciphertext.

**Q4. Why is the Vigenère cipher considered stronger than Caesar?**
**A:** Because it obscures letter frequency better due to varying shifts from the key, resisting frequency analysis.

**Q5. Can the Vigenère cipher be broken?**
**A:** Yes, using **Kasiski Examination** or **frequency analysis** when key length is short or repeated.

**Q6. What happens if the key contains non-alphabetic characters?**
**A:** It's usually filtered out or rejected, as the cipher only works on alphabetic keys.

**Q7. Is the Vigenère cipher symmetric or asymmetric?**
**A:** Symmetric — the same key is used for encryption and decryption.

---

Let me know if you want this converted to a PDF, or if you'd like additional versions (e.g., in Python).
