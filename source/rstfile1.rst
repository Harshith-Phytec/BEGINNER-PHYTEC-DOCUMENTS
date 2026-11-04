.. START:section:0:0
**Connect the Ethernet**
========================

.. END:section:0:0

.. START:main_content:0:0
Connect the Ethernet cable to the board's Ethernet connector.
Connect the other end to the network switch or router.
Power on the board and wait for Linux to start.

.. END:main_content:0:0

**Configure Ethernet Using DHCP** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The PHYTEC Ethernet guide for phyCORE-AM62x documents the following procedure:

Connect the board to a DHCP-enabled network.
Allow the network interface to negotiate the connection.
Check the assigned IP address using:
ip addr
If the network is configured correctly and the DHCP server is available, the interface should receive an IP address.

**systemd-networkd** 

It is a Linux service responsible for managing network interfaces and their configurations on BSPs that use it.
On the target board, PHYTEC documents the network configuration files under:
/lib/systemd/network/
The configuration method can differ between BSP releases. Check the Ethernet guide for the relevant board and software version.

**Understand the IP Address Output** 

For example, a PHYTEC Ethernet guide shows an interface with an address similar to:

end0: <BROADCAST,MULTICAST,UP,LOWER_UP>
inet 192.168.3.11/24
The important fields mean:
Field
Meaning
end0
Network interface name
UP
Interface is administratively enabled
LOWER_UP
The lower-layer connection is operational
inet
IPv4 address information
192.168.3.11
Example IPv4 address
/24
Subnet prefix

An interface marked UP does not necessarily mean that the Ethernet cable has a working physical link. Check the other interface flags and the physical connection as well.

**Final Verification Checklist** 

Board powered using the supply specified in its PHYTEC Quickstart Guide.
Ethernet cable connected to the intended port.
Network interfaces checked using ip link show.
IP addresses checked using ip addr show.
Routing table checked using ip route show.
Ethernet connectivity tested using ping.
Any network configuration changes verified.
Official reference: PHYTEC Ethernet Guide — phyCORE-AM62x. For other PHYTEC boards, select the corresponding Ethernet guide from the PHYTEC documentation portal.

**Ethernet** 

Ethernet is a wired networking technology that allows devices to exchange data over a network.

**DHCP** 

DHCP stands for Dynamic Host Configuration Protocol. It allows a network's DHCP server to assign an IP address and other network settings automatically.

**Configure a Static IP Address** 

A static IP address is an address configured manually rather than obtained automatically from DHCP.

For example, the PHYTEC phyCORE-AM62x Ethernet guide documents changing the static address of an interface through its network configuration file.

For a board whose configuration uses 10-eth0.network, the documented procedure is:

Open the configuration file:
vi /lib/systemd/network/10-eth0.network
Modify the Address setting under the [Network] section. For example:

[Network]
Address=192.168.3.11/24

Restart the network service:
systemctl restart systemd-networkd

Verify the result:
ip addr show

**What does /24 mean?** 

The /24 is the subnet prefix. It identifies how much of the IP address belongs to the network portion. In this example, 192.168.3.11/24 belongs to the 192.168.3.0/24 network.

Important: The filename, interface name, and example address above follow a PHYTEC BSP configuration example. Other boards may use different interface names and configuration files. Check the applicable PHYTEC Ethernet guide before editing a file.

To change the default static address permanently in a Yocto-built image, the PHYTEC guide also describes modifying the corresponding configuration in the BSP layer and rebuilding the image.

Official reference: PHYTEC Ethernet Guide — phyCORE-AM62x.

**Check IP Addresses, Routes, and Network Interfaces** 

**Check Network Interfaces** 

Run:

ip link show
This displays the network interfaces available on the board and their link states.
To display the IP addresses associated with the interfaces, run:
ip addr show
The PHYTEC Ethernet documentation uses ip addr to show assigned addresses.

Common interface names include:
lo — the loopback interface used for communication within the same system.
eth0 — a common Ethernet interface name.
eth1 — a possible second Ethernet interface.

Some PHYTEC BSPs use other names, such as end0 and end1. Always use the names displayed by the board itself.

**Check the Routing Table** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A routing table tells Linux where to send network packets to reach a destination.

Run:

ip route show -  This displays the IPv4 routing table.
A default route, when configured, specifies the next hop used for destinations without a more specific matching route.
For example, a routing entry may contain :

default via 192.168.3.1 dev eth0

Here, default means the route is used when no more specific route matches.
via 192.168.3.1 identifies the next-hop gateway.
dev eth0 identifies the network interface used.

The example is illustrative; the actual gateway and interface depend on the network configuration.

**Test Network Connectivity** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The ping command tests whether another host responds to network echo requests.
To test an external IP address, the PHYTEC Ethernet guide uses this command format:
ping 8.8.8.8 -c 10
This sends 10 ping requests to the example destination. Use an appropriate reachable destination for your own network.
A successful test displays replies and ping statistics. If packets are lost, investigate the physical link, IP configuration, routes, and network accessibility.
A successful ping to an external IP address does not by itself verify DNS resolution. DNS can be checked separately by testing a hostname.

**Basic Ethernet Troubleshooting** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check the interface list and the board's supported Ethernet configuration.
Interface is up but no IP address is assigned
Check DHCP availability or the static IP configuration.
No physical link
Check the Ethernet cable, switch port, connector LEDs, and physical connection.
IP address exists but ping fails
Check the destination address, subnet, routing table, and network accessibility.
External IP works but hostname does not
Check DNS configuration.
