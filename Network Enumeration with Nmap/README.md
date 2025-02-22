**Windows Default TTL:** `128`

**Linux Default TTL:** `64`

### OS without -O Flag:

```jsx
radeel@htb[/htb]$ sudo nmap 10.129.2.18 -sn -oA host -PE --packet-trace --disable-arp-ping 

Starting Nmap 7.80 ( https://nmap.org ) at 2020-06-15 00:12 CEST
SENT (0.0107s) ICMP [10.10.14.2 > 10.129.2.18 Echo request (type=8/code=0) id=13607 seq=0] IP [ttl=255 id=23541 iplen=28 ]
RCVD (0.0152s) ICMP [10.129.2.18 > 10.10.14.2 Echo reply (type=0/code=0) id=13607 seq=0] IP [ttl=128 id=40622 iplen=28 ]
Nmap scan report for 10.129.2.18
Host is up (0.086s latency).
MAC Address: DE:AD:00:00:BE:EF
Nmap done: 1 IP address (1 host up) scanned in 0.11 seconds
```

***OS -> Windows***
### How I determined this:

- **TTL (Time-To-Live) Value:**
    
    - The ICMP Echo Reply from `10.129.2.18` has a TTL of **128**.
    - Windows systems typically use a default TTL of **128**.
    - Linux systems usually have a TTL of **64**, and other systems like Cisco routers use **255**.

---

### Port States:

There are a total of 6 different states for a scanned port we can obtain:

| **State**          | **Description**                                                                                                                                                                                         |
| ------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `open`             | This indicates that the connection to the scanned port has been established. These connections can be **TCP connections**, **UDP datagrams** as well as **SCTP associations**.                          |
| `closed`           | When the port is shown as closed, the TCP protocol indicates that the packet we received back contains an `RST` flag. This scanning method can also be used to determine if our target is alive or not. |
| `filtered`         | Nmap cannot correctly identify whether the scanned port is open or closed because either no response is returned from the target for the port or we get an error code from the target.                  |
| `unfiltered`       | This state of a port only occurs during the **TCP-ACK** scan and means that the port is accessible, but it cannot be determined whether it is open or closed.                                           |
| `open\|filtered`   | If we do not get a response for a specific port, `Nmap` will set it to that state. This indicates that a firewall or packet filter may protect the port.                                                |
| `closed\|filtered` | This state only occurs in the **IP ID idle** scans and indicates that it was impossible to determine if the scanned port is closed or filtered by a firewall.                                           |

---

### Style Sheets

With the XML output, we can easily create HTML reports that are easy to read, even for non-technical people. This is later very useful for documentation, as it presents our results in a detailed and clear way. To convert the stored results from XML format to HTML, we can use the tool `xsltproc`.

  Saving the Results

```jsx
radeel@htb[/htb]$ xsltproc target.xml -o target.html
```

If we now open the HTML file in our browser, we see a clear and structured presentation of our results.

---

### Result Formats:

`Nmap` can save the results in 3 different formats.

- Normal output (`-oN`) with the `.nmap` file extension
- Grepable output (`-oG`) with the `.gnmap` file extension
- XML output (`-oX`) with the `.xml` file extension

---

### NSE(Nmap Scripting Engine):

There are a total of 14 categories into which these scripts can be divided:

|**Category**|**Description**|
|---|---|
|`auth`|Determination of authentication credentials.|
|`broadcast`|Scripts, which are used for host discovery by broadcasting and the discovered hosts, can be automatically added to the remaining scans.|
|`brute`|Executes scripts that try to log in to the respective service by brute-forcing with credentials.|
|`default`|Default scripts executed by using the `-sC` option.|
|`discovery`|Evaluation of accessible services.|
|`dos`|These scripts are used to check services for denial of service vulnerabilities and are used less as it harms the services.|
|`exploit`|This category of scripts tries to exploit known vulnerabilities for the scanned port.|
|`external`|Scripts that use external services for further processing.|
|`fuzzer`|This uses scripts to identify vulnerabilities and unexpected packet handling by sending different fields, which can take much time.|
|`intrusive`|Intrusive scripts that could negatively affect the target system.|
|`malware`|Checks if some malware infects the target system.|
|`safe`|Defensive scripts that do not perform intrusive and destructive access.|
|`version`|Extension for service detection.|
|`vuln`|Identification of specific vulnerabilities.|

---

- `-T 0` / -T paranoid
- `-T 1` / -T sneaky
- `-T 2` / -T polite
- `-T 3` / -T normal
- `-T 4` / -T aggressive
- `-T 5` / -T insane

### EASY - IDS IPS and Firewall Evasion Command: 

```jsx
sudo nmap -sV 10.129.78.188
```

### MEDIUM - IDS IPS and Firewall Evasion Command: 

```jsx
sudo nmap -sSU -p 53 --script dns-nsid 10.129.2.48
```

### HARD - IDS IPS and Firewall Evasion Command: 

```jsx
sudo ncat -nv -p 53 10.129.173.161 50000
```

### Scanning Options
- `10.10.10.0/24`:	Target network range.
- `-sn`:	Disables port scanning.
- `-Pn`:	Disables ICMP Echo Requests
- `-n`:	Disables DNS Resolution.
- `-PE`:	Performs the ping scan by using ICMP Echo Requests against the target.
- `--packet-trace`:	Shows all packets sent and received.
- `--reason`:	Displays the reason for a specific result.
- `--disable-arp-ping`:	Disables ARP Ping Requests.
- `--top-ports=<num>`:	Scans the specified top ports that have been defined as most frequent.
- `-p-`:	Scan all ports.
- `-p22-110`:	Scan all ports between 22 and 110.
- `-p22,25`:	Scans only the specified ports 22 and 25.
- `-F`:	Scans top 100 ports.
- `-sS`:	Performs an TCP SYN-Scan.
- `-sA`:	Performs an TCP ACK-Scan.
- `-sU`:	Performs an UDP Scan.
- `-sV`:	Scans the discovered services for their versions.
- `-sC`:	Perform a Script Scan with scripts that are categorized as "default".
- `--script <script>`:	Performs a Script Scan by using the specified scripts.
- `-O`:	Performs an OS Detection Scan to determine the OS of the target.
- `-A`:	Performs OS Detection, Service Detection, and traceroute scans.
- `-D`: RND:5	Sets the number of random Decoys that will be used to scan the target.
- `-e`:	Specifies the network interface that is used for the scan.
- `-S`: 10.10.10.200	Specifies the source IP address for the scan.
- `-g`:	Specifies the source port for the scan.
- `--dns-server <ns>`:	DNS resolution is performed by using a specified name server.

### Output Options
- `-oA filename`:	Stores the results in all available formats starting with the name of "filename".
- `-oN filename`:	Stores the results in normal format with the name "filename".
- `-oG filename`:	Stores the results in "grepable" format with the name of "filename".
- `-oX filename`:	Stores the results in XML format with the name of "filename".

### Performance Options
- `--max-retries <num>`:	Sets the number of retries for scans of specific ports.
- `--stats-every=5s`:	Displays scan's status every 5 seconds.
- `-v/-vv`:	Displays verbose output during the scan.
- `--initial-rtt-timeout 50ms`:	Sets the specified time value as initial RTT timeout.
- `--max-rtt-timeout 100ms`:	Sets the specified time value as maximum RTT timeout.
- `--min-rate 300`:	Sets the number of packets that will be sent simultaneously.
- `-T <0-5>`:	Specifies the specific timing template.
