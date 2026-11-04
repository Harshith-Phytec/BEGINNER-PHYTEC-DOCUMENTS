.. START:section:0:0
**System Information on a PHYTEC Board**
========================================

.. END:section:0:0

.. START:main_content:0:0
This guide explains how to check the software version, Linux kernel, CPU frequency, RAM, storage, and filesystem usage on a PHYTEC embedded Linux board.


.. END:main_content:0:0

.. START:table:0:0:0

.. csv-table:: Illustrative output based on PHYTEC's documentation.
   :header: "Part", "Meaning"

   "BSP-Yocto", "PHYTEC's Yocto-based Board Support Package"
   "i.MX93", "Processor platform"
   "PD24.1.0", "BSP release identifier"

.. END:table:0:0:0

**Before you start you need:**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A PHYTEC board with Linux already booted.
Access to the board through a serial console or terminal.
A login account with permission to run the required commands.
Important: PHYTEC supports multiple processor families and BSP versions. Some commands or output files may differ depending on the board and software image. The examples below are for learning; your actual output may be different.

**Identify the BSP version running on the board** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**BSP** 

BSP stands for Board Support Package. It contains the board-specific software needed to run Linux on a PHYTEC platform, including components such as the kernel, bootloader, and board configuration.

PHYTEC identifies its BSP releases using version names such as BSP-Yocto-i.MX93-PD24.1.0. 

Step 1: Log in to the board
Open your serial terminal, log in to the board, and wait for the Linux command prompt.
Step 2: Run the command
lsb_release -r

This command is specifically documented by PHYTEC for checking the BSP release on supported images. 

Step 3: Understand the output
Example:
Release: BSP-Yocto-i.MX93-PD24.1.0

.. START:list:0:0:3

.. LIST_META: {"type": "bullet", "head": "", "items": ["Why is this useful?", "To confirm which BSP release is installed.", "To check whether the correct software image is running.", "To help identify software compatibility issues during debugging.", "If the command is unavailable: Your image may not include the lsb_release utility. Check the release notes and BSP manual for your specific board and image.", "Official reference: PHYTEC BSP Documentation"]}

- Why is this useful?
- To confirm which BSP release is installed.
- To check whether the correct software image is running.
- To help identify software compatibility issues during debugging.
- If the command is unavailable: Your image may not include the lsb_release utility. Check the release notes and BSP manual for your specific board and image.
- Official reference: PHYTEC BSP Documentation

.. END:list:0:0:3

**Check the Linux kernel version and system information** 

What is the Linux kernel?
The Linux kernel is the core of the operating system. It manages processor resources, memory, hardware devices, and communication between software and hardware.

PHYTEC maintains board-specific BSP documentation that identifies the kernel used by each supported platform. The kernel version can differ between BSP releases. 

Step 1: Check the kernel version
Run:
uname -r

Example output:
6.1.55-phytec

Illustrative example only; this is not a claim about your board's installed kernel.

The output tells you the running kernel's release string.
Step 2: Check the complete kernel and machine information
Run:
uname -a

This displays a longer summary containing information such as the kernel release, machine architecture, and build details.
Step 3: Check processor architecture
Run:
uname -m

This helps identify the architecture reported by the running Linux system.
How do you use this information?
Suppose a board is not detecting a peripheral or a driver behaves unexpectedly. Record the BSP release and kernel version before investigating. This information helps you locate the appropriate PHYTEC documentation and verify whether you are using the expected software release.

Important: These are standard Linux diagnostic commands. PHYTEC's documentation portal provides the board-specific BSP and kernel details needed to interpret your results. 
BSP Documentation

Official reference: PHYTEC BSP Documentation Portal

**Check the CPU frequency range and current operating frequency** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

