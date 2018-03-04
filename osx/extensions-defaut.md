<!-- TITLE: Lier une application à une extension -->
<!-- SUBTITLE: Solution la plus rapide trouvée -->

Suite à la mise à jour OS X High Sierra 10.13.3, XCode est devenu l'application par défaut pour ouvrir les fichiers MarkDown (extension `.md`)...

![Xcode par défaut](/uploads/osx-extensions-defaut/osx-extensions-defaut-xcode.png "Osx Extensions Defaut Xcode")

Impossible d'indiquer à OS X de plutôt ouvrir l'application [MacDown](https://macdown.uranusjr.com/), même avec le bouton droit sur un fichier `.md`, puis dans le lenu contextuel `Ouvrir avec` > `Autre`.

![Menu contextuel](/uploads/osx-extensions-defaut/osx-extensions-defaut-contextuel.png "Osx Extensions Defaut Xcode")

En effet, lorsque que l'on coche `Toujours ouvrir avec` dans la fenêtre de sélection de l'application, cela ne concerne que le fichier sélectionné et non tous les fichiers du même type. 😬

![Grrrrr](/uploads/osx-extensions-defaut/osx-extensions-defaut-toujours.png "Osx Extensions Defaut Xcode")

Pour résoudre ce problème simplement, utiliser le gestionnaire `Brew` pour installer l'application `duti` et définir l'application par défaut.

- Installer l'application `duti` via homebrew :

```
ludo@ludo-mac ~ brew install duti
==> Downloading https://homebrew.bintray.com/bottles/duti-1.5.3.high_sierra.bottle.2.tar.gz
######################################################################## 100.0%
==> Pouring duti-1.5.3.high_sierra.bottle.2.tar.gz
🍺  /usr/local/Cellar/duti/1.5.3: 6 files, 41.7KB
```

- Si l'application (MacDown dans mon cas) est déjà liée à une extension (prenons par exemple `.txt`), s'en servir pour récupérer l'identifiant de l'application :

```
ludo@ludo-mac ~ duti -x txt
MacDown.app
/Applications/MacDown.app
com.uranusjr.macdown
```

L'identifiant de l'application MacDown sera donc `com.uranusjr.macdown`.

- Sinon utiliser la commande `osascript` :

```
ludo@ludo-mac ~ osascript -e 'id of app "MacDown"'
com.uranusjr.macdown
```

On retrouve du coup le même identifiant  `com.uranusjr.macdown`.

- `Duti` peut dès lors définir MacDown comme application par défaut pour le fichiers `.md` en utiliser l'identifiant de l'application trouvé plus haut :

```
ludo@ludo-mac ~ duti -s com.uranusjr.macdown .md all
```

Tadaaam!
![Grrrrr](/uploads/osx-extensions-defaut/osx-extensions-defaut-macdown.png "Osx Extensions Defaut Xcode")

Si vous tenez absolument à ré-utiliser XCode, définir simplement l'association par défaut :

```
ludo@ludo-mac ~ duti -s com.apple.dt.Xcode .md all
```
