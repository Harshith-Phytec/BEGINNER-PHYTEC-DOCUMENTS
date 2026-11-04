.. START:section:0:0
**Flashing Images and Application Deployment**
==============================================

.. END:section:0:0

.. START:main_content:0:0
This documentation will explain how to download, select, flash, and use Linux images for PHYTEC development boards, using the phyBOARD-WEGA AM335x and phyBOARD-Pollux i.MX 8M Plus as examples.

.. END:main_content:0:0

**Linux Image format Example : Wega Board** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Linux image**

A Linux image is a prepared collection of software that allows a development board to start and run Linux.

Depending on the image format and BSP, it may contain:
Bootloader: Starts the board and loads the operating system.
Linux kernel: Manages the processor, memory, drivers, and hardware peripherals.
Device tree: Describes the board hardware to the Linux kernel.
Root filesystem (rootfs): Contains Linux commands, libraries, services, configuration files, and applications.

Additional software: For example, Qt demo applications, multimedia components, or other packages selected for the image.

An image may contain all these components together, or only one component. For example, a kernel image alone is not the same as a complete bootable SD-card image.

First, understand how WEGA boots
When you power on the WEGA board, it cannot immediately run Linux.
It must load and start several components in the correct order.
Power ON

The processor starts executing its built-in boot code.
MLO
First-stage bootloader (SPL). Prepares the system for the next stage.
U-Boot
Loads the Linux kernel and other required files.
zImage + device tree
The kernel starts with the hardware description.
Root filesystem
Linux mounts its filesystem and starts services and applications.

This is the general boot sequence for the WEGA AM335x. The exact filenames, storage locations, and boot commands depend on the BSP version and boot configuration.

**File Type and usage**

**MLO**  — First-stage bootloader
Simple meaning: MLO is a small program that helps the processor prepare the board to load the next bootloader.
It is used early in the boot process.
On AM335x, the processor's ROM boot code loads MLO from a supported boot device.
MLO initializes enough hardware, particularly memory, to load the next boot stage.

In the WEGA BSP, MLO is provided as a prebuilt file, so you normally do not need to modify it for routine use.
Why do we need it?
Without a valid first-stage bootloader, the processor may not be able to continue to the next boot stage.

**oftree**  — Device tree
Simple meaning: oftree is a binary description of the board's hardware, also known as a device tree blob (DTB).
It tells Linux which hardware is available, such as:
UART serial ports 
I²C and SPI controllers
GPIO controllers 
Ethernet interfaces
Memory 
Board-specific devices

Why do we need it?
The same AM335x processor can be used on different boards. Linux needs to know how the processor's interfaces are connected and configured on the particular board.

If you change the hardware configuration, such as enabling a peripheral or changing pin multiplexing, you may need to update the device tree and rebuild it.

Important: oftree is a filename used by some PHYTEC boot setups; it is not a separate file format from the device tree blob concept.

**zImage**  — Linux kernel
zImage is a compressed Linux kernel image in a bootable format.
The kernel is the core of the operating system. It manages:

Processor and memory
Hardware drivers
Filesystems
Processes and applications
Communication with peripherals

**Why do we need it?**
MLO and U-Boot prepare the board for Linux, but the kernel is what actually starts the Linux operating system.
For example, PHYTEC's WEGA download resources provide kernel files such as zImage-phyboard-wega-am335x-

**bin.**  The exact naming depends on the BSP release and machine configuration.

**ubifs** — Root filesystem for UBI-managed flash
A .ubifs file contains a filesystem designed for flash storage managed through UBI.
UBI is commonly used with raw NAND flash.
UBIFS works on top of UBI and manages files and directories while accounting for the characteristics of flash memory.

**Why do we need it?**
When a WEGA system is configured to boot its root filesystem from supported NAND storage, a UBIFS image can be used to provide the Linux files, libraries, commands, configuration, and applications.

It is not the same as a kernel image, and it is not necessarily suitable for writing directly to an SD card as a complete disk image.

**tar / .tar.gz**  — Root filesystem archive
Simple meaning: A TAR archive packages files and directories together. A .tar.gz file is a TAR archive compressed using gzip.

For example, a PHYTEC root filesystem archive may contain directories such as:
bin/
etc/
lib/
usr/
var/
These directories contain Linux commands, configuration, libraries, and application files.

Why do we need it?
A root filesystem archive is useful when you need to extract a filesystem into a prepared root partition, set up a development filesystem, or use another BSP-supported installation workflow.

For example, the general extraction command is:
tar -xzf rootfs.tar.gz -C /path/to/rootfs
The destination must be a correctly prepared root filesystem directory. This command alone does not make a board bootable.

PHYTEC's AM335x BSP resources provide both filesystem archives and flash-oriented filesystem images, depending on the release.
**root.ubi Vs  .ubifs**
They are related, but they are not the same.

.. START:table:0:0:3

.. csv-table::
   :header: "File", "Simple explanation", "Purpose"

   ".ubifs", "Filesystem image", "Contains the filesystem that UBIFS uses on UBI-managed flash."
   ".ubi", "UBI image", "Packages UBI volumes and their contents for programming into suitable flash storage."
   ".tar.gz", "Filesystem archive", "Contains files and directories that can be extracted."
   ".wic", ".wic", "Contains a prepared disk layout and partition contents."

