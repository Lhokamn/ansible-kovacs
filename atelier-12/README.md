
# A vous de jouer

# Atelier 12

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
Ce playbook est bien idempotent et on obtient bien la bonne configuration dans le fichier `/etc/chrony.conf`
[Atelier suivant ->](../atelier-14/)
