# Linux Core Concepts

## 1. Core Components of Linux

Linux follows the **ASK** principle:

| Component | Description |
|-----------|-------------|
| **Application** | User-facing applications |
| **Shell** | The interactive layer between the user and the kernel |
| **Kernel** | The core computing program understood by the machine — user code (e.g., C programs) is compiled into binaries the kernel executes |
| **User Space** | A protected memory area where all non-kernel applications and user processes execute with limited privileges |
| **Init / systemd** | The first process started by the kernel during boot (PID 1); functions as the default init system and parent of all processes |

---

## 2. How Processes Are Created and Managed

Processes are created using the **fork() and exec() model** and managed by the kernel through states, scheduling, and signals.

### Creation

- **`fork()`** — A running process (the *parent*) uses `fork()` to create a near-identical copy of itself called the *child process*.
- **`exec()`** — After `fork()`, the child typically calls an `exec()` variant (e.g., `execve`, `execlp`) to replace its memory space with a new program. The PID stays the same, but the process now runs different code from its entry point.

### Process States

| State | Symbol | Description |
|-------|--------|-------------|
| Running | `R` | Executing on the CPU or ready to be scheduled |
| Sleeping | `S` | Waiting for an event or resource (e.g., I/O, signal, timer) |
| Stopped | `T` | Suspended, usually via `Ctrl+Z` or `SIGSTOP` |
| Zombie | `Z` | Finished execution, but the parent hasn't called `wait()` to collect its exit status |
| Terminated | — | Fully finished; all resources and process table entries removed |

---

## 3. What systemd Does and Why It Matters

**systemd** is both an **init system** and a **service manager** for Linux.

- It holds **PID 1** — the first process the kernel hands control to after boot.
- It reads configuration files, activates mount points and paths, and starts unit files according to their dependencies.
- Its goal is to bring the system to a defined target state:
  - `multi-user.target` — for servers
  - `graphical.target` — for desktops

### Boot Sequence

1. Hardware checks via BIOS/UEFI
2. Bootloader (e.g., GRUB) loads the kernel and initial RAM disk
3. Kernel starts and passes control to **systemd**
4. systemd acts as the "conductor," orchestrating all subsequent startup

### `systemctl`

`systemctl` is the primary CLI tool for interacting with systemd. Use it to start, stop, enable, disable, and inspect services and units.

```bash
systemctl start <service>    # Start a service
systemctl stop <service>     # Stop a service
systemctl enable <service>   # Enable at boot
systemctl disable <service>  # Disable at boot
systemctl status <service>   # Check service status
```

---

## 4. Five Daily-Use Commands

| Command | Purpose |
|---------|---------|
| `cd` | Change directory |
| `mv` | Move or rename a file |
| `mkdir` | Create a new directory |
| `ip addr` | Show the IP address of the system |
| `ping <website>` | Test network connectivity to a host |

---

## 5. Key Man Page Commands

| Command | Description |
|---------|-------------|
| `ps` | Displays a snapshot of current processes and their status |
| `top` | Provides a real-time, dynamic view of running processes and system resource usage |
| `kill` | Sends signals to a process by PID (e.g., `SIGTERM` for graceful termination, `SIGKILL` for forceful) |
| `systemctl` | Primary utility to interact with and manage the systemd init system |
| `jobs` | Lists background jobs in the current shell session |
