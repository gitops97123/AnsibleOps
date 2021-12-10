# Ansible Playbooks and Ad Hoc Commands
Ad hoc commands can run a single, simple task against a set of targeted hosts as a one-time command. The real power of Ansible, however, is in learning how to use playbooks to run multiple, complex tasks against a set of targeted hosts in an easily repeatable manner.

A play is an ordered set of tasks run against hosts selected from your inventory. A playbook is a text file containing a list of one or more plays to run in a specific order.

Plays allow you to change a lengthy, complex set of manual administrative tasks into an easily repeatable routine with predictable and successful outcomes. In a playbook, you can save the sequence of tasks in a play into a human-readable and immediately runnable form. The tasks themselves, because of the way in which they are written, document the steps needed to deploy your application or infrastructure.

# Formatting an Ansible Playbook

To help you understand the format of a playbook, review this ad hoc command from a previous
chapter:

    [student@workstation ~]$ ansible -m user -a "name=newbie uid=4000 state=present"  servera.lab.example.com

This can be rewritten as a single task play and saved in a playbook. The resulting playbook appears
as follows:

Example 1. A Simple Playbook

   
```yml
---
- name: Configure important user consistently
  hosts: servera.lab.example.com
  tasks:
  - name: newbie exists with UID 4000
    user:
      name: newbie
      uid: 4000
      state: present
```

A playbook is a text file written in YAML format, and is normally saved with the extension yml. The playbook uses indentation with space characters to indicate the structure of its data. YAML does not place strict requirements on how many spaces are used for the indentation, but there are two basic rules.

- Data elements at the same level in the hierarchy (such as items in the same list) must have the same indentation.
- Items that are children of another item must be indented more than their parents.

You can also add blank lines for readability.

#### Important 
For example, you can add the following line to your
$HOME/.vimrc file, and when vi detects that you are editing a YAML file, it
performs a 2-space indentation when you press the Tab key and autoindents
subsequent lines.

    autocmd FileType yaml setlocal ai ts=2 sw=2 et

A playbook begins with a line consisting of three dashes (---) as a start of document marker. It
may end with three dots (...) as an end of document marker, although in practice this is often
omitted.
In between those markers, the playbook is defined as a list of plays. An item in a YAML list starts
with a single dash followed by a space. For example, a YAML list might appear as follows:

  - apple
  - orange
  - grape
  
In Example 3.1, “A Simple Playbook”, the line after --- begins with a dash and starts the first (and only) play in the list of plays.

The play itself is a collection of key-value pairs. Keys in the same play should have the same indentation. The following example shows a YAML snippet with three keys. The first two keys have simple values. The third has a list of three items as a value.

    name: just an example
    hosts: webservers
    tasks:
      - first
      - second
      - third
  
The original example play has three keys, **name, hosts,** and **tasks**, because these keys all have the same indentation.

The first line of the example play starts with a dash and a space (indicating the play is the first item of a list), and then the first key, the **name** attribute. The **name** key associates an arbitrary string with the play as a label. This identifies what the play is for. The **name** key is optional, but is recommended because it helps to document your playbook. This is especially useful when a playbook contains multiple plays.

    - name: Configure important user consistently
  
The second key in the play is a hosts attribute, which specifies the hosts against which the play's tasks are run. Like the argument for the ansible command, the hosts attribute takes a host pattern as a value, such as the names of managed hosts or groups in the inventory.

    hosts: servera.lab.example.com

Finally, the last key in the play is the **tasks** attribute, whose value specifies a list of tasks to run for this play. This example has a single task, which runs the **user** module with specific arguments (to ensure user **newbie** exists and has UID 4000).

    tasks:
    - name: newbie exists with UID 4000
      user:
        name: newbie
        uid: 4000
        state: present

The tasks attribute is the part of the play that actually lists, in order, the tasks to be run on the managed hosts. Each task in the list is itself a collection of key-value pairs. 

In this example, the only task in the play has two keys:

- **name** is an optional label documenting the purpose of the task. It is a good idea to name all your tasks to help document the purpose of each step of the automation process.
- **user** is the module to run for this task. Its arguments are passed as a collection of key-value pairs, which are children of the module (name, uid, and state).
  
The following is another example of a **tasks** attribute with multiple tasks, using the **service** module to ensure that several network services are enabled to start at boot:

    tasks:
    - name: web server is enabled
      service:
        name: httpd
        enabled: true
    - name: NTP server is enabled
      service:
        name: chronyd
        enabled: true
    - name: Postfix is enabled
      service:
        name: postfix
        enabled: true

# Running Playbooks

The **ansible-playbook** command is used to run playbooks. The command is executed on the control node and the name of the playbook to be run is passed as an argument:

    [student@workstation ~]$ ansible-playbook site.yml

When you run the playbook, output is generated to show the play and tasks being executed. The
output also reports the results of each task executed.
The following example shows the contents of a simple playbook, and then the result of running it.

    [student@workstation playdemo]$ cat webserver.yml
    ---
    - name: play to setup web server
      hosts: servera.lab.example.com
      tasks:
      - name: latest httpd version installed
        yum:
          name: httpd
          state: latest
    ...output omitted...

    [student@workstation playdemo]$ ansible-playbook webserver.yml

    PLAY [play to setup web server] ************************************************

    TASK [Gathering Facts] *********************************************************
    ok: [servera.lab.example.com]

    TASK [latest httpd version installed] ******************************************
    changed: [servera.lab.example.com]

    PLAY RECAP *********************************************************************
    servera.lab.example.com :   ok=2     changed=1      unreachable=0      failed=0

Note that the value of the **name** key for each play and task is displayed when the playbook is run.
(The **Gathering Facts** task is a special task that the **setup** module usually runs automatically
at the start of a play. This is covered later in the course.) For playbooks with multiple plays and
tasks, setting **name** attributes makes it easier to monitor the progress of a playbook's execution.

You should also see that the **latest httpd version installed** task is **changed** for **servera.lab.example.com**. This means that the task changed something on that host to ensure its specification was met. In this case, it means that the httpd package probably was not installed or was not the latest version.

In general, tasks in Ansible Playbooks are idempotent, and it is safe to run a playbook multiple
times. If the targeted managed hosts are already in the correct state, no changes should be made.
For example, assume that the playbook from the previous example is run again:

    [student@workstation playdemo]$ ansible-playbook webserver.yml
    PLAY [play to setup web server] ************************************************
    TASK [Gathering Facts] *********************************************************

    ok: [servera.lab.example.com]
    TASK [latest httpd version installed] ******************************************

    ok: [servera.lab.example.com]
    PLAY RECAP *********************************************************************

    servera.lab.example.com :    ok=2     changed=0     unreachable=0      failed=0

This time, all tasks passed with status ok and no changes were reported.

# Increasing Output Verbosity

The default output provided by the **ansible-playbook** command does not provide detailed
task execution information. The **ansible-playbook -v** command provides additional
information, with up to four total levels.

### Configuring the Output Verbosity of Playbook Execution

|Option |Description|
|-------|-----------|
|-v |The task results are displayed.|
-vv |Both task results and task configuration are displayed|
|-vvv |Includes information about connections to managed hosts|
|-vvvv|Adds extra verbosity options to the connection plug-ins, including users being used in the managed hosts to execute scripts, and what scripts have been executed|

# Syntax Verification

Prior to executing a playbook, it is good practice to perform a verification to ensure that the
syntax of its contents is correct. The **ansible-playbook command** offers a **--syntax-check**
option that you can use to verify the syntax of a playbook. The following example shows the
successful syntax verification of a playbook.

    [student@workstation ~]$ ansible-playbook --syntax-check webserver.yml
    playbook: webserver.yml

    [student@workstation ~]$ ansible-playbook --syntax-check webserver.yml
    ERROR! Syntax Error while loading YAML.
      mapping values are not allowed in this context

    The error appears to have been in ...output omitted... line 3, column 8, but may
    be elsewhere in the file depending on the exact syntax problem.

    The offending line appears to be:
    
    - name:play to setup web server
      hosts: servera.lab.example.com
          ^ here


# Implementing Multiple Plays

### Writing Multiple Plays

A playbook is a YAML file containing a list of one or more plays. Remember that a single play is an
ordered list of tasks to execute against hosts selected from the inventory. Therefore, if a playbook
contains multiple plays, each play may apply its tasks to a separate set of hosts.

