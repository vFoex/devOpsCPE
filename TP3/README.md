<<<<<<< HEAD
# TP3

## 3-1

Setup :
````yml
all:
  vars:
    ansible_user: centos
    ansible_ssh_private_key_file: ../../../id_rsa
  children:
    prod:
      hosts: valentin.foex.takima.cloud
````

On test la connexion à l'aide du fichier setup et d'un ping:
````ansible all -i inventories/setup.yml -m ping````

Commandes pour voir l'os du serveur : 
````ansible all -i inventories/setup.yml -m setup -a "filter=ansible_distribution*"````

On retire le serveur Apache avec : 
```ansible all -i inventories/setup.yml -m yum -a "name=httpd state=absent" --become```



## 3-2

```yml
- hosts: all
  gather_facts: false
  become: yes

  # Install Docker
  tasks:
    - name: Clean packages
      command:
        cmd: dnf clean -y packages

    - name: Install device-mapper-persistent-data
      dnf:
        name: device-mapper-persistent-data
        state: latest

    - name: Install lvm2
      dnf:
        name: lvm2
        state: latest

    - name: add repo docker
      command:
        cmd: sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo

    - name: Install Docker
      dnf:
        name: docker-ce
        state: present

    - name: install python3
      dnf:
        name: python3

    - name: Pip install
      pip:
        name: docker

    - name: Make sure Docker is running
      service: name=docker state=started
      tags: docker

  roles:
    - docker #Ajout du roles
```
=======
# TP 3

Ajout du front (voir le TP extra)
>>>>>>> 26f4bbe627b380e86ba6cb00459942dbcc8cadbc
