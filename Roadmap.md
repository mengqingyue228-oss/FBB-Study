# Roadmap

This file maintains the 120-day learning route only.

Course content belongs in each `DayXXX` folder, not here.

## Route Principle

The course order is adjusted to:

1. IP bearer network
2. Optical transport network
3. Optical access network

The learner has a software engineering background but should be treated as a networking beginner. Each day should open one main topic only. If another domain is needed for context, mention it briefly and defer the full explanation to that domain's stage.

## Stage 1: IP Bearer Network From Zero

Goal: understand how operator networks carry user traffic with IP, routing, addressing, sessions, policies, and service edges.

Planned days: Day003 - Day040

Focus:

- What an IP bearer network is and why operators need it
- IP address, subnet, gateway, route, next hop, packet, and forwarding table
- Ethernet, MAC, ARP, VLAN, QinQ, and broadcast domain as supporting knowledge
- DHCP, DNS, NAT, PPPoE, IPoE, and user session basics
- BNG/BRAS, aggregation router, core router, and service edge roles
- Routing protocols, starting from static routing, then OSPF and IS-IS basics
- MPLS/SR-MPLS/SRv6 as later bearer technologies, introduced only after IP routing is clear
- Huawei-oriented device examples such as NetEngine carrier routers, explained as examples rather than product training
- Planning, construction, maintenance, operations, optimization, and marketing perspectives for IP broadband services
- Wireshark/containerlab/FRRouting observation labs

Beginner rule: start with everyday analogies, then formal terms, then protocol behavior, then operator engineering meaning.

## Stage 2: Optical Transport Network

Goal: understand how large amounts of traffic are transported over long-distance optical networks before reaching IP service nodes.

Planned days: Day041 - Day075

Focus:

- Why IP networks still need optical transport underneath
- Optical fiber basics: wavelength, light path, attenuation, dispersion, and optical power
- SDH/OTN/ROADM/WDM concepts, with OTN as the main modern focus
- ODUk, OTUk, client/service mapping, protection, and grooming concepts
- Huawei-oriented examples such as OptiX OSN/OptiXtrans scenarios, explained as operator transport examples
- Typical scenarios: metro aggregation, provincial backbone, data center interconnect, mobile backhaul, fixed broadband uplink
- Engineering topics: fiber route planning, site survey, rack/power, optical budget, turn-up, acceptance, alarm handling, capacity expansion, and optimization

Cross-domain rule: when IP or access terms appear, only explain what is needed for transport context and leave detailed IP/access teaching to their own stages.

## Stage 3: Optical Access Network

Goal: understand how fiber broadband reaches homes and enterprises, from OLT to ODN to ONT/CPE and user services.

Planned days: Day076 - Day120

Focus:

- PON/FTTH/FTTR architecture and why access is different from transport
- OLT, splitter, ODN, fiber distribution box, joint closure, optical cross-connect cabinet, drop cable, ONT, CPE, and home router roles
- GPON, XG-PON, XGS-PON, and 10G PON basics
- ONU/ONT registration, DBA, GEM port, T-CONT, VLAN service model, multicast basics, and broadband service provisioning
- Home network equipment categories: bridge ONT, routed ONT, Wi-Fi ONT, standalone home router, mesh router, and enterprise CPE
- Huawei-oriented examples such as MA5800 OLT and EchoLife/OptiXstar ONT/CPE families, explained as examples in real operator deployments
- ODN engineering: splitter ratio, connector loss, splicing, fiber laying, labeling, acceptance testing, OTDR, and fault isolation
- Planning, construction, maintenance, operations, optimization, and marketing perspectives for FTTH services

## Review Rule

Daily lessons should not keep rotating through the same concepts. New lessons should be different each day. Previous mistakes or low-score knowledge points should be reviewed in a short "Knowledge Review" section, not used to replace the day's main topic unless the learner is clearly blocked.

## Notes

The roadmap should remain lightweight. Adjustments should preserve the project goal: helping the learner steadily master fixed broadband without increasing maintenance cost.
