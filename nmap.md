## Nmap Full Port Scan

Command used:

```bash
nmap -p- -Pn --open --min-rate 1200 172.16.7.199 -sVC -vvv
```

---

## Command Breakdown

### `-p-`

Scans **all TCP ports**.

By default, Nmap scans only the **top 1000 ports**.
Using `-p-` forces Nmap to scan the full range:

```
1 → 65535
```

This is important during penetration testing because vulnerable services may run on **non-standard ports**.

---

### `-Pn`

Tells Nmap to **skip host discovery**.

Normally Nmap performs a **ping/ICMP check** to verify if the host is alive.

Some machines block ICMP or ping requests, which may cause Nmap to report:

```
Host seems down
```

Using `-Pn` forces Nmap to assume the host is **alive** and proceed with the scan.

---

### `--open`

Displays **only open ports** in the results.

Without this flag, Nmap will show multiple port states:

* closed
* filtered
* open

Using `--open` makes the output cleaner by showing only:

```
open ports
```

---

### `--min-rate 1200`

Defines the **minimum number of packets per second** sent during the scan.

Without rate adjustments, scans can be slow (sometimes **20+ minutes**).

Example tuning options:

```
--min-rate 500
--min-rate 1000
--min-rate 1200
--min-rate 2000
```

Higher rates speed up the scan but may cause:

* packet loss
* missed responses
* false negatives

Use carefully depending on the environment.

---

### `-sVC`

This combines two flags:

#### `-sV`

Service version detection.

Attempts to identify the **software and version** running on each open port.

Example:

```
Apache 2.4.58
OpenSSH 9.6p1
```

#### `-C`

Runs **default NSE scripts** for initial enumeration.

Common examples:

```
http-title
http-methods
ssh-hostkey
ssl-cert
```

These scripts often provide valuable early information about services.

---

### `-vvv`

Increases **scan verbosity**.

Verbosity levels:

```
-v     verbose
-vv    more verbose
-vvv   maximum verbosity
```

Higher verbosity shows:

* scan progress
* discovered ports in real time
* additional diagnostic information
