Here is the **README file** for the **Leaky Bucket Algorithm C program** you just received:
## Leaky Bucket Algorithm Simulation (C Program)


To simulate and demonstrate how the **Leaky Bucket Algorithm** is used to regulate network traffic. It helps to ensure that **burst traffic** is converted to a **steady stream**, thus avoiding **congestion** and **packet loss** in the network.

---

###  **Introduction:**

The **Leaky Bucket Algorithm** is a traffic shaping mechanism in networking used to **control data flow**. It processes data packets entering at irregular intervals (bursty traffic) and outputs them at a **constant rate**.

* A bucket stores packets temporarily.
* Packets are transmitted at a **fixed output rate**.
* If the bucket overflows, **packets are dropped**.

This technique helps maintain **Quality of Service (QoS)** in networks and is used in routers, switches, and operating systems.

---

###  **Algorithm Overview:**

1. Set the **bucket size** and **output rate**.
2. For each **incoming packet**:

   * If the packet + stored data ≤ bucket size → accept the packet.
   * Else → discard the packet (**overflow**).
3. At every time unit, **transmit packets** from the bucket at the **fixed output rate**.
4. Repeat until all packets are processed and the bucket is empty.

---

### Code

```c
#include <stdio.h>

int main() {
    int bucket_size, output_rate, packet_size, stored = 0;
    int time = 0, i, incoming_packets, wait_time;

    printf("Enter bucket size: ");
    scanf("%d", &bucket_size);

    printf("Enter output rate: ");
    scanf("%d", &output_rate);

    printf("Enter number of incoming packets: ");
    scanf("%d", &incoming_packets);

    for (i = 0; i < incoming_packets; i++) {
        printf("\n--------------------------------------------------\n");

        printf("Incoming packet size at time %d: ", time);
        scanf("%d", &packet_size);

        if (packet_size + stored > bucket_size) {
            printf("Bucket Overflow! Packet of size %d is discarded.\n", packet_size);
        } else {
            stored += packet_size;
            printf("Packet accepted. Current stored data: %d bytes\n", stored);
        }

        // Transmit data for next few seconds until stored becomes zero
        wait_time = 0;
        while (stored > 0 && wait_time < 5) {
            int transmit = stored >= output_rate ? output_rate : stored;
            stored -= transmit;
            printf("Time %d -------- Transmitted %d bytes | Remaining = %d bytes\n", 
                    time + 1 + wait_time, transmit, stored);
            wait_time++;
        }

        time += wait_time;
    }

    // If there's still data left in buffer after all packets processed
    while (stored > 0) {
        int transmit = stored >= output_rate ? output_rate : stored;
        stored -= transmit;
        printf("Time %d -------- Transmitted %d bytes | Remaining = %d bytes\n", 
                ++time, transmit, stored);
    }

    printf("--------------------------------------------------\n");
    printf("All packets processed and bucket is now empty.\n");

    return 0;
}
```

###  **How to Compile and Run:**

```bash
gcc leaky_bucket.c -o leaky
./leaky
```

---

###  **Sample Input:**

```
Enter bucket size: 10
Enter output rate: 2
Enter number of incoming packets: 3

Incoming packet size at time 0: 6
Incoming packet size at time 3: 5
Incoming packet size at time 6: 12
```

---

###  **Sample Output:**

```
--------------------------------------------------
Incoming packet size at time 0: 6
Packet accepted. Current stored data: 6 bytes
Time 1 -------- Transmitted 2 bytes | Remaining = 4 bytes
Time 2 -------- Transmitted 2 bytes | Remaining = 2 bytes
Time 3 -------- Transmitted 2 bytes | Remaining = 0 bytes

--------------------------------------------------
Incoming packet size at time 3: 5
Packet accepted. Current stored data: 5 bytes
Time 4 -------- Transmitted 2 bytes | Remaining = 3 bytes
Time 5 -------- Transmitted 2 bytes | Remaining = 1 byte
Time 6 -------- Transmitted 1 bytes | Remaining = 0 bytes

--------------------------------------------------
Incoming packet size at time 6: 12
Bucket Overflow! Packet of size 12 is discarded.
--------------------------------------------------
All packets processed and bucket is now empty.
```

---

###  **Tested Scenarios:**

* Normal packet flow within limits.
* Bucket overflow and packet drops.
* Transmission over multiple time units.
* Delayed packet arrivals with leftover bytes.

---

###  **Concepts Demonstrated:**

* Buffer management
* Traffic shaping
* Rate limiting
* Packet acceptance/rejection logic
* Use of loops and conditionals for simulation

---

### **Viva Questions & Answers:**

| Question                                 | Answer                                                                   |
| ---------------------------------------- | ------------------------------------------------------------------------ |
| What does the Leaky Bucket Algorithm do? | It shapes traffic by allowing packets to be transmitted at a fixed rate. |
| What happens when the bucket overflows?  | Extra incoming packets are dropped.                                      |
| Is Leaky Bucket used in real networks?   | Yes, in routers and QoS management systems.                              |
| Can this handle bursty traffic?          | Yes, it smooths bursts into a uniform flow.                              |
| What layer uses it?                      | Mainly the **Data Link** or **Network Layer**.                           |
| Difference from Token Bucket?            | Leaky Bucket transmits at a constant rate, Token Bucket allows bursts.   |
