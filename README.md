# Purpose
Playbooks to onboard an EOS device onto CVaaS.

There are two playbooks in the playbooks folder:
- **cvaas-onboarding.yml** - will connect to CVaaS, create an onboarding token, upload the token to flash on the EOS device(s), and then configure termiAttr
- **cvaas-onboarding-renew.yml** - will connect to CVaaS, create an onboarding token, upload the token to flash on the EOS device(s), and then restart terminAttr.  Thi
    - Useful for an EOS device that was offline when the certificates used to authenticate TerminAttr expire, and couldn't be renewed.


# Instructions for use
1. Create an inventory.yml file that contains the hosts to onboard to CVaaS.
2. Log into CVaaS and create a service account and token.
3. Create a file called `cvaas-service.tok` and place the token inside it.
4. Edit the playbook and update: 
    - `cvaas_url:` variable to be your CVaaS instance url
    - `ansible_user:` and `ansible-password:` variables with credentials for the eAPI call to the EOS device
5. Run the appropriate playbook:
    `ansible-playbook -i <inventory_file> playbooks/cvaas-onboarding.yml`
    `ansible-playbook -i <inventory_file> playbooks/cvaas-onboarding-renew.yml`


# Sample inventory file
```
---
all:
  children:
    hosts:
      spine1:
        ansible_host: <ip>
      spine2:
        ansible_host: <ip>
      leaf1:
        ansible_host: <ip>
      leaf2:
        ansible_host: <ip>
```