# System-Call-with-xv6
Academic Project of CSE323 Operating System

* **Group Members:
* **Hasnat Karibul Islam** – NSU ID: 2211275042
* **Qm Asif Tanjim** – NSU ID: 2211402042
* **Nowren Mah Jabin Khan** – NSU ID: 2211129642


---

# 🖥️ xv6 Kernel Extensions: Process Statistics & Heap Growth Tracking

## 📌 Project Overview

This project extends the **xv6 operating system kernel** by implementing two new system calls to enhance **process observability, debugging, and learning**:

1. **Process Statistics System Call (`getpinfo`)**
2. **Heap Growth Tracking System Call (`hapinfo`)**

The default xv6 kernel provides limited visibility into runtime process behavior and dynamic memory usage. This project addresses those limitations by exposing structured, kernel-level insights to user space.

📘 **Course:** CSE 323 – Operating Systems Design
🏫 **University:** North South University (NSU)
📅 **Semester:** Fall 2025
👨‍🏫 **Instructor:** Md. Salman Shamil



---

## 🎯 Project Objectives

* Improve **kernel transparency** for debugging and experimentation
* Track **heap memory changes** over time per process
* Provide **global process statistics** in a single system call
* Strengthen understanding of **kernel data structures, locking, and system calls**



---

## 🧩 Implemented System Calls

### 1️⃣ Heap Growth Tracking System Call – `hapinfo`

Tracks every heap modification made using `sbrk()` by a process.

**Features:**

* Logs heap expansion/shrinkage events
* Records the exact system timestamp (`ticks`)
* Maintains a chronological history per process
* Exposes data safely to user space

**Use Case:**
Memory debugging, heap behavior analysis, educational inspection of dynamic memory allocation.

---

### 2️⃣ Process Statistics System Call – `getpinfo`

Returns system-wide process statistics in a structured format.

**Includes:**

* Process ID (PID)
* Process state
* Memory size
* Scheduling-related metadata (where applicable)

**Use Case:**
Monitoring system health, analyzing scheduling behavior, building user-level monitoring tools.



---

## 🛠️ Kernel Modifications

### 🔧 New / Modified Files

#### For `hapinfo`

* `hapinfo.h` – Defines `struct haplog` and `struct hapinfo`
* `kernel/proc.h` – Extended `struct proc` with:

  * `haplog[HAPLOG_SIZE]`
  * `haplog_index`
* `kernel/sysproc.c`

  * Modified `sys_sbrk()` to log heap events
  * Implemented `sys_hapinfo()` system call

#### For `getpinfo`

* `pstat.h` – Defines `struct pstat`
* `kernel/proc.h` – Extended process structure for statistics
* `kernel/sysproc.c`

  * Implemented `sys_getpinfo()` with proper locking
* Updated:

  * `syscall.h`
  * `syscall.c`
  * `usys.pl`



---

## 🔒 Concurrency & Safety

* Used **spinlocks** to protect:

  * System time (`tickslock`)
  * Global process table
* Ensured safe kernel → user memory transfer using `copyout()`
* Minimized lock holding time to reduce kernel latency



---

## 🧪 User-Level Test Programs

Two user programs were written to validate functionality:

### 🧪 `hapinfo_test.c`

* Performs multiple `sbrk()` calls
* Retrieves and prints heap operation logs
* Confirms correct ordering and timestamps

### 🧪 `pstat_test.c`

* Spawns multiple processes
* Calls `getpinfo()`
* Displays a formatted table of active processes

**Results:**
Both system calls were validated successfully and demonstrated correct kernel behavior.



---

## 📈 Outcomes & Improvements

### ✅ Achievements

* Enhanced xv6 kernel observability
* Accurate heap operation tracking
* Reliable global process statistics
* Improved debugging and learning support

### 🔮 Future Improvements

* Reduce `getpinfo` lock contention (e.g., RCU or fine-grained locking)
* Replace fixed-size heap log with dynamic or circular buffer
* Add system calls to reset or clear heap logs



---

## 👥 Team Members

* **Hasnat Karibul Islam** – NSU ID: 2211275042
* **Qm Asif Tanjim** – NSU ID: 2211402042
* **Nowren Mah Jabin Khan** – NSU ID: 2211129642

All members contributed equally to:

* Kernel structure modification
* System call integration
* Core implementation
* Testing & validation



---

## 📜 License

This project is intended for **educational purposes** as part of an academic course.
Feel free to fork and experiment with the code for learning and research.

---

