Getting started with audit
==========================


Overview
--------

Ansible remediation for security benchmarks now utilises an opensource
go binary called `goss <https://goss.rocks>`_ to audit the system.

Enabling an alternative tool to check and ensure that the remediation
role is working as expected.

Ensuring consistency in checks by using the same settings and controls
that have been enabled in the remediation steps, are the same ones
checked by the audit.

It can be run in two ways:

- Enabled to run as part of the ansible playbook and is setup to capture pre remediation and post remediation states. Using the same configured variables as used in remediation

- Standalone script

  - run_audit.sh (Linux (shell))
  - run_audit.ps1 (Windows(powershell))

Currently enabled playbooks
~~~~~~~~~~~~~~~~~~~~~~~~~~~

`CIS Benchmarks <https://lockdown-readthedocs.readthedocs.io/en/latest/CIS/benchmarks_CIS.html>`_

`STIG Benchmarks <https://lockdown-readthedocs.readthedocs.io/en/latest/CIS/STIG_table.html>`_


Setup auditing - Standalone
---------------------------

It is assumed that as you have the script you have downloaded the audit content already from source control or your own configired location.

The following requirements are needed OS independant

- Super user or permissions to run privilege commands

  - Linux sudo can work
  - Windows ability to run security audits and query group or local policy.

- goss binary appropriate for the OS
  
  - to be accessible to the OS so this can be run

    - The expected path is found inside the OS script and can be adjuested as required.

      - linux - default /usr/local/bin/goss
      - Windows - 

  - Linux

    - `binary <https://github.com/aelsabbahy/goss/releases/download/v0.3.16/goss-linux-amd64>`_
    - `checksum <https://github.com/aelsabbahy/goss/releases/download/v0.3.16/goss-linux-amd64.sha256>`_

  - Windows

    - `Binary <https://github.com/aelsabbahy/goss/releases/download/v0.3.16/goss-alpha-windows-amd64.exe>`_
    - `checksum <https://github.com/aelsabbahy/goss/releases/download/v0.3.16/goss-alpha-windows-amd64.exe.sha265>`_

Defining the audit
~~~~~~~~~~~~~~~~~~

Each script runs against a configures variables file found in the content location in

.. code-block:: shell

   {downloaded content}/vars/{benchmark}.yml

These are the variables that configure which controls are run along with some configurable settings during an audit.

Each script has the ability for you to set several variables depending on your environment requirements.
e.g. locations on where to find binary or output locations

There is also switch options to allow you to run a couple of these at run time.

Script runtime options

- The group option allows a meta field to be assigned against the report for use in analysis if servers can be grouped together.
If more than one group this can be comma seperated

- The outfile is the filename and location to save the full audit report to.

Running on Linux
~~~~~~~~~~~~~~~~

- Script 

  - run_audit.sh (found in content directory)

This is written that:

- Uppercase variable are the only ones that should need changing
- lowercase variables are the ones that are discovered or built from existing.

script variables
example:

.. code-block:: shell

   AUDIT_BIN="${AUDIT_BIN:-/usr/local/bin/goss}"  # location of the goss executable
   AUDIT_FILE="${AUDIT_FILE:-goss.yml}"  # the default goss file used by the audit provided by the audit configuration
   AUDIT_CONTENT_LOCATION="${AUDIT_CONTENT_LOCATION:-/var/tmp}"  # Location of the audit configuration file as available to the OS


script help

.. code-block:: shell

   Script to run the goss audit

   Syntax: ./run_audit.sh [-f|-g|-o|-v|-w|-h]
   options:
   -f     optional - change the format output (default value = json)
   -g     optional - Add a group that the server should be grouped with (default value = ungrouped)
   -o     optional - file to output audit data
   -v     optional - relative path to thevars file to load (default e.g. /var/tmp/RHEL7-CIS/vars/CIS.yml)
   -w     optional - Sets the system_type to workstation (Default - Server)
   -h     Print this Help.

   Other options can be assigned in the script itself

Windows
~~~~~~~

- Script 

  - run_audit.sh (found in content directory)

Variables can be set within the script

# Set Variables for Audit

$DEFAULT_CONTENT_DIR = "C:\remediation_audit_logs"  # This can be changed using cli
$DEFAULT_AUDIT_BIN = "$DEFAULT_CONTENT_DIR\goss.exe"  # This can be changed using cli option

.. raw:: shell

   NAME
       C:\remediation_audit_logs\Windows-2019-CIS-Audit\run_audit.ps1

   SYNOPSIS
       Wrapper script to run an audit


   SYNTAX
       C:\remediation_audit_logs\Windows-2016-CIS-Audit\run_audit.ps1 [[-auditbin] <String>] [[-auditdir] <String>]
       [[-varsfile] <String>] [[-group] <String>] [[-outfile] <String>] [<CommonParameters>]


   DESCRIPTION
       Wrapper script to run an audit on the system using goss.
       This allows for bespoke variables to be set


   PARAMETERS
       -auditbin <String>

       -auditdir <String>
           default: $DEFAULT_CONTENT_DIR
           Ability to change the location of where the content can be found
           This is where the audit content is stored
           e.g. c:/windows_audit

       -varsfile <String>
           default: $DEFAULT_VARS_FILE
           Ability to set a variable file defined with the settings to match your requirements

       -group <String>
           default: none
           Ability to set a group that the system belongs to
           Can be used when matching similar system in that same group

       -outfile <String>
           default: $AUDIT_CONTENT_DIR\audit_$host_os_hostname_$host_epoch.json
           Ability to set an outfile to send the full audit output to
           Requires path to be set.
           e.g. c:/windows_audit_reports

       <CommonParameters>
           This cmdlet supports the common parameters: Verbose, Debug,
           ErrorAction, ErrorVariable, WarningAction, WarningVariable,
           OutBuffer, PipelineVariable, and OutVariable. For more information, see
           about_CommonParameters (http://go.microsoft.com/fwlink/?LinkID=113216).

       -------------------------- EXAMPLE 1 --------------------------

       PS C:\>./run_audit.ps1

       ./run_audit.ps1 -auditbin c:\path_to\binary.name
       ./run_audit.ps1 -auditdir c:\somepath_for _audit_content
       ./run_audit.ps1 -varsfile myvars.yml
       ./run_audit.ps1 -outfile path\to\audit\output.json
       ./run_audit.ps1 -group webserver
