# MAC Address Changer (mchanger)

A Linux command-line utility written in **C** that allows you to **view, change, randomize, restore, and manage MAC addresses** of network interfaces using low-level system calls (`ioctl`, `ethtool`). This project is designed for learning and demonstrating Linux networking, device control, and CLI parsing.

 **Requires root privileges** to modify network interfaces.

---

## Features

* Change MAC address to a user-specified value
* Generate and apply a **valid random locally-administered MAC**
* Restore the **original (permanent) MAC address** using `ethtool`
* Print current or permanent MAC address
* Bring network interfaces **up or down**
* Check interface **status (UP/DOWN)**
* List all available network interfaces
* Robust MAC validation
* GNU-style **short and long command-line options**

---

## How It Works (Technical Overview)

* Uses `socket(AF_INET, SOCK_DGRAM, 0)` with `ioctl()` calls
* Modifies MAC addresses via `SIOCSIFHWADDR`
* Retrieves current MAC via `SIOCGIFHWADDR`
* Retrieves permanent MAC via `SIOCETHTOOL + ETHTOOL_GPERMADDR`
* Interface state controlled using `SIOCSIFFLAGS`
* Command-line parsing implemented with `getopt_long()`



---

## Build Instructions

```bash
make all
```

> Ensure required headers are available:
>
> * `<linux/if.h>`
> * `<linux/ethtool.h>`
> * `<sys/ioctl.h>`
> * `<ifaddrs.h>`

---

## Usage

```bash
sudo ./mchanger <interface> [options]
```

### Examples

Important: The network interface must be UP before performing most operations (changing, printing, or restoring MAC addresses).
If the interface is DOWN, bring it up first using the -u / --up option.


```bash
# List all interfaces
sudo ./mchanger -l

# Print current MAC
sudo ./mchanger eth0 -p current

# Print permanent MAC
sudo ./mchanger eth0 -P

# Change MAC to a specific value
sudo ./mchanger eth0 -c 02:11:22:33:44:55

# Change MAC to a random value
sudo ./mchanger eth0 -r

# Restore original MAC
sudo ./mchanger eth0 -R

# Bring interface down
sudo ./mchanger eth0 -d

# Bring interface up
sudo ./mchanger eth0 -u

# Check interface status
sudo ./mchanger eth0 -s
```

---

## Command-Line Options

| Option      | Long Option       | Description                   |                   |
| ----------- | ----------------- | ----------------------------- | ----------------- |
| `-c <MAC>`  | `--change <MAC>`  | Change MAC address            |                   |
| `-r`        | `--random`        | Generate and apply random MAC |                   |
| `-R`        | `--restore`       | Restore permanent MAC         |                   |
| `-p [type]` | `--print [current | permanent]`                   | Print MAC address |
| `-P`        | `--permanent`     | Print permanent MAC           |                   |
| `-s`        | `--status`        | Check interface status        |                   |
| `-d`        | `--down`          | Bring interface down          |                   |
| `-u`        | `--up`            | Bring interface up            |                   |
| `-l`        | `--list`          | List all interfaces           |                   |
| `-h`        | `--help`          | Show help message             |                   |

---

## MAC Address Rules Enforced

* Rejects invalid formats
* Rejects multicast MACs (I/G bit set)
* Rejects globally-administered MACs when randomizing
* Ensures locally-administered unicast MACs

---

## Security & Permissions

* Must be run as **root**
* Modifying MAC addresses may disrupt network connectivity
* Some drivers may not support changing MAC addresses

---

## Project Structure

```
.
├── mchanger.c        # MAC manipulation logic
├── command_parser.c # CLI parsing and helpers
├── mchanger.h       # Declarations and macros
├── main.c           # Entry point
```

---

## Learning Outcomes

This project demonstrates:

* Linux `ioctl()` programming
* Network interface management
* MAC address internals
* GNU-style CLI tools
* Defensive input validation
* Low-level C memory handling

Ideal for **systems programming**, **networking**, or **security-focused** portfolios.

---

## Disclaimer

This tool is intended for **educational and legitimate administrative use only**. The author is not responsible for misuse.

---


## Author

Lucas Onyiego

