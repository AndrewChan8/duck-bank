# 🦆 Duck Bank - A Multithreaded Banking System Simulator

**Author:** Andrew Chan  
**Language:** C (POSIX Threads & IPC)  
**Project Type:** Systems Programming / OS Simulation  
**Last Updated (Code):** December 2024
**Last Updated (Documentation):** April 2025

---

## 🚀 Overview

Duck Bank is a multithreaded, inter-process banking simulation system written in C. Designed to process large volumes of financial transactions safely and efficiently, Duck Bank demonstrates advanced operating systems concepts such as:

- Thread synchronization with mutexes, condition variables, and barriers
- Inter-process communication via pipes
- Shared memory access with `mmap`
- Coordinated parallelism between threads and multiple processes

It supports basic banking operations such as deposit, withdrawal, fund transfer, and balance inquiry while applying periodic interest based on account-specific reward rates. The system ensures data consistency, avoids race conditions, and logs auditing information through a separate process.

---

## 🛠 Features

- ✅ Thread-safe transaction handling
- ✅ 10 worker threads for concurrent transaction processing
- ✅ Bank thread to apply periodic reward interest
- ✅ Separate Auditor process using pipe-based IPC
- ✅ Puddles Bank process using `mmap` shared memory for savings accounts
- ✅ Per-account logs and centralized summaries
- ✅ Clean thread coordination with condition variables and barriers

---

## 📁 Project Structure

```text
.
├── bank.c             # Main source file
├── account.h          # Account struct and declarations
├── Makefile           # Build system
├── input.txt          # Sample input (you provide this)
├── output.txt         # Final account balances (generated)
├── output/            # Per-account update logs (generated)
├── savings/           # Puddles Bank savings logs (generated)
├── ledger.txt         # Auditor process logs (generated)
└── README.md          # This file
```

---

## 📦 Compilation & Execution

### 🔧 Dependencies

- GCC
- POSIX-compliant OS (Linux recommended)

### 🔨 Build

```bash
make
```

This compiles the `bank` executable using `gcc -g -lpthread`.

### ▶️ Run the Program

```bash
./bank input.txt
```

Where `input.txt` contains the account data and transaction requests.

### 🧹 Clean Build

```bash
make clean
```

Removes compiled binaries and output artifacts.

---

## 🧾 Input File Format

### Account Setup

The first line specifies the number of accounts, followed by 5 lines per account:

```
<number_of_accounts>
<index>               # (ignored)
<account_number>      # e.g. 1234567890123456
<password>            # e.g. secret123
<initial_balance>     # e.g. 5000.00
<reward_rate>         # e.g. 0.03
...
```

### Transactions

Each line afterward is a transaction command:

```
D <acc_num> <pw> <amount>         # Deposit
W <acc_num> <pw> <amount>         # Withdraw
T <src_acc> <pw> <dst_acc> <amt>  # Transfer
C <acc_num> <pw>                  # Check Balance
```

---

## 📤 Output Files

- `output.txt`: Final account balances after all transactions and interest updates.
- `output/act_<index>.txt`: Logs each account’s balance after every interest update.
- `savings/act_<index>.txt`: Initial balance snapshots for the Puddles Bank (20% of initial balance).
- `ledger.txt`: Logs from the Auditor process every 500th balance check and each interest update.

---

## 🧠 Internal Architecture

### Thread Model

- **10 Worker Threads**: Process transactions concurrently.
- **1 Banker Thread**: Applies interest when total valid transactions reach 5000.

### Process Model

- **Main Process**: Coordinates thread creation and program logic.
- **Auditor Process**: Receives logs via a pipe and writes them to `ledger.txt`.
- **Puddles Bank Process**: Receives account data via `mmap`, simulates savings accounts.

### IPC Techniques Used

- **Pipes**: Connect threads to the auditor process for audit logging.
- **Shared Memory (`mmap`)**: Transfers account information to the Puddles Bank subprocess.

### Synchronization Techniques

- **Mutexes (`pthread_mutex_t`)**: One per account for fine-grained locking.
- **Condition Variables**: Used to coordinate pauses and updates.
- **Barriers**: Used to align thread startup and pause cycles.

---

## 📈 Logging Behavior

### Interest Application

Triggered every time 5000 valid transactions are processed. The banker thread applies:

```c
balance += transaction_tracter * reward_rate;
```

And resets the tracker.

### Auditor Log

The `ledger.txt` file logs:

- Every 500th balance check across all worker threads
- Every interest update from the banker thread

Each entry is timestamped and includes the account and balance.

---

## 📄 Makefile

```makefile
all: bank

bank: bank.c
	gcc -g -o bank bank.c -lpthread

clean:
	rm -rf *.o bank output.txt ledger.txt output

check:
	diff output.txt outout.txt
```

---

## 🧪 Valgrind Tips

Run the following for memory leak checks:

```bash
valgrind --leak-check=full ./bank input.txt > valgrind_log.txt 2>&1
```

The program has been tested to avoid memory leaks and invalid accesses.

---

## 📂 Example Output Structure

```text
output.txt               # Final balances
ledger.txt               # Audit log (200+ entries typical)
output/
├── act_0.txt
├── act_1.txt
├── ...
savings/
├── act_0.txt
├── act_1.txt
```

Each `act_#.txt` contains the balance after each reward application.

---

## 🔚 Final Notes

Duck Bank is a robust simulation of a real-world banking backend, showcasing concurrency, IPC, shared memory, and synchronization. This project reflects strong systems-level programming principles and is built for scalability, correctness, and transparency.

---

## 📬 Contact

Feel free to reach out with questions, feedback, or collaboration inquiries!

📧 [andrewsushi.c8@gmail.com]  
💼 [linkedin.com/in/andrew-chan8]

---