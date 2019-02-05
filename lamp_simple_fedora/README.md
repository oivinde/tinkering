Building a simple LAMP stack and deploying Application using Ansible Playbooks.
-------------------------------------------
These playbooks require Ansible 1.2.

These playbooks are meant to be a reference and starter's guide to building
Ansible Playbooks. These playbooks were tested on Fedora Server 29 so I recommend
that you use Fedora to test these modules.

This LAMP stack can be on a single node or multiple nodes. The inventory file
'hosts' defines the nodes in which the stacks should be configured.

        [webservers]
        lab01.demo.local

        [dbservers]
        lab02.demo.local

Here the webserver would be configured on "lab01.demo.local" and the dbserver on a
server called "lab02.demo.local". The stack can be deployed using the following
command:

        ansible-playbook -i hosts site.yml

Once done, you can check the results by browsing to http://lab01.demo.local/index.php.
You should see a simple test page and a list of databases retrieved from the
database server.

#### SELinux set to permissive on web server. This needs to be sorted out and done right.
