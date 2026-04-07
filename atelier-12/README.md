Playbook pour gérer chrony sur la famille redhat

```yml
---  # chrony.yml

- hosts: redhat
  tasks:
    - name: Install Chrony
      dnf:
        name: chrony

    - name: Activate and Enable Chrony
      service:
        name: chronyd
        state: started
        enabled: true


    - name: Configure Chrony
      copy:
        dest: /etc/chrony.conf
        src: chrony.conf


  handlers:
    - name: Configure Chrony
      service:
        name: chronyd
        state: reload

...
```

[Atelier suivant ->](../atelier-14/)