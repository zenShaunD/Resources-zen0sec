# Wireshark Quick Reference Card – Essential Filters

## 1. Noise Elimination – Clear the Clutter

**Filter:**
```
!(arp || icmp || ssdp || mdns || llmnr || nbnns)
```

**What it does:**  
Hides broadcast and background noise – ARP, ICMP (ping), UPnP (SSDP), Bonjour (mDNS), Windows Link-Local Multicast Name Resolution (LLMNR), and NetBIOS (NBNS).

**Why use it:**  
On a busy network, these protocols can make up over 80% of captured packets. Removing them leaves only routable traffic worth investigating.

**Critical notes:**  
- Add `&& !(dhcp || stp)` if DHCP or Spanning Tree Protocol packets clutter your view.  
- The `!( ... )` negates everything inside. Order of protocols does not matter.  
- This filter does not hide DNS or TCP/UDP traffic.

---

## 2. Port Scan Detection – Spot the Sweep

**Filter:**
```
tcp.flags.syn==1 && tcp.flags.ack==0 && tcp.window_size<=1024
```

**What it does:**  
Finds TCP SYN packets that are *not* replies (ACK=0) and have a tiny TCP window size (<=1024). This is a classic fingerprint of tools like Nmap.

**Why use it:**  
Legitimate clients use normal window sizes (typically 8192 or higher). Port scanners often send a flood of SYNs with a small, fixed window to minimise overhead.

**Critical notes:**  
- Combine with `&& tcp.srcport<1024` to also detect scans originating from privileged ports.  
- Window size can be spoofed – correlate with packet rate (e.g., >100 SYNs per second).  
- For vertical scans (many ports to one IP), add `ip.dst==<target>`.

---

## 3. Host Isolation – Stalk One IP

**Filter:**
```
ip.addr==192.168.1.23 && (tcp || udp) && !dns
```

**What it does:**  
Shows all TCP and UDP traffic to/from a specific IP address, while suppressing DNS queries and responses.

**Why use it:**  
When a single host looks suspicious, focus entirely on its connections. Removing DNS noise lets you see actual data transfers, handshakes, and beacons.

**Critical notes:**  
- Replace `!dns` with `!icmp && !arp` if you still see ping traffic.  
- Use `ip.src==x.x.x.x` to see only packets *from* that host, or `ip.dst==x.x.x.x` for only packets *to* it.  
- To also exclude broadcast traffic, add `&& !(dst net 255.255.255.255)`.

---

## 4. Suspicious Outbound – Unusual Ports

**Filter:**
```
tcp.flags.syn==1 && !(ip.dst==192.168.0.0/16) && tcp.dstport!=80 && tcp.dstport!=443
```

**What it does:**  
Captures new outbound TCP connection attempts (SYN packets) to destinations outside the local /16 network, on ports other than 80 (HTTP) and 443 (HTTPS).

**Why use it:**  
Reverse shells, command-and-control (C2) beacons, and malware often use non‑standard ports like 4444, 6666, 31337, 9001, or high ephemeral ports (49152–65535).

**Critical notes:**  
- Extend the exclusion list: `&& tcp.dstport!=53 && tcp.dstport!=22` for DNS and SSH.  
- If you monitor a /8 or /24, adjust the private range: e.g., `!(ip.dst==10.0.0.0/8)`.  
- For UDP‑based C2, replace `tcp.flags.syn==1` with `udp`.

---

## 5. Data Exfiltration – Large, Fast Pushes

**Filter:**
```
tcp.len>1400 && !(ip.dst==192.168.0.0/16) && tcp.flags.push==1
```

**What it does:**  
Shows TCP packets with a payload larger than 1400 bytes, destined outside the local network, and with the PSH (push) flag set.

**Why use it:**  
Data exfiltration sends large chunks of data in rapid bursts. The PSH flag tells the receiving OS to deliver data immediately (no buffering). This combination is a strong indicator of bulk data leaving your network.

