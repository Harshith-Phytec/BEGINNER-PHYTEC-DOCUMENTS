.. START:section:0:0
**RAUC on PHYTEC**
==================

.. END:section:0:0

.. START:main_content:0:0
RAUC stands for Robust Auto-Update Controller. It is a Linux-based software update framework used to update components such as the Linux kernel, Device Tree, and root filesystem.
RAUC supports A/B update systems, where one system runs while the other is updated. If the updated system fails to boot, the bootloader can select the previous working system, provided rollback is configured correctly.

.. END:main_content:0:0

**Important terms** 
~~~~~~~~~~~~~~~~~~~~

.. START:table:0:0:1

.. csv-table::
   :header: "Term", "Meaning"

   "RAUC", "Software update framework"
   "RAUC", "Non-volatile flash memory"
   "MTD", "Memory Technology Device interface for flash"
   "UBI", "Unsorted Block Images; manages NAND flash and its erase blocks"
   "UBIFS", "Filesystem designed for UBI-managed flash"
   "Bundle (.raucb)", "Update package containing images and a manifest"
   "manifest.raucm", "Describes the images and compatibility information in the update bundle"
   "A/B slots", "Two system configurations that allow updates with a fallback option"

.. END:table:0:0:1

**PHYTEC Wega AM335x support RAUC with  NAND** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The official PHYTEC Wega product page lists NAND flash as a supported storage option. 

PHYTEC's published RAUC example for NAND uses BSP-Yocto-AM335x-PD19.10 with the machine phycore-r2-am335x-1. This example must not be assumed to apply directly to Wega without checking the exact board and BSP version. 

**Official references:** 
• PHYTEC Wega AM335x product page
• PHYTEC Yocto AM335x BSP Manual

**NAND partitions and update slots work** 

NAND flash is divided into partitions. UBI manages the NAND partition used for the system, and UBI volumes can hold the root filesystem, kernel, and Device Tree.

.. START:table:0:0:3

.. csv-table::
   :header: "Component", "System A", "System B"

   "Root filesystem", "/dev/ubi0_2", "/dev/ubi0_5"
   "Kernel", "/dev/ubi0_0", "/dev/ubi0_3"
   "Device Tree", "/dev/ubi0_1", "/dev/ubi0_4"
   "Boot name", "system0", "system1"

.. END:table:0:0:3

Important: These device names are from PHYTEC's documented example. Do not assume that your Wega board uses the same layout. Confirm the actual configuration on your board first.

**Commands to check the current configuration** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Run these commands on the target board.
Step 1: Identify the board and software version
cat /proc/device-tree/model
cat /etc/os-release
Step 2: Check whether RAUC is installed
rauc --version
If the command is not found, RAUC may not be installed in the image.
Step 3: Check the RAUC configuration
cat /etc/rauc/system.conf
Look for the compatible board name, bootloader configuration, and configured root filesystem, kernel, and Device Tree slots.
Step 4: Check RAUC status
rauc status
This can show configured slots and their status, depending on the system configuration.
Step 5: Inspect flash partitions and UBI volumes
cat /proc/mtd
ubinfo -a
cat /proc/mtd lists MTD flash partitions. ubinfo -a lists UBI devices and volumes, if the ubinfo utility is installed.

**How to check an update bundle**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A RAUC bundle normally has the .raucb extension.
Inspect a bundle:
rauc info update-bundle.raucb
Check the bundle's compatible value and confirm that it matches the board's RAUC configuration. Also verify that the bundle contains the images and slots expected by the target system.
To install a compatible bundle, when the board has been configured for RAUC updates:
rauc install update-bundle.raucb
reboot
Do not install an arbitrary bundle simply because it uses the .raucb extension. The bundle must match the board and its configured slots. 

Important precautions
    • Confirm the exact board model and BSP release before following the PHYTEC RAUC example.
    • Verify that the A/B slots, bootloader state framework, and rollback mechanism are configured.
    • Do not run NAND initialization or partition-creation commands on a working board without confirming the procedure. Such operations may erase existing data.
    • For production systems, use an appropriate production signing key and certificate rather than development credentials.

Conclusion: PHYTEC documents RAUC updates using NAND and A/B slots for a specific AM335x BSP and machine. To determine whether your Wega AM335x board supports RAUC updates, first check the installed BSP version, /etc/rauc/system.conf, rauc status, and the actual NAND/UBI layout.
