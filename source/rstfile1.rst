.. START:section:0:0
**Beginner's guide |Board Bring-Up and Serial Console Setup**
=============================================================

.. END:section:0:0

.. START:main_content:0:0
Board bring-up is the initial process of powering on a new embedded board, checking its basic hardware operation, and confirming that its software starts correctly.

For a PHYTEC board, the first checks include:
- Connecting the board to a host PC through its debug interface.
- Opening a serial console to observe boot messages.
- Checking whether the bootloader and Linux start.
- Confirming that a login prompt is available.
- Identifying boot problems from the console output.
PHYTEC's board-specific Quick Start Guides and BSP manuals describe the required connections, serial settings, boot procedure, and initial verification steps. The exact connector and boot configuration depend on the board model.

.. END:main_content:0:0

**Board bring-up** 
~~~~~~~~~~~~~~~~~~~

Board bring-up is the initial process of powering on a new embedded board, checking its basic hardware operation, and confirming that its software starts correctly.  For a PHYTEC board, the first checks include: - Connecting the board to a host PC through its debug interface. - Opening a serial console to observe boot messages. - Checking whether the bootloader and Linux start. - Confirming that a login prompt is available. - Identifying boot problems from the console output. PHYTEC's board-specific Quick Start Guides and BSP manuals describe the required connections, serial settings, boot procedure, and initial verification steps. The exact connector and boot configuration depend on the board model. 

**Identify the debug interface** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The serial console provides access to the board's boot messages and command-line interface. It helps developers observe the boot process and investigate problems when the board does not start normally.

For the phyBOARD-Nash i.MX93, PHYTEC specifies the USB-C debug connection at connector X37. Other PHYTEC boards may use a different USB debug port or a serial interface on an evaluation board. Always refer to the relevant Quick Start Guide.

**Connection procedure** 
~~~~~~~~~~~~~~~~~~~~~~~~~

1. Locate the debug connector specified in the board's PHYTEC documentation.
2. Connect the board's debug interface to a USB port on the host PC.
3. Prepare the appropriate serial terminal application on the host.
4. Configure the serial connection using the settings specified by PHYTEC.
5. Connect the board's power supply according to its hardware manual.
6. Power on the board and observe the messages appearing in the terminal.

**Configure and Use Minicom on Linux**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Minicom is a serial terminal application used to communicate with a board through a serial port. PHYTEC documents its use for accessing the serial console on supported boards. 
download.phytec.de+1

Step 1: Install Minicom
On a supported Ubuntu/Linux host, run:
sudo apt update
sudo apt install minicom

Step 2: Identify the serial device
Connect the board's debug USB port to the host PC. Then run:
ls /dev/ttyUSB*

The serial device may appear as /dev/ttyUSB0. The exact device name depends on the host and connected interface. 

Step 3: Open Minicom
Run the following command, replacing the device name if required:
sudo minicom -D /dev/ttyUSB0

Step 4: Configure the serial settings
PHYTEC's Minicom instructions use the following settings:

- Baud rate: 115200
- Data bits: 8
- Parity: None
- Stop bits: 1
- Hardware flow control: No
- Software flow control: No
To configure these settings through Minicom, open its configuration menu: sudo minicom -s

Select Serial port setup, enter the correct serial device, configure the settings, and save the configuration.

**Configure a Serial Terminal on Windows** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PHYTEC recommends PuTTY for Windows-based serial console access in its Quick Start documentation. 

Step 1: Connect the board
Connect the PHYTEC board's documented debug port to a USB port on the Windows PC.

Step 2: Identify the COM port
1. Open Device Manager.
2. Expand Ports (COM & LPT).
3. Identify the COM port associated with the connected board.
For example, Windows might display COM6. This is an example; use the COM port shown on your PC. 

Step 3: Open PuTTY
1. Launch PuTTY.
2. Select Serial as the connection type.
3. Enter the COM port identified in Device Manager.
4. Set the speed to 115200.
5. Open the connection.
Configure the serial connection for 8 data bits, no parity, 1 stop bit, and no flow control, as specified in the relevant PHYTEC manual. 

Capture Boot Logs and Identify Common Boot Failures
Boot logs : Boot logs are messages printed by the board while starting. Depending on the platform, the serial console can display output from the bootloader and Linux kernel.

PHYTEC's i.MX93 BSP manual instructs users to connect the debug console, power on the board, and observe the bootloader and Linux startup messages.

**How to capture the boot log** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Connect the serial console before powering on the board.
2. Open Minicom or PuTTY using the correct serial settings.
3. Start the terminal session.
4. Power on or reset the board.
5. Observe the messages from the beginning of startup through the Linux login prompt.
Keep the terminal session open while the board boots. If your terminal application supports logging to a file, use its logging function to save the output for later analysis. The exact logging menu depends on the terminal application.



**Common symptoms to observe** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Symptom	What to check first :

No console output - Confirm the correct debug connector, serial device or COM port, baud rate, and cable connection.
Unreadable characters - Verify that the serial settings match the PHYTEC manual, especially the baud rate and 8N1 format.
Bootloader messages appear, but Linux does not start	Record the last visible messages and consult the BSP manual for the relevant boot stage.
Linux starts but no login prompt appears - Check the remaining console output and the board-specific login instructions.
Board does not boot from the expected device - Check the documented boot-mode switch setting and the bootable media.

These are initial diagnostic checks based on the serial connection and boot procedure documented by PHYTEC; they are not a complete list of every possible failure or a claim that each symptom has one specific cause.

**Basic Checks After Powering On a New Board** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Use this checklist during initial board bring-up.

Connections: Confirm that the debug cable and the correct power supply are connected as specified by the hardware manual.

Boot configuration: Verify that the boot-mode switches are set for the intended boot source.

Serial console: Confirm that the correct port and serial settings are selected.


Bootloader: Check whether bootloader messages appear.

Linux startup: Check whether Linux startup messages follow.

Login prompt: Confirm that the expected login prompt appears.

Record the result: Save the boot log if possible and note any errors or the last visible startup message.

For the phyBOARD-Nash i.MX93, PHYTEC documents the X37 USB-C debug interface and 115200 baud, 8N1 serial settings. Always verify the corresponding connector and boot procedure for other PHYTEC boards.
