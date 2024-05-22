- CONTROL STATION:
//Keep in mind openssh-server has to be installed on both control station and target station//
sudo apt-add-repository ppa:ansible/ansible
sudo apt update
sudo apt install ansible
ansible --version //check for ansible version

==> ssh-keygen

--------------------------------------------------------

- TARGET STATION - Controlled by CONTROL STATION:
sudo apt update
 - sudo apt install openssh-server -y
 - sudo systemctl status sshd

--------------------------------------------------------

ON BOTH STATIONS (and for any other target you want to use/control)
sudo adduser ansible ; echo "ansible ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/ansible

 -------------------------------------------------------

- FROM CONTROL STATION
ssh-copy-id ansible@your.target.ip

--------------------------------------------------------

- TARGET STATION
sudo usermod -L ansible	//gets rid of password verificatio on target station

--------------------------------------------------------

- CONTROL STATION
ssh ansible@your.target.ip

ansible all -i ./inventory -u ansible -m ping //check if target station responds

ansible-playbook -i ./inventory -u ansible restart.yml

--------------------------------------------------------
