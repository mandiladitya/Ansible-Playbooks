# configuring delegation
In order to complete some configuration tasks, it may be necessary for actions to be taken on a different server than the one being configured. Some examples of this might include an action that requires waiting for the server to be restarted (Selinux disabled to enforcing mode), adding a server to a load balancer or a monitoring server, or making changes to the DHCP or DNS database needed for the server being configured. Delegation can help by performing necessary actions for tasks on hosts other than the managed host being targeted by the play in the inventory.

# Some scenarios that delegation can handle include: <br/>

    • Delegating a task to the local machine
    • Delegating a task to a host outside the play
    • Delegating a task to a host that exists in the inventory
    • Delegating a task to a host that does not exist in the inventory

 
 Delegating tasks to the local machine When any action needs to be performed on the node running Ansible, it can be delegated to localhost by
 using.

    delegate_to: localhost
lab localhost ( processes, uptime, and ip addresses )

# Understanding delegate_to  <br/>

    • The delegated module will run on the host specified by delegate_to
    • Facts availabe will be the ones of the original host ( and not the delegated host )
    • The task has the context of the orignal target host, but gets excuted on the host the task is delegated to 
    • This can be useful to tell another host to do something with the managed hosts
       • Think of removing / adding a host to /from a load balancer or DNS

# configuring delegation  <br/>

    • Use delegeate_to: hostname to use delegation
    • Use local_action a shortcut to delegate_to: localhost
    • This will run on the ANsible master node
    • Notice that the localhost entry is implicit and doesn't have to be defined in the inventory
    • Addressing hosts that are in the inventory is straightforwardd: just address the hostname  <br/>

    ansible localhost -m command -a'hostname'
