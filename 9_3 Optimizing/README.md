# configuring delegation
In order to complete some configuration tasks, it may be necessary for actions to be taken on a different server than the one being configured.
Some examples of this might include an action that requires waiting for the server to be restarted (Selinux disabled to enforcing mode), adding
a server to a load balancer or a monitoring server, or making changes to the DHCP or DNS database needed for the server being configured.
Delegation can help by performing necessary actions for tasks on hosts other than the managed host being targeted by the play in the inventory.
Some scenarios that delegation can handle include:
 • Delegating a task to the local machine
 • Delegating a task to a host outside the play
 • Delegating a task to a host that exists in the inventory
 • Delegating a task to a host that does not exist in the inventory
 
 Delegating tasks to the local machine When any action needs to be performed on the node running Ansible, it can be delegated to localhost by
 using #delegate_to: localhost.
