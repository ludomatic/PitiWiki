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
![Terminal RPi](/uploads/live-youtube-r-pi/rpi-stream-yt-terminal-1.jpg "Rpi Stream Yt Terminal 1")

Puis exécuter la commande suivante dans le terminal :
```
$ sudo raspistill -o tester.jpg
```

