# Ansible or Red Hat Ansible Automation?
Red Hat provides Ansible software in special channels as a convenience to Red Hat Enterprise Linux subscribers, and you can use these software packages normally.

However, if you want formal support for Ansible and its modules, Red Hat offers a special subscription for this, Red Hat Ansible Automation. This subscription includes support for Ansible itself, as Red Hat Ansible Engine. This adds formal technical support with SLAs and a published scope of coverage for Ansible and its core modules. More information on the scope of this support is available at Red Hat Ansible Engine Life Cycle [https://access.redhat.com/support/policy/updates/ansible-engine].

# Control Nodes
Ansible is simple to install. The Ansible software only needs to be installed on the control node (or nodes) from which Ansible will be run. Hosts that are managed by Ansible do not need to have Ansible installed. This installation involves relatively few steps and has minimal requirements.

The control node should be a Linux or UNIX system. Microsoft Windows is not supported as a control node, although Windows systems can be managed hosts.

Python 3 (version 3.5 or later) or Python 2 (version 2.7 or later) needs to be installed on the
control node.

Register your system to Red Hat Subscription Manager.
   
    [root@workstation ~]# subscription-manager register --auto-attach 

Set a role for your system.

    [root@workstation ~]# subscription-manager role --set="Red Hat Enterprise Linux Server"

Enable the Red Hat Ansible Engine repository.
    
    [root@workstation ~]# subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms

Install Red Hat Ansible Engine.
    
    [root@workstation ~]# yum install ansible

Verify the Ansible version. 

    [root@workstation ~]# ansible --version 

Generating SSH keygen 
    
    student@workstation:~$ ssh-keygen
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/student/.ssh/id_rsa): enter
    Created directory '/home/student/.ssh'.
    Enter passphrase (empty for no passphrase): enter
    Enter same passphrase again: enter
    Your identification has been saved in /home/student/.ssh/id_rsa
    Your public key has been saved in /home/student/.ssh/id_rsa.pub
    The key fingerprint is:
    SHA256:pV4abFJvWcEPvbPdDY0bP093cznKdTO12OXi2/SDRo0 student@workstation.example.com
    The key's randomart image is:
    +---[RSA 3072]----+
    |           ...   |
    |            o..  |
    |        . . .o + |
    |       o + o  B +|
    |      . S =   =XB|
    |       + =   E+%%|
    |        o   o.+o&|
    |             =.+o|
    |            . ..+|
    +----[SHA256]-----+

Checking the Ansible Version

    student@workstation:~$ ansible --version
    ansible 2.9.6
    config file = /etc/ansible/ansible.cfg
    configured module search path = ['/home/student/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python3/dist-packages/ansible
    executable location = /usr/bin/ansible
    python version = 3.8.10 (default, Sep 28 2021, 16:10:42) [GCC 9.3.0]

Insert the hosts entry on Control node. 

    student@workstation:~$ vim /etc/hosts 
    192.168.245.131 workstation.example.com workstation 
    192.168.245.132 servera.example.com servera 
    192.168.245.133 serverb.example.com serverb 

copy the hosts file to the both servera and serverb machine. 

    student@workstation:~$ for a in servera serverb ; do scp -r /etc/hosts root@$hosts:/etc/hosts ;done

copy the public-key to the remote user student.

    student@workstation:~$ ssh-copy-id root@servera
    student@workstation:~$ ssh-copy-id root@serverb 
    student@workstation:~$ ssh-copy-id student@servera
    student@workstation:~$ ssh-copy-id student@serverb

Copy the sudoers file to the both servera and serverb machine.  

    student@workstation:~$ scp -r /etc/sudoers.d/student root@servera:/etc/sudoers.d/student
    student@workstation:~$ scp -r /etc/sudoers.d/student root@serverb:/etc/sudoers.d/student