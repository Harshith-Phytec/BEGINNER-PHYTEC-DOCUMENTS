.. START:section:0:0
**Board Connectivity**
======================

.. END:section:0:0

.. START:main_content:0:0
 Ethernet Configuration, Network Verification, and Power-On

.. END:main_content:0:0

**Objective** 
This document describes how to establish and verify Ethernet connectivity on a PHYTEC development board, inspect network interfaces and IP routing, and identify the correct input voltage before powering on the board.

**Ethernet Connectivity** 
~~~~~~~~~~~~~~~~~~~~~~~~~~

 **Requirements** 

PHYTEC development board with a supported Ethernet interface.

Ethernet cable with a compatible RJ45 connection.

Host PC or network switch.

DHCP-enabled network, or a correctly configured static IP network.

Serial console or another supported method of accessing the board's Linux terminal.

**Establish an Ethernet Connection** 
Power on the board using the supply specified in its hardware manual.

Connect the board's Ethernet port to a network switch or directly to a host PC, as supported by the setup.

Boot Linux and log in to the target.

Check the network interface and assigned IP address.

Test connectivity with ping.

PHYTEC's Ethernet documentation describes automatic link negotiation and DHCP-based IPv4 address assignment when the board is connected to a suitable network. The exact interface name and default network configuration depend on the board and BSP version.

**Run the following commands on the board:** 

ip link show
ip addr show

Check the following:

The expected Ethernet interface is present.

The interface has the appropriate operational state.

The interface has an IPv4 address when required.

The Ethernet cable is connected and the physical link is established.

To test connectivity to another host:

ping -c 4 <destination-ip>

For example, use the IP address of the host PC on the same network.

Successful replies indicate that ICMP communication is working between the two endpoints. A failed ping does not necessarily mean that Ethernet is disconnected; firewall rules, routing, or ICMP restrictions may also prevent replies.


**Display Network Interfaces** 

ip addr show

This command displays the network interfaces, MAC addresses, IPv4 addresses, IPv6 addresses, and interface status.

To inspect a particular interface:

ip addr show <interface-name>

Replace <interface-name> with the actual interface name reported by the board, such as eth0, eth1, or end0.

**Display Network Interfaces** 

ip route show

This displays the IPv4 routing table, including connected network routes and any configured default route.

Check the route used to reach a particular destination:

ip route get <destination-ip>

Verify that the destination is reachable through the expected interface and gateway.

.. START:list:0:0:8

.. LIST_META: {"type": "enumerated", "head": "", "items": ["**Verify Connectivity**", "Test communication with a host on the same subnet:", "ping -c 4 <host-ip>", "If internet connectivity is required, test an appropriate external IP address permitted by the network. DNS resolution can be tested separately using a hostname.", "Configuring an IP Address", "PHYTEC's Ethernet guide documents the use of systemd-networkd for network configuration on the relevant BSPs. Network configuration files are located under /lib/systemd/network/, with corresponding build-time configuration in the BSP layer."]}

1. **Verify Connectivity**
2. Test communication with a host on the same subnet:
3. ping -c 4 <host-ip>
4. If internet connectivity is required, test an appropriate external IP address permitted by the network. DNS resolution can be tested separately using a hostname.
5. Configuring an IP Address
6. PHYTEC's Ethernet guide documents the use of systemd-networkd for network configuration on the relevant BSPs. Network configuration files are located under /lib/systemd/network/, with corresponding build-time configuration in the BSP layer.

.. END:list:0:0:8

**DHCP Configuration** 
~~~~~~~~~~~~~~~~~~~~~~~

Connect the board to a DHCP-enabled network. If the BSP is configured for DHCP, the network service can obtain an address automatically.

Verify the result:

ip addr show
ip route show

**Powering On the PHYTEC Board** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Identify the Required Input Voltage** 

Before applying power:

Identify the exact board and carrier-board model.

Open the corresponding official PHYTEC hardware manual or Quick Start Guide.

Locate the power-supply section.

Check the permitted input-voltage range, connector pinout, polarity, and recommended supply current.

Confirm whether the board uses a dedicated DC connector, a power module, or USB Power Delivery.

Do not assume that the input voltage is identical across PHYTEC boards. Some models have different power-supply variants or connectors.

**Connect the Power Supply** 
Ensure that the power supply is switched off.

Verify the voltage rating and polarity against the board's documentation.

Connect the supply to the designated power connector.

Switch on the supply.

Check the documented power indicator, if present.

Observe the serial console or other documented boot interface to confirm that Linux starts.

Do not change module connections or jumper settings while the board is powered unless the relevant hardware manual explicitly permits it.

