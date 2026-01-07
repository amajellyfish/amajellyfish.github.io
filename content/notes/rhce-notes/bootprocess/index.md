---
title: 'Boot Process'
description: 'Managing Boot Process with Ansible'
showDate: false
---

### Managing the Boot Process

No modules for managing boot process.

**file** module
- manage the systemd boot targets

**lineinfile** module 
- manage the GRUB configuration.

**reboot** module
- enables you to reboot a host and pick up after the reboot at the exact same location.

#### Managing Systemd Targets

To manage the default systemd target:
- /etc/systemd/system/default.target file must exist as a symbolic link to the desired default target.

```bash
ls -l /etc/systemd/system/default.target
    lrwxrwxrwx. 1 root root 37 Mar 23 05:33 /etc/systemd/system/default.target -> /lib/systemd/system/multi-user.target
```

```yaml
---
- name: set default boot target
    hosts: ansible2
    tasks:
    - name: set boot target to graphical
      file:            
        src: /usr/lib/systemd/system/graphical.target
        dest: /etc/systemd/system/default.target
        state: link
```

#### Rebooting Managed Hosts

**reboot** module.
- Restart managed nodes. 

**test_command** argument
- Verify the renewed availability of the managed hosts
- Specifies an arbitrary command that Ansible should run successfully on the managed hosts after the reboot. The success of this command indicates that the rebooted host is available again.

Equally useful while using the reboot module are the arguments that
relate to timeouts. The reboot module uses no fewer than four of them:

• **connect_timeout:** The maximum seconds to wait for a successful
connection before trying again

• **post_reboot_delay:** The number of seconds to wait after the
**reboot** command before trying to validate the managed host is
available again

• **pre_reboot_delay:** The number of seconds to wait before actually
issuing the reboot

• **reboot_timeout:** The maximum seconds to wait for the rebooted
machine to respond to the **test** command

When the rebooted host is back, the current playbook continues its
tasks. This scenario is shown in the example in [Listing
14-7](#ch14.xhtml#list14_7), where first all managed hosts are rebooted,
and after a successful reboot is issued, the message "successfully
rebooted" is shown. [Listing 14-8](#ch14.xhtml#list14_8) shows the
result of running this playbook. In [Exercise 14-2](#ch14.xhtml#exe14_2)
you can practice rebooting hosts using the reboot module.

**Listing 14-7** Rebooting Managed Hosts

::: pre_1
    ---
    - name: reboot all hosts
      hosts: all
      gather_facts: no
      tasks:
      - name: reboot hosts
        reboot:
          msg: reboot initiated by Ansible
          test_command: whoami
      - name: print message to show host is back
        debug:
          msg: successfully rebooted
:::

**Listing 14-8** Verifying the Success of the reboot Module

::: pre_1
    [ansible@control rhce8-book]$ ansible-playbook listing147.yaml
    
    PLAY [reboot all hosts] *************************************************************************************************
    
    TASK [reboot hosts] *****************************************************************************************************
    changed: [ansible2]
    changed: [ansible1]
    changed: [ansible3]
    changed: [ansible4]
    changed: [ansible5]
    
    TASK [print message to show host is back] *******************************************************************************
    ok: [ansible1] => {
        "msg": "successfully rebooted"
    }
    ok: [ansible2] => {
        "msg": "successfully rebooted"
    }
    ok: [ansible3] => {
        "msg": "successfully rebooted"
    }
    ok: [ansible4] => {
        "msg": "successfully rebooted"
    }
    ok: [ansible5] => {
        "msg": "successfully rebooted"
    }
    
    PLAY RECAP **************************************************************************************************************
    ansible1                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible2                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible3                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible4                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
    ansible5                   : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
:::

::: box
**Exercise 14-2 Managing Boot State**

1\. As a preparation for this playbook, so that it actually changes the
default boot target on the managed host, use **ansible ansible2 -m file
-a "state=link src=/usr/lib/systemd/system/graphical.target
dest=/etc/systemd/system/default.target"**.

2\. Use your editor to create the file exercise142.yaml and write the
following playbook header:

``` pre1
---
- name: set default boot target and reboot
  hosts: ansible2
  tasks:
```

3\. Now you set the default boot target to multi-user.target. Add the
following task to do so:

``` pre1
- name: set default boot target
  file:
    src: /usr/lib/systemd/system/multi-user.target
    dest: /etc/systemd/system/default.target
    state: link
```

4\. Complete the playbook to reboot the managed hosts by including the
following tasks:

``` pre1
- name: reboot hosts
  reboot:
    msg: reboot initiated by Ansible
    test_command: whoami
- name: print message to show host is back
  debug:
    msg: successfully rebooted
```

5\. Run the playbook by using **ansible-playbook exercise142.yaml**.

6\. Test that the reboot was issued successfully by using **ansible
ansible2 -a "systemctl get-default"**.
:::

### Lab: Managing the Boot Process and Services


The advanced exercise, 14-3, is a relatively easy task this time. You
are guided through the procedure of creating a playbook that runs a
command before the reboot, schedules a cron job at the next reboot, and,
using that cron job, ensures that after rebooting a specific command is
used as well. To make sure you see what happens when, you work with a
temporary file to which lines are added.

::: box
**Exercise 14-3 Managing the Boot Process and Services**

1\. Use your editor to create the file exercise143.yaml and write the
playbook header as follows:

``` pre1
---
- name: exercise143
  hosts: ansible2
  tasks:
```

2\. Write the first task. This task is not really functional but enables
you to check what is happening in the remaining tasks in the playbook.
In this task, you use the lineinfile module to add a line to the end of
the check file /tmp/rebooted. Notice how the time, including a second
indicator, is written using two Ansible facts. It's required to operate
this way because not one single fact has the time in an hh:mm:ss format.
Write this task code as follows:

``` pre1
- name: add a line to file before rebooting
  lineinfile:
    create: true
    state: present
    path: /tmp/rebooted
    insertafter: EOF
    line: rebooted at {{ ansible_facts[’date_time’][’time’] }}:{{ ansible_facts[’date_time’][’second’] }}
```

3\. At this point, you can add a cron job that runs at reboot. The job
that runs will add a message to the /tmp/rebooted file, and to make sure
that it is working correctly, you use bash shell command substitution to
print the results of the Linux **date** command. Notice that this is
possible in the cron module because commands are executed by a bash
shell, but it's not possible in the previous task that uses the
lineinfile module because in that task the commands are not processed by
a shell. Now add the task as follows:

``` pre1
- name: run a cron job on reboot
  cron:
    name: "run on reboot"
    state: present
    special_time: reboot
    job: "echo rebooted at $(date) >> /tmp/rebooted"
```

4\. Add another task that uses the reboot module to reboot the managed
host:

``` pre1
- name: reboot managed host
  reboot:
    msg: reboot initiated by Ansible
    test_command: whoami
- name: show reboot success
  debug:
    msg: just rebooted successfully
```

5\. Now that the playbook is complete, you can run it by using
**ansible-playbook exercise143.yaml**. Notice that it needs a minute
because it has to wait until the target host is back. When it is back,
use **ansible ansible2 -a "cat /tmp/rebooted"**. You then see that both
reboot messages are written, and there is about 30 seconds between the
pre-reboot and the post-reboot commands.
:::


### Lab 14-1

Write a playbook according to the following specifications:

• The cron module must be used to restart your managed servers at 2 a.m.
each weekday.

• After rebooting, a message must be written to syslog, with the text
"CRON initiated reboot just completed."

• The default systemd target must be set to multi-user.target.

• The last task should use service facts to show the current version of
the cron process.
