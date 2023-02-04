# How to create Nginx + PHP8.1 + MySQL cluster using Ansible

You'll need at least 2 Debian nodes with configured sudo.
```
su -
apt update && apt upgrade -y
apt install sudo
usermod -aG sudo $user
reboot
```

## Frist configuration
### **Ansible host**
1. Update packages and install pip & ansible
    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo apt install python3-pip sshpass
    sudo pip install ansible
    ```
2. Generate ssh key
    ```bash
    ssh-keygen -b 2048 -t rsa
    ```
3. Copy you ssh key to each node and configure non-password ssh communication
    ```bash
    scp ~/.ssh/id_pub.rsa $node_user@$node_ip:/home/$node_user/id_pub.rsa
    ```
### **Cluster nodes**
1. Make sure that hidden directory `.ssh` and file `authorized_keys` are present
    ```bash
    mkdir ~/.ssh
    touch ~/.ssh/authorized_keys
    ```
2. Copy content of copied ssh key into `authorized_keys` file, then delete ssh key
    ```bash
    cat ~/id_pub.rsa >> ~/.ssh/authorized_keys
    rm ~/id_pub.rsa
    ```

## How to run ansible
---
```
ansible-playbook -i environments/test/inventory.ini cluster_init.yml -Kk
```