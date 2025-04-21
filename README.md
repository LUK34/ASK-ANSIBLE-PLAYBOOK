
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