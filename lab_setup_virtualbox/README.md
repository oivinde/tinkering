## How to set up Fedora Server lab environment for Ansible in VirtualBox.

I like to create three virtual machines for playing with Ansible. One control node where I actually run Ansible and store playbooks, two other snaphotted machines, that are as clean as possible.

![alt text](https://github.com/oivinde/tinkering/blob/master/lab_setup_virtualbox/images/VirtualBox_LabSetup.png "VirtualBox Lab Setup")

#### In VirtualBox, create NAT Network "DemoNet" - 192.168.90.0/24 (Preferences - Network)

If you want to create port forwarding rules in the NAT Network right away, look at the bottom of this file.

#### Create new virtual machine:
- Name: ansible.demo.local
- CPU: 2
- RAM: 2048MB
- HDD: 8GB
- CD: fedora_server.iso
- IP: 192.168.90.10/24
- GW: 192.168.90.1
- DNS: 192.168.90.1
- Hostname: ansible.demo.local

Select NAT Network "DemoNet" in machine settings

Default minimal install.

Dismount/remove mounted ISO when install is finished.

Logon to console in new machine and run the following:

Install general requirements and nmtui for convencience in changing NIC information.

`yum install NetworkManager-tui python2 python2-libselinux -y`

`yum update -y`

##### Shutdown machine.

Create full clone of ansible.demo.local to first lab-machine. (Regenerate MAC addresses)

- Name: lab01.demo.local

Select NAT Network "DemoNet" in machine settings

Create linked clone of lab01.demo.local. (Regenerate MAC addresses)

- Name: lab02.demo.local

Select NAT Network "DemoNet" in machine settings

Boot each machine and correct hostname and IP info (with `nmtui`):

lab01
- IP: 192.168.90.20/24
- GW: 192.168.90.1
- DNS: 192.168.90.1
- hostname: lab01.demo.local

lab02
- IP: 192.168.90.30/24
- GW: 192.168.90.1
- DNS: 192.168.90.1
- hostname: lab02.demo.local

Reboot both lab machines.

Logon to ansible.demo.local:

`yum install ansible git lynx -y`

`nano /etc/hosts`

        192.168.90.10 ansible ansible.demo.local
        192.168.90.20 lab01 lab01.demo.local
        192.168.90.10 lab02 lab02.demo.local

`ssh-keygen`

`ssh-copy-id lab01.demo.local`

`ssh-copy-id lab02.demo.local`

`scp /etc/hosts lab01.demo.local:/etc/hosts`

`scp /etc/hosts lab02.demo.local:/etc/hosts`

##### Take snapshot of lab01 and lab02 in VirtualBox! Now these machines can be reused easy!

Supress deprecation warnings. Not important, but looks nicer when demoing!

`nano /etc/ansible/ansible.cfg`

        deprecation_warnings = False

`git clone https://github.com/oivinde/tinkering`

`cd tinkering`

Enter wanted subfolder and change "hosts" file to reflect lab01 and lab01 names.

If you want to use local client to access guest machines, create port forwarding rules on the NAT network DemoNet in VirtualBox, or install GUI in ansible.demo.local.

Here a sample config.
![alt text](https://github.com/oivinde/tinkering/blob/master/lab_setup_virtualbox/images/Port_Forwarding_Rules.png "Port Forwarding Rules")


On your client SSH to ansible.demo.local, would be done like this: `ssh -p 2222 root@localhost`

Or just run lynx on the ansible.demo.local machine to show result of for instance LAMP deploy.

`lynx http://lab01.demo.local/index.php`
