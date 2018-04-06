## Purpose

This Ansible Playbook will assist on establishing passwordless SSH logins with the remote hosts you wish to manage. Passwordless logins is a great convenience when connecting to multiple servers, via Ansible or not!

---

## Download the tool

Clone the repository to your ansible-enabled host:

`git clone https://github.com/ilias-sp/ansible-setup-passwordless-ssh.git`

Alternatively, you can download the `ansible_setup_passwordless_ssh.yml` and `hosts` from this repository.

---

## Prerequisites

Make sure your Ansible host is equipped with the utilities, and that they are available to the PATH of the user you will be running the playbook as.

- ssh-keygen
- ssh-copy-id

If you dont have them, before continuing you will have to install them using the recommended ways for your Linux distribution.

---

## Preparations before you run

Edit the `hosts` file and define your environment's information. Fill in using the below matrix:

| Name | Description |
| ----------------------- | ---------------------------------------------- |
| ssh_key_filename | the filename of the new SSH key to be generated and stored under your .ssh folder of your localhost. |
| remote_machine_username | the username of the remote machines. If you are applying the procedure to multiple hosts. |
| remote_machine_username | the password of the "remote_machine_username" remote machines. |
| [ansible_setup_passwordless_setup_group] | fill in the list of hosts that you want to establish the passwordless login with. |

If you are planning to run the script towards multiple hosts, make sure the username/password you defined is the same to all of them!

## Example

```
[local_host]
localhost

[local_host:vars]
ssh_key_filename="ansible_rsa"
remote_machine_username="root"
remote_machine_password="root_password"


[ansible_setup_passwordless_setup_group]
rhel-green
rhel-red
```

---

## How to run it

`ansible-playbook -i hosts ansible_setup_passwordless_ssh.yml`

Last task in the playbook is to connect to each of those hosts and run some commands ("hostname" and "id"), check the output to verify the success of the tool!

[Output from Demo run](demo/SampleRun.md)

---

## What happens in the background to your machines when you run the playbook

By running this playbook, these things happen to your hosts:

Localhost:
- An SSH key is generated and placed under .ssh folder. Its file name is configurable, default is ansible_rsa.
- This SSH key is added to the ~/.ssh/config file for SSH client to utilize it when connecting to remote hosts.

Remote hosts:
- The generated SSH key is propagated to the list of remote hosts you configured in hosts inventory file, and added to their ~/.ssh/authorized_keys file. This is done using the ssh-copy-id linux utility that is meant for this job.

---
