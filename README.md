Ansible playbook for <client_name>
==================================

A brief description of the scope of the playbook goes here.

Requirements
------------

Any pre-requisites that may not be covered by Ansible itself should be mentioned here. For instance, if the role uses the EC2 module, it may be a good idea to mention in this section that the boto package is required. If any non standard Ansible modules are needed they should also be listed here; for example GCP dynamic inventory needs the gcp_compute Ansible plugin available in the google.cloud collection.

Also include any info needed to use dynamic inventories and such in this section .

Variables
---------

A description of the settable variables and in which files should go here, including any variables that can/should be set via parameters. All sensitive variables (passwords, access keys, tokens, etc.) in files must be ecrypted using ansible-vault and the password stored in Passbolt, the Passbolt entry name with the password should be mentioned here as well.

Dependencies
------------

A list of any other needed roles should go here, the needed roles must be also listed in the requirements.yaml file so as to be easily installed using ansible-galaxy:
```bash
ansible-galaxy install -p ./roles -r requirements.yaml
```
NOTE: the .gitignore file is set to ignore the roles directory so as to not have multiple copies of the same role. If and when a role is updated care must be taken to align any local copies, this should not happen frequently possibly only in the testing phase.

Notes
-----

Add any other notes here.

License
-------
Copyright 2023-TODAY Rapsodoo Italia S.r.L. (www.rapsodoo.com)

License LGPL-3.0 or later (https://www.gnu.org/licenses/lgpl)

Author Information
------------------
Rapsodoo Italia srl
 - https://rapsodoo.com
 - contact us at: system(at)rapsodoo.com
