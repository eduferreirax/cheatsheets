# FFUF Cheat Sheet — Web Fuzzing

## Basic Syntax

```bash
ffuf -u http://target/FUZZ -w wordlist.txt
```

---

## Core Parameters

| Flag  | Description                            |
| ----- | -------------------------------------- |
| `-u`  | Target URL (use `FUZZ` as placeholder) |
| `-w`  | Wordlist                               |
| `-c`  | Enable colored output                  |
| `-t`  | Number of concurrent threads           |
| `-mc` | Match HTTP status codes                |
| `-fc` | Filter HTTP status codes               |
| `-fs` | Filter by response size                |
| `-fl` | Filter by number of lines              |
| `-fw` | Filter by number of words              |

---

## Basic Directory Fuzzing

```bash
ffuf -u http://target/FUZZ -w fuzz.txt -c -t 100
```

---

## Filtering Noise

### Filter by lines (common 404 behavior)

```bash
ffuf -u http://target/FUZZ -w fuzz.txt -fl 1
```

### Filter by response size

```bash
ffuf -u http://target/FUZZ -w fuzz.txt -fs 4242
```

### Match useful responses only

```bash
ffuf -u http://target/FUZZ -w fuzz.txt -mc 200,301,302,403
```

---

## File Fuzzing

```bash
ffuf -u http://target/FUZZ.php -w wordlist.txt
```

```bash
ffuf -u http://target/FUZZ.txt -w wordlist.txt
```

---

## Parameter Fuzzing

```bash
ffuf -u "http://target/index.php?FUZZ=test" -w params.txt
```

Used to discover hidden GET parameters.

---

## Subdomain / Virtual Host Fuzzing

```bash
ffuf -u http://target \
-H "Host: FUZZ.target.com" \
-w subdomains.txt
```

⚠️ This is **virtual host fuzzing**, not DNS brute force.

---

## Real Example (WebFlow Lab)

```bash
ffuf -u http://webflow.hc/ \
-H "Host: FUZZ.webflow.hc" \
-w subdomains-top1million.txt \
-c -t 100 -fl 8
```

---

## Bug Bounty Common Command

```bash
ffuf -u http://target/FUZZ \
-w /usr/share/seclists/Discovery/Web-Content/common.txt \
-mc 200,301,302,403 \
-t 100 \
-c
```

---

## Pentester Tip (VERY IMPORTANT)

Before fuzzing, always analyze the baseline response:

```bash
curl http://target
```

Check:

* Response size
* Number of lines
* Number of words

Then apply filters:

```bash
-fl  (lines)
-fs  (size)
-fw  (words)
```

This avoids false positives and speeds up enumeration.

---

## Key Concepts

### Threads (`-t`))

Controls concurrency (parallel requests), not exact requests per second.

### Filtering vs Matching

* `-mc` → show only matching status codes
* `-fc` → hide specific status codes
* `-fs`, `-fl`, `-fw` → remove noise based on response similarity

---

## Quick Mental Model

```text
FUZZ → replace with payload
↓
Send request
↓
Analyze response
↓
Filter noise
↓
Keep interesting results
```

---

## Pro Workflow

1. `curl target`
2. Identify baseline response
3. Run ffuf without filters
4. Observe patterns
5. Apply filters (`-fs`, `-fl`, `-fw`)
6. Narrow results

---

## Final Tip

Good fuzzing is not about sending more requests —
it's about **filtering correctly and thinking like the application**.
