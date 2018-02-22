<!-- TITLE: Caster sur Youtube depuis votre RaspberryPi via Docker -->
<!-- SUBTITLE: Live ((o)) Broadcast -->

Castez le flux vidéo de votre Raspicam vers Youtube Live en utilisant une image Docker toute prête à l’emploi. Aucune compilation fastidieuse ni configuration de `ffmpeg` n’est nécessaire : branchez la caméra, installez Docker et lancez votre flux en une commande :)

> L’article original en anglais est disponible sur le site d’Alex Ellis [https://blog.alexellis.io/live-stream-with-docker/](https://blog.alexellis.io/live-stream-with-docker/). Le contenu ci-dessous est un résumé en français, mis à jour lors de mon installation. 

# Installer son Raspberry Pi

- J’ai utilisé un RaspberryPi 3, mais l’installation est valable sur un RPi version 1, 2 ou Zero.
- Une carte microSD de 2Gb est suffisante.
- Concernant la caméra, il s’agit d’une Raspicam standard
- Rendez-vous sur [https://www.raspberrypi.org/downloads/raspbian/](https://www.raspberrypi.org/downloads/raspbian/) pour télécharger Raspbian Stretch version Desktop (bureau graphique). Vous y trouverez également un guide d’installation selon votre environnement (Linux, OSX ou Windows). Le projet a été également testé sur Raspbian Jessie Lite et Pixel desktop.


Une fois le matériel branché, Raspbian installé et votre Raspberry Pi démarré, commencer par valider le retour de votre caméra à l’écran. Pour cela, cliquer sur l’icône du “terminal” dans la barre de raccourcis :

![Raccourci Terminal](/uploads/live-youtube-raspberry-pi-docker/rpi-stream-yt-terminal-1.jpg "Raccourci Terminal")


Puis exécuter la commande suivante dans le terminal :
```
$ sudo raspistill -o tester.jpg
```
![Terminal RPi](/uploads/live-youtube-raspberry-pi-docker/rpi-stream-yt-terminal-2.jpg "Terminal RPi")


Avant de passer à l’étape suivante, vous devez obtenir à l’écran l’image prise avec la caméra du Raspberry Pi :

![Selfie](/uploads/live-youtube-raspberry-pi-docker/rpi-stream-yt-tester.jpg "Selfie")


 # Installer Docker
 
Toujours dans le terminal, exécutez les commandes suivantes pour installer Docker (installation, ajout du compte utilisateur “pi” au groupe Docker puis redémarrage):
```
$ curl -sSL https://get.docker.com | sh
$ sudo usermod pi -aG docker
$ reboot
```

Alex Ellis fourni son image Docker tout-en-un (approx. 500Mb) que vous pouvez à présent télécharger grâce à la commande suivante (toujours depuis un terminal) :
```
$ docker pull alexellis2/streaming:17-5-2017
```

Pendant ce temps, suivez l’étape ci-dessous afin de récupérer votre clé de diffusion Youtube 🎥


# Récupérer sa clé secrète de diffusion Youtube

Rendez vous sur votre tableau de bord YouTube afin de récupérer la clé fournie en bas de page dans la zone “__Stream name/key__“. Elle est composée de 4 groupes de chiffres et lettres `xxxx-xxxx-xxxx-xxxx`. Copiez/notez-la pour la suite.

![Selfie](/uploads/live-youtube-raspberry-pi-docker/rpi-stream-yt_dashboard.jpg "Selfie")

# Lancer l’application de streaming

Il est temps de démarrer votre flux temps réel sur YouTube! Grace au travail d’Alex Ellis, une simple commande suffit. Veillez seulement à remplacer la chaîne `xxxx-xxxx-xxxx-xxxx` par votre propre clé de diffusion YouTube :

```
$ docker run --privileged --name cam -ti alexellis2/streaming:17-5-2017 xxxx-xxxx-xxxx-xxxx
```

Quelques remarques :

- Le flux se coupe si la connexion échoue, ou si vous appuyez sur le raccourci `Control + C`.
- Afin de lancer l’application en tâche de fond, remplacez l’option `-ti` par `-d`.
- Si vous relancez l’application par la suite, exécutez simplement `docker start cam`.
- Si vous souhaitez supprimer l’instance de votre application pour en relancer une nouvelle, arrêtez puis supprimez-là avec `docker stop cam` puis `docker rm cam`.
- Afin de diffuser le flux automatiquement à chaque démarrage du RaspberryPi, ajoutez l’option `--restart=always` à votre commande Docker.

```
$ docker run --privileged --name cam -ti --restart=always alexellis2/streaming:17-5-2017 xxxx-xxxx-xxxx-xxxx
```

Pour la configuration avancée, voir la section suivante. Je n’ai pas eu besoin d’aller modifier le flux audio mais voici la traduction au cas où.


# Flux audio

Youtube Live oblige à intégrer un flux audio. Dans l’image fournie par Alex Ellis, un flux audio “vide” est transmis avec la vidéo. Si vous souhaitez transmettre le son avec votre vidéo, il vous faudra reconstruire l’image Docker à partir des sources en modifiant quelques paramètres dans le fichier `streaming/entry.sh`. Voici la marche à suivre.

## Reconstruire l’image de zéro

Commencer par récupérer les sources depuis le dépôt Github d’Alex Ellis:
```
$ git clone https://github.com/alexellis/raspberrypi-youtube-streaming/
$ cd raspberrypi-youtube-streaming
$ cd streaming
```

Modifier ensuite le fichier entry.sh selon vos besoins avant de passer à la construction proprement dite de l’image.

Le processus de construction de l’image va prendre plusieurs heures sur un Raspberry Pi Zero, mais si vous disposez d’un Pi 2 ou 3 vous pouvez tirer avantage du processeur multi-cœur: éditer le fichier streaming/Dockerfile en modifiant la ligne RUN make en RUN make -j 4.
Passez ensuite à la construction de l’image:
```
$ docker build -t alexellis2/streaming .
```

## Modifier des paramètres

Une autre méthode plus rapide consiste à simplement modifier le fichier entry.sh et de l’utiliser dans votre propre image, par dessus celle d’Alex Ellis. Ainsi, la construction ne partira pas de zéro et votre nouvelle image sera disponible en quelques secondes.

Pour cela, créer un nouveau dossier contenant 2 fichiers. Le premier sera le fichier Dockerfile avec le contenu suivant:
```
FROM alexellis2/streaming:17-5-2017
COPY entry.sh entry.sh
```

Le second sera le fichier entry.sh que vous pouvez récupérer des sources du le dépôt Github d’Alex Ellis, et que vous pouvez modifier à votre guise. Vous pouvez à présent construire votre propre image depuis ce nouveau dossier:
```
$ docker build -t mon-image .
```
…et la lancer:
```
$ docker run --privileged --name cam2 -ti monimage xxxx-xxxx-xxxx-xxxx
```

## Accéder à la console bash

Vous pouvez vous connecter à la console bash du conteneur en modifiant le point d’entrée entrypoint comme ceci:
```
$ docker run --entrypoint=/bin/bash --privileged --name cam -ti alexellis2/streaming:17-5-2017
```

Bon live!
