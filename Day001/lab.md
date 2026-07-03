# Day001 Lab

Topic: Observe Your Local Network

Status: Ready

## Lab Goal

Observe your Mac's local network information and connect it to the fixed broadband architecture learned today.

This lab does not check or grade your configuration. The goal is to understand what you are observing.

## Environment

- Mac
- Terminal
- Wireshark, optional

No Docker or containerlab is required today. Day001 is an observation lab.

## Background

Your Mac is usually not directly connected to the operator network. It normally connects first to a home gateway or CPE.

In this lab, you will identify:

- Your local IP address
- Your default gateway
- Your DNS server
- The Layer 2 neighbor discovery behavior between your Mac and the gateway

## Steps

### 1. Identify Your Local IP Address

Run:

```sh
ifconfig
```

Look for the active interface.

Common examples:

- Wi-Fi: `en0` or `en1`
- Ethernet adapter: may appear as another `enX` interface

Find the `inet` address.

Record:

```text
Local IP:
Interface:
```

### 2. Identify the Default Gateway

Run:

```sh
route -n get default
```

Find:

```text
gateway:
interface:
```

Record:

```text
Default Gateway:
Outgoing Interface:
```

### 3. Identify DNS Servers

Run:

```sh
scutil --dns
```

Find one or more `nameserver` entries.

Record:

```text
DNS Server 1:
DNS Server 2:
```

### 4. Check Reachability to the Gateway

Run:

```sh
ping -c 4 <default-gateway-ip>
```

Expected result:

- You should receive replies from the gateway.
- Latency should usually be very low on a home LAN.

Record:

```text
Gateway Reachable: Yes / No
Average Latency:
```

### 5. Observe ARP Neighbor Information

Run:

```sh
arp -a
```

Find the entry for your default gateway.

Record:

```text
Gateway IP:
Gateway MAC:
Interface:
```

### 6. Optional Wireshark Observation

Open Wireshark and capture on the active interface.

Then run:

```sh
ping -c 2 <default-gateway-ip>
```

If needed, clear ARP first by waiting or changing network state. Do not force risky system changes today.

Try to observe:

- ARP request
- ARP reply
- ICMP echo request
- ICMP echo reply

## Expected Result

After this lab, you should be able to explain:

- Your Mac uses a local IP address inside the home network.
- The default gateway is the first Layer 3 next hop.
- ARP is used to find the gateway's MAC address on the local LAN.
- DNS is a separate service from basic gateway reachability.
- A successful ping to the gateway only proves the home-side path, not full internet health.

## Reference Output

Example shape only:

```text
Local IP: 192.168.1.23
Default Gateway: 192.168.1.1
DNS Server: 192.168.1.1
Gateway MAC: aa:bb:cc:dd:ee:ff
```

Your values will be different.

## Lab Notes

To be filled by the learner.