.. END:table:0:0:3

The exact packaging depends on the BSP. Do not assume that every .ubi file has the same layout or that every .ubifs file can be flashed directly to NAND.

PHYTEC's WEGA manual documents using root.ubi when programming the NAND root filesystem on the older bootloader-based setup. 
download.phytec.de

**Quick comparison  root.ubi Vs  .ubifs** 


**Purpose of Images** 
~~~~~~~~~~~~~~~~~~~~~~

.. START:table:0:0:5

.. csv-table:: Table 2 : Purpose
   :header: "File", "Main purpose", "Normally modified by an application developer?"

   "MLO", "First boot stage", "No"
   "barebox.bin", "Bootloader for the documented legacy WEGA setup", "Only for bootloader changes"
   "oftree / .dtb", "Board hardware description", "When hardware configuration changes"
   "zImage", "Linux kernel", "When rebuilding or updating the kernel"
   "ubifs", "Flash filesystem", "When preparing a compatible root filesystem"
   "ubi", "UBI flash image", "When preparing or updating the NAND image"
   ".tar.gz", "Filesystem archive", "When building or extracting a filesystem"
   ".wicPu", "Complete disk image", "Normally generated by the BSP build"

.. END:table:0:0:5

The WEGA PD23.1.0 download server provides a newer Yocto BSP file set, including MLO, barebox.bin, oftree, zImage, .ubifs, .tar.gz, and .wic images. 

**Different image types** 

Different image formats solve different problems.
For example:
    • To prepare a new SD card, you usually want a complete disk image.
    • To transfer a root filesystem to a board, you may use a rootfs archive.
    • To update the kernel, you may need a kernel image or a BSP-specific update package.
    • To update a production device, you may use a supported update mechanism such as RAUC or a PHYTEC-provided update package.
PHYTEC documents the WIC image as a complete disk image for initial setup, while other formats serve different purposes. 

Understanding the different image formats

The following formats are relevant when working with PHYTEC BSPs. 
The exact files provided depend on the board, BSP version, image recipe, and release.

**.wic** — Complete disk image
Main format for initial setup

A .wic file contains a prepared disk layout and the contents of its partitions. Depending on the BSP, it includes bootloader files, kernel, device tree, and root filesystem.
When to use it: When preparing a bootable SD card or when the BSP supports using the WIC image to initialize eMMC.

Example filename: phytec-qt6demo-image-phyboard-pollux-imx8mp-3.wic
Official reference: PHYTEC Pollux BSP Manual. 

**.wic.xz** — Compressed complete disk image and Convenient for downloading

This is a WIC disk image compressed using XZ. It takes less storage space and generally downloads faster than the uncompressed .wic file.

When to use it: When downloading a complete image for SD-card flashing using a compatible tool such as bmaptool or a graphical image writer.

Example filename: phytec-image.wic.xz
You can decompress it with:
unxz -k phytec-image.wic.xz

The -k option keeps the original compressed file.
Reference: PHYTEC SD-card flashing guide - PhyCORE-AM64x

**.tar.xz** — Compressed filesystem archive

A .tar.xz file is an archive containing files and directories compressed using XZ. For example, it can contain the Linux root filesystem.
When to use it: When the BSP provides a rootfs archive for extracting into a prepared filesystem, setting up an NFS root filesystem, or performing another documented manual installation procedure.
Important: A rootfs archive is not automatically a complete bootable disk image. It normally does not provide the full partition layout and boot setup required by a board.
Example filename:
phytec-image.tar.xz
Reference: PHYTEC pre-built binaries documentation. 

**.partup**  — PHYTEC update package
This format is used by the partup utility to install a supported complete image to storage such as eMMC. PHYTEC documents its use for production deployment and partition adjustment.

When to use it: When the particular BSP provides a .partup package and its documentation supports the target storage and installation method.

Example command from PHYTEC's documented workflow:
partup install image.rootfs.partup /dev/mmcblk0
Warning: The device path is only an example. Verify the correct target device before executing the command.
Reference: PHYTEC image types and FitImage guide. 

4.5 .dtb — Device tree blob

A compiled device tree describing the board hardware.

When to use it: When updating the board hardware description as part of a BSP-supported software update.

A .dtb file alone is not a complete Linux image.

.**itb / FIT image** — Boot container
A FIT image can package the kernel, device tree, and optionally other boot components. PHYTEC's i.MX 8M Plus BSP uses a FIT-based boot arrangement starting with PD24.1.0.

When to use it: As part of the boot process when the BSP and bootloader are configured for FIT images. It is not a replacement for the complete disk image needed for initial storage setup.

Reference: PHYTEC FitImage documentation. 


What is the difference between a complete image and individual components?

Think of a Linux system as a complete machine.
Complete disk image — .wic
Prepared partitions and their contents
Boot components
Bootloader, kernel and device tree, depending on the BSP.
Root filesystem
Linux commands, libraries, services and applications.

The exact partition arrangement varies by BSP and board.

If you flash a complete disk image, the image-writing tool writes the prepared layout and data to the target storage.

If you download only a kernel or root filesystem archive, you must follow the BSP's instructions for installing that component and ensuring all other required components are present.
