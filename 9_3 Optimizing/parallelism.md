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

# Rolling updates
If there is a website being deployed on 100 web servers, only 10 of them should be updatedat the same time.
The serial key can be set to 10 in the playbook to reduce the number ofsimultaneous deployments (assuming that
the fork key was set to something higher). The serial keyword can also be specified as a percentage which will be
applied to the total numberof hosts in the play. If the number of hosts does not divide equally into the number 
of passes, the final pass will contain the modulus. Regardless of the percentage, the number of hosts per passwill
always be 1 or greater.

    ----
    - name: Limit the number of hosts this play runs on at the same time
      hosts: webservers
      serial: 2
      tasks:
          
    ...   
           
    ansible-playbook play.yml  --forks 2
   
 -
The serial keyword can also be specified as a percentage, which will be applied to the total number of hosts in a play,
in order to determine the number of hosts per pass:

    ---
    - name: test play
      hosts: webservers
      serial: "30%"

If the number of hosts does not divide equally into the number of passes, the final pass will contain the remainder.
As of Ansible 2.2, the batch sizes can be specified as a list, as follows:

    ---
    - name: test play
      hosts: webservers
      serial:
       - 1
       - 5
       - 10

In the above example, the first batch would contain a single host, the next would contain 5 hosts, and (if there are any hosts
left), every following batch would contain 10 hosts until all available hosts are used.
It is also possible to list multiple batch sizes as percentages:

    ---
    - name: test play
      hosts: webservers
      serial:
       - "10%"
       - "20%"
       - "100%"

You can also mix and match the values:

     ---
    - name: test play
      hosts: webservers
      serial:
       - 1
       - 5
       - "20%"
 


# Asynchronous tasks
There are some system operations that take a while to complete. For example, when downloadinga large file or rebooting a server,
such tasks takes a long time to complete. Using parallelism andforks, Ansible starts the command quickly on the managed hosts, then
polls the hosts for statusuntil they are all finished.

To run an operation in parallel, use the async and poll keywords. The async keyword triggers Ansible to run the job in the background 
and can be checked later, and its value will be the maximum time that Ansible will wait for the command to complete. The value of poll 
indicates to Ansible how often to poll to check if the command has been completed. The default poll value is 10 seconds
In the example, the get_url module takes a long time to download a file and async: 3600 instructs Ansible to wait for 3600 
seconds to complete the task and poll: 10 is the polling time in seconds to check if the download is complete.

    ----
    - name: Long running task 
      hosts: all  
      tasks:    
       - name: Download big file
         get_url: 
          url: http://crux.example.com/bigfile.tar.gz
          async: 3600
          poll: 10
/.

    ansible-playbook waitforme.yml
    ansible-playbook waitforme2.yml
        
 
