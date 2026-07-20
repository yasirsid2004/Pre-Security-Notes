# Pre-Security-Notes
These are the notes that i made during i was learning and going through the pre security path of cybersecurity.

 Day 1 
Module 1 — Introduction to Cybersecurity
Offensive Security
Think like an attacker. Find the weakness before someone bad does — with permission.

Scope — exact systems + actions allowed. Outside scope = off limits. Vulnerability — weakness in a system Exploit — technique that takes advantage of that weakness Enumeration — collecting info (users, ports, services) to find weak spots Dictionary attack — trying a wordlist to guess passwords
Defensive Security
Prevention — firewalls, antivirus, patching Detection — logs, alerts, SIEM Mitigation — isolate systems, block traffic, disable accounts mid-attack Analysis — what happened, how, which systems Response + Improve — recover, fix the root cause

Careers in Cybersecurity
Red Team / Pen Tester — simulate attacks, find flaws legally SOC Analyst — monitor, detect, respond in real time Threat Intelligence — research attacker behaviour, predict moves Security Engineer — build and maintain security infrastructure Malware Analyst — reverse-engineer malicious software Incident Responder — called in after something’s already gone wrong

 Day 2
Module 2 — Network Fundamentals
What is Networking?
IPv4–32-bit, 4 octets, ²³² addresses e.g. 86.157.52.21 IPv6 — 128-bit, 8 groups, 2¹²⁸ addresses e.g. 2a00:22c4:a531:c500:425f:cce6:c36b:f64d (created because IPv4 ran out) MAC — 12-char hex, hardware-level ID e.g. a4:c3:f0:85:ac:2d — first 6 = manufacturer, last 6 = device

Ping — ICMP packets, tests connection between two devices, measures round-trip time and packet loss

ping 8.8.8.8   /   ping google.com
Intro to LAN
Star — all devices connect to a central switch/hub. Most common. Switch fails = everything fails. Bus — all share one backbone cable. Can’t handle heavy load. Ring — devices in a loop. One broken cable = whole ring down.

Network address — identifies the network e.g. 192.168.1.0 Host address — identifies a device e.g. 192.168.1.100 Default gateway — address devices use to send traffic outside their network (usually the router)

ARP — Address Resolution Protocol. Maintains a table of IP → MAC mappings.

ARP Request — broadcasts “who has 192.168.1.100?” to all
ARP Reply — that device responds with its MAC
Attack: ARP Spoofing — fake replies poison the table
DHCP — auto-assigns IPs when a device joins a network. Flow = DORA:

Discover — device: “I need an IP”
Offer — server offers one
Request — device confirms
ACK — server locks it in
OSI Model
7 layers. Every exam, every interview.

# Layer What happens 7 Application DNS, HTTPS, SSH, FTP, SMTP 6 Presentation encryption, encoding, compression 5 Session connection setup, session handling, checkpoints 4 Transport TCP/UDP, ports, segmentation, reliability 3 Network IP addressing, routing (OSPF, RIP) — packets 2 Data Link MAC, frames, error detection (CRC), ARP — frames 1 Physical cables, electrical signals, binary

Bottom to top: Please Do Not Throw Sausage Pizza Away

Packets & Frames
Packet — Layer 3 data unit. IP header + payload. Frame — Layer 2 data unit. Packet + MAC address. TTL — timer on a packet. Decrements each hop. Hits 0 = dropped.

TCP Three-Way Handshake:

Client → SYN       (want to connect)
Server → SYN/ACK   (acknowledged)
Client → ACK       (let's go)
After: DATA transfers → FIN closes cleanly → RST = abrupt end (error/low resources)

TCP — reliable, ordered, error-checked → web, email, file transfer UDP — fast, no guarantee → streaming, gaming, DNS

TCP headers: Source IP, Destination IP, Source port, Destination port, Checksum, Flag, Data UDP headers: TTL, Source IP, Destination IP, Source port, Destination port, Data

Extending Your Network
Intranet — private internal network, same org Firewall:

Stateful — tracks the full connection, smarter
Stateless — checks packets against rules only. No exact match = allows.
VPN types:

PPP — auth + basic encryption between two direct endpoints. Non-routable.
PPTP — tunnels PPP traffic across networks. Easy but weak encryption.
IPSec — encrypts and authenticates IP packets at network layer. Strong, complex config.
Press enter or click to view image in full size

Module 3 — How the Web Works
DNS in Detail
Domain reads right → left: admin.tryhackme.com

com → TLD
tryhackme → SLD (Second Level Domain)
admin → subdomain
gTLD — generic: .com .org .net ccTLD — country: .in .uk .co.uk Rules: a-z, 0-9, - only. Each label ≤ 63 chars, total ≤ 253.

DNS Records:

A → IPv4
AAAA → IPv6
CNAME → alias, one domain points to another (CDN, third-party services)
MX → mail server + priority
TXT → SPF, DMARC, domain verification
nslookup --type=CNAME shop.website.thm
nslookup --type=MX website.thm
nslookup --type=TXT website.thm
DNS lookup flow:

Local cache → Recursive DNS (ISP) → Root server → TLD server → Authoritative server
Recursive — finds the answer by asking around Authoritative — actually stores the record, gives the final answer TTL — how long the response is cached before querying again

DNS = Layer 7, uses UDP (fast) / TCP (large responses) Attacks: DNS Spoofing, Cache Poisoning, DNS Tunneling

HTTP in Detail
URL: https://tryhackme.com/room/index

Scheme — https (protocol used)
Host — tryhackme.com
Path — /room/index (resource requested, / = index.html)
Localhost = 127.0.0.1
HTTP Methods (9 core): GET — fetch, params in URL POST — send data, body carries payload PUT — create/replace PATCH — partial update DELETE — remove HEAD — headers only, no body OPTIONS — what methods does the server support? CONNECT — create a tunnel (proxies) TRACE — diagnostic, echoes request back


How Websites Work
Response = header (metadata: content-type, cookies, 
HTML — structure. Skeleton — headings, forms, links. CSS — style. Colours, fonts, layout. JavaScript — behaviour. Interaction, fetch data, validate forms without page reload. Backend — server-side logic + database. Processes logins, stores data.

Note: HTML comments can leak paths, JS files expose endpoints, unsanitised forms = injection targets.

Putting It All Together
What happens when you type tryhackme.com and hit Enter:

DNS lookup → TCP handshake → TLS (HTTPS) → HTTP GET → Server responds → Browser renders
Every step = a potential attack surface.

