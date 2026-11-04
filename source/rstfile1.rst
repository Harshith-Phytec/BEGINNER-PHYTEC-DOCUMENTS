Run GUI Guider Project on phyBOARD-NASH i.MX93
==============================================

This guide describes how to import a project generated with GUI Guider into
Eclipse, build it for the PHYTEC phyBOARD-NASH i.MX93, copy the application to
the board, and verify that it runs.

The workflow follows the general Eclipse cross-compilation and execution
process in the PHYTEC Development Environment Guide (L-833e.A15). A generated
GUI Guider project may need additional configuration for its build system,
framework, libraries, and resource files. Confirm those details for the
specific project and BSP release.

.. contents:: In this guide
   :local:
   :depth: 2

Objective
---------

The objective is to build a GUI Guider application on a development computer
using the PHYTEC software development kit (SDK), then transfer and run the
result on the phyBOARD-NASH i.MX93.

This guide covers the standard application workflow. It does not provide
project-specific GUI framework settings or instructions for writing a bootable
image to eMMC or an SD card.

Prerequisites
-------------

Before starting, ensure that you have:

* A GUI Guider project generated for the application.
* A PHYTEC development environment and SDK for the phyBOARD-NASH i.MX93.
* Eclipse installed in, or available from, that development environment.
* A booted phyBOARD-NASH i.MX93 with network connectivity to the development
  computer.
* Login credentials and the board's IP address.
* The target-side GUI framework, shared libraries, and other dependencies
  required by the application.

.. note::

   Use an SDK intended for the target board and BSP release. A host-native
   compiler can produce a program that builds successfully but cannot run on
   the i.MX93.

Basic terminology
-----------------

* **GUI Guider:** A graphical user interface design tool for creating screens,
  buttons, labels, and other visual elements.
* **Eclipse:** An integrated development environment used to open, edit, build,
  and debug supported projects.
* **phyBOARD-NASH i.MX93:** The PHYTEC target board on which the application is
  intended to run.
* **BSP (Board Support Package):** The software components and configuration
  that provide operating system and hardware support for the board.
* **SDK (Software Development Kit):** The development tools, libraries,
  headers, and cross-compiler used to build applications for the target.
* **Cross-compilation:** Building on one computer architecture, such as x86-64,
  to create an application for another architecture, such as the ARM-based
  i.MX93.
* **Executable:** A compiled program that the target operating system can run.
* **SCP (Secure Copy Protocol):** A method for securely copying files between
  computers over a network.

Development workflow
--------------------

The overall workflow is:

#. Generate the application project using GUI Guider.
#. Check that the target BSP and development SDK are compatible.
#. Set up the SDK environment and launch Eclipse.
#. Import the generated project into the Eclipse workspace.
#. Configure the target toolchain and project dependencies.
#. Build the application and locate the resulting executable.
#. Copy the executable and any required resources to the board.
#. Run the application on the board and verify its operation.

Check the board and SDK versions
--------------------------------

The SDK and BSP must be compatible. PHYTEC specifies that the major BSP
version on the target should match the major version of the SDK used for
development.

First, boot the board and log in to it. Check the installed BSP release:

.. code-block:: console

   lsb_release -r

On the development computer or PHYTEC virtual machine, inspect the installed
SDK version using the method appropriate for that environment. For the
documented PHYTEC virtual machine, use:

.. code-block:: console

   cat /opt/version-*

Compare the BSP and SDK versions and select the environment setup file for the
installed i.MX93 SDK. The following is the example given for the documented
environment:

.. code-block:: console

   source /opt/environment-setup-cortexa55-phytec-linux

The exact filename and path can vary by BSP release and SDK installation. Use
the environment file supplied with the SDK you installed.

Open Eclipse
------------

Start Eclipse from a terminal in the configured development environment:

.. code-block:: console

   eclipse &

If Eclipse asks for a workspace, select or create the directory where you want
to store your projects. A workspace is Eclipse's directory for project data
and development settings.

Import the GUI Guider project
-----------------------------

In Eclipse:

