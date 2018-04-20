## Output from Demo Run

```bash
[root@optima-ansible ansible-setup-passwordless-ssh]# cat hosts 
[local_host]
localhost

[local_host:vars]
ssh_key_filename="ansible_rsa"
remote_machine_username="root"
remote_machine_password="xxxxxxxxxxxxxxxxxxxxxx"


[ansible_setup_passwordless_setup_group]
rhel-green
rhel-red
[root@optima-ansible ansible-setup-passwordless-ssh]# ansible-playbook -i hosts ansible_setup_passwordless_ssh.yml
Type 'YES' to establish passwordless login to the remote hosts: [NO]: YES

PLAY [local_host] ***************************************************************************************************************************************************************************************************

TASK [Check Confirmation] *******************************************************************************************************************************************************************************************
skipping: [localhost]

TASK [check .ssh local directory exists] ****************************************************************************************************************************************************************************
ok: [localhost]

TASK [create ~/.ssh local directory] ********************************************************************************************************************************************************************************
skipping: [localhost]

TASK [check .ssh key file exists] ***********************************************************************************************************************************************************************************
ok: [localhost] => (item=ansible_rsa)
ok: [localhost] => (item=ansible_rsa.pub)

TASK [generate ssh key on local machine] ****************************************************************************************************************************************************************************
changed: [localhost]

TASK [check .ssh/config file exists] ********************************************************************************************************************************************************************************
ok: [localhost]

TASK [create the ~/.ssh/config file] ********************************************************************************************************************************************************************************
skipping: [localhost]

TASK [add the new ssh key to the ~/.ssh/config file] ****************************************************************************************************************************************************************
changed: [localhost]

TASK [distribute the ssh key to the remote hosts] *******************************************************************************************************************************************************************
changed: [localhost] => (item=rhel-green)
changed: [localhost] => (item=rhel-red)

PLAY [ansible_setup_passwordless_setup_group] ***********************************************************************************************************************************************************************

TASK [check ssh to remote host works] *******************************************************************************************************************************************************************************
changed: [rhel-green]
changed: [rhel-red]

TASK [debug] ********************************************************************************************************************************************************************************************************
ok: [rhel-green] => {
    "ssh_connection_test.stdout_lines": [
        "rhel-green", 
        "uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023"
    ]
}
ok: [rhel-red] => {
    "ssh_connection_test.stdout_lines": [
        "rhel-red", 
        "uid=0(root) gid=0(root) groups=0(root) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023"
    ]
}

PLAY RECAP **********************************************************************************************************************************************************************************************************
localhost                  : ok=6    changed=3    unreachable=0    failed=0   
rhel-green                 : ok=2    changed=1    unreachable=0    failed=0   
rhel-red                   : ok=2    changed=1    unreachable=0    failed=0   

[root@optima-ansible ansible-setup-passwordless-ssh]# 
```
