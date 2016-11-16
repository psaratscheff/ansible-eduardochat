# Installation
On Ubuntu 16.04 Xenial
```
sudo apt-get update
sudo apt-get install ansible
```
# Execution
```
cd <this_folder>
ansible-playbook -i hosts <master/slaves>.yml
```

# Configuration
## ssh connection
Configure ssh connections at `~/..ssh/config` <br/>
Sample configuration blocks:
```
Host arqui9
    HostName assw9.ing.puc.cl
    User administrator
    ControlMaster auto
    ControlPath ~/.ssh/socket-%r@%h:%p
    ServerAliveInterval 120
Host <next_server>
    HostName <url>
    User <user>
    ControlMaster auto
    ControlPath ~/.ssh/socket-%r@%h:%p
    ServerAliveInterval 120
```
## Servers to deploy
Change this using roles at execution file, such as slaves.yml
```
- hosts: deployment_slaves
  roles:
  - dashboard
  - arquicoins
  - chat
```
## Roles, commands to execute
Change this in corresponding folders, path matters. <br/>
Example:
```
---
# This Playbook runs all the common plays in the deployment

- name: GIT-PULL
  git:  repo=<repo-url> dest=<absolute-path-to-server-folder> refspec=+refs/pull/*:refs/heads/*
  become_user: <user>

- name: NPM install dependencies
  command: /usr/bin/npm install

- name: Restart Services PM2
  command: /usr/bin/pm2 restart <pm2-app-name>
```

