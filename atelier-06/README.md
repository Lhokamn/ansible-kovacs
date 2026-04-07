# A vous de jouer

# Atelier 

| Machine virtuelle | Adresse IP |
| - | - |
| control | 192.168.56.10 |
| target01 | 192.168.56.20 |
| target02 | 192.168.56.30 |
| target03 | 192.168.56.40 |


# Réponse

- Éditez ``/etc/hosts`` de manière à ce que les Target Hosts soient joignables par leur nom d'hôte simple.

```
$ cat /etc/hosts | head -n 7
127.0.0.1 localhost
127.0.1.1 vagrant

192.168.56.10	control
192.168.56.20	target01
192.168.56.30	target02
192.168.56.40	target03
```

- Configurez l'authentification par clé SSH avec les trois *Target Hosts*.

```bash
# Generate SSH Key
ssh-keygen

# Get key in know host
ssh-keyscan -t rsa target01 target02 target03 >> ~/.ssh/known_hosts

# Copy Id
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target01
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target02
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target03
```

- Installez Ansible.

```bash
sudo apt update
sudo apt install python3-venv -y
python3 -m venv ~/.venv/ansible
source ~/.venv/ansible/bin/activate
pip install --upgrade pip
pip install ansible
cat ~/.bashrc | grep "ansible"
    alias ansible="/home/vagrant/.venv/ansible/bin/ansible"
```

- Envoyez un premier ``ping`` Ansible sans configuration.

```bash
ansible all -i target01,target02,target03 -u vagrant -m ping
```

- Créez un répertoire de projet ``~/monprojet``

```bash
mkdir -p ~/monprojet
```

- Créez un fichier vide ``ansible.cfg`` dans ce répertoire.

```bash
touch ansible.cfg
```

- Vérifiez si ce fichier est bien pris en compte par Ansible.

```bash
$ ansible --version
ansible [core 2.17.14]
config file = /home/vagrant/monprojet/ansible.cfg
```

- Spécifiez un inventaire nommé ``hosts``

```bash
$ cat ansible.cfg 
    [defaults]
    inventory = ./hosts
```

- Activez la journalisation dans ``~/journal/ansible.log``

```bash
$ cat ansible.cfg 
[defaults]
inventory = ./hosts
log_path = ~/journal/ansible.log
```

- Testez la journalisation

```bash
$ ansible all -m ping
[WARNING]: provided hosts list is empty, only localhost is available. Note that the
implicit localhost does not match 'all'

$ cat ~/journal/ansible.log 

2026-03-16 09:20:04,496 p=2790 u=vagrant n=ansible | [WARNING]: provided hosts list is empty, only localhost is available. Note that the
implicit localhost does not match 'all'
```

- Créez un groupe ``[testlab]`` avec vos trois Target Hosts

```bash
$ cat hosts 
[testlab]
target01
target02
target03

```

- Définissez explicitement l'utilisateur ``vagrant`` pour la connexion à vos cibles

```bash
$ cat hosts 
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant

```


- Envoyez un ``ping`` Ansible vers le groupe de machines ``[all]``

```bash
$ ansible all -m ping
[WARNING]: Platform linux on host target01 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host target02 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
[WARNING]: Platform linux on host target03 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3.10"
    },
    "changed": false,
    "ping": "pong"
}
```

- Définissez l'élévation des droits pour l'utilisateur ``vagrant`` sur les Target Hosts

```bash
$ cat hosts 
[testlab]
target01
target02
target03

[testlab:vars]
ansible_user=vagrant
ansible_become=yes
```

- Affichez la première ligne du fichier ``/etc/shadow`` sur tous les Target Hosts

```bash
$ ansible all -a "head -n 1 /etc/shadow"
[WARNING]: Platform linux on host target02 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target02 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
[WARNING]: Platform linux on host target03 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target03 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
[WARNING]: Platform linux on host target01 is using the discovered Python interpreter at
/usr/bin/python3.10, but future installation of another Python interpreter could change
the meaning of that path. See https://docs.ansible.com/ansible-
core/2.17/reference_appendices/interpreter_discovery.html for more information.
target01 | CHANGED | rc=0 >>
root:*:19977:0:99999:7:::
```


[Atelier suivant ->](../atelier-07/)