What is CPU frequency?
CPU frequency indicates how fast the processor's clock is running. It is usually expressed in MHz or GHz.
- Minimum frequency: The lowest frequency the active CPU frequency policy permits.
- Maximum frequency: The highest frequency the active policy permits.
- Current frequency: The frequency reported at the time of checking.
The available frequencies depend on the processor, board configuration, and Linux software. Use the PHYTEC hardware manual and BSP documentation for the specific board to confirm the processor's supported operating characteristics.
Step 1: Check whether Linux exposes CPU frequency information
Run:
ls /sys/devices/system/cpu/


Look for CPU directories and, depending on the kernel configuration, a cpufreq interface under a CPU directory.

For example, a system may expose:
/sys/devices/system/cpu/cpu0/cpufreq/


The cpufreq interface may not exist on every board or kernel configuration.
Step 2: Check the minimum frequency
If the following file exists, run:
cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_min_freq


Step 3: Check the maximum frequency
cat /sys/devices/system/cpu/cpu0/cpufreq/cpuinfo_max_freq


Step 4: Check the current frequency
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq


These interfaces typically report frequency in kHz. Divide the value by 1,000 to convert kHz to MHz.

For example, 800000 kHz equals 800 MHz.
Step 5: Check the permitted frequency range
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq


These report the minimum and maximum frequencies configured by the active scaling policy, which may differ from the processor's hardware limits.
What if cpufreq does not exist?
Do not assume the processor is faulty. The interface may be unavailable because of the processor's configuration, kernel support, or device-tree setup. Consult the appropriate PHYTEC BSP documentation for your platform.

**Check RAM, available storage, and filesystem usage** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

It is important to distinguish between RAM and storage.
- RAM is temporary memory used by running applications and the operating system.
- Storage is persistent memory, such as eMMC, NAND, or an SD card.
- Filesystem usage indicates how much space is used and available on mounted filesystems.
PHYTEC's board documentation describes the available platform configurations and storage options. The exact capacity depends on the System on Module and board variant. 

A. Check RAM
Run:
cat /proc/meminfo

.. START:table:0:0:7

.. csv-table::
   :header: "Field", "What it means"

   "MemTotal", "Total RAM available to Linux"
   "MemFree", "RAM that is currently unused"
   "MemAvailable", "Estimated RAM available for new applications without swapping"
   "SwapTotal", "Total configured swap space"
   "SwapFree", "Unused swap space"

.. END:table:0:0:7

For a shorter summary, you can also try:
free -h


The -h option displays sizes in readable units such as MiB and GiB.

Example: If MemTotal is around 900 MiB, that does not necessarily mean the board has only 900 MiB physically installed. Some memory may be reserved for hardware or other purposes.
B. Check storage devices
Run:
ls /dev/mmcblk*


This can help identify eMMC or SD/MMC block-device nodes when they are present.

The output alone does not tell you how much storage is free. It identifies device names that you can investigate further.

To inspect mounted filesystems and their usage, proceed to the next step.
C. Check filesystem usage
Run:
df -h

.. START:table:0:0:9

.. csv-table:: This reports filesystem capacity and usage for mounted filesystems.
   :header: "Column", "Meaning"

   "Filesystem", "Device or filesystem name"
   "Size", "Total filesystem capacity"
   "Used", "Space currently used"
   "Avail", "Space available to users"
   "Use%", "Percentage"
   "Mounted on", "Directory where the filesystem is accessible"

.. END:table:0:0:9

.. START:table:0:0:10

.. csv-table:: Example:
   :header: "Filesystem", "Size", "Used", "Avail", "Use%", "Mounted on"

   "/dev/root", "3.5G", "2.0G", "1.3G", "61%", "/"

.. END:table:0:0:10

In this example, / is the root filesystem. It has 3.5 GB of capacity, with 2.0 GB used.


**Quick reference: commands to remember** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Check BSP version - lsb_release -r
Check kernel version - uname -r
Check full system information - uname -a
Check CPU architecture - uname -m
Check CPU frequency interface - ls /sys/devices/system/cpu/
Check RAM details - cat /proc/meminfo
Summarize RAM - free -h
Identify MMC device nodes - ls /dev/mmcblk*
Check all mounted filesystem usage - df -h
Check filesystem containing /var - df -h /var
