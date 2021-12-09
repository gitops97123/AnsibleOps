# Managing Ansible Configuration Files

### Configuring Ansible
The behavior of an Ansible installation can be customized by modifying settings in the Ansible configuration file. Ansible chooses its configuration file from one of several possible locations on the control node.
### Using /etc/ansible/ansible.cfg
The ansible package provides a base configuration file located at /etc/ansible/ansible.cfg.
This file is used if no other configuration file is found.
### Using ~/.ansible.cfg
Ansible looks for a .ansible.cfg file in the user's home directory. This configuration is used instead of the /etc/ansible/ansible.cfg if it exists and if there is no ansible.cfg file in the current working directory.
### Using ./ansible.cfg
If an ansible.cfg file exists in the directory in which the ansible command is executed, it is used instead of the global file or the user's personal file. This allows administrators to create a directory structure where different environments or projects are stored in separate directories, with each directory containing a configuration file tailored with a unique set of settings.

### Using the ANSIBLE_CONFIG environment variable
You can use different configuration files by placing them in different directories and then executing Ansible commands from the appropriate directory, but this method can be restrictive and hard to manage as the number of configuration files grows. A more flexible option is to define the location of the configuration file with the **ANSIBLE_CONFIG** environment variable. When this variable is defined, Ansible uses the configuration file that the variable specifies instead of any of the previously mentioned configuration files.

    [root@workstation ~]# ansible --version
    ansible 2.9.27
    config file = /etc/ansible/ansible.cfg
    configured module search path = ['/root/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python3.6/site-packages/ansible
    executable location = /usr/bin/ansible
    python version = 3.6.8 (default, Sep  9 2021, 07:49:02) [GCC 8.5.0 20210514 (Red Hat 8.5.0-3)]

Another way to display the active Ansible configuration file is to use the -v option when executing Ansible commands on the command line.

    [root@workstation ~]# ansible webservers --list-hosts -v
    Using /etc/ansible/ansible.cfg as config file
    hosts (3):
        servera.example.com
        serverb.example.com
        192.168.245.132

### Managing Settings in the Configuration File
 
The Ansible configuration file consists of several sections, with each section containing settings
defined as key-value pairs. Section titles are enclosed in square brackets. For basic operation use

the following two sections:

- **[defaults]** sets defaults for Ansible operation
- **[privilege_escalation]** configures how Ansible performs privilege escalation on managed hosts

For example, the following is a typical ansible.cfg **ansible.cfg** file:

    [defaults]
    inventory = /etc/ansible/hosts 
    remote_user = student
    ask_pass = false

    [privilege_escalation]
    become = true
    become_method = sudo
    become_user = root
    become_ask_pass = false

## Ansible Configuration: 

|Directive |Description|
|----------|-----------|
|**inventory** | Specifies the path to the inventory file.|
|**remote_user** |The name of the user to log in as on the managed hosts. If not specified, the current user's name is used.|
|**ask_pass**| Whether or not to prompt for an SSH password. Can be false if using SSH public key authentication.|
|**become**| Whether to automatically switch user on the managed host (typically to root) after connecting. This can also be specified by a play.|
|**become_method**| How to switch user (typically sudo, which is the default, but su is an option).|
|**become_user**| The user to switch to on the managed host (typically root, which is the default).|
|**become_ask_pass**| Whether to prompt for a password for your **become_method**. Defaults to **false**.|