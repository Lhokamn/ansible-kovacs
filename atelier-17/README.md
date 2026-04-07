# Atelier 

| Machine virtuelle | Adresse IP |
| - | - |
| ansible | 192.168.56.10 |
| rocky | 192.168.56.20 |
| debian | 192.168.56.30 |
| suse | 192.168.56.40 |
| ubuntu | 192.168.56.50 |

# Compte rendu

- Le premier playbook ``chrony-01.yml`` utilisera les modules de gestion de paquets natifs ``apt``, ``dnf`` et ``zypper`` et s'inspirera de la méthode « gros sabots » utilisée plus haut dans cet atelier.

```yml
---  # playbooks/chrony-01.yml

- hosts: all

  tasks:
    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
      when: ansible_os_family == "Debian"

    - name: Install Chrony on Rocky
      dnf:
        name: chrony
      when: ansible_distribution == "Rocky"

    - name: Install Chrony on OpenSUSE
      zypper:
        name: chrony
      when: ansible_distribution == "openSUSE Leap"

    - name: Start Chrony service for Suse and Rocky
      service:
        name: chronyd
        state: started
        enabled: true
      when: ansible_os_family != "Debian"

    - name: Start Chrony service for Debian/Ubuntu
      service:
        name: chrony
        state: started
        enabled: true
      when: ansible_os_family == "Debian"

    - name: Configure Chrony file Rocky and Suse
      copy:
        dest: /etc/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Configure Chrony
      when: ansible_os_family != "Debian"
    
    - name: Configure Chrony file Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Configure Chrony
      when: ansible_os_family == "Debian"


  handlers:
    - name: Configure Chrony for Rocky
      service:
        name: chronyd
        state: reload
      when: ansible_distribution == "Rocky"
      listen: Configure Chrony

    - name: Configure Chrony for Suse
      service:
        name: chronyd
        state: reload
      when: ansible_distribution == "openSUSE Leap"
      listen: Configure Chrony

    - name: Configure Chrony for Debian
      service:
        name: chrony
        state: restarted
      when: ansible_os_family == "Debian"
      listen: configure Chrony
...
```

- Le deuxième playbook ``chrony-02.yml`` définira trois variables ``chrony_package``, ``chrony_service`` et ``chrony_confdir`` et utilisera le module de gestion de paquets générique ``package``.

```yml
---  # playbooks/chrony-02.yml

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
      copy:
        dest: "{{chrony[ansible_distribution].chrony_confdir}}/chrony.conf"
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
        notify: configure Chrony

  handlers:
    - name: handler for chrony
      service:
        name: "{{chrony[ansible_distribution].chrony_service}}"
        state: restarted
      listen: configure Chrony

...
```

[Atelier suivant ->](../atelier-18/)