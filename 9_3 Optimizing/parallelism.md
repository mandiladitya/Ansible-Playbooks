# Configuring Parallelism
    Configure parallelism in Ansible using forksAnsible allows much more control over the execution of the playbook by 
    running the tasks in parallel on all hosts. By default, Ansible only fork up to five times, so it will run a particular
    task on five different machines at once. This value is set in the Ansible configuration file ansible.cfg.
  .

    [student@crux~]$ grep forks /etc/ansible/ansible.cfg
    #forks          =  5
.

    • Ruuning tasks in parallel will make Ansible faster
    • Ansible can run tasks in parallel on all hosts
    • By default, tasks can run on 5 hosts at once
    • set forsk = nn in /etc/ansible/ansible.cfg to increase this number
    • Alternetively, use the --forks options with the ansible-playbook or ansible command
    • Use the serial keyword in a playbook to reduce the number of parallel tasks to a value that is lower than what is 
     specified with the forks option. 
