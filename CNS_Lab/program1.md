
##  CRC-CCITT Error Detection Program in C

###  Introduction

Error detection is a crucial part of data communication systems, ensuring that data transmitted from a sender to a receiver is accurate and reliable. One of the most efficient and widely used methods for detecting errors is the **Cyclic Redundancy Check (CRC)**.

This project implements **CRC-CCITT** (International Telegraph and Telephone Consultative Committee) in C. CRC-CCITT uses a **16-bit polynomial** to calculate a **checksum** for a given data stream. This checksum is appended to the data before transmission. At the receiving end, the same CRC logic is used to verify whether the received data is valid or has been corrupted during transmission.

###  Polynomial Used

The **generating polynomial** for CRC-CCITT is:

```
Binary: 10001000000100001
Hex:    0x11021
Degree: 16 (so we pad 16 zeros during checksum calculation)
```

---

##  C Program Code

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

#define POLY 0x11021       // CRC-CCITT polynomial
#define POLY_BITS 17       // Total bits including MSB
#define MAX 100

// Convert binary string to integer
unsigned long long str_to_bin(char *data) {
    unsigned long long val = 0;
    while (*data) {
        val = (val << 1) | (*data++ - '0');
    }
    return val;
}

// Convert integer to binary string
void bin_to_str(unsigned long long val, int bits, char *out) {
    out[bits] = '\0';
    for (int i = bits - 1; i >= 0; i--) {
        out[i] = (val & 1) + '0';
        val >>= 1;
    }
}

// Perform CRC division and get remainder
unsigned int crc_remainder(unsigned long long data, int len) {
    unsigned long long divisor = (unsigned long long)POLY << (len - POLY_BITS);
    for (int i = 0; i <= len - POLY_BITS; i++) {
        if (data & ((unsigned long long)1 << (len - 1 - i))) {
            data ^= divisor;
        }
        divisor >>= 1;
    }
    return (unsigned int)(data & ((1 << (POLY_BITS - 1)) - 1));
}

int main() {
    char data_str[MAX], padded_data_str[MAX + POLY_BITS], final_code_str[MAX + POLY_BITS];
    int insert_error, error_pos;

    printf("Enter data: ");
    scanf("%s", data_str);

    int data_len = strlen(data_str);
    unsigned long long data_bin = str_to_bin(data_str);

    // Pad data with 16 zeros (POLY_BITS - 1)
    data_bin <<= (POLY_BITS - 1);
    int padded_len = data_len + POLY_BITS - 1;

    bin_to_str(data_bin, padded_len, padded_data_str);
    printf("Generating polynomial : 10001000000100001\n");
    printf("Modified data is : %s\n", padded_data_str);

    // Calculate remainder
    unsigned int remainder = crc_remainder(data_bin, padded_len);
    printf("Checksum is : ");
    bin_to_str(((str_to_bin(data_str) << (POLY_BITS - 1)) | remainder), padded_len, final_code_str);
    printf("%s\n", final_code_str);

    // Final codeword = original data + checksum
    unsigned long long codeword = (str_to_bin(data_str) << (POLY_BITS - 1)) | remainder;
    bin_to_str(codeword, padded_len, final_code_str);
    printf("Final code word is : %s\n", final_code_str);

    printf("Test error detection 0 (yes) 1(no)? : ");
    scanf("%d", &insert_error);

    if (insert_error == 0) {
        printf("Enter the position where error is to be inserted: ");
        scanf("%d", &error_pos);

        // Flip the bit at specified position
        codeword ^= 1ULL << (padded_len - error_pos);
        char erroneous_data[MAX + POLY_BITS];
        bin_to_str(codeword, padded_len, erroneous_data);
        printf("Erroneous data: %s\n", erroneous_data);

        unsigned int new_remainder = crc_remainder(codeword, padded_len);
        if (new_remainder == 0)
            printf("No Error detected\n");
        else
            printf("Error detected\n");
    } else {
        unsigned int new_remainder = crc_remainder(codeword, padded_len);
        if (new_remainder == 0)
            printf("No error detected in transmission.\n");
        else
            printf("Error detected in transmission.\n");
    }

    return 0;
}
```

---

##  Code Explanation (Step-by-Step)

| Step               | Description                                                                                                               |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------- |
| 1. Input           | User enters binary data to be transmitted.                                                                                |
| 2. Padding         | The data is padded with 16 zeros (n-1, where n is the degree of polynomial).                                              |
| 3. CRC Division    | The XOR-based polynomial division is performed to get the CRC remainder (checksum).                                       |
| 4. Final Code Word | The original data is appended with the checksum to form the transmitted codeword.                                         |
| 5. Error Insertion | The user is optionally allowed to flip a bit to simulate an error.                                                        |
| 6. Validation      | The receiver side performs the same CRC check. If the remainder is 0, the data is valid. Otherwise, an error is detected. |

---

##  Sample Output

### Example 1: With Error

```
Enter data: 1101
Generating polynomial : 10001000000100001
Modified data is : 11010000000000000000
Checksum is : 1101000110101101
Final code word is : 11011101000110101101
Test error detection 0 (yes) 1(no)? : 0
Enter the position where error is to be inserted: 2
Erroneous data: 10011101000110101101
Error detected
```

### Example 2: No Error

```
Enter data: 1101
Generating polynomial : 10001000000100001
Modified data is : 11010000000000000000
Checksum is : 1101000110101101
Final code word is : 11011101000110101101
Test error detection 0 (yes) 1(no)? : 1
No error detected in transmission.
```

---

##  Viva Questions & Answers

| Question                                     | Answer                                                                             |
| -------------------------------------------- | ---------------------------------------------------------------------------------- |
| What is CRC?                                 | Cyclic Redundancy Check is an error-detecting technique using polynomial division. |
| What is the polynomial used in this program? | CRC-CCITT: 0x11021 (binary: 10001000000100001).                                    |
| Why do we pad zeros?                         | To make room for the CRC checksum during division.                                 |
| What does XOR do in CRC?                     | It performs polynomial subtraction in GF(2), replacing division logic.             |
| What indicates an error?                     | A non-zero remainder after CRC check on received data.                             |
| Can CRC correct errors?                      | No, CRC only detects errors but does not correct them.                             |
| How many bits is CRC-CCITT?                  | 16 bits.                                                                           |
| Can CRC detect burst errors?                 | Yes, up to a certain length (typically up to 16 bits for CRC-CCITT).               |
| How is the final codeword constructed?       | Final codeword = original data + CRC remainder.                                    |
| Where is CRC used in real life?              | Networking (Ethernet), modems, storage devices, digital communication.             |

---

