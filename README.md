# Network Security - Two-Site Segmented Enterprise Network (Packet Tracer)

A group project: 
Designed, built, and tested a segmented two-site enterprise network (Stockholm + Göteborg) in Cisco Packet Tracer, then threat-modelled the design at Layer 2 and applied hardening.

The structure below follows **build → break → defend**.

---

## Build

- **Two sites** (Stockholm, Göteborg), each segmented into zones: **Kontor (VLAN 10)**, **Guest (VLAN 20)**, **Admin (VLAN 99)**, plus a native VLAN (100) and a blackhole VLAN (999) for unused ports.
- **IP plan** calculated from scratch - /26 subnets (62 usable hosts per zone).
- **Site-to-site GRE tunnel** between the two routers; static routing over the tunnel (each site reaches the other's subnet via the tunnel).
- **Inter-VLAN routing** handled on Layer 3 switches.


<img width="616" height="591" alt="Topology" src="https://github.com/user-attachments/assets/31a20a02-c45f-484d-b07f-0bdc3d58fbd7" />

---

## Break (threat model)

I modelled a Layer 2 attack against our own design:

- **VLAN hopping via switch spoofing** - DTP left on dynamic auto lets an attacker negotiate a trunk (Yersinia) and reach all VLANs, including Admin.
- **GRE tunnel is unencrypted** - GRE encapsulates but does not encrypt, so tunnel traffic between the sites can be sniffed in cleartext (Wireshark/tcpdump).
- **Denial of service** - DHCP starvation (exhaust the pool) and ICMP flood (hping3).

Impact was rated against CIA: confidentiality high (cleartext capture), availability high (DoS), integrity medium/high (possible MITM).

<img width="586" height="298" alt="Defend" src="https://github.com/user-attachments/assets/e4bad90e-0821-46ce-ae67-c0d5a8d86f4c" />

---

## Defend (Hardening)

Key controls applied and tested:

- **ACLs** — guest VLAN blocked from internal resources; SSH restricted to authorised sources.
- **SSH with RSA keys instead of Telnet**, plus a short session timeout.
- **Port security** — each access port locked to its learned MAC, so an unauthorised laptop on that port is shut down.
- **Disable DTP and CDP**; send unused ports to the blackhole VLAN.

Recommended next step: wrap the GRE tunnel in **IPsec** to encrypt site-to-site traffic and close the sniffing vector identified above.

---

## Tools

Cisco Packet Tracer. Threat model referenced Yersinia (DTP), hping3 (ICMP flood), and Wireshark (sniffing).

---

## My contribution

Led the group work, coordinated the build, kept the team aligned, and made sure every member could speak to every part. We worked across all sections together.

---
