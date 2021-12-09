# Running Arbitrary Commands on Managed Hosts

Create a new directory 

    [student@workstation ~]$ mkdir ansible

Go to the directory and create ansible.cfg & inventory.

    [student@workstation ansible]$ cat ansible.cfg
    [defaults]
    inventory = inventory
    remote_user = student

    [privilege_escalation]
    become=true
    become_method=sudo
    become_user=root
    become_ask_pass=false


    [student@workstation ansible]$ cat inventory
    localhost

    [dev]
    servera

    [test]
    serverb
    serverc

    [prod]
    serverd
    servere

    [datacenter]
    serverf
    serverg

List hosts :

    [student@workstation ansible]$ ansible dev --list-hosts
    hosts (1):
        servera

Using ping module : 
   
    [student@workstation ansible]$ ansible dev -m ping
    servera | UNREACHABLE! => {
        "changed": false,
        "msg": "Failed to connect to the host via ssh: student@servera: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).",
        "unreachable": true
    }

As you can see the problem is occurring, because the ssh key is not paired to the remote hosts. 

    [student@workstation ansible]$ ansible dev -m ping -e "ansible_user=root ansible_password=redhat"
    servera | SUCCESS => {
        "ansible_facts": {
            "discovered_interpreter_python": "/usr/libexec/platform-python"
        },
        "changed": false,
        "ping": "pong"
    }

pass the variable using '-e' using the credentials by root and now ping is success.

    [student@workstation ansible]$ ansible dev -m command -a 'uptime' -e "ansible_user=root ansible_password=redhat"
    servera | CHANGED | rc=0 >>
    23:46:22 up  1:14,  1 user,  load average: 0.02, 0.01, 0.00

    [student@workstation ansible]$ ansible dev -m command -a 'hostname' -e "ansible_user=root ansible_password=redhat"
    servera | CHANGED | rc=0 >>
    servera.example.com

## Ansible Command-line Options

|Configuration file directives |Command-line option|
|------------------------------|-------------------|
|**inventory**| -i|
|**remote_user**| -u|
|**become**| --become, -b
|**become_method**| --become-method|
|**become_user**|--become-user|
|**become_ask_pass**| --ask-become-pass, -K|


    [student@workstation ~]$ ansible --help
    ...output omitted...
        -b, --become run operations with become (nopasswd implied)
        --become-method=BECOME_METHOD
                    privilege escalation method to use (default=sudo),
                    valid choices: [ sudo | su | pbrun | pfexec | runas |
                    doas ]
        --become-user=BECOME_USER
        ...output omitted...
        -u REMOTE_USER, --user=REMOTE_USER
        connect as this user (default=None)

Copy modules 

    [student@workstation deploy-adhoc]$ ansible dev -m copy \
    > -a 'content="Managed by Ansible\n" dest=/etc/motd' -e "ansible_user=root ansible_password=redhat"

    [student@workstation deploy-adhoc]$ ansible dev -m copy \
    > -a 'src=/etc/hosts" dest=/etc/hosts' -e "ansible_user=root ansible_password=redhat"
    
User Modules: 

    [student@workstation ~]$ ansible -m user -a "name=newbie uid=4000 state=present" \
    > servera.lab.example.com
    [student@workstation ~]$ ansible -m user -a "name=newbie state=absent" \
    > servera.lab.example.com