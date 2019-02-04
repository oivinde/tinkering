Building a simple LAMP stack and deploying Application using Ansible Playbooks.
-------------------------------------------
(Original text)
These playbooks require Ansible 1.2.

These playbooks are meant to be a reference and starter's guide to building
Ansible Playbooks. These playbooks were tested on Fedora Server 29 so we recommend
that you use Fedora to test these modules.

This LAMP stack can be on a single node or multiple nodes. The inventory file
'hosts' defines the nodes in which the stacks should be configured.

        [webservers]
        localhost

        [dbservers]
        bensible

Here the webserver would be configured on the local host and the dbserver on a
server called "bensible". The stack can be deployed using the following
command:

        ansible-playbook -i hosts site.yml

Once done, you can check the results by browsing to http://localhost/index.php.
You should see a simple test page and a list of databases retrieved from the
database server.

-------
(My additions)
### My demo setup based on VirtualBox:

In VirtualBox, create NAT Network "DemoNet" - 192.168.90.0/24

##### Create new virtual machine:
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

Install general requirements
´yum install NetworkManager-tui python2 python2-libselinux -y´

´yum update -y´

Shutdown machine.

Create full clone of ansible.demo.local to first lab-machine. Regenerate MAC addresses.

- Name: lab01.demo.local

Select NAT Network "DemoNet" in machine settings

Create linked clone of lab01.demo.local. Regenerate MAC addresses.

- Name: lab02.demo.local

Select NAT Network "DemoNet" in machine settings

Boot each machine and correct hostname and IP info (with "nmtui" for convenience):

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

´yum install ansible git lynx -y´

´nano /etc/hosts´

        ´192.168.90.10 ansible ansible.demo.local
        192.168.90.20 lab01 lab01.demo.local
        192.168.90.10 lab02 lab02.demo.local´

´ssh-keygen´

´ssh-copy-id lab01.demo.local´

´ssh-copy-id lab02.demo.local´

´scp /etc/hosts lab01.demo.local:/etc/hosts´

´scp /etc/hosts lab02.demo.local:/etc/hosts´

##### Take snapshot of lab01 and lab02 in VirtualBox! Now these machines can be reused easy!

Supress deprecation warnings. Not important, but looks nicer when demoing!

´nano /etc/ansible/ansible.cfg´

        deprecation_warnings = False

´git clone https://github.com/oivinde/tinkering´

´cd tinkering´

Enter wanted subfolder and change "hosts" file to reflect lab01 and lab01 names.

Since the environemnt is isolated, I just run lynx on the ansible.demo.local machine to show result of LAMP deploy.

´lynx http://lab01.demo.local/index.php´
