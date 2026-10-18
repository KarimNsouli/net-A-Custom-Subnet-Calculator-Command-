<!-- TODO before pushing:
     1. Replace the LinkedIn link at the bottom with your real URL.
     2. Replace the "Example run" block with your real net++ output.
     3. Adjust the install steps and repo name to match your repo. -->

<h1 align="center">net++</h1>

<p align="center">
  <b>A custom subnet calculator command written in Bash</b><br>
  Enter an IP address, the CIDR notation and the subnet mask, and net++ works out the rest.
</p>

<p align="center">
  <img alt="Bash" src="https://img.shields.io/badge/Bash-4EAA25?style=for-the-badge&logo=gnubash&logoColor=white">
  <img alt="IPv4 Subnetting" src="https://img.shields.io/badge/Networking-IPv4%20Subnetting-0b7a75?style=for-the-badge">
  <img alt="Bitwise operations" src="https://img.shields.io/badge/Bitwise-Operations-6f42c1?style=for-the-badge">
  <img alt="CCNA" src="https://img.shields.io/badge/CCNA-Study%20Project-1BA0D7?style=for-the-badge&logo=cisco&logoColor=white">
</p>

---

## Why I built this

While studying for my CCNA with Jeremy's IT Lab, I ran into **subnetting** and couldn't wrap my head around it. All the calculations and details made it even more confusing.

So I built **net++**: a custom subnet calculator command. It uses the **host formula** and **bitwise operations** on the addresses, which helped me sharpen my Bash skills and understand what subnetting really is.

## Input

| Input | Example |
|---|---|
| IP address | `192.168.10.37` |
| CIDR notation | `/26` |
| Subnet mask | `255.255.255.192` |

## Output

| Output | What it tells you |
|---|---|
| **Network address** | The first address of the subnet (all host bits are 0) |
| **Broadcast address** | The last address of the subnet (all host bits are 1) |
| **CIDR notation** | The prefix length, e.g. `/26` |
| **IP class & privacy type** | Class A to E, and whether the address is private or public |
| **Number of hosts in subnet** | How many addresses can be assigned to devices |
| **Bit-map of the IP address** | The address in binary, octet by octet |

## Example run

```text
$ net++
IP address        : 192.168.10.37
CIDR notation     : /26
Subnet mask       : 255.255.255.192

Network address   : 192.168.10.0
Broadcast address : 192.168.10.63
CIDR notation     : /26
Class & privacy   : Class C, Private
Hosts in subnet   : 62
Bit-map           : 11000000.10101000.00001010.00100101
```

## How it works

```text
host bits = 32 - CIDR
hosts     = 2^(host bits) - 2        # network and broadcast addresses can't be assigned
network   = IP   AND mask            # keeps the network bits, zeroes the host bits
broadcast = network OR (NOT mask)    # sets every host bit to 1
```

1. **Host formula**: the number of usable hosts is `2^h - 2`, where `h` is the number of host bits.
2. **Network address**: a bitwise **AND** between the IP address and the subnet mask.
3. **Broadcast address**: a bitwise **OR** between the network address and the inverted mask.
4. **Class and privacy**: the first octet decides the class, and the private ranges (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`) decide private vs. public.
5. **Bit-map**: each octet is converted to 8 binary digits.

The core idea in Bash, simplified:

```bash
# dotted address <-> 32-bit integer
ip2int() { local IFS=.; read -r a b c d <<< "$1"; echo $(( (a<<24) | (b<<16) | (c<<8) | d )); }
int2ip() { echo "$(( ($1>>24)&255 )).$(( ($1>>16)&255 )).$(( ($1>>8)&255 )).$(( $1&255 ))"; }

mask=$(( (0xFFFFFFFF << (32 - cidr)) & 0xFFFFFFFF ))
network=$(( ip & mask ))                          # AND
broadcast=$(( network | (~mask & 0xFFFFFFFF) ))   # OR with the inverted mask
hosts=$(( 2**(32 - cidr) - 2 ))                   # host formula
```

## What I practiced

- Bash scripting
- IPv4 addressing, subnetting and CIDR
- Binary and bitwise operations (AND, OR, NOT, shifts)
- Building a command-line tool
- CCNA fundamentals

## Author

All the information about this project is on my [LinkedIn](https://www.linkedin.com/in/your-profile).
