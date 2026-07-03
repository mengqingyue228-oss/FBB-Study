# Day001 Lesson

Topic: Fixed Broadband Overview

Status: Ready

## Learning Goals

- Understand what fixed broadband is.
- Build a first mental map of an operator fixed broadband network.
- Distinguish access, aggregation, core, and service control roles.
- Connect home internet access to real operator network elements.
- Learn the first troubleshooting question: where is the fault domain?

## Key Concepts

- Fixed Broadband
- FTTH
- CPE
- ONT
- OLT
- Access Network
- Aggregation Network
- Core Network
- BNG/BRAS
- Default Gateway
- DNS

## 1. What Fixed Broadband Means

Fixed broadband is a network service that gives a fixed location, such as a home, office, shop, or campus, stable internet access through a wired access network.

In modern operator networks, the common residential form is FTTH: Fiber To The Home.

The important idea is not the fiber itself. The important idea is that a user at a fixed place is connected into an operator network through a chain of devices and protocol boundaries.

A very simplified path is:

```text
Home device
  -> CPE / home router
  -> ONT
  -> OLT
  -> Access network
  -> Aggregation network
  -> BNG / BRAS
  -> Core network
  -> Internet / service platforms
```

Different operators may combine or split some functions, but this map is a useful starting point.

## 2. Home Side: Device, CPE, and ONT

Your laptop or phone normally does not talk directly to the operator core network.

At home, it usually talks first to a CPE, often called a home router.

The CPE commonly provides:

- Wi-Fi access
- Local LAN switching
- Private IP address distribution by DHCP
- NAT
- Default gateway for home devices
- DNS forwarding or DNS relay

In FTTH, the ONT is the optical network terminal. It converts the optical access network into an Ethernet-facing interface for the home side.

Sometimes the CPE and ONT are separate devices. Sometimes they are combined into one device.

Engineering meaning:

If one laptop cannot access the internet, the fault may be only inside the home LAN. If all home devices fail, the problem may be the CPE, ONT, fiber access, authentication, or upstream network.

The first practical skill is to avoid treating every problem as an internet problem.

## 3. Access Network: Bringing Users Into the Operator Network

The access network is the part closest to users.

In FTTH, the OLT sits on the operator side and connects many ONTs. The OLT is a major access-layer device.

Its job is not just physical connectivity. In real operator networks, access devices often participate in:

- User line identification
- VLAN handling
- Service separation
- Basic traffic control
- Access-side fault isolation

At this stage, do not worry about exact PON details yet. The important mental model is:

```text
Many users -> access device -> operator aggregation network
```

Huawei practice note:

In Huawei-oriented troubleshooting, engineers often start by checking whether the access device sees the user line, service VLAN, port state, and upstream path. The exact commands will be learned later, but the display-oriented mindset starts now.

## 4. Aggregation Network: Collecting Many Access Nodes

An operator does not connect every OLT directly to the internet.

The aggregation network collects traffic from many access nodes and carries it toward service control and core layers.

It usually focuses on:

- High-capacity Ethernet transport
- VLAN or QinQ service transport
- Redundancy
- Regional traffic collection
- Routing or Layer 2 forwarding, depending on design

Engineering meaning:

If one household fails, suspect the home side or access side first. If many users under one access node fail, suspect access equipment or uplink. If many access nodes in a region fail, suspect aggregation or upstream service control.

This is why network engineers always ask: how wide is the impact?

## 5. BNG / BRAS: Where Broadband Users Become Managed Subscribers

BNG means Broadband Network Gateway. BRAS means Broadband Remote Access Server.

For today, treat them as the service control boundary for broadband users.

The BNG/BRAS may handle:

- User session control
- Subscriber authentication
- IP address assignment
- Policy enforcement
- Accounting
- Traffic steering toward the internet or service platforms

In many operator networks, this is where a home broadband user becomes a recognized subscriber rather than just traffic coming from an access port.

Do not memorize configuration yet. Memorize the role:

```text
Access brings users in.
Aggregation carries users upward.
BNG/BRAS controls broadband subscriber service.
Core carries traffic at scale.
```

## 6. Core Network and Internet Connectivity

The core network carries large-scale traffic between regions, service platforms, peering points, and the public internet.

The core is designed for:

- High capacity
- High reliability
- Fast routing convergence
- Interconnection with other networks
- Service reachability

For a fixed broadband learner, the core matters because user experience depends on the whole path. But in troubleshooting, you should not jump to the core too early.

Most user complaints start closer to the edge:

- Home Wi-Fi
- CPE
- ONT
- Access line
- Access VLAN or service path
- BNG/BRAS session or address assignment

## 7. Why This Matters in the AI Era

AI can help generate commands, summarize logs, and compare configurations.

But AI is weak if the engineer does not know the network boundary being investigated.

Long-term valuable knowledge is the ability to reason:

- Which layer am I looking at?
- Which device owns this function?
- What should be true if the network is healthy?
- Is this a single-user, access-node, regional, or core-wide issue?
- Which evidence would separate these possibilities?

This is why Day001 starts from architecture, not commands.

## 8. First Troubleshooting Map

When a home user says "the internet is not working", ask:

1. Is the device connected to Wi-Fi or Ethernet?
2. Does the device have an IP address?
3. Can it reach the home gateway?
4. Can the home gateway reach the operator side?
5. Is the ONT online?
6. Is the access service path normal?
7. Is the broadband session normal on BNG/BRAS?
8. Is DNS working?
9. Is the target service reachable?

You do not need to solve all of these today. You only need to see that "internet not working" is not one problem. It is a symptom across many possible boundaries.

## Summary

Fixed broadband is a service chain, not a single device.

The first useful map is:

```text
Home device -> CPE/ONT -> OLT/access -> aggregation -> BNG/BRAS -> core -> internet
```

Today's key learning outcome is to explain what each major part does and why fault isolation starts by locating the affected boundary.
