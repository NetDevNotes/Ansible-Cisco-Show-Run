# Ansible Pefroming a Show Run on a Cisco Switch :musical_keyboard:

* This play performs a `show run` on a cisco switch and outputs the config to a text file. 
* The inventory file uses groups-of-groups but we are only interested in the switch in this play.
* The inventrory file uses group vars to define key value pairs for passing through the username and password.
* The playbook uses the `ios_command` Ansible module.
* The password var is in clear text!


