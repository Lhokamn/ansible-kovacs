# Atelier 

| Machine virtuelle | Adresse IP |
| - | - |
| ansible | 192.168.56.10 |
| rocky | 192.168.56.20 |
| debian | 192.168.56.30 |
| suse | 192.168.56.40 |

# playbook Debian

```yml
---  # apache-debian.yml

- hosts: debian
  tasks:
    - name: Update package apt
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache2 Debian
      apt:
        name: apache2

    - name: Start and Enable
      service:
        name: apache2
        state: started
        enabled: true

    - name: Custom Web Page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!Doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>First web page</title>
            </head>
            <body>
              <h1>First Playbook</h1>
              <p>Write by Louise Arnoud and Corentin Claudel for Debian</p>
            </body>
          </html>

...
```

# playbook Rocky

```yml
---  # apache-rocky.yml

- hosts: rocky
  tasks:
    - name: Install Apache2 rocky
      dnf:
        name: httpd

    - name: Start and Enable
      service:
        name: httpd
        state: started
        enabled: true

    - name: Custom Web Page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!Doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>First web page</title>
            </head>
            <body>
              <h1>First Playbook</h1>
              <p>Write by Louise Arnoud and Corentin Claudel for Rocky</p>
            </body>
          </html>

...
```

# playbook Suse

```yml
---  # apache-suse.yml

- hosts: suse
  tasks:
    - name: Install Apache2 suse
      zypper:
        name: apache2

    - name: Start and Enable
      service:
        name: apache2
        state: started
        enabled: true

    - name: Custom Web Page
      copy:
        dest: /srv/www/htdocs/index.html
        mode: 0644
        content: |
          <!Doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>First web page</title>
            </head>
            <body>
              <h1>First Playbook</h1>
              <p>Write by Louise Arnoud and Corentin Claudel for Suse</p>
            </body>
          </html>

...
```

# Resultat

## Debian

```console
[vagrant@ansible playbooks]$ curl debian
<!Doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>First web page</title>
  </head>
  <body>
    <h1>First Playbook</h1>
    <p>Write by Louise Arnoud and Corentin Claudel for Debian</p>
  </body>
</html>
```

## Rocky

```console
[vagrant@ansible playbooks]$ curl rocky
<!Doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>First web page</title>
  </head>
  <body>
    <h1>First Playbook</h1>
    <p>Write by Louise Arnoud and Corentin Claudel for Rocky</p>
  </body>
</html>
```

## Suse

```console
[vagrant@ansible playbooks]$ curl suse
<!Doctype html>
<html>
  <head>
    <meta charset="utf-8">
    <title>First web page</title>
  </head>
  <body>
    <h1>First Playbook</h1>
    <p>Write by Louise Arnoud and Corentin Claudel for Suse</p>
  </body>
</html>
```

[Atelier suivant ->](../atelier-12/)