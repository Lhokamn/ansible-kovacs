# Atelier 03


## Préambule

On démarre les VM de l'atelier 3 puis on se connecte sur l'hôte de control

```bash
[ema@homelab-cclaudel:atelier-03] $ vagrant up
[ema@homelab-cclaudel:atelier-03] $ vagrant ssh control
```

et on remarque bien que ansible est déjà installé sur cette hôte

```bash
vagrant@control:~$ type ansible
ansible is /usr/bin/ansible
```

## Commande Bash

- Enregistrement DNS

La première étape est de modifier le fichier ``/etc/hosts`` pour indiquer les IP des VM

```bash
cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 vagrant

192.168.56.10	control
192.168.56.20	target01
192.168.56.30	target02
192.168.56.40	target03
```

On vérifie que les ping fonctionne

```bash
for HOST in target01 target02 target03; do ping -c 1 -q $HOST; donePING target01 (192.168.56.20) 56(84) bytes of data.

--- target01 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.603/0.603/0.603/0.000 ms
PING target02 (192.168.56.30) 56(84) bytes of data.

--- target02 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.529/0.529/0.529/0.000 ms
PING target03 (192.168.56.40) 56(84) bytes of data.

--- target03 ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 0.426/0.426/0.426/0.000 ms
```

- Récupérer les clés publiques des hotes distants

```bash
ssh-keyscan -t rsa target01 target02 target03 >> ~/.ssh/known_hosts
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.13
```

- On va ensuite se générer une clé ssh pour se connecter au machine distante

```bash
ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:B1+VRCZR/We5jdJe3xRsecFlYu6PY/bbVzlIaxNnyyE vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|            o=Xo+|
|             *.=.|
|        .   . o =|
|         o . E X=|
|        S o ..@oX|
|         .  .=oO=|
|            .o*o*|
|             o.o=|
|               .=|
+----[SHA256]-----+
```

- puis on va donner notre clé à toutes les machines distantes

```bash
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target01
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target02
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@target03
```

- test des ping avec le module ansible

```bash
ansible all -i target01,target02,target03 -u vagrant -m ping
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

[Atelier suivant ->](../atelier-06/)