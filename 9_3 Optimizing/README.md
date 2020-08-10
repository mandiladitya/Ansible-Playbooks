# configuring delegation
    In order to complete some configuration tasks, it may be necessary for actions to be taken on a different server 
    than the one being configured. Some examples of this might include an action that requires waiting for the server
    to be restarted (Selinux disabled to enforcing mode), adding a server to a load balancer or a monitoring server,
    or making changes to the DHCP or DNS database needed for the server being configured. Delegation can help by performing
    necessary actions for tasks on hosts other than the managed host being targeted by the play in the inventory.

![Think_Declaratively](https://github.com/sonulodha/ansible/blob/master/9_8%20images/Think_Declaratively.jpg)

# Some scenarios that delegation can handle include: <br/>

    • Delegating a task to the local machine
    • Delegating a task to a host outside the play
    • Delegating a task to a host that exists in the inventory
    • Delegating a task to a host that does not exist in the inventory

 
    Delegating tasks to the local machine When any action needs to be performed on the node running Ansible, it can be
    delegated to localhost by using.
    delegate_to: localhost
#    lab-1  localhost ( processes, uptime, and ip addresses )

# Understanding delegate_to  <br/>

    • The delegated module will run on the host specified by delegate_to
    • Facts availabe will be the ones of the original host ( and not the delegated host )
    • The task has the context of the orignal target host, but gets excuted on the host the task is delegated to 
    • This can be useful to tell another host to do something with the managed hosts
       • Think of removing / adding a host to /from a load balancer or DNS

# configuring delegation  <br/>

    • Use delegeate_to: hostname to use delegation
    • Use local_action a shortcut to delegate_to: localhost
    • This will run on the Ansible master node
    • Notice that the localhost entry is implicit and doesn't have to be defined in the inventory
    • Addressing hosts that are in the inventory is straightforwardd: just address the hostname
          ansible localhost -m command -a'hostname'


# Delegating task to a host outside the play
    Ansible can be configured to run a task on a host other than the one that is part of the playwith delegate_to.
    The delegated module will still run once for every machine, but instead of running on the target machine, it 
    will run on the host specified by delegate_to.
    Addressing hosts that are in the inventory is straightforwardd: just address the hostname.

# lab-2 outside the play ( dbbackup )
    The following example shows Ansible code that will delegate a task to an outside machine. We are using two-tier architecture
    (database and webserver). now backup the database server with the help of ansible
    control node ,  before that we will have to stop  the  service of webserver and start the service again when the 
    backup is done.



# Delegating a task to a host that does not exist in the inventory
    When delegating a task to a host that does not exist in the inventory, Ansible will use the same connection type 
    and details used for the managed host to connect to the delegating host. Toadjust the connection details, use the
    add_host module to  create an ephemeral host in yourinventory with connection data defined
    
    • When addressing a host that is not in the inventory, credentials are needed.
    • Also, the delegated host needs to be configured to be connected by Ansible (sudo, ssh)
    • When accessing a host outside of the inventory, a temporary entry in the inventory must be  created by 
      using add_host
    • Ansible will u se the same connection type and details used for the managed host to connect to the 
      delegating hosts
        ansible-playbook outside_inv.yml -vvv


