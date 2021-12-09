Now its time to remove "-e" extra variables when you the playbooks.

you need to edit this playbook. 

  [student@workstation ansible]$ vim 1.SSH-KeyPair.yaml 

  
```yml
--- 
- name: SSH-KeyPair lab setup
  hosts: all
  tasks: 
  - name: Copying Sudoers file  
    copy: 
      src: /etc/sudoers.d/student
      dest: /etc/sudoers.d/student
      remote_src: yes 
  - name: Copy authorized key 
    authorized_key: 
      user: student
      state: present
      key: "{{ lookup('file', '/home/student/.ssh/id_rsa.pub') }}"
```

  [student@workstation ansible]$ ansible-playbook  1.SSH-KeyPair.yaml  -e "ansible_user=root ansible_password=redhat"

now play the playbook again, 

  [student@workstation ansible]$ ansible-playbook  1.SSH-KeyPair.yaml

  PLAY [SSH-KeyPair lab setup] *********************************************************************************************

  TASK [Gathering Facts] ***************************************************************************************************
  ok: [servera]

  TASK [Copying Sudoers file] **********************************************************************************************
  ok: [servera]

  TASK [Copy authorized key] ***********************************************************************************************
  ok: [servera]

  PLAY RECAP ***************************************************************************************************************
  servera                    : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

  [student@workstation ansible]$