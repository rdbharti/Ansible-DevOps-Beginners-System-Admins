# Notify and Handlers

## In a playbook

- In Ansible, a handler refers to a particular task that executes when triggered by the notify module. 
- Handlers perform an action defined in the task when a change occurs in the remote host.
- Handlers are helpful when you need to perform a task that relies on a specific task’s success or failure. 
- For example, you can set a handler to send Apache logs if the service goes down.

- In a nutshell, handlers are special tasks that only get executed when triggered via the notify directive. Handlers are executed at the end of the play, once all tasks are finished.

- Example: We will start httpd service only when httpd is installed