#. Select **File > Import**.
#. Expand **General** and select **Existing Projects into Workspace**.
#. Select **Next**.
#. Browse to the directory containing the generated project.
#. Select the project found by Eclipse and choose **Finish**.

The imported project should appear in the Eclipse Project Explorer.

.. important::

   These are the standard Eclipse project-import steps. A GUI Guider-generated
   project may require additional project configuration, build files, GUI
   framework libraries, or resource paths. Confirm that the generated project
   format is supported by the selected Eclipse build system before continuing.

Configure and build the project
-------------------------------

Before building, check that the project uses the SDK's i.MX93 cross-compiler,
not the development computer's native compiler. Review the project settings
and confirm that:

* The compiler and linker target the i.MX93 architecture.
* Required include directories and libraries are available in the SDK.
* The GUI framework and other dependencies match the target's software image.
* All generated source files and GUI resources are included in the project.
* The build configuration points to the intended output directory.

Build the project by selecting **Project > Build Project**. Review the Eclipse
Console for compiler and linker errors. Resolve errors before proceeding.

When the build succeeds, locate the executable in the output directory
specified by the project configuration.

.. note::

   The output is not necessarily a ``.elf`` or ``.bin`` file. A Linux GUI
   application may produce an executable with no file extension. A ``.bin``
   file can also represent a different kind of binary, so confirm the expected
   output from the project's build configuration.

Transfer the application to the board
-------------------------------------

After a successful build, use SCP from the development computer to copy the
executable to the board:

.. code-block:: console

   scp ./application root@<BOARD_IP>:/home/root/

Replace ``application`` with the actual executable filename and
``<BOARD_IP>`` with the board's IP address. If the application needs images,
fonts, configuration files, or other resources, copy those files as well.
Preserve the directory structure expected by the application.

Run the application
-------------------

Connect to the board over SSH:

.. code-block:: console

   ssh root@<BOARD_IP>

On the board, change to the directory containing the application, make it
executable, and start it:

.. code-block:: console

   cd /home/root
   chmod +x application
   ./application

Replace ``application`` with the actual executable filename.

For a GUI application, verify that the required display environment, GUI
framework, libraries, and permissions are available on the target. A successful
compilation alone does not guarantee that the application will start.

Verify the result
-----------------

Confirm that:

* The application built without compiler or linker errors.
* The executable and required resources were copied to the correct locations.
* The application starts without missing-library or permission errors.
* The GUI appears on the intended display, if the application uses one.
* Buttons, labels, and other GUI elements behave as expected.
* Required images, fonts, and other resources load correctly.

If the application does not run, examine the terminal output. Check that the
target has the required shared libraries and GUI runtime components, and
confirm that the application can find its resource files.

Copying an application versus flashing an image
-----------------------------------------------

These operations have different purposes:

* **Copying an application** transfers an executable to the board's existing
  Linux filesystem. This is normally suitable for testing a standalone
  application.
* **Flashing an image** writes an image to storage such as eMMC or an SD card.
  This may be required to prepare or update the bootable operating system, or
  to write another image specifically designed for that purpose.

The standard PHYTEC Eclipse workflow demonstrates building an application,
transferring it to the target, and running it. It does not mean that every
generated executable should be flashed directly to eMMC or an SD card.

For normal application deployment, copying the executable to the filesystem
may be sufficient. For permanent integration, the application can be included
in the appropriate Yocto image. Follow the instructions for the exact board
and BSP release for any storage-writing procedure.

Conclusion
----------

Using a compatible PHYTEC SDK and cross-compiler, developers can build a GUI
Guider project on a host computer and run it on the phyBOARD-NASH i.MX93. The
essential steps are to import the project into Eclipse, configure the target
build environment and dependencies, compile the application, transfer the
executable and its resources, and test it on the board.

References and scope
--------------------

* **Official source:** PHYTEC Development Environment Guide (L-833e.A15).
* The guide describes the general Eclipse cross-compilation and execution
  workflow. Exact GUI Guider project settings and any eMMC/SD flashing
  procedure must be confirmed against the project and the relevant
  phyBOARD-NASH BSP documentation.
