Download and install the Virtual Box program.
Download and install Vagrant.

Open a terminal and make sure Vagrant is running: Powershell can be used as a terminal.
C:\> vagrant -v

Create a "Vagrant" folder on the C:\ drive.

Then we search for the image of the machine we want to download and run in the portal of vagrant and execute it in the CLI
C:\> vagrant init [ geerlingguy/centos7 ]

After that, a .vagrantfile will be created in the "Vagrant" folder on the C:\ drive.

You open the .vagrantfile and change the necessary locations.

C:\> vagrant up
This command automatically creates and configures a virtual machine if it does not already exist, or simply starts it if it already exists.

2 machines are installed in Virtual Box. ControlNode and Server (login: Vagrant / pass: Vagrant)

We login to ControlNod with SSH. $vagrant SSH ControlNode
Configuring SSH.
$sudo apt install sshpass
$ssh-keygen -t ed25519
This command creates a new SSH key pair (Public and Private key).
$ssh-copy-id vagrant@192.168.10.20

Using Visual Virtual Code, open the C:\Vagrant folder and create the playbook.yml and hosts.ini files

In the hosts.ini file, we show the ip and login password of the server.
192.168.10.20 ansible_user=vagrant ansible_password=vagrant

We enter the Controlnode and execute these commands. (vagrant ssh controlnode)
$ sudo apt update
$sudo apt-add-repository ppa:ansible/ansible
$sudo apt install ansible
$ansible --version

Inside the Playbook.yml file we write the following scripts
---
- hosts: "all"
 become: true
 tasks:
 #Installing Nginx
 - name: "Install nginx via apt"
 ansible.builtin.apt:
 name: "nginx"
 state: "latest"
 update_cache: true

 - name: "Delete /var/www/html folder"
 ansible.builtin.file:
 path: "/var/www/html"
 state: "absent"

 - name: "Copy our landing to /var/www/html folder"
 ansible.builtin.copy:
 src: "files/html"
 dest: "/var/www/"
 owner: "vagrant"
 group: "vagrant"
 mode: "0644"

 #installing Mysql
 - name: "Install mysql via apt"
 ansible.builtin.apt:
 name: "mysql-server"
 state: "latest"
 update_cache: true

 - name: "Install pymysql via apt"
 ansible.builtin.apt:
 name: "python3-pymysql"
 state: "latest"
 update_cache: true

 - name: "Set up root user"
 community.mysql.mysql_user:
 name: "root"
 password: "password"
 login_user: "root"
 login_password: "password"
 check_implicit_admin: true
 login_unix_socket: "/var/run/mysqld/mysqld.sock"

 - name: "Remove anonymous users"
 community.mysql.mysql_user:
 name: ""
 state: "absent"
 login_user: "root"
 login_password: "password"

 - name: "Remove test database"
 community.mysql.mysql_db:
 name: "test"
 state: "absent"
 login_user: "root"
 login_password: "password"

#Installing PHP

 - name: "Installing php fpm and php mysql"
 ansible.builtin.apt:
 name: "{{ item }}"
 state: "latest"
 update_cache: true
 with_items:
 - "php-fpm"
 - "php-mysql"

 - name: "Copy php files to /var/www"
 ansible.builtin.copy:
 src: "files/test-php/php_test"
 dest: "/var/www/"
 owner: "vagrant"
 group: "vagrant"
 mode: "0644"

 - name: "Copy nginx config for php testing"
 ansible.builtin.copy:
 src: "files/test-php/nginx.conf"
 dest: "/etc/nginx/sites-available/php_test.conf"
 owner: "vagrant"
 group: "vagrant"
 mode: "0644"

 - name: "Create link"
 ansible.builtin.file:
 src: "/etc/nginx/sites-available/php_test.conf"
 dest: "/etc/nginx/sites-enabled/php_test"
 state: "link"


We run the playbook on the controlnode.
$ansible-playbook playbook.yml -i hosts.ini

In the hosts file on your computer to test PHP functionality
Add 192.168.10.20 as php-test.com and open the page in the browser.
php-test.com/test-connection.php in web browser for MySQL connection








