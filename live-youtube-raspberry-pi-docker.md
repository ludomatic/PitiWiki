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

