# Purpose
Playbooks to onboard an EOS device onto CVaaS.

There are three playbooks in the playbooks folder:
- **cvaas-onboarding.yml** - will connect to CVaaS, create an onboarding token, upload the token to flash on the EOS device(s), and then configure termiAttr
- **cvaas-onboarding-renew.yml** - will connect to CVaaS, create an onboarding token, upload the token to flash on the EOS device(s), and then restart terminAttr.  Thi
    - Useful for an EOS device that was offline when the certificates used to authenticate TerminAttr expire, and couldn't be renewed.
- **multi_cluster_cvaas_onprem.yml** - use if you are dual-streaming to CVP and CVaaS as part of a migration to CVaaS


# Instructions for use - cvaas-onboarding.yml or cvaas-onboarding-renew.yaml
1. Create an inventory.yml file that contains the hosts to onboard to CVaaS.
2. Log into CVaaS and create a service account and token.
3. Create a file called `cvaas-service.tok` and place the token inside it.
4. Edit the playbook and update: 
    - `cvaas_url:` variable to be your CVaaS instance url (if not using Production CVaaS or if you wish to change to regional URL)
    - `cvaas_api_url` - variable to be the CVaaS URL in terminAttr. ie `apiserver.arista.io` (if not using Production CVaaS or if you wish to change to regional URL)
    - `ansible_user:` and `ansible-password:` variables with credentials for the eAPI call to the EOS device
    - `terminattr_vrf` - VRF to use to communicate with CVaaS (default if no VRF is used)
    - On the `hosts:` line after the `"- name: Onboard devices to CVaaS"` line, change it to the inventory group to use (if not all devices)
5. Run the appropriate playbook:
    `ansible-playbook -i <inventory_file> playbooks/cvaas-onboarding.yml`
    `ansible-playbook -i <inventory_file> playbooks/cvaas-onboarding-renew.yml`

# Instructions for use - multi_cluster_cvaas_onprem.yml
1. Create an inventory.yml file that contains the hosts to onboard to CVaaS.
2. Log into CVaaS and create a service account and token.
3. Log into CVP and create a service account and token.
4. Create a file called `cvaas-service.tok` and place the CVaaS token inside it.
5. Create a file called `onprem.tok` and place the CVP token inside it.
4. Edit the playbook and update: 
    - `cvaas_url:` variable to be your CVaaS instance url
    - `cvaas_api_url` - variable to be the CVaaS URL in terminAttr. ie `apiserver.arista.io`
    - `cvp_url:` variable to be your CVP  url/IP
    - `ansible_user:` and `ansible-password:` variables with credentials for the eAPI call to the EOS device
    - `terminattr_vrf` - VRF to use to communicate with CVaaS (default if no VRF is used)
    - On the `hosts:` line after the `"- name: Onboard devices to CVaaS and on-prem"` line, change it to the inventory group to use (if not all devices)
5. Run the following playbook:
    `ansible-playbook -i <inventory_file> playbooks/multi_cluster_cvaas_onprem.yml`



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