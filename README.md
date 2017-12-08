ansible-winupdates
===============
Install Windows Updates

Summary
--------
This playbook will install windows updates on a AWS EC2 Windows 2016 server

Prerequisities
----------------
- Creation of an ansible-vault encrypted string to seed the `admin_pass` variable.  See [ansible-vault encrypt_string](https://docs.ansible.com/ansible/2.4/vault.html#use-encrypt-string-to-create-encrypted-variables-to-embed-in-yaml)

Example Playbook execution
-------------------------
`ansible-playbook site.yml --ask-vault-pass`

role
--------
ansible-role-winupdates

default role vars
------------

| variable name | description | default |
|---------------|-------|---------|
|ansible_connection| method of connection |winrm|
|ansible_port| port to connect  |5986|
|ansible_user| User Account |Administrator|
|admin_pass| ansible-vault encrypted string for initial Windows Administrator password |N/A|
|ansible_password| ansible password to use to login to new Windows instance |"{{ admin_pass }}"|
|ansible_winrm_server_cert_validation| Validate or ignore certificate| ignore|
|win_updates_reboot| set to true or false | true|
|win_updates_category_names| Set update categories | CriticalUpdates,SecurityUpdates,UpdateRollups,Updates|

Example playbook
-----------
```
---
- name: Install Windows updates
  win_updates:
    category_names: "{{ win_updates_category_names }}"
  register: winup_result
  failed_when: winup_result.failed is defined and winup_result.failed and win_updates_reboot == False

- name: "Reboot when updates installed."
  win_reboot:
    shutdown_timeout_sec: 300
    reboot_timeout_sec: 300
  when: winup_result.reboot_required == True and win_updates_reboot
```

License
-------
BSD
