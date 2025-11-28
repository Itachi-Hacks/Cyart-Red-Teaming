# Binary Exploitation: Stack Overflow Demo

This repository demonstrates a classic stack-based buffer overflow in a deliberately vulnerable C program. It includes the vulnerable source code, compilation steps, binary analysis commands, debugging workflow, and exploitation methodology.

---

## 🔥 Overview

The project showcases how an unbounded `strcpy()` call can overwrite stack memory, allowing control of EIP and redirection of program execution. It is intended for **learning purposes only**.

---

## 📁 Repository Structure

```
├── vulnerable.c          # Source code with stack overflow vulnerability
├── vulnerable            # Compiled binary (optional)
└── README.md             # Documentation
```

---

## ⚙️ Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
```

### 2. Compile the vulnerable binary

```bash
gcc -fno-stack-protector -z execstack -no-pie -g -o vulnerable vulnerable.c
```

### 3. Verify the build

```bash
ls
```

You should see: `vulnerable`, `vulnerable.c`, `README.md`.

---

## 🛠️ Compilation (Disable Protections)

Compile the program with stack protections disabled to demonstrate exploitation:

```bash
gcc -fno-stack-protector -z execstack -no-pie -g -o vulnerable vulnerable.c
```

---

## 🔍 Binary Analysis & Usage Documentation

Search for useful values such as passwords, flags, or strings:

```bash
strings vulnerable | grep -i -E 'password|admin|key|flag'
```

Test basic overflow behavior:

```bash
./vulnerable $(python3 -c "print('A' * 100)")
```

---

### 🐞 GDB Debugging

Start debugging:

```bash
gdb ./vulnerable
```

Run with overflow input:

```gdb
run $(python3 -c "print('A' * 100)")
info registers eip
x/20wx $esp
```

---

### 💥 Exploitation Workflow

### 1. Identify Offset to EIP

```bash
msf-pattern_create -l 100
msf-pattern_offset -q EIP_VALUE
```

### 2. Confirm EIP Control

```bash
./vulnerable $(python3 -c "print('A'*76 + 'BBBB')")
```

---

## 📘 Technical Summary (50 Words)

1. The program uses unbounded `strcpy()`, allowing writes past a 64‑byte buffer.
2. EIP control occurs at **76 bytes** (64 buffer + 8 EBP + 4 EIP).
3. With no stack protections (no canaries, executable stack, no PIE), exploitation is reliable and demonstrates a classic stack overflow vulnerability.

---

## ⚠️ Disclaimer

This repository is for **educational and research purposes only**. Do not use these techniques on systems you do not own or have permission to test.

---

## 🔬 Radare2 Analysis

Radare2 can also be used for binary inspection, disassembly, and stack analysis.

### Start radare2

```bash
r2 vulnerable
```

### Basic commands inside radare2

```bash
aaa         # Analyze all
afl         # List functions
pdf @ main  # Disassemble main function
px 100 @ esp  # Inspect stack memory
```

### Run the program with input

```bash
r2 -d vulnerable
```

Inside debug mode:

```bash
aeip        # Show current EIP
dc          # Continue execution
ds          # Step instruction
```

### Set breakpoint and inspect registers

```bash
db main
pdf @ main
ar          # Show registers
```

---

## 🧾 Main Commands Cheat Sheet

### Build / Setup

```bash
gcc -fno-stack-protector -z execstack -no-pie -g -o vulnerable vulnerable.c
```

### Analysis

```bash
strings vulnerable | grep -i -E 'password|admin|key|flag'
./vulnerable $(python3 -c "print('A' * 100)")
```

### GDB

```bash
gdb ./vulnerable
```

Inside GDB:

```gdb
run $(python3 -c "print('A' * 100)")
info registers eip
x/20wx $esp
```

### Exploitation

```bash
msf-pattern_create -l 100
msf-pattern_offset -q <EIP_VALUE>
./vulnerable $(python3 -c "print('A'*76 + 'BBBB')")
```
