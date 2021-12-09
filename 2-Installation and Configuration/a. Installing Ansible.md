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

If you are using the version with limited support provided with your Red Hat Enterprise Linux subscription, use the following procedure:

Enable the Red Hat Ansible Engine repository.
    
    [root@workstation ~]# subscription-manager refresh
    [root@workstation ~]# subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms

