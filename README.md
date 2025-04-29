
### YAML - YAML Ain't Markup Language™
- The official site is specified here
- https://yaml.org/
- To check indent spacing of the YAML code.Refer the below website.
- https://www.yamllint.com/

### Steps in Git Repo : https://github.com/ashokitschool/DevOps-Documents/blob/main/11-Ansible-Setup.md
@@@ Ansible Setup Video Reference : https://youtu.be/bm1J4ED-ZUo?si=zEULHcMHY4bibUhY

### Command to execute ansible playbook
- ansible-playbook 01-ping.yml

## 02-ping.yml
- After the ansible installation in Controller node and manager nodes
- Create the file
- vi 01-ping.yml
- Copy the code in Controller Node
- **CMD: ** ansible-playbook 02-ping.yml
- On executing what will happen is the control node will go and ping all the manage node.
- If all the manage node return pong then it means that the all the nodes are active and is responding to control node.

## 03-create-file.yml
- Control node will create a file called `f1.txt` in all managed node.

## 04-copy.yml
- Control node will copy a text to `f1.txt` in all managed node.

## 05-append-line.yml
- Control node will append a line to `f1.txt` in all managed node.

## 06-append-block.yml
- Control node will append a block of code to `f1.txt` in all managed node.

## 07-copy-data-to-file.yml
- Control node will copy data from one text file to another text file in all managed node.

## 08-website.yml
- This code will help you to install static website to manageed nodes belonging to webserver group.
- This file is executed in control node which setsup up the static website in all managed node in webserver group.
- Uses the yum module to install the Apache web server (httpd) 
- Ensures the latest version is installed.
- If already installed, it upgrades if a newer version is available.
- Uses the copy module to upload a file from the Ansible control node to the managed node.
- src: index.html: Local file on the Ansible control machine (must be in the same directory as the playbook or specify path).
- dest: /var/www/html/index.html: Path where Apache serves the website content.
- Overwrites the file if it already exists.
- Uses the service module to start the Apache service on the managed node.
- Ensures httpd is running so the website can be accessed via browser.
- Once the playbook is executed:
- Apache is installed
- Your index.html is in place
- The Apache server is started 
- Your static website is now live on port 80 of your EC2 instance 

## 09-website-notify-handler.yml
- notify is used inside a task.
- It tells Ansible to trigger a handler if the task results in a change.
- Here, notify: restart apache means if the httpd package was changed (e.g., newly installed), Ansible will trigger the handler named "restart apache".
- handler is a special type of task that runs only when notified.
- Usually used for services that should be restarted/reloaded only if something changed (like config file updates, package installations).

## 10-tags.yml
- In Ansible, tags are labels you can assign to tasks, roles, or blocks so you can selectively run specific parts of your playbook.
- Allow you to run only certain tasks instead of the whole playbook.
- Great for testing or rerunning a subset of tasks (like restarting a service or deploying code).
- **CMD:**
- ansible-playbook 10-tags.yml --list-tags
- ansible-playbook 10-tags.yml --tags "install"
- ansible-playbook 10-tags.yml --tags "copy"
- ansible-playbook 10-tags.yml --tags "install, copy"
- ansible-playbook 10-tags.yml --skip-tags "install"
- ansible-playbook 10-tags.yml --skip-tags "copy"

## 11-facts.yml
- In Ansible, facts are pieces of information automatically gathered about the target system(s) 
- before executing tasks. These facts describe the system’s environment, including:
- OS version
- IP address
- Hostname
- CPU, memory
- Network interfaces
- Disk information

## 12-register.yml
- In Ansible, register and debug are commonly used features to capture and display output during the execution of a playbook.
- register is used to store the result/output of a task into a variable. You can later refer to this variable in other tasks.
- debug is used to print messages or variable values to the console for visibility during playbook execution.
- This playbook:
- Runs the date command on your local machine.
- Stores the result.
- Prints the current date using a debug message.

## 13-error-handle.yml
- Error handling in Ansible refers to the techniques and mechanisms used to manage failures during playbook execution, 
- so your automation can react intelligently to problems.
- Uses the command: dates (which is incorrect, correct one is date).
- This command will fail.
- ignore_errors: yes is used, so Ansible will not stop even if the command fails.
- Uses the command: whoami.
- This command will succeed and print the current user.
- It runs regardless of the previous task's failure, thanks to ignore_errors.
- ignore_errors: yes: Prevents task failure from halting the playbook.
- Ansible continues to the next task even after an error if instructed.
- Useful for graceful handling of non-critical failures.

## 14-runtimeVariable.yml
- We can pass variable value during runtime.
- **CMD:**
- ansible-playbook 14-runtimeVariable.yml --extra-vars package_name=httpd
- ansible-playbook 14-runtimeVariable.yml --extra-vars package_name=git
- ansible-playbook 14-runtimeVariable.yml --extra-vars package_name=maven


## 15-playbookVariable.yml
- In Ansible, a playbook variable is a way to store dynamic values that can be reused throughout your playbook. 
- These variables help customize automation tasks, making them more flexible and efficient
- **CMD:**
- ansible-playbook 15-playbookVariable.yml

## 16-installSwGroupWise.yml
- This programme will execute task to install webservers group : java, db servers group : git
- This is not efficent but easy to understand code.
- For efficency refer 17-vars.yml


## 17-vars.yml
- ungrouped servers : httpd, webservers group : java, db servers group : git
- To achieve above requirement we need to use group_vars and host_vars concept.
- We need to supply variable value based on group name and based on host name.
- group_vars concept is used to specify variable value for group of managed nodes as per inventory file group name.
- Managed nodes we are configuring host inventory file like below
- Refer Screenshot: `variables scenario.png`

## 18-apache-roles-main.yml
- Refer `ROLES_APACHE_FOLDER_IMAGES`. This is the output obtained after executing the below steps
### Step-1: Connect with control node and switch to ansible user

$ sudo su ansible
$ cd ~

### Step-2 : Create a role using 'ansible-galaxy'

$ mkdir roles

$ cd roles

$ ansible-galaxy init apache

$ sudo yum install tree

$ tree apache

### Step-3 : Create tasks inside "tasks/main.yml" like below

---
# tasks file for apache
- name: install httpd
  yum:
    name: httpd
    state: latest
- name: copy index.html
  copy:
    src=index.html
    dest=/var/www/html/
  notify:
    - restart apache
...

### Step-4 : Copy required files into "files" directory

Note: keep index.html file in files directory

### Step-5 : configure handlers in "handler/main.yml"

---
# handlers file for apache
- name: restart apache
  service:
    name: httpd
    state: restarted
...

Note: With above 5 steps our "apache"  role is ready now we can execute that role like below

### Step-6 : Create main playbook to invoke role using role name

$ cd ~
$ vi invoke-roles.yml

---
- hosts: all
  become: true
  roles:
    - apache
...

Note: Roles will provide abstraction for ansible configuration in a modular and re-usable format.



