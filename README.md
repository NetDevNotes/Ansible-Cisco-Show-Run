# Ansible Pefroming a Show Run on a Cisco Switch :musical_keyboard:

* This play performs a `show run` on a cisco switch and outputs the config to a text file. 
* The inventory file uses groups-of-groups but we are only interested in the switch in this play.
* The inventrory file uses group vars to define key value pairs for passing through the username and password.
* The playbook uses the `ios_command` Ansible module.
* The password var is in clear text!

```
➜  ansible_cisco_show_run git:(master) sudo ansible-playbook showrun.yml
Password:

PLAY [Backup show run (enable mode commands)] ***************************************************************************************

TASK [run enable level commands] ****************************************************************************************************
ok: [10.1.1.250]

TASK [debug] ************************************************************************************************************************
ok: [10.1.1.250] => {
    "print_output.stdout_lines": [
        [
            "Building configuration...",
            "",
            "Current configuration : 3307 bytes",
            "!",
            "version 12.2",
            "no service pad",
            "service timestamps debug datetime msec",
            "service timestamps log datetime msec",
            "service password-encryption",
            "!",
            "hostname SW1-2960",
```
```
TASK [save output to a file] ********************************************************************************************************
changed: [10.1.1.250]

PLAY RECAP **************************************************************************************************************************
10.1.1.250                 : ok=3    changed=1    unreachable=0    failed=0

➜  ansible_cisco_show_run git:(master) ✗
```
# How did I create this play?

1. Created a new repository called *ansible_cisco_show_run* at [GitHub](www.github.com)
2. When creating repository tick the box that automatically creates a README.md
3. In GitHub, click **Clone or Download** and copy the repositories URL
> https://github.com/NetDevNotes/ansible_cisco_show_run.git
4. On my workstation, performed a `git clone https://github.com/NetDevNotes/ansible_cisco_show_run.git`
5. The `git clone` downloaded the folder and README.md to my local workstation, cd into the folder:
```
➜  ansible git:(master) ✗ cd ansible_cisco_show_run
➜  ansible_cisco_show_run git:(master) ✗ pwd
/Users/nico/scripts/ansible/ansible_cisco_show_run
```
6. I created an inventory file (calles `hosts`)which is much more complex than it needs to be, but I wanted to learn how to use groups and groups-of-groups.  The key here is that I defined host vars for the username and password, and some other vars we need to connect to the switch:
```
[all:vars]
# These defaults can be overridden for any group in the [group:vars] section

[ios:children]
# This is a group-of-groups called ios
switch
asa

[switch]
# This is the switch group and a child of the ios group
10.1.1.250

[switch:vars]
# These are group vars for the switch group only
ansible_connection=network_cli
ansible_become=yes
ansible_become_method=enable
ansible_network_os=ios
ansible_user=admin
ansible_ssh_pass=12121212
```
7. I created a simple playbook `showrun.yml` using the ios_command ansible module:
```
---
- name: Backup show run (enable mode commands)
  hosts: switch
  gather_facts: false
  connection: local

  tasks:
    - name: run enable level commands
      ios_command:
        authorize: yes
        commands:
          - show run

      register: print_output

    -  debug: var=print_output.stdout_lines

    - name: save output to a file
      copy: content="{{ print_output.stdout[0] }}" dest="./output/{{ inventory_hostname }}.txt"

# Description:
# Use the ios commands module to get show run information
# Go to enable mode
# Save the output to files
# http://docs.ansible.com/ansible/latest/ios_command_module.html

# Commands to run:
#1) sudo ansible-playbook showrun.yml
➜  ansible_cisco_show_run git:(master) ✗
```




