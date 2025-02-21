Ansible playbook for Turing
===========================

Ansible repo with a basic playbook and a sample directory structure.

Requirements
------------

### Connecting to GCP 
GCP dynamic inventory needs the `gcp_compute` Ansible plugin available in the `google.cloud` collection, check if you already have the plugin installed by running:
```bash
ansible-galaxy collection list | grep google.cloud 
```
If you dont have the inventory plug in you can install it with:
```bash
ansible-galaxy collection install google.cloud
```
To authenticate on GCP the `google-auth` and `requests` Python library is required, install it using pip:
```bash
pipx inject --include-deps google-auth
pipx inject requests
```

The `gcp_compute` plugin will need a service account to be able to enumerate the resources, create a service account in the IAM & Admin page of the GCP console with the following Roles:
* Compute OS Admin Login
* Compute OS Login
* Service Account User

Download the service account key to allow ansible to access the project, you will need appropriate IAM access rights (editor, owner or Service Account Key Admin); you can save this key in the project as JSON files are ignored in the repo. The key name will need to be specified in the gcp.yaml file (note: Ansible expects the inventory file to be called gcp.yaml or gcp.yml) using the `service_account_file:` key.

Test your setup using the following command from the root of the repository (you should get a list with the GCE resources in the target project):
```bash
ansible-inventory -i inventory/gcp.yaml --graph
```
### Allowing the service account to login to instances
We need to allow the service account to login the instances and to elevate privileges so we can use ansible to update instances, the service account has the ability to impersonate users via the Service Account User role but we will need to add our user ssh keys to the service account first.
Also, GCE must have a Custom Metadata key called `enable-oslogin` set to `TRUE`.

You will need to pass sone info to ansible, this can be done using an ansible.cfg file, copy and rename the ansible.cfg.EXAMPLE file to ansible.cfg and fill in the needed info (private_key_file and remote_user).

Activate the ansible service account, we will need the service account key to do this:
```
gcloud auth activate-service-account <service-account email> --key-file=/path/to/keyfile.json
```

Once activate we can add a personal key to the ansible service account (needs the Cloud OS Login and Identity and Access Management (IAM) APIs enabled); first get the service-account uniqueID:
```
gcloud iam service-accounts describe <service-account email> --format='value(uniqueId)'
```

 Then add your ssh key:
```
 gcloud compute os-login ssh-keys add --key-file=/path/to/your/sshkey.pub
```

Note the username from the result (sa_ + uniqueID) and try to login with ssh:
```
ssh -i /path/to/your/sshkey.pub sa_<uniqueID>@server.ip  
```

If you can successfully login then you can test ansible (ping all instances in the group zone_europe_west1_b):
```
# ansible -i gcp.yaml zone_europe_west1_b -m ping
``` 

You should get results in the form:
```
34.122.92.187 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

Variables
---------

N/A

Dependencies
------------

N/A

Notes
-----

Add any other notes here.

License
-------
License WTFPL (http://www.wtfpl.net/txt/copying/).

Author Information
------------------
Angel Corpuz
 - contact me at: angel.corpuz.jr(at)gmail.com
