# OS Jackfruit – Mini Container Runtime

## 📌 Overview

This project demonstrates a mini container runtime with kernel-level monitoring.
It implements process isolation, container lifecycle management, logging, and memory control using a kernel module.

---

## 🚀 Features

* Container creation using `chroot`
* Multi-container support
* Supervisor-based architecture
* Logging of container output
* CPU scheduling using nice values
* Kernel module for memory monitoring
* Soft and hard memory limits enforcement

---

## 🧠 Architecture

### 1. User Space (Engine)

* `engine.c` handles:

  * start
  * stop
  * logs
  * ps
* Uses UNIX socket for communication with supervisor

### 2. Supervisor

* Long-running process
* Manages containers
* Tracks container states and PIDs

### 3. Kernel Module

* `monitor.c`
* Tracks memory usage of containers
* Enforces:

  * Soft limits (warnings)
  * Hard limits (restrictions)

---

## 📂 Files

* `engine.c` – Container runtime logic
* `monitor.c` – Kernel module
* `monitor_ioctl.h` – IOCTL interface
* `cpu_hog.c` – CPU stress workload
* `memory_hog.c` – Memory stress workload
* `Makefile` – Build instructions
* `README.md` – Documentation

---

## ⚙️ Setup & Execution

### 1. Build

```bash
make
make module
```

---

### 2. Load Kernel Module

```bash
sudo insmod monitor.ko
```

---

### 3. Start Supervisor

```bash
sudo ./engine supervisor ../rootfs-base
```

---

### 4. Run Container

```bash
sudo ./engine start demo ../rootfs-demo "/cpu_hog 5"
```

---

### 5. View Logs

```bash
sudo ./engine logs demo
```

---

### 6. Multi-Container

```bash
sudo ./engine start alpha ../rootfs-alpha "/cpu_hog 5"
sudo ./engine start beta ../rootfs-beta "/cpu_hog 5"
```

---

### 7. Memory Test

```bash
sudo ./engine start mem ../rootfs-demo "/memory_hog 200" --soft-mib 16 --hard-mib 32
sudo dmesg | tail -n 20
```

---

## 📊 Concepts Used

* Process isolation (`chroot`)
* Linux system calls (`fork`, `exec`)
* UNIX domain sockets
* IOCTL communication
* Kernel module programming
* Memory monitoring

---

## 🎯 Conclusion

This project demonstrates how container runtimes work internally by combining user-space management with kernel-level monitoring.

---

## 👨‍💻 Author

* Name: Bhuvi Bagga
* Course: Operating Systems
