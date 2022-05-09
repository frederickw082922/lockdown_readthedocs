
CIS Overview
------------

What is CIS?
~~~~~~~~~~~~

Centre for Internet Security

CIS is home to the Multi-State Information Sharing and Analysis Center® (MS-ISAC®), 
the trusted resource for cyber threat prevention, protection, response, and recovery 
for U.S. State, Local, Tribal, and Territorial government entities, 
and the Elections Infrastructure Information Sharing and Analysis Center® (EI-ISAC®), which supports the rapidly changing cybersecurity needs of U.S. elections offices.


What does this role do?
~~~~~~~~~~~~~~~~~~~~~~~


This role follows the CIS provides guide released for the OS/Platform/application.
Each guide is different, some have in excess of 200 controls and apply to various part of an OS but each guide is
updated regularly by CIS.

.. note::
   CIS is often used if there is absence for an appropriate released STIG version.

Control Severities
~~~~~~~~~~~~~~~~~~

Controls are divided into groups based on the following properties:

- Level 1
  The majority of control are based at this level.
  These controls have are condidered to have a low impact to a system.
  By implemnetting these controls is considered to medium change for disruption.

- Level 2
  These controls are considered high risk with a chance of system disruption if implemented.

.. note::

   All of the default configurations are found within 
   - remediation - ``defaults/main.yml``
   - audit 
     - standalone ``vars/CIS.yml``
     - combined ``vars/[system_hostname].yml``