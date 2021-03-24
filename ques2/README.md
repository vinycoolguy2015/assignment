Change your Nginx server ip in hosts file and execute
ansible-playbook -i hosts playbook.yml on the ansible machine

**We are using CentOS7 as our Nginx Web Server**

**Ansible server should be able to communicate with Nginx server.Also downlaod ngnix source which we'll copy to web server**

**pcre pcre-devel zlib zlib-devel gcc should be installed on Nginx server**