**Critical notes:**  
- Maximum safe data payload per packet is usually 1460 bytes (1500 MTU minus 40 bytes for TCP/IP headers).  
- Multiple PSH frames from the same source in under 0.1 seconds is highly suspicious.  
- Add a time filter to isolate bursts: `&& frame.time_delta < 0.01`.

---

## 6. DNS Anomaly / Tunneling – Long Queries

**Filter:**
```
dns.flags.response==0 && dns.qry.name && frame.len>80
```

**What it does:**  
Shows DNS queries (not responses) where the full Ethernet frame length exceeds 80 bytes. This catches abnormally long domain names.

**Why use it:**  
DNS tunneling encodes data (e.g., base64, hex) into subdomains of a malicious domain. Legitimate DNS queries rarely exceed 60 bytes. Long, repetitive queries with high entropy indicate C2 or data theft over DNS.

**Critical notes:**  
- To whitelist known long names: `&& !(dns.qry.name contains "windowsupdate")`.  
- Check TXT record responses as well: `dns.resp.type==16 && frame.len>200`.  
- For tunneling detection, also filter by query rate: `dns.qry.name matches "[a-zA-Z0-9+/=]{20,}"` catches base64‑like strings.

---

## 7. Credential Hunt – Plaintext Passwords

**Filter:**
```
http.request.method=="POST" && (frame contains "pass" || frame contains "user" || frame contains "login")
```

**What it does:**  
Finds HTTP POST requests whose raw payload contains common credential‑related keywords (`pass`, `user`, `login`). Catches plaintext form submissions.

**Why use it:**  
Legacy applications, internal tools, and misconfigured web apps often send username/password pairs over unencrypted HTTP. This filter surfaces them immediately.

**Critical notes:**  
- This filter does *not* catch Basic Authentication (`Authorization: Basic ...`) – use `frame contains "Authorization: Basic"` for that.  
- It also does not decrypt HTTPS. For TLS‑encrypted traffic, you need the private key or a decryption proxy.  
- Real passwords will appear in clear text – use this filter with caution and only on networks you own.

---

## 8. Reverse Shell Beaconing – Zero‑Window Keepalives (Bonus)

**Filter:**
```
tcp.flags.ack==1 && tcp.len==0 && tcp.window_size==0
```

**What it does:**  
Shows TCP ACK packets that carry no data (`tcp.len==0`) and announce a zero receive window (`tcp.window_size==0`). This pattern is used by some reverse shells to send periodic keepalives when they cannot send actual data.

**Why use it:**  
Malware that is waiting for commands often sends empty ACKs with a zero window to signal “I’m still alive but can’t receive more data yet.” Legitimate connections rarely exhibit this pattern for extended periods.

**Critical notes:**  
- High false positive potential during slow file transfers or when a receiver is genuinely overwhelmed.  
- Reduce false positives by excluding your internal DNS servers: `&& ip.dst!=your_dns_ip`.  
- Combine with time analysis: `&& frame.time_delta > 5` (beacon every 5 seconds).

---

## Pro Tips

| Filter Component               | Meaning / Use Case                                     |
|--------------------------------|--------------------------------------------------------|
| `tcp.flags.push==1`            | Sender flushes buffer – data must go now.              |
| `tcp.analysis.ack_rtt>0.2`    | Delayed ACK – possible data staging or latency issue. |
| `frame.time_delta>5`           | Gap between packets – potential beacon interval.      |
| `dns.qry.name matches "[a-zA-Z0-9+/=]{20,}"` | Regex for base64 in DNS names (tunneling).  |
| `!(ip.dst==192.168.0.0/16)`    | Excludes local /16 range – focus on external traffic.  |

**Workflow recommendation:**  
Noise elimination → Host isolation → Port scan check → Suspicious outbound ports → DNS long queries → Data exfiltration pushes → Credential hunt.  

Save these filters as **buttons** in Wireshark (`Analyze → Display Filter Buttons`) for one‑click threat hunting.
```
