# Conditions

- Ansible supports conditional evaluations before executing a specific task on the target hosts. 
- If the set condition is true, Ansible will go ahead and perform the task. 
- If the condition is not true (unmet), Ansible will skip the specified task.
- In a playbook, you may want to execute different tasks, or have different goals, depending on the value of a fact (data about the remote system), a variable, or the result of a previous task. 
- You may want the value of some variables to depend on the value of other variables. 
- Or you may want to create additional groups of hosts based on whether the hosts match other criteria. 
- You can do all of these things with conditionals.