**Official PHYTEC References** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PHYTEC BSP Documentation — Ethernet, phyCORE-AM64x
https://docs.phytec.com/projects/yocto-phycore-am64x/en/latest/interfaceguides/eth.html
Covers Ethernet connection requirements, DHCP, IP-address inspection, static IP configuration, network service configuration, and connectivity testing.

PHYTEC BSP Documentation — Ethernet, phyCORE-AM62x
https://docs.phytec.com/projects/yocto-phycore-am62x/en/latest/interfaceguides/eth.html
Provides a related example of Ethernet interface inspection and network configuration.

PHYTEC Official Documentation Portal
https://docs.phytec.com/latest/
Use this portal to select the correct product and BSP documentation.

PHYTEC Quick Start Guide — phyBOARD-Segin i.MX 6UL/ULL
https://www.phytec.de/fileadmin/legacy/downloads/Manuals/L-840e_2.pdf
Covers hardware preparation, powering the board, serial-console access, and Ethernet connection setup for the specified board.

PHYTEC Hardware Documentation — phyBOARD-Pollux
https://www.phytec.de/cdocuments/?doc=ZYaQPw
Contains power-supply information for the phyBOARD-Pollux. Its voltage specifications apply to that board and must not be assumed to apply to other PHYTEC products.

**Scope and Applicability** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The commands in this document are intended as a practical Linux connectivity checklist. Network interface names, default IP addresses, configuration filenames, and supported power-input options vary by product and BSP.

Before using this document for a specific board, confirm all model-dependent settings using the corresponding official PHYTEC documentation.

.. START:list:0:0:17

.. LIST_META: {"type": "auto", "head": "**Verification Checklist**", "items": ["Test Command or action", "Expected result", "Power supply", "Check hardware-manual specifications", "Correct voltage, polarity, and current rating", "Board startup", "Observe serial console", "Boot output and Linux login prompt", "Interface detection", "ip link show", "Expected Ethernet interface is present", "IP configuration", "ip addr show", "Expected IP address and subnet", "Routing", "ip route show", "Appropriate connected and default routes", "Local connectivity", "ping -c 4 <host-ip>", "Replies from the destination, if ICMP is permitted", "Network connectivity", "Test an appropriate external destination", "Successful replies where permitted", "Record the actual command output and test result when completing a board validation report."]}

.. container:: spx-list-head

    **Verification Checklist**

1. Test Command or action
2. Expected result
3. Power supply
4. Check hardware-manual specifications
5. Correct voltage, polarity, and current rating
6. Board startup
7. Observe serial console
8. Boot output and Linux login prompt
9. Interface detection
10. ip link show
11. Expected Ethernet interface is present
12. IP configuration
13. ip addr show
14. Expected IP address and subnet
15. Routing
16. ip route show
17. Appropriate connected and default routes
18. Local connectivity
19. ping -c 4 <host-ip>
20. Replies from the destination, if ICMP is permitted
21. Network connectivity
22. Test an appropriate external destination
23. Successful replies where permitted
24. Record the actual command output and test result when completing a board validation report.

.. END:list:0:0:17

**Static IP Configuration** 

For BSPs that use the PHYTEC-documented systemd-networkd configuration, edit the applicable interface configuration file. The filename depends on the interface and BSP.

Example configuration:

[Match]
Name=<interface-name>

[Network]
Address=192.168.3.20/24

Use an IP address and subnet appropriate for the network. Configure the gateway and DNS settings as required by the deployment.

Restart the network service when appropriate:

systemctl restart systemd-networkd

Then verify the new configuration:

ip addr show
ip route show
ping -c 4 <host-ip>

Important: The example IP address is illustrative. Do not use it without checking the host network and existing address assignments. For a persistent change, update the relevant BSP configuration and rebuild the image according to the applicable PHYTEC documentation.


**Verify Board Startup** 

After powering on, check for:

The expected power indicator.

Bootloader output, if available through the serial console.

Linux kernel startup messages.

A login prompt.

Detection of the expected Ethernet interface.

If the board does not start, switch off the supply and verify the voltage, polarity, connector, and current rating against the correct hardware manual before proceeding.

**Display Network Interfaces** 

ip link show

Check whether the Ethernet interface exists and whether it is administratively enabled.

To enable an interface, when appropriate:

ip link set <interface-name> up

This command changes the interface state; it does not by itself guarantee an IP address or working network connectivity.


**Checking IP Addresses, Network Interfaces, and Routes** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Display IP Addresses** 
ip addr show

This command displays the network interfaces, MAC addresses, IPv4 addresses, IPv6 addresses, and interface status.

To inspect a particular interface:

ip addr show <interface-name>

Replace <interface-name> with the actual interface name reported by the board, such as eth0, eth1, or end0.
