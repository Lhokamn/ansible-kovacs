# A vous de jouer

# Atelier 14


- Écrivez un playbook ``myvars1.yml`` qui affiche respectivement votre voiture et votre moto préférée en utilisant le module debug et deux variables ``mycar`` et ``mybike`` définies en tant que play vars.

```yml
---  # playbooks/myvars1.yml

- hosts: all
  vars:
    mycar: "Peugeot 208"
    mybike: "Harley Davindson"

  tasks:
    - debug:
        msg: "My car is {{mycar}} and my byke is an {{mybike}}"
...
```

- En utilisant les extra vars, remplacez successivement l'une et l'autre marque - puis les deux à la fois - avant d'exécuter le play.

On remplace d'abord le modèle de voiture, cela fonctionne. 

```console
[vagrant@control playbooks]$ ansible-playbook myvars1.yml -e mycar="camaro"

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target01]
ok: [target03]
ok: [target02]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is camaro and my byke is an Harley Davindson"
}
ok: [target02] => {
    "msg": "My car is camaro and my byke is an Harley Davindson"
}
ok: [target03] => {
    "msg": "My car is camaro and my byke is an Harley Davindson"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```
Puis on essaie de remplacer le modèle de la moto, cela fonctionne aussi. 
```console
[vagrant@control playbooks]$ ansible-playbook myvars1.yml -e mybike="h2r"

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target03]
ok: [target02]
ok: [target01]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is Peugeot 208 and my byke is an h2r"
}
ok: [target02] => {
    "msg": "My car is Peugeot 208 and my byke is an h2r"
}
ok: [target03] => {
    "msg": "My car is Peugeot 208 and my byke is an h2r"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
Enfin on remplace les deux modèles. 
```console
[vagrant@control playbooks]$ ansible-playbook myvars1.yml -e mycar="camaro" -e mybike="h2r"

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target01]
ok: [target03]
ok: [target02]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is camaro and my byke is an h2r"
}
ok: [target02] => {
    "msg": "My car is camaro and my byke is an h2r"
}
ok: [target03] => {
    "msg": "My car is camaro and my byke is an h2r"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

- Écrivez un playbook ``myvars2.yml`` qui fait essentiellement la même chose que ``myvars1.yml``, mais en utilisant une tâche avec set_fact pour définir les deux variables.

```yml
---  # playbooks/myvars2.yml
- hosts: all
  tasks:
    - name: Define Variables
      set_fact:
        mycar: "Peueot 208"
        mybike: "Harley Davindson"
    - debug:
        msg: "My car is {{mycar}} and my byke is an {{mybike}}"
...
```

- Là aussi, essayez de remplacer les deux variables en utilisant des extra vars avant l'exécution du play.

```console
[vagrant@control playbooks]$ ansible-playbook myvars2.yml -e mycar="camaro" -e mybike="h2r"

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target03]
ok: [target01]
ok: [target02]

TASK [Define Variables] ********************************************************
ok: [target01]
ok: [target02]
ok: [target03]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is camaro and my byke is an h2r"
}
ok: [target02] => {
    "msg": "My car is camaro and my byke is an h2r"
}
ok: [target03] => {
    "msg": "My car is camaro and my byke is an h2r"
}

PLAY RECAP *********************************************************************
target01                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

- Écrivez un playbook ``myvars3.yml`` qui affiche le contenu des deux variables ``mycar`` et ``mybike`` mais sans les définir. Avant d'exécuter le playbook, définissez VW et BMW comme valeurs par défaut pour ``mycar`` et ``mybike`` pour tous les hôtes, en utilisant l'endroit approprié.

```yml
---  # playbooks/myvars3.yml

- hosts: all
  tasks:
    - debug:
        msg: "My car is {{mycar}} and my byke is an {{mybike}}"
...
```
On définit les valeurs par défauts dans `group_var/all.yml`. 
```yml
---  # group_var/all.yml
mycar: "VW"
mybike: "BMW"
...
```

Lorsqu'on lance le playbook, on retrouve bien les valeurs par défauts définies pour tous les hôtes. 
```console
[vagrant@control ema]$ ansible-playbook playbooks/myvars3.yml 

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target02]
ok: [target03]
ok: [target01]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is VW and my byke is an BMW"
}
ok: [target02] => {
    "msg": "My car is VW and my byke is an BMW"
}
ok: [target03] => {
    "msg": "My car is VW and my byke is an BMW"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

- Effectuez le nécessaire pour remplacer VW et BMW par Mercedes et Honda sur l'hôte target02
On remplace les valeurs par defauts dans le fichier `host_vars/target02.yml` pour qu'il prenne nos nouvelles valeurs. 
```yml
---  # host_vars/target02.yml
mycar: "Mercedes"
mybike: "Honda"
...
```
On obtiens bien les nouvelles valeurs définis pour l'host target02. 
```console
[vagrant@control ema]$ ansible-playbook playbooks/myvars3.yml 

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target03]
ok: [target02]
ok: [target01]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "My car is VW and my byke is an BMW"
}
ok: [target02] => {
    "msg": "My car is Mercedes and my byke is an Honda"
}
ok: [target03] => {
    "msg": "My car is VW and my byke is an BMW"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

- Écrivez un playbook ``display_user.yml`` qui affiche un utilisateur et son mot de passe correspondant à l'aide des variables ``user`` et ``password``. Ces deux variables devront être saisies de manière interactive pendant l'exécution du playbook. Les valeurs par défaut seront ``microlinux`` pour ``user`` et ``yatahongaga`` pour ``password``. Le mot de passe ne devra pas s'afficher pendant la saisi

```yml
---  # playbook/display_user.yml
- hosts: all
  vars_prompt:
    - name: user
      prompt: "Provide user name"
      default: "microlinux"
      private: false
    - name: password
      prompt: "Provide user password (as secret)"
      default: "yatahongaga"
      private: true

  tasks:
    - debug:
        msg: "Welcome home {{user}} this is you super password:{{password}}"

...
```
Avec les valeurs par défauts, `microlinux` pour `user` et `yatahongaga` pour `password` .
```console
ansible-playbook playbooks/display_user.yml 
Provide user name [microlinux]: 
Provide user password (as secret) [yatahongaga]: 

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target02]
ok: [target01]
ok: [target03]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "Welcome home microlinux this is you super password:yatahongaga"
}
ok: [target02] => {
    "msg": "Welcome home microlinux this is you super password:yatahongaga"
}
ok: [target03] => {
    "msg": "Welcome home microlinux this is you super password:yatahongaga"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```

Et avec les valeurs modifiées par l'utilisateur. 
```console
[vagrant@control ema]$ ansible-playbook playbooks/display_user.yml 
Provide user name [microlinux]: Louise
Provide user password (as secret) [yatahongaga]: 

PLAY [all] *********************************************************************

TASK [Gathering Facts] *********************************************************
ok: [target03]
ok: [target01]
ok: [target02]

TASK [debug] *******************************************************************
ok: [target01] => {
    "msg": "Welcome home Louise this is you super password:Corecorentin"
}
ok: [target02] => {
    "msg": "Welcome home Louise this is you super password:Corecorentin"
}
ok: [target03] => {
    "msg": "Welcome home Louise this is you super password:Corecorentin"
}

PLAY RECAP *********************************************************************
target01                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target02                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
target03                   : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

[Atelier suivant ->](../atelier-15/)
