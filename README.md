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
