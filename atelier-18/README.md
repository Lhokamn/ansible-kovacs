# A vous de jouer

# Atelier 18

| Machine virtuelle | Adresse IP |
| - | - |
| ansible | 192.168.56.10 |
| rocky | 192.168.56.20 |
| debian | 192.168.56.30 |
| suse | 192.168.56.40 |
| ubuntu | 192.168.56.50 |

# Compte rendu

- On a ce fichier template de ``chrony`` sur chacune cible. 

```j2
# {{ chrony[ansible_distribution].chrony_confdir }}chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```
On écrit un playbook qui permet d'installer un fichier de configuration personnalisé sur nos cibles. 
```yml
---  # playbooks/chrony_jinja.yml
- hosts: all
  vars:
    chrony:
      Debian:
        chrony_package: chrony
        chrony_service: chrony
        chrony_confdir: /etc/chrony/
      Ubuntu:
        chrony_package: chrony
        chrony_service: chrony
        chrony_confdir: /etc/chrony/
      Rocky:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc/
      openSUSE Leap:
        chrony_package: chrony
        chrony_service: chronyd
        chrony_confdir: /etc/

  tasks:
    - name: Update Package for Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install chrony
      package:
        name: "{{chrony[ansible_distribution].chrony_package}}"

    - name: Start and Enable chrony
      service:
        name: "{{chrony[ansible_distribution].chrony_service}}"
        state: started
        enabled: true

    - name: Copy Chrony config
      template:
        dest: "{{chrony[ansible_distribution].chrony_confdir}}/chrony.conf"
        src: chrony.conf.j2
      notify: configure Chrony

  handlers:
    - name: handler for chrony
      service:
        name: "{{chrony[ansible_distribution].chrony_service}}"
        state: restarted
      listen: configure Chrony
      
...
```
