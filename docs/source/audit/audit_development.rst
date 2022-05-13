How to contribute to audit development
--------------------------------------

Adding code
~~~~~~~~~~~

We are trying to maintain a set of standards we would like to achieve for all code that is written.

Considerations
~~~~~~~~~~~~~~

- Keep it as simple as possible to aid investigation and debug
- Ensure it aligns with the remediate portion (the two are intrinsically linked when combined)

Layout
~~~~~~

- Each control should be in its own file.
- Use variables wherever possible
- some controls where asociated maybe grouped together
  
  - Multiple stages tests (config and runnig tests)
  - Similar tests (filesystem mount options)

- Structure should be where appropriate

.. code-block:: raw

   |- control group e.g. section_1
   |-- grouped controls
   |---- control test

**e.g.**

.. code-block:: raw

    |- section_1
    |-- cis_1.1
    |--- cis_1.1.x.yml

Content
""""""""

- Each task requires the following to be included

  - Title
  - At least one control variable (whether control ID is to be run or not).
    
    - If only one test add the variable prior to to the goss_module itself
  
  - Use appropriate goss_modules before using command

    - There are circumstances when command allows discovery/filters results

For a full list of goss and how to use the goss_modules (tests).
`Goss Docs <https://github.com/aelsabbahy/goss/blob/master/docs/manual.md>`_

**Example**

*Basic test*

..  code-block:: yaml

    {{ if .Vars.rhel9cis_level_1 }}
      {{ if .Vars.rhelcis9_1_1_10 }}
    command:
      usb-storage:
        title: 1.1.10 | Disable USB Storage
        exit-status: 0
        exec: "modprobe -n -v usb-storage | grep -E '(usb-storage|install)'"
        stdout: 
        - install /bin/true
        meta:
          server: 1
          workstation: 2
          CIS_ID: 1.1.10
          CISv8: 
          - 10.3
          CISv8_IG1: true
          CISv8_IG2: true
          CISv8_IG3: true
      {{ end }}
    {{ end }}


**Breakdown**

..  code-block:: raw

    {{ if .Vars.rhel9cis_level_1 }}                                                     ## if rhel9cis_level_1 is true
      {{ if .Vars.rhelcis9_1_1_10 }}                                                    ## if rhelcis9_1_1_10 is true
    command:                                                                            ## goss_module
      usb-storage:                                                                      ## unique name associated with the command
        title: 1.1.10 | Disable USB Storage                                             ## title  {{ control id }}| {{ control title }}
        exit-status: 0                                                                  ## Options for goss_module
        exec: "modprobe -n -v usb-storage | grep -E '(usb-storage|install)'"            ## Options for goss_module
        stdout:                                                                         ## Options for goss_module
        - install /bin/true                                                             ## Options for goss_module
        meta:                                                                           ## Meta data used for reporting (see metadata)
          server: 1
          workstation: 2
          CIS_ID: 1.1.10
          CISv8: 
          - 10.3
          CISv8_IG1: true
          CISv8_IG2: true
          CISv8_IG3: true
      {{ end }}                                                                         ## Close if statement
    {{ end }}                                                                           ## Close if statement

**Variable precedence**

The greater impact that a variable has the higher in the test it should be added

.. code-block:: raw
   {{ .Vars.section_1 }}
     {{ .Vars.rhelcis8_1_1_1_1 }}


Metadata
""""""""

This is added to the audit benchmark for reference across compliance requirements
It uses two level of metadata

- audit metadata - this is general system information and audit information
- control metadata - this is added to every audit control and is specific to each control.


**Audit Metadata** (required)

  - This is items set/discovered about the system within the script set via vars in the script
  - Referenced in the goss.yml file.

Contains:

..  csv-table:: Discovered audit variables
    :header: "Variable Title", "Script variable name", "Purpose"
    :widths: 20, 20, 60

    "host_machine_uuid:", "{{ .Vars.machine_uuid }}", "discovered UUID of system (used as unique identifier)"
    "host_epoch:", "{{ .Vars.epoch }}", "epoch time that script initiated (part of output filename)"
    "host_os_locale:", "{{ .Vars.os_locale }}", "system locale (TZ)"
    "host_os_release:", "{{ .Vars.os_release }}", "OS version (e.g. 7)"
    "host_os_distribution:", "{{ .Vars.os_distribution }}", "OS distribution ( e.g. rhel)"
    "host_hostname:", "{{ .Vars.os_hostname }}", "hostname"
    "host_system_type:", "{{ .Vars.system_type }}"
    " ", "Linux", "Server/Workstation Manually set (default server)"
    " ", "Windows", "pulled from regkey and set"

**Special Variables**

- host_automation_group: {{ .Vars.auto_group }}

    - Used to group like systems when reporting
    - If run via remediate uses host group memberships
    - If run via script is an optional value or null

**Control Metadata** (required) 
  
  - This consists of data found in the benchmark documentation
  - This potentially changes with each release update (this will need to be correct for the release being worked on)

*CIS Specific*

This contains the following:

- server: cis level options: (1|2)
- workstation: cis level: (1|2|NA)
- CIS_ID: control reference
- CISv8: list of associated groups the control is associated to
- CISv8_IG1: Boolean if meets that association

.. code-block:: yaml

    meta:
      server: 1
      workstation: 1
      CIS_ID: 1.1.1.1
      CISv8:
      - 4.8
      CISv8_IG1: false
      CISv8_IG2: true
      CISv8_IG3: true

*STIG Specific*

All can be found in the details of the control itself

- Cat: the category this control is associated with (1|2|3)
- CCI: Common identifier This is found in  the stig documentation
- Group_Title: associated group the control is part of.
- Rule_ID: This changes with every interation of the control details
- STIG_ID: control id as known by STIG
- Vul_ID: vulnernability identifier

.. code-block:: yaml

    meta:
      Cat: 1
      CCI:
      - CCI-001494
      - CCI-001496
      - CCI-002165
      - CCI-002235
      Group_Title: SRG-OS-000257-GPOS-00098
      Rule_ID: SV-204392r646841_rule
      STIG_ID: RHEL-07-010010
      Vul_ID: V-204392
