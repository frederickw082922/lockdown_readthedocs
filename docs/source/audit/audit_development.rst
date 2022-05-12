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

Content
~~~~~~~

Each test requires the following to be included

- title - this needs to be in the format of

  - title: {benchmark-id} | {benchmark-heading}
  
Metadata
""""""""

This is added to the audit benchmark for reference across compliance requirements
It uses two level of metadata

- audit metadata - this is general system information and audit information
- control metadata - this is added to every audit control and is specific to each control.


**audit metadata** (required)

  - This is items set/discovered about the system within the script set via vars in the script
  - Referenced in the goss.yml file.

Contains:

- host_machine_uuid: {{ .Vars.machine_uuid }} - discovered UUID of system (used as unique identifier)
- host_epoch: {{ .Vars.epoch }} - epoch time that script initiated (part of output filename)
- host_os_locale: {{ .Vars.os_locale }} - system locale (TZ)
- host_os_release: {{ .Vars.os_release }} - OS version (e.g. 7)
- host_os_distribution: {{ .Vars.os_distribution }} - OS distribution ( e.g. rhel)
- host_automation_group: {{ .Vars.auto_group }} 

  - If set allows a meta field to be used to group like systems
  - If run via remediate uses host group memberships
  - if run via script is an optional value or null

- host_hostname: {{ .Vars.os_hostname }} - hostname
- host_system_type: {{ .Vars.system_type }} 

  - Linux server/workstation
  - Windows (domain_member or standalone or domain_controller) -refer to windows system types
  
- benchmark_type: {{ .Vars.benchmark_type }} - CIS or STIG
- benchmark_version: {{ .Vars.benchmark_version }} - Benchmark version (e.g.0.0)
- benchmark_os: {{ .Vars.benchmark_os } - Benchmark OS title (e.g. RHEL7)


**control metadata** (required) 
  
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
