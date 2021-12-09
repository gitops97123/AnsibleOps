# Building an Ansible Inventory

### Defining the Inventory
An inventory defines a collection of hosts that Ansible will manage. These hosts can also be
assigned to groups, which can be managed collectively. Groups can contain child groups, and hosts
can be members of multiple groups. The inventory can also set variables that apply to the hosts
and groups that it defines.


### Specifying Managed Hosts with a Static Inventory
A static inventory file is a text file that specifies the managed hosts that Ansible targets. You
can write this file using a number of different formats, including INI-style or YAML.

In its simplest form, an INI-style static inventory file is a list of host names or IP addresses of managed hosts, each on a single line:<br>

servera.example.com<br>
serverb.example.com<br>
192.168.245.132<br>
192.168.245.133<br>

Normally, however, you organize managed hosts into host groups. Host groups allow you to more effectively run Ansible against a collection of systems. In this case, each section starts with a host group name enclosed in square brackets ([]). This is followed by the host name or an IP address for each managed host in the group, each on a single line.

In the following example, the host inventory defines two host groups: **webservers** and **dbservers**.<br>

    [webservers]
    servera.example.com
    serverb.example.com
    192.168.245.132

    [db-servers]
    serverc.example.com
    serverd.example.com

Hosts can be in multiple groups. In fact, recommended practice is to organize your hosts into multiple groups, possibly organized in different ways depending on the role of the host, its physical location, whether it is in production or not, and so on. This allows you to easily apply Ansible plays to specific hosts.<br>

    [webservers]
    servera.example.com
    serverb.example.com
    192.168.245.132

    [db-servers]
    serverc.example.com
    serverd.example.com

    [east-datacenter]
    servera.example.com
    serverc.example.com

    [west-datacenter]
    serverb.example.com
    serverd.example.com

    [production]
    servera.example.com
    serverb.example.com
    serverc.example.com 
    serverd.example.com

    [development]
    192.168.245.132
    192.168.245.133

### Defining Nested Groups
Ansible host inventories can include groups of host groups. This is accomplished by creating a host group name with the :children suffix. The following example creates a new group called north-america, which includes all hosts from the usa and canada groups.

    [usa]
    serverd.example.com
    servere.example.com

    [canada]
    serverf.example.com
    serverg.example.com

    [north-america:children]
    canada
    usa


### Simplifying Host Specifications with Ranges
You can specify ranges in the host names or IP addresses to simplify Ansible host inventories. You can specify either numeric or alphabetic ranges. Ranges have the following syntax:

    [START:END]

Ranges match all values from START to END, inclusively. Consider the following examples:

- 192.168.[4:7].[0:255] matches all IPv4 addresses in the 192.168.4.0/22 network (192.168.4.0 through 192.168.7.255).
- server[01:20].example.com matches all hosts named server01.example.com through server20.example.com.
- [a:c].dns.example.com matches hosts named a.dns.example.com, b.dns.example.com, and c.dns.example.com.
- 2001:db8::[a:f] matches all IPv6 addresses from 2001:db8::a through 2001:db8::f.

    [usa]
    server[a:d].example.com

    [canada]
    server[1:4].example.com

### Verifying the Inventory
When in doubt, use the ansible command to verify a machine's presence in the inventory:


