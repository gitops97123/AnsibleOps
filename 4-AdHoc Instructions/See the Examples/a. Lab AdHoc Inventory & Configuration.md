# Managing Ansible Configuration Files

Create the /home/student/deploy-manage directory, which will contain the files for
this exercise. Change to this newly created directory.

    [student@workstation ~]$ mkdir ~/deploy-manage
    [student@workstation ~]$ cd ~/deploy-manage
    [student@workstation deploy-manage]$

In your **/home/student/deploy-manage** directory, use a text editor to start editing a new file, ansible.cfg. 

Create a **[defaults]** section in that file. In that section, add a line which uses the
inventory directive to specify the **./inventory** file as the default inventory.

    [defaults]
    inventory = ./inventory

Save your work and exit the text editor.

In the **/home/student/deploy-manage** directory, use a text editor to start editing the new static inventory file, **inventory**.

The static inventory should contain four host groups:

- **[myself]** should contain the host localhost.
- **[intranetweb]** should contain the host servera.lab.example.com.
- **[internetweb]** should contain the host serverb.lab.example.com.
- **[web]** should contain the intranetweb and internetweb host groups.

In **/home/student/deploy-manage/inventory**, create the myself host group by adding the following lines: 

    [myself]
    localhost


In **/home/student/deploy-manage/inventory**, create the intranetweb host
group by adding the following lines:

    [intranetweb]
    servera.lab.example.com

In **/home/student/deploy-manage/inventory**, create the internetweb host
group by adding the following lines:

    [internetweb]
    serverb.lab.example.com

In **/home/student/deploy-manage/inventory**, create the web host group by
adding the following lines:

    [web:children]
    intranetweb
    internetweb

Confirm that your final inventory file looks like the following:

    [myself]
    localhost

    [intranetweb]
    servera.lab.example.com

    [internetweb]
    serverb.lab.example.com

    [web:children]
    intranetweb
    internetweb

Save your work and exit the text editor.

Use the ansible command with the --list-hosts option to test the configuration of your inventory file's host groups. This does not actually connect to those hosts.

    [student@workstation deploy-manage]$ ansible myself --list-hosts
    hosts (1):
    localhost

    [student@workstation deploy-manage]$ ansible intranetweb --list-hosts
    hosts (1):
    servera.lab.example.com

    [student@workstation deploy-manage]$ ansible internetweb --list-hosts
    hosts (1):
    serverb.lab.example.com

    [student@workstation deploy-manage]$ ansible web --list-hosts
    hosts (2):
    servera.lab.example.com
    serverb.lab.example.com

    [student@workstation deploy-manage]$ ansible all --list-hosts
    hosts (3):
    localhost
    servera.lab.example.com
    serverb.lab.example.com

Open the **/home/student/deploy-manage/ansible.cfg** file in a text editor. Add a
**[privilege_escalation]** section to configure Ansible to automatically use the sudo command to switch from student to root when running tasks on the managed hosts.
Ansible should also be configured to prompt you for the password that student uses for the sudo command.

Create the [privilege_escalation] section in the /home/student/deploymanage/ansible.cfg configuration file by adding the following entry:

    [privilege_escalation]

Enable privilege escalation by setting the become directive to true.
    
    become = true

Set the privilege escalation to use the sudo command by setting the
become_method directive to sudo.
    
    become_method = sudo

Set the privilege escalation user by setting the become_user directive to root.

    become_user = root

Enable prompting for the privilege escalation password by setting the  become_ask_pass directive to true.

    become_ask_pass = true

Confirm that the complete ansible.cfg file looks like the following:
  
    [defaults]
    inventory = ./inventory
    [privilege_escalation]
    become = true
    become_method = sudo
    become_user = root
    become_ask_pass = true

Save your work and exit the text editor.

Run the ansible --list-hosts command again to verify that you are now prompted for
the sudo password.
When prompted for the sudo password, enter student, even though it is not used for this dry run.

    [student@workstation deploy-manage]$ ansible intranetweb --list-hosts
    BECOME password: student
    hosts (1):
    servera.lab.example.com

## Finish
On workstation, run the lab deploy-manage finish script to clean up this exercise.

    [student@workstation ~]$ rm -rf deploy-manage
