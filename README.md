## What is this?

This is a network I built in Packet Tracer while learning networking from scratch. It started as a simple setup with 2 switches and a few PCs, and I kept adding stuff as I learned new things. The idea is a fictional company called Copa.hr with different departments, a server, and even WiFi.

Here's how it started — just 2 switches, a router, and a few PCs:

![Starting topology with 2 subnets](https://github.com/izscaryu/Cisco-packet-tracer-projekt/blob/f5e0a8afd02669644ca7b85627cf5ce7808af425/naPocetkuMreza.png)
## What's in the network?

The network has 5 subnets across 2 routers. Here's a rough picture:

```
                        [Wireless Router]
                              |
                          [HWIC-4ESW]
                              |
[PC0]---[SwitchAccounts]---[MainRouter]---[SwitchDelivery]---[PC2]
[PC1]---/    Subnet 1     / Gig0/0  \     Subnet 2     \---[PC3]
[Printer1]               /           \              [Printer2]
                         / Gig0/2
                        /
                   [10.0.0.0/30]
                      /
                     / Gig0/0
              [Router2]
              / Gig0/1 \  Gig0/2
             /           \
     [Switch0]      [ServerSwitch]
      /     \             |
   [PC4]  [PC5]       [Server]
    Subnet 3          Subnet 5
                   (DNS, Web, Email, FTP)
```
And here's what it actually looks like in Packet Tracer now:
![Final network topology](https://github.com/izscaryu/Cisco-packet-tracer-projekt/blob/f5e0a8afd02669644ca7b85627cf5ce7808af425/dodaliSwitchIServer.png)

## What I configured
- **Subnetting** — split the network into /25 subnets, did the math with block sizes and the 2^n formula
- **DHCP** — so PCs get their IPs automatically instead of me typing each one manually
- **OSPF** — routers share routes with each other so all subnets can talk to each other
- **DNS + Web server** — you can type www.copa.hr in a browser and it actually loads a page
- **Email** — users can send emails to each other using @copa.hr addresses (SMTP/POP3)
- **FTP** — file server with different permissions (admin gets full access, delivery can only read)
- **WiFi** — a laptop connects wirelessly and can reach everything on the network
- **SSH** — all routers and switches are password protected with encrypted remote access
- **VLANs** — tried them out to see how they separate traffic (then removed them because inter-VLAN routing needs extra subnets I didn't plan for)

## Subnets

| Subnet | Network | Gateway | What it's for |
|--------|---------|---------|---------------|
| 1 | 192.168.40.0/25 | .1 | Accounts dept |
| 2 | 192.168.40.128/25 | .129 | Delivery dept |
| 3 | 192.168.41.0/25 | .1 | Users behind Router2 |
| 4 | 192.168.42.0/25 | .1 | Wireless |
| 5 | 192.168.43.0/25 | .1 | Server |
| Link | 10.0.0.0/30 | — | Between the 2 routers |

## Things that went wrong (and what I learned from them)

Honestly a lot of the learning came from breaking things:

- Router rebooted and I lost all my config because I forgot `copy running-config startup-config`. Won't make that mistake again
- Spent way too long confused why DHCP wasn't working until I realized VLANs were blocking the traffic from reaching the router
- Kept mixing up the Default Gateway field with the IP Address field when checking DHCP on PCs
- Tried to set up NAT but the HWIC-4ESW module doesn't support it properly in Packet Tracer. Learned a lot about NAT concepts even though I couldn't finish the implementation
- Tried to assign an IP to a FastEthernet port on the HWIC module and got errors — turns out those are switch ports (Layer 2) not router ports (Layer 3)

## How to try it

1. Install [Cisco Packet Tracer](https://www.netacad.com/courses/packet-tracer)
2. Download the .pkt file from this repo
3. Open it and try stuff:
   - Open a web browser on any PC and go to `http://www.copa.hr`
   - Send an email from one PC to another
   - `ping 192.168.43.10` from any PC to reach the server
   - `ssh -l Patrik 192.168.40.1` to remotely access the router
   - `ftp 192.168.43.10` to test file server permissions

## Documentation

There's a full Word doc in this repo (Copa_Network_Documentation.docx) with all the details — every command I used, IP tables, how subnetting works, OSPF config, the whole thing. It's like 40+ pages with a glossary and everything.

## Tools

- Cisco Packet Tracer
- Cisco IOS CLI
