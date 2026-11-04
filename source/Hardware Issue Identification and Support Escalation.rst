.. START:section:0:0
**Hardware Issue Identification and Support Escalation**
========================================================

.. END:section:0:0

.. START:main_content:0:0
Hardware issue identification is the process of checking whether a problem originates from the System-on-Module (SoM), carrier board, power supply, connections, boot configuration, or software.


.. END:main_content:0:0

**Checks When a Board Does Not Power On or Boot** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Confirm that the power supply matches the board's specified voltage and current requirements.
2. Check that the power cable is connected to the correct power input.
3. Inspect the connectors for visible damage or poor connections.
4. Observe the board's power-indicator LEDs, if provided.
5. Check the hardware manual for the expected power-up procedure.
PHYTEC highlights power sequencing and reset timing as important factors in carrier-board startup. Incorrect power sequencing can prevent a system from booting correctly. 

If the board does not power on: Record the power supply details, LED behaviour, and visible observations. Do not conclude that the SoM is defective based on this symptom alone.

**Check the serial console** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Connect the board to the host PC through the debug interface specified in its Quick Start Guide.
- Confirm that the correct serial port is selected.
- Verify the baud rate and other serial settings.
- Open the terminal before powering on the board.
- Capture the messages displayed during startup.
PHYTEC provides board-specific serial-console instructions in its BSP manuals and Quick Start Guides.

The console output helps establish whether the board reaches the bootloader, begins Linux startup, or stops earlier in the boot process.

**Check the boot configuration** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If the board powers on but does not boot, verify:
- The boot-mode switch settings.
- Whether the intended boot device is connected.
- Whether the storage contains a compatible bootable image.
- Whether the bootloader produces any error messages.
PHYTEC identifies incorrect boot-mode strapping and wrong boot-source selection as potential reasons a carrier board may fail to boot. 

Check the software and BSP
If the bootloader starts but Linux fails to load, inspect the boot log and compare the installed software with the BSP documentation.

PHYTEC documents that boot problems can result from incorrect software integration or configuration. For example, an error in OP-TEE integration can prevent the boot process from completing on applicable platforms. 
PHYTEC BSP Documentation+1


Do not immediately classify a boot failure as a hardware defect. First establish which stage fails and whether the software image and configuration are correct.

Distinguish Hardware Faults from BSP or Software Issues
The following table provides an initial troubleshooting approach.
Check or observation	Possible direction of investigation
No power indication	Power supply, connections, or board power circuitry
Power is present, but no console output	Debug connection, reset, power, or early boot problem
Bootloader appears, but Linux fails	BSP image, boot configuration, kernel, or hardware
Linux boots, but a peripheral is unavailable	Physical interface, device-tree configuration, driver, or peripheral
A known-compatible software image boots successfully	The original image or configuration may be contributing to the failure
The same failure persists with verified connections and compatible software	Further hardware investigation may be required

These are diagnostic possibilities, not definitive fault classifications. A hardware problem and a software problem can produce similar symptoms.

PHYTEC recommends using the appropriate Quick Start Guide, hardware manual, BSP manual, and reference design to investigate board-specific issues.

**Information to Collect Before Support Escalation** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Before reporting a suspected defective SoM or carrier board, collect enough information for the support team to understand and reproduce the issue.

Board and software details
Record the following information:
- Board and SoM product name.
- Carrier-board model and hardware revision.
- SoM revision, if available.
- BSP version and Linux kernel version, if accessible.
- Boot media used, such as SD card or eMMC.
- Whether the board is new, previously working, or recently reworked.
PHYTEC provides product-specific hardware manuals and BSP documentation because hardware configurations and software support vary between products and releases


. Logs and test results
Collect the following where available:
- Complete serial boot log, beginning before power-on.
- The last visible boot message if startup stops.
- Power supply specifications and observed power-indicator behaviour.
- Results of the connection and boot-mode checks.
- The software image and BSP version used during testing.
- A clear description of the failure and the steps required to reproduce it.
- Results from any comparison test using a known-good, compatible component, if available and permitted by the hardware documentation.
Keep the original logs unchanged. If the board cannot boot, record that the log is unavailable rather than assuming what the output would have been.

Photographs and visual evidence
Take clear photographs of:
- The complete board, showing its overall condition.
- The SoM and carrier-board identification labels.
- The connectors and cables used during testing.
- Any visible damage or unusual condition.
- Relevant switch positions, where these can be photographed safely.
Photographs should help support identify the exact hardware and understand the reported condition. PHYTEC's support resources and product documentation can then be used to identify the appropriate product-specific troubleshooting procedure. 

 Escalate the Issue to PHYTEC Support

Once the initial checks are complete, prepare a concise issue report.

Include:
1. Product identification: SoM, carrier board, and hardware revisions.
2. Problem description: What happens when power is applied or the board boots.
3. Expected behaviour: What should happen according to the applicable manual.
4. Tests performed: Power, connections, serial console, boot configuration, and software checks.
5. Evidence: Boot logs, photographs, and test results.
6. Reproduction steps: The sequence that consistently produces the problem.
Use the official PHYTEC Support Portal to contact its technical support team. PHYTEC provides access to technical support, hardware manuals, Quick Start Guides, and BSP downloads through its support resources.

**Important Embedded System Help** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Do not declare a SoM or carrier board defective until the available evidence has been reviewed. If the cause remains unclear after the documented checks, report the observed symptoms and request further diagnosis.**

**Identify Potential Hardware Failures** 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

PHYTEC's hardware documentation and guidance for carrier-board bring-up highlight power sequencing, boot-mode configuration, reset timing, and debug access as important factors in reliable startup. 

