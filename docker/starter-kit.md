<!-- TITLE: Docker Starter Kit -->
<!-- SUBTITLE: Installation de Docker et des outils qui vont bien sur OS X et Debian -->

# Debian Jessie x86_64 / OS X & Docker-ce
## Préparation

### Sources

Éditer `/etc/apt/sources.list`:

```
deb http://ftp.au.debian.org/debian/ stretch main
deb http://security.debian.org/debian-security stretch/updates main
deb http://ftp.au.debian.org/debian/ stretch-updates main
```

### Paquets utiles

```
# apt update
# apt install mc nmap elinks
```

### Dépôts Docker

```
# apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
# curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -
# add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable"
# apt-get update
```

## Installation de Docker-ce

### - En production : version spécifique

```
# apt-cache madison docker-ce
# apt-get install docker-ce=<PACKAGE_VERSION_STRING>
```

### - En dev/test : dernière version

```
# apt-get install docker-ce
# docker -v
Docker version 17.12.1-ce, build 7390fc6
```

## Ajout d'un utilisateur standard *docker* non root

```
# adduser docker
```

Ajouter sa clé publique depuis son poste de travail:

```
secure-pc ~$ ssh-copy-id docker@docker-pc-distant
```

Tester d'abord l'accès SSH avec l'utilisateur docker...

```
secure-pc ~$ ssh docker@docker-pc-distant
docker@docker-pc-distant ~$ 
```

...avant de retirer l'accès par mot de passe du serveur *SSH* en modifiant la ligne comme suit: `PasswordAuthentication no`

```
# mcedit /etc/ssh/sshd_config
```

Désactiver ensuite le mot de passe de l'utilisateur *docker*, et ajouter l'utilisateur *docker* au groupe *docker* afin de lui permettre d'accéder au *Docker Engine*:

```
# passwd -d docker
# addgroup docker docker
```

Tester l'accès au *Docker Engine* depuis une nouvelle connexion de l'utilisateur *docker* (afin de charger les bons groupes):

```
docker@docker-pc-distant ~$ docker -v
Docker version 17.12.1-ce, build 7390fc6
```

## Utilitaires

### docker-compose

[OS X] docker-compose est déjà installé

[Linux] Consulter la dernière version disponible sur https://github.com/docker/compose/releases puis utiliser le numéro de version dans la commande ci-dessous:
```
# curl -L https://github.com/docker/compose/releases/download/<LATEST_RELEASE_VERSION_STRING>/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
# chmod +x /usr/local/bin/docker-compose
# docker-compose -v
docker-compose version 1.20.0-rc1, build 86428af
```

### ctop

Installer la dernière version depuis https://github.com/bcicen/ctop:
[OS X]
```
# brew install ctop
```

[Linux]
```
# wget https://github.com/bcicen/ctop/releases/download/v0.7/ctop-0.7-linux-amd64 -O /usr/local/bin/ctop
# chmod +x /usr/local/bin/ctop
# ctop -v
ctop version 0.7, build 233259b go1.9.2
```

### screen

```
# apt install screen
```

```
secure-pc ~$ scp .screenrc docker@docker-pc-distant:.
```

