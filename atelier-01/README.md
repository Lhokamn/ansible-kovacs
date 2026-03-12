# Atelier 01

- [Challenge 01](#challenge-01)
- [Challenge 02](#challenge-02)
- [Challenge 03](#challenge-03)

## Challenge 01

- démarrer la VM et se connecter avec vagrant
```bash
[ema@homelab-cclaudel:atelier-01] $ vagrant up ubuntu
[ema@homelab-cclaudel:atelier-01] $ vagrant ssh ubuntu 

vagrant@ubuntu:~$ cat /etc/os-release 
PRETTY_NAME="Ubuntu 22.04.5 LTS"
NAME="Ubuntu"
VERSION_ID="22.04"
VERSION="22.04.5 LTS (Jammy Jellyfish)"
VERSION_CODENAME=jammy
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=jammy
```

- Rafraîchissez les informations sur les paquets

```bash
vagrant@ubuntu:~$ sudo apt update
Get:1 http://security.ubuntu.com/ubuntu jammy-security InRelease [129 kB]                          
Hit:2 http://us.archive.ubuntu.com/ubuntu jammy InRelease                                          
Get:3 http://us.archive.ubuntu.com/ubuntu jammy-updates InRelease [128 kB]
Get:4 http://security.ubuntu.com/ubuntu jammy-security/main amd64 Packages [3,023 kB]
Get:5 http://us.archive.ubuntu.com/ubuntu jammy-backports InRelease [127 kB]
Get:6 http://us.archive.ubuntu.com/ubuntu jammy-updates/main amd64 Packages [3,286 kB]
Get:7 http://security.ubuntu.com/ubuntu jammy-security/main Translation-en [431 kB]
Get:8 http://security.ubuntu.com/ubuntu jammy-security/restricted amd64 Packages [5,251 kB]     
Get:9 http://us.archive.ubuntu.com/ubuntu jammy-updates/main Translation-en [499 kB]               
Get:10 http://us.archive.ubuntu.com/ubuntu jammy-updates/restricted amd64 Packages [5,414 kB]     
Get:11 http://security.ubuntu.com/ubuntu jammy-security/restricted Translation-en [1,000 kB]
Get:12 http://security.ubuntu.com/ubuntu jammy-security/universe amd64 Packages [1,019 kB]        
Get:13 http://security.ubuntu.com/ubuntu jammy-security/universe Translation-en [225 kB]           
Get:14 http://security.ubuntu.com/ubuntu jammy-security/multiverse amd64 Packages [51.9 kB]        
Get:15 http://security.ubuntu.com/ubuntu jammy-security/multiverse Translation-en [10.6 kB]      
Get:16 http://us.archive.ubuntu.com/ubuntu jammy-updates/restricted Translation-en [1,028 kB]      
Get:17 http://us.archive.ubuntu.com/ubuntu jammy-updates/universe amd64 Packages [1,257 kB]
Get:18 http://us.archive.ubuntu.com/ubuntu jammy-updates/universe Translation-en [315 kB]
Get:19 http://us.archive.ubuntu.com/ubuntu jammy-updates/multiverse amd64 Packages [59.0 kB]
Get:20 http://us.archive.ubuntu.com/ubuntu jammy-updates/multiverse Translation-en [13.5 kB]
Get:21 http://us.archive.ubuntu.com/ubuntu jammy-backports/main amd64 Packages [69.4 kB]
Get:22 http://us.archive.ubuntu.com/ubuntu jammy-backports/universe amd64 Packages [31.7 kB]
Get:23 http://us.archive.ubuntu.com/ubuntu jammy-backports/universe Translation-en [16.9 kB]
Fetched 23.4 MB in 5s (4,403 kB/s)                           
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
76 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

- Recherchez le paquet ``ansible`` avec les options qui vont bien

```bash
vagrant@ubuntu:~$ apt-cache search --names-only ansible
ansible - Configuration management, deployment, and task execution system
ansible-core - Configuration management, deployment, and task execution system
ansible-lint - lint tool for Ansible playbooks
ansible-mitogen - Fast connection strategy for Ansible
python-ansible-runner-doc - library that interfaces with Ansible (docs)
python-networking-ansible-doc - OpenStack virtual network service - Ansible plugin (docs)
python3-ansible-runner - library that interfaces with Ansible (Python 3.x)
python3-networking-ansible - OpenStack virtual network service - Ansible plugin (Python 3.x)
```

- Installez le paquet officiel fourni par la distribution.

```bash
vagrant@ubuntu:~$ sudo apt install ansible -y
```

- Vérifiez si l'installation s'est bien déroulée.

```bash
$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0]
```

- Notez la version d'Ansible.

version vieille 2.10.8
```
$ ansible --version
ansible 2.10.8
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0]
```

- Déconnectez-vous et supprimez la VM.

```bash
vagrant@ubuntu:~$ exit
logout
[ema@homelab-cclaudel:atelier-01] $ vagrant destroy -f ubuntu
```

## Challenge 02

On reprend les commande précédente

- démarrer la VM et se connecter avec vagrant
```bash
[ema@homelab-cclaudel:atelier-01] $ vagrant up ubuntu
[ema@homelab-cclaudel:atelier-01] $ vagrant ssh ubuntu 
```

- Rafraîchissez les informations sur les paquets

```bash
vagrant@ubuntu:~$ sudo apt update
```

- Installer le paquet avec un dépot PPA

```bash
 sudo apt-add-repository ppa:ansible/ansible
Repository: 'deb https://ppa.launchpadcontent.net/ansible/ansible/ubuntu/ jammy main'
Description:
Ansible is a radically simple IT automation platform that makes your applications and systems easier to deploy. Avoid writing scripts or custom code to deploy and update your applications— automate in a language that approaches plain English, using SSH, with no agents to install on remote systems.

http://ansible.com/
```

```bash
$ apt-cache search --names-only ansible
ansible-lint - lint tool for Ansible playbooks
ansible-mitogen - Fast connection strategy for Ansible
python-ansible-runner-doc - library that interfaces with Ansible (docs)
python-networking-ansible-doc - OpenStack virtual network service - Ansible plugin (docs)
python3-ansible-runner - library that interfaces with Ansible (Python 3.x)
python3-networking-ansible - OpenStack virtual network service - Ansible plugin (Python 3.x)
ansible - Ansible collections for ansible-core
ansible-core - Ansible IT Automation
ansible-test - Ansible IT Automation
```

```bash
vagrant@ubuntu:~$ sudo apt install ansible -y
```

- Vérifiez si l'installation s'est bien déroulée et noter la version 

```bash
vagrant@ubuntu:~$ ansible --version
ansible [core 2.17.14]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.10.12 (main, Aug 15 2025, 14:32:43) [GCC 11.4.0] (/usr/bin/python3)
  jinja version = 3.0.3
  libyaml = True
```

La version est plus récente que la précédente

## Challenge 03

- On démarre la VM Rocky Linux 9 avec vagrant et on se connecte en ssh

```bash
[ema@homelab-cclaudel:atelier-01] $ vagrant up rocky
[ema@homelab-cclaudel:atelier-01] $ vagrant ssh rocky
```

- on rafrachit les paquets

```bash
[vagrant@rocky ~]$ sudo dnf makecache
```

- Création du venv pour ansible

```bash
python3 -m venv ~/.venv/ansible
```

- On charge l'environement

```bash
source ~/.venv/ansible/bin/activate
(ansible) [vagrant@rocky ~]$ 
```

- On met à jour pip 

```bash
pip install --upgrade pip
```

- On installe ansible

```bash
pip install ansible
```

- On vérifie la version

```bash
(ansible) [vagrant@rocky ~]$ ansible --version
ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.21 (main, Aug 19 2025, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-5)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.6
  libyaml = True
```

- On crée un alias pour ansible 

Pour se simplifier la vie, je vais me créer un alias pour utiliser ansible sans devoir charger le venv.

Dans le fichier ``~/.bashrc`` on va rajouter la commande suivante :

```bash
alias ansible="/$HOME/.venv/ansible/bin/ansible"
```

puis si on recharge le fichier ``~/.bashrc``, on va pouvoir appeler ansible sans être dans le venv

```
[vagrant@rocky ~]$ ansible --version
ansible [core 2.15.13]
  config file = None
  configured module search path = ['/home/vagrant/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /home/vagrant/.venv/ansible/lib64/python3.9/site-packages/ansible
  ansible collection location = /home/vagrant/.ansible/collections:/usr/share/ansible/collections
  executable location = /home/vagrant/.venv/ansible/bin/ansible
  python version = 3.9.21 (main, Aug 19 2025, 00:00:00) [GCC 11.5.0 20240719 (Red Hat 11.5.0-5)] (/home/vagrant/.venv/ansible/bin/python3)
  jinja version = 3.1.6
  libyaml = True
```