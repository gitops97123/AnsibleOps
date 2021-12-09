# How to Install Ansible on Ubuntu 20.04

### System requirements for Ansible

Minimal Installed Ubuntu 20.04 LTS 
sudo user with root privileges
1 CPU / vCPU
2 GB RAM or more
10 GB Hard drive

## My lab Environment for Practice ansible

* Ansible Control Node – workstation.example.com 
* Ansible Managed Nodes – servera.example.com & serverb.example.com

## Ansible Control Node – workstation.example.com 
 
Create a new student user. 

    root@workstation:~# adduser student

    New password: student
    Retype new password: student
    passwd: password updated successfully

Adding sudoers entry for student user. 

    root@workstation:~# echo "student ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/student

Install dependencies and configure Ansible Repository

    root@workstation:~# su - student
    student@workstation:~$ sudo apt install -y software-properties-common

Install the latest version of ansible 

    student@workstation:~$ apt install -y ansible 
    student@workstation:~$ ansible --version

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

    student@workstation:~$ ansible --version
    ansible 2.9.6
    config file = /etc/ansible/ansible.cfg
    configured module search path = ['/home/student/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python3/dist-packages/ansible
    executable location = /usr/bin/ansible
    python version = 3.8.10 (default, Sep 28 2021, 16:10:42) [GCC 9.3.0]

    student@workstation:~$ vim /etc/hosts 
    192.168.245.131 workstation.example.com workstation 
    192.168.245.132 servera.example.com servera 
    192.168.245.133 serverb.example.com serverb 

    student@workstation:~$ for a in servera serverb ; do scp -r /etc/hosts root@$hosts:/etc/hosts ;done

    student@workstation:~$ ssh-copy-id root@servera
    student@workstation:~$ ssh-copy-id root@serverb 
    student@workstation:~$ ssh-copy-id student@servera
    student@workstation:~$ ssh-copy-id student@serverb

    student@workstation:~$ scp -r /etc/sudoers.d/student root@servera:/etc/sudoers.d/student
    student@workstation:~$ scp -r /etc/sudoers.d/student root@serverb:/etc/sudoers.d/student