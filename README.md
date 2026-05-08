# rustcounter

Author: Marleena Rose Limbrick

`rustcounter` is a Linux kernel module written in Rust. It creates a character device at `/dev/rustcounter` that stores a single atomic counter.

- Reading from the device prints the current counter value
- Writing to the device increments the counter

---

## Build

```bash
make clean && make
```

---

## Load the Module

```bash
sudo insmod rustcounter.ko
```

Verify that the device exists:

```bash
ls -la /dev/rustcounter
```

Example output:

```text
crw------- 1 root root 10, 263 May 8 01:25 /dev/rustcounter
```

---

## Testing the Counter

Read the initial value:

```bash
sudo cat /dev/rustcounter
```

Increment the counter:

```bash
echo bump | sudo tee /dev/rustcounter > /dev/null
```

Read again:

```bash
sudo cat /dev/rustcounter
```

Increment two more times:

```bash
echo bump | sudo tee /dev/rustcounter > /dev/null
echo bump | sudo tee /dev/rustcounter > /dev/null
```

Read the updated value:

```bash
sudo cat /dev/rustcounter
```

Expected output sequence:

```text
0
1
3
```

---

## Unload the Module

```bash
sudo rmmod rustcounter
```

---

## Implementation Notes

This project demonstrates:

- Writing Linux kernel modules in Rust
- Using the Linux `miscdevice` subsystem
- Creating character devices in kernel space
- Atomic synchronization with `AtomicU64`
- Safe kernel development patterns in Rust

The counter value is stored using atomic operations instead of mutexes, allowing thread-safe increments from multiple writes.

