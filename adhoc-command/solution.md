## Task
Just Receive an Email from your Teamlead That says you should install nginx on all the webserver group in the inventory using ansible.

### Solution
create 3 webserver in aws using ubuntu machine
name them Master,Target1 and Target 2
ssh into all servers
Type ssh-keygen on the master server (This will creat both private and public key)
cd .ssh and enter on the master server
On the ssh directory type ls to show authorized_keys (id_ed25519 and id_ed25519.pub)
cat id_ed25519.pub and copy key
On target 1 machine type ifconfig then sudo apt install net-tool
type vi authorized_key and paste the public authorized key from the master while leaving two spaces and save.
Do same for Target 2 Machine.
In your Master  cd .ssh then
ssh your target1 ip and also do same for target 2 ip
Exit and go back to your controller and install ansible
Run ansible --version and if not found
type vi ansible.sh  copy this file into your script
#! /bin/bash
#This is the script to install the ansible
sudo apt update -y
sudo apt-add-repository ppa:ansible/ansible -y

sudo apt update -y 

sudo apt install ansible -y

#confirm installation

ansible –version

Then save
run chmod 777 ansible.sh
run ./ansible.sh

Creat a file in your ansible machine  touch inventory
 run vi inventory once open
 type [server name] leave two space and paste the ip for both private ip for both target machines. Esc and save.

 To install Nginx on the webserver we creat a file 
 install-nginx on our vscode and copy the command
 - hosts: webservers
  become: true
  tasks:
  - name: installing nginx software
    apt:
      name: nginx
      state: latest

  - name: start nginx service
    service:
      name: nginx
      state: started
then we create same file on our ansible machine (master) using the command 
touch install-nginx.yml
vi install-nginx.yml
copy the code into the script and save (learn how to write code yourself)
 then run the command

 ansible-playbook -i inventory install-nginx.yml
 then do curl localhost nginx
 put the private ip in your browser with port 80 and you would see nginx installed.

 to stop nginx server
 change state:absent