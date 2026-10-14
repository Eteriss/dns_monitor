# DNS Monitor

A command-line DNS traffic analyzer written in C++. It captures DNS messages from a live network interface or reads them from a `.pcap` file, decodes them at the packet level, and prints the queries and responses in a readable form.

Built as part of the Network Applications course at Brno University of Technology (2024).

## Features

- **Live capture or offline analysis**: sniff DNS traffic on a network interface, or analyze an existing `.pcap` capture
- **Link layer detection**: supports Ethernet and Linux cooked capture (SLL)
- **IPv4 and IPv6** support
- **DNS record parsing** for A, AAAA, NS, MX, SOA, CNAME and SRV records
- **Compact or verbose output**: a one-line summary per message, or the full breakdown of every section (question, answer, authority, additional)
- **Domain extraction**: writes all observed domain names to a file
- **Resolution tracking**: writes domain-to-IP translations seen in responses to a file


## Project structure

| File | Purpose |
| --- | --- |
| `main.cpp` | Entry point |
| `arg_parser.cpp/.hpp` | Command-line argument parsing and validation |
| `dns_monitor.cpp/.hpp` | Packet capture, link/IP layer handling and DNS message parsing |
| `section.cpp/.hpp` | Parsing of DNS resource records |
| `packets/` | Sample `.pcap` files for testing |
| `manual.pdf` | Detailed documentation (in Slovak) |

## Requirements

- Linux
- C++ compiler (g++)
- libpcap
- make

## Build

```sh
make
```

## Usage

```sh
./dns-monitor (-i <interface> | -p <pcapfile>) [-v] [-d <domainsfile>] [-t <translationsfile>]
```

| Option | Description |
| --- | --- |
| `-i <interface>` | Network interface to capture DNS traffic from |
| `-p <pcapfile>` | `.pcap` file to analyze instead of a live interface |
| `-v` | Verbose mode: print full details of each DNS message |
| `-d <domainsfile>` | File to write observed domain names to |
| `-t <translationsfile>` | File to write domain-to-IP translations to |

Exactly one of `-i` or `-p` must be given. Live capture usually requires root privileges.

### Examples

Monitor DNS traffic on `eth0` in verbose mode:

```sh
sudo ./dns-monitor -i eth0 -v
```

Analyze a capture file and save the domains and resolutions it contains:

```sh
./dns-monitor -p capture.pcap -d domains.txt -t translations.txt
```
