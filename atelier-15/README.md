# Atelier 

| Machine virtuelle | Adresse IP |
| - | - |
| ansible | 192.168.56.10 |
| rocky | 192.168.56.20 |
| debian | 192.168.56.30 |
| suse | 192.168.56.40 |

# Compte rendu

- Écrivez un *playbook* ``kernel.yml`` qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande ``uname -a`` et le module ``debug`` avec le paramètre ``msg``

```yml
---  # playbooks/kernel.yml

- hosts: all

  tasks:
    - name: Retrive Kernel information
      command: uname -a
      changed_when: false
      register: uname_cmd

    - debug:
        msg: "{{uname_cmd.stdout_lines}}"
...
```

```console
$ ansible-playbook kernel.yml 

PLAY [all] *****************************************************************************************

TASK [Gathering Facts] *****************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Retrive Kernel information] ******************************************************************
ok: [debian]
ok: [rocky]
ok: [suse]

TASK [debug] ***************************************************************************************
ok: [rocky] => {
    "msg": [
        "Linux rocky 5.14.0-570.52.1.el9_6.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Oct 15 13:59:22 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux"
    ]
}
ok: [debian] => {
    "msg": [
        "Linux debian 6.1.0-40-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.153-1 (2025-09-20) x86_64 GNU/Linux"
    ]
}
ok: [suse] => {
    "msg": [
        "Linux suse 6.4.0-150600.23.73-default #1 SMP PREEMPT_DYNAMIC Tue Oct  7 08:43:02 UTC 2025 (46f6a23) x86_64 x86_64 x86_64 GNU/Linux"
    ]
}

PLAY RECAP *****************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0 
```

- Essayez d'obtenir le même résultat en utilisant le paramètre ``var`` du module ``debug``

```yml
---  # playbooks/kernel.yml

- hosts: all

  tasks:
    - name: Retrive Kernel information
      command: uname -a
      changed_when: false
      register: uname_cmd

    - debug:
        var: uname_cmd.stdout_lines
...
```

```console
$ ansible-playbook kernel.yml 

PLAY [all] *****************************************************************************************

TASK [Gathering Facts] *****************************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [Retrive Kernel information] ******************************************************************
ok: [debian]
ok: [suse]
ok: [rocky]

TASK [debug] ***************************************************************************************
ok: [rocky] => {
    "uname_cmd.stdout_lines": [
        "Linux rocky 5.14.0-570.52.1.el9_6.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Oct 15 13:59:22 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux"
    ]
}
ok: [debian] => {
    "uname_cmd.stdout_lines": [
        "Linux debian 6.1.0-40-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.153-1 (2025-09-20) x86_64 GNU/Linux"
    ]
}
ok: [suse] => {
    "uname_cmd.stdout_lines": [
        "Linux suse 6.4.0-150600.23.73-default #1 SMP PREEMPT_DYNAMIC Tue Oct  7 08:43:02 UTC 2025 (46f6a23) x86_64 x86_64 x86_64 GNU/Linux"
    ]
}

PLAY RECAP *****************************************************************************************
debian                     : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
```


- Écrivez un *playbook* ``packages.yml`` qui affiche le nombre total de paquets RPM installés sur les hôtes ``rocky`` et ``suse`` (``rpm -qa | wc -l``).

```yml
---  # playbook/packages.yml

- hosts: rocky, suse
  tasks:
    - name: Count package for Rocky and Suse
      shell: "rpm -qa | wc -l"
      changed_when: false
      register: rpm_count

    - name: Display count
      debug:
        msg: "{{ansible_hostname}} have {{rpm_count.stdout}} packages"
...
```

```console
$ ansible-playbook packages.yml 

PLAY [rocky, suse] *********************************************************************************

TASK [Gathering Facts] *****************************************************************************
ok: [suse]
ok: [rocky]

TASK [Count package for Rocky and Suse] ************************************************************
ok: [rocky]
ok: [suse]

TASK [Display count] *******************************************************************************
ok: [rocky] => {
    "msg": "rocky have 642 packages"
}
ok: [suse] => {
    "msg": "suse have 504 packages"
}

PLAY RECAP *****************************************************************************************
rocky                      : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

[Atelier suivant ->](../atelier-16/)