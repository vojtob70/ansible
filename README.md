# Ansible na WSL2

### Prereq:
cez windows power shell v admin rezime:

    wsl --install -d Ubuntu
### na novej WSL instancii:
    sudo apt update
    sudo apt install ansible
    git clone https://github.com/vojtob70/ansible.git
    cd ansible
### potom mozu ist playbooky:
    ansible-playbook ./playbooks/system-update.yml --connection=local
    ansible-playbook ./playbooks/set-sudoers.yml --connection=local
    ansible-playbook ./playbooks/docker-install.yml --connection=local
sudo visudo a doplnit na posledny riadok, miesto "vojto" vlozit login daneho usera
    
    sudo visudo
    
    vojto ALL=(ALL) NOPASSWD: /usr/bin/dockerd

    vi .bashrc  
a doplnit na koniec suboru:

    # Start Docker daemon automatically when logging in if not running.
    RUNNING=`ps aux | grep dockerd | grep -v grep | grep -v sudo`
    if [ -z "$RUNNING" ]; then
        sudo dockerd > /dev/null 2>&1 &
        disown
    fi

vytvor docker volume pre portainer: (https://docs.docker.com/engine/reference/commandline/volume_create/)

    docker volume create portainer_data

vytvorenie portainer kontajnera bez ansible, iba ako dockewr nasadenie - https://docs.portainer.io/v/ce-2.9/start/install/server/docker/linux

a pomocou ansible:

    sudo ansible-playbook ./playbooks/docker-containers-create.yml --connection=local
Su vytvorene 3 docker konrajnery:

http://localhost:9000   -> portainer

a watchtower bez dostupnosti URL

Ostatne utilitky:

    ansible-playbook ./playbooks/docker-containers-restart.yml --connection=local
    ansible-playbook ./playbooks/docker-containers-stop.yml --connection=local
    ansible-playbook ./playbooks/docker-containers-start.yml --connection=local
    ansible-playbook ./playbooks/docker-containers-remove.yml --connection=local
