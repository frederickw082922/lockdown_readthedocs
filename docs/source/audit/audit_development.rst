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

- title
- meta data - This consists of data found in the benchmark documentation

  - This potentially changes with each release (this will need to be correct for the release being worked on)

**CIS Specific**
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

**STIG Specific**

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
