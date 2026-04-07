# Atelier 

| Machine virtuelle | Adresse IP |
| - | - |
| ansible | 192.168.56.10 |
| rocky | 192.168.56.20 |
| debian | 192.168.56.30 |
| suse | 192.168.56.40 |

# Compte rendu

- ``pkg-info.yml`` pour afficher le gestionnaire de paquets utilisé

```yml
---  # playbooks/pkg.yml

- hosts: all
  tasks:
    - name: "Print package manager"
      debug:
        msg: "{{ansible_hostname}} use package {{ansible_pkg_mgr}}"

...
```

```console
ansible-playbook playbooks/pkg.yml 

PLAY [all] **************************************************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [Print package manager] ********************************************************************************
ok: [rocky] => {
    "msg": "rocky use package dnf"
}
ok: [debian] => {
    "msg": "debian use package apt"
}
ok: [suse] => {
    "msg": "suse use package zypper"
}
```

- ``python-info.yml`` pour afficher la version de Python installée

```yml
---  # playbooks/python-info.yml

- hosts: all
  tasks:
    - name: "Print Python version"
      debug:
        msg: "{{ansible_python_version}} is install on {{ansible_hostname}}"

...
```

```console
$ ansible-playbook playbooks/python-info.yml 

PLAY [all] **************************************************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [suse]
ok: [rocky]
ok: [debian]

TASK [Print Python version] *********************************************************************************
ok: [rocky] => {
    "msg": "3.9.21 is installed on rocky"
}
ok: [debian] => {
    "msg": "3.11.2 is installed on debian"
}
ok: [suse] => {
    "msg": "3.6.15 is installed on suse"
}
```

- ``dns-info.yml`` pour afficher le(s) serveur(s) DNS utilisé(s)

```yml
---  # playbooks/dns-info.yml

- hosts: all
  tasks:
    - name: "Print DNS"
      debug:
        msg: "List of dns register for {{ansible_hostname}}: {{ansible_dns.nameservers}}"
...
```

```console
$ ansible-playbook playbooks/dns-info.yml 

PLAY [all] **************************************************************************************************

TASK [Gathering Facts] **************************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Print DNS] ********************************************************************************************
ok: [rocky] => {
    "msg": "List of dns register for rocky: ['10.0.2.3']"
}
ok: [debian] => {
    "msg": "List of dns register for debian: ['10.0.2.3']"
}
ok: [suse] => {
    "msg": "List of dns register for suse: ['10.0.2.3']"
}
```

[Atelier suivant ->](../atelier-17/)