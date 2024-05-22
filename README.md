- CONTROL STATION:
> Keep in mind openssh-server has to be installed on both control station and target station

Install Ansible using:
```
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
ansible --version //check for ansible version
```
Then create your ssh key using:
```
ssh-keygen
```
--------------------------------------------------------

- TARGET STATION  (Controlled by CONTROL STATION)

Install opennsh-server and chekc if it is active
```
sudo apt update
sudo apt install openssh-server -y
sudo systemctl status sshd
```
If not then run:
```
sudo systemctl start ssh.service
```
--------------------------------------------------------

- ON BOTH STATIONS (and for any other target you want to use/control)

Add a user and add them to the sudoers file
```
sudo adduser ansible ; echo "ansible ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible
```
 -------------------------------------------------------

- FROM CONTROL STATION

Copy your ssh-key to the target station
```
ssh-copy-id ansible@your.target.ip
```
--------------------------------------------------------

- TARGET STATION

This command gets rid of any password verification
```
sudo usermod -L ansible	
```

--------------------------------------------------------

- CONTROL STATION

Connect to your target station
```
ssh ansible@your.target.ip
```
Ping all target stations in
```
ansible all -i ./inventory -u ansible -m ping
```
Run restart.yml on every target station
```
ansible-playbook -i ./inventory -u ansible restart.yml
```
--------------------------------------------------------
