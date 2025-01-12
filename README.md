# Configuration-of-the-GRE-Tunnel-in-Cisco-Packet-Tracer
Network Overview
This topology connects two remote networks using a GRE tunnel over the internet (or simulated ISP). The GRE tunnel acts like a virtual cable, making the two networks behave as if they are directly connected, even though they are far apart.

How It Works
1. Local Network Communication
On PC0, when you send data (like pinging PC1), the data travels from PC0 to Switch0 and then to Router1 (the gateway).
Similarly, PC1 sends data through Switch1 to Router2 (the gateway).
This is basic local network communication, where devices talk to their gateway to reach other networks.

2. Role of Routers
Router1 and Router2 are responsible for routing data between the two networks (10.1.1.0/24 and 10.2.2.0/24).
But here’s the problem: Router1 and Router2 are not directly connected. They are separated by the ISP, so they can’t exchange data normally.
3. ISP Network
The ISP network (100.0.0.0/24) connects Router1 to Router2 over the internet.
However, the ISP network only knows about its own addresses (100.0.0.x and 101.0.0.x). It doesn’t understand the private networks (10.1.1.0/24 and 10.2.2.0/24).
This is where the GRE tunnel comes in.
4. GRE Tunnel (Virtual Connection)
The GRE tunnel is like a private, virtual cable running inside the ISP network.
The tunnel is configured using the public IP addresses of the routers (100.0.0.1 for Router1 and 101.0.0.1 for Router2).
Data from Router1 is "wrapped" (encapsulated) inside another packet and sent through the tunnel to Router2.
When the data arrives at Router2, it "unwraps" (decapsulates) the packet and sends it to the correct destination (PC1).
Think of it like putting a letter (your data) into an envelope (the tunnel packet) for secure delivery.

5. Routing Table
Both Router1 and Router2 have routing tables that tell them how to send data to the other network:
On Router1: If data is for 10.2.2.0/24, send it through the tunnel.
On Router2: If data is for 10.1.1.0/24, send it through the tunnel.
6. Encapsulation Process
PC0 sends data to PC1 (e.g., a ping).
Router1 receives the data. It sees that PC1 is on 10.2.2.0/24, which is reachable through the GRE tunnel.
Router1 encapsulates the data into a new packet with a source IP of 100.0.0.1 and a destination IP of 101.0.0.1 (tunnel endpoints).
The packet is sent through the ISP.
7. Decapsulation Process
Router2 receives the encapsulated packet from the tunnel.
It decapsulates the packet to retrieve the original data.
Router2 then forwards the data to PC1 through its local network.
8. Return Path
PC1 sends a reply to PC0. The process works in reverse:
Data goes from PC1 to Router2.
Router2 encapsulates it and sends it through the tunnel to Router1.
Router1 decapsulates it and sends it to PC0
