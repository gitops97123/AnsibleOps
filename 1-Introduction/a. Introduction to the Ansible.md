# Automation and Linux System Administration 

For many years, most system administration and infrastructure management has relied on manual
tasks performed through graphical or command-line user interfaces. System administrators often
work from checklists, other documentation, or a memorized routine to perform standard tasks.
This approach is error-prone. It is easy for a system administrator to skip a step or perform a step
mistakenly. Often there is limited verification that the steps were performed properly or that they
result in the expected outcome.</br>

Furthermore, by managing each server manually and independently, it is very easy for many
servers that are supposed to be identical in configuration to be different in minor (or major)
ways. This can make maintenance more difficult and introduce errors or instability into the IT
environment.</br>

Automation can help avoid the problems caused by manual system administration and
infrastructure management. As a system administrator, you can use it to ensure that all your
systems are quickly and correctly deployed and configured. This allows you to automate the
repetitive tasks in your daily schedule, freeing up your time and allowing you to focus on more
critical things. For your organization, this means you can more quickly roll out the next version of
an application or updates to a service.

## Infrastructure as Code
A good automation system allows you to implement Infrastructure as Code practices.
Infrastructure as Code means that you can use a machine-readable automation language to
define and describe the state you want your IT infrastructure to be in. Ideally, this automation
language should also be very easy for humans to read, because then you can easily understand
what the state is and make changes to it. This code is then applied to your infrastructure to ensure
that it is actually in that state.

If the automation language is represented as simple text files, it can easily be managed in a version
control system like software code. 

The advantage of this is that every change can be checked into
the version control system, so you have a history of the changes you make over time. If you want to
revert to an earlier known-good configuration, you simply can check out that version of the code
and apply it to your infrastructure.



## Mitigating Human Error

Automation allows you to use code review, peer review by multiple subject matter experts, and
documentation of the procedure by the automation itself to reduce your operational risks.

Ultimately, you can enforce that changes to your IT infrastructure must be made through
automation in order to mitigate human error

# What is Ansible?
Ansible is an open source automation platform. It is a simple automation language that can
perfectly describe an IT application infrastructure in Ansible Playbooks. It is also an automation
engine that runs Ansible Playbooks.

Ansible can manage powerful automation tasks and can adapt to many different workflows
and environments. At the same time, new users of Ansible can very quickly use it to become
productive.

## Ansible Is Simple
Ansible Playbooks provide human-readable automation. This means that playbooks are
automation tools that are also easy for humans to read, comprehend, and change. No special
coding skills are required to write them.

Playbooks execute tasks in order. The simplicity of playbook design makes them usable by every team, which allows people new to Ansible to get
productive quickly

## Ansible Is Powerful
You can use Ansible to deploy applications, for configuration management, for workflow
automation, and for network automation. Ansible can be used to orchestrate the entire application
life cycle

## Ansible Is Agentless
Ansible is built around an agentless architecture. Typically, Ansible connects to the hosts it
manages using OpenSSH or WinRM and runs tasks, often (but not always) by pushing out small
programs called Ansible modules to those hosts. These programs are used to put the system in
a specific desired state. Any modules that are pushed are removed when Ansible is finished with
its tasks. You can start using Ansible almost immediately because no special agents need to be
approved for use and then deployed to the managed hosts. Because there are no agents and no
additional custom security infrastructure, Ansible is more efficient and more secure than other
alternatives

## Ansible Concepts and Architecture

Ansible has a number of important strengths:

* Cross platform support: Ansible provides agentless support for Linux, Windows, UNIX, and
network devices, in physical, virtual, cloud, and container environments.

* Human-readable automation: Ansible Playbooks, written as YAML text files, are easy to read and
help ensure that everyone understands what they will do.
Perfect description of applications: Every change can be made by Ansible Playbooks, and every
aspect of your application environment can be described and documented.

* Easy to manage in version control: Ansible Playbooks and projects are plain text. They can be
treated like source code and placed in your existing version control system.

* Support for dynamic inventories: The list of machines that Ansible manages can be dynamically
updated from external sources in order to capture the correct, current list of all managed servers
all the time, regardless of infrastructure or location.

* Orchestration that integrates easily with other systems: HP SA, Puppet, Jenkins, Red Hat
Satellite, and other systems that exist in your environment can be leveraged and integrated into
your Ansible workflow.

# Ansible: The Language of DevOps

![dev1 logo](https://github.com/gitops97123/AnsibleOps/blob/main/icons/dev.PNG?raw=true "dev1 logo")

Communication is the key to DevOps. Ansible is the first automation language that can be read
and written across IT. It is also the only automation engine that can automate the application life
cycle and continuous delivery pipeline from start to finish.


# Ansible Concepts and Architecture
There are two types of machines in the Ansible architecture: control nodes and managed hosts.
Ansible is installed and run from a control node, and this machine also has copies of your Ansible
project files. A control node could be an administrator's laptop, a system shared by a number of
administrators, or a server running Red Hat Ansible Tower.

Managed hosts are listed in an inventory, which also organizes those systems into groups for
easier collective management. The inventory can be defined in a static text file, or dynamically
determined by scripts that get information from external sources.

Instead of writing complex scripts, Ansible users create high-level plays to ensure a host or group
of hosts are in a particular state. A play performs a series of tasks on the hosts, in the order
specified by the play. These plays are expressed in YAML format in a text file. A file that contains
one or more plays is called a playbook.

Each task runs a module, a small piece of code (written in Python, PowerShell, or some other
language), with specific arguments. Each module is essentially a tool in your toolkit. Ansible ships
with hundreds of useful modules that can perform a wide variety of automation tasks. They can act
on system files, install software, or make API calls.

![dev2 logo](https://github.com/gitops97123/AnsibleOps/blob/main/icons/dev2.PNG?raw=true "dev2 logo")