This can be very useful when orchestrating a complex deployment which may involve different
tasks on different hosts. You can write a playbook that runs one play against one set of hosts, and
when that finishes runs another play against another set of hosts.

Writing a playbook that contains multiple plays is very straightforward. Each play in the playbook
is written as a top-level list item in the playbook. Each play is a list item containing the usual play
keywords.

The following example shows a simple playbook with two plays. The first play runs against
**web.example.com**, and the second play runs against **database.example.com**.

    ---
    # This is a simple playbook with two plays
    - name: first play
      hosts: web.example.com
      tasks:
      - name: first task
        yum:
          name: httpd
          status: present
      - name: second task
        service:
          name: httpd
          enabled: true
    
    - name: second play
      hosts: database.example.com
      tasks:
      - name: first task
        service:
          name: mariadb
          enabled: true

### Remote Users and Privilege Escalation in Plays
Tasks in playbooks are normally executed through a network connection to the managed hosts. As
with ad hoc commands, the user account used for task execution depends on various keywords in
the Ansible configuration file, **/etc/ansible/ansible.cfg**. The user that runs the tasks can be
defined by the **remote_user** keyword. However, if privilege escalation is enabled, other keywords
such as **become_user** can also have an impact.

Plays can use different remote users or privilege escalation settings for a play than what is
specified by the defaults in the configuration file. These are set in the play itself at the same level
as the **hosts** or **tasks** keywords.

### User Attributes

If the remote user defined in the Ansible configuration for task execution is not suitable, it can be
overridden by using the **remote_user** keyword within a play.

    remote_user: remoteuser

### Privilege Escalation Attributes

Additional keywords are also available to define privilege escalation parameters from within a
playbook. The **become** boolean keyword can be used to enable or disable privilege escalation
regardless of how it is defined in the Ansible configuration file. It can take **yes** or **true** to enable
privilege escalation, or **no** or **false** to disable it.

    become: true

If privilege escalation is enabled, the **become_method** keyword can be used to define the
privilege escalation method to use during a specific play. The example below specifies that **sudo**
be used for privilege escalation.

    become_method: sudo

Additionally, with privilege escalation enabled, the **become_user** keyword can define the user
account to use for privilege escalation within the context of a specific play.

    become_user: privileged_user

The following example demonstrates the use of these keywords in a play:

    - name: /etc/hosts is up to date
      hosts: webservers
      remote_user: student
      become: yes
      tasks:
      - name: server.example.com in /etc/hosts
        lineinfile:
          path: /etc/hosts
          line: '192.168.245.131 servera.example.com server'
          state: present

### YAML Strings
Strings in YAML do not normally need to be put in quotation marks even if there are spaces
contained in the string. You can enclose strings in either double quotes or single quotes.

    this is a string

    'this is another string'

    "this is yet another a string"


There are two ways to write multiline strings. You can use the vertical bar (|) character to denote
that newline characters within the string are to be preserved.

    include_newlines: |
            Example Company
            123 Main Street
            Atlanta, GA 30303

    fold_newlines: >
            This is an example
            of a long string,
            that will become
            a single sentence once folded.

### YAML Dictionaries

You have seen collections of key-value pairs written as an indented block, as follows:

    name: svcrole
    svcservice: httpd
    svcport: 80

Dictionaries can also be written in an inline block format enclosed in curly braces, as follows:

     {name: svcrole, svcservice: httpd, svcport: 80}

### YAML Lists
You have also seen lists written with the normal single-dash syntax:

    hosts:
      - servera
      - serverb
      - serverc
  
Lists also have an inline format enclosed in square braces, as follows:

    hosts: [servera, serverb, serverc]

You should avoid this syntax because it is usually harder to read.

### Obsolete key=value Playbook Shorthand

Some playbooks might use an older shorthand method to define tasks by putting the key-value
pairs for the module on the same line as the module name. For example, you might see this syntax:

    tasks:
    - name: shorthand form
    service: name=httpd enabled=true state=started

Normally you would write the same task as follows:

    tasks:
    - name: normal form
      service:
        name: httpd
        enabled: true
        state: started
        
You should generally avoid the shorthand form and use the normal form.

