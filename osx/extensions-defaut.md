<!-- TITLE: Lier une application à une extension -->
<!-- SUBTITLE: Solution la plus rapide trouvée -->

Suite à la mise à jour OS X High Sierra 10.13.3, XCode est devenu l'application par défaut pour ouvrir les fichiers MarkDown (extension `.md`).
Impossible d'utiliser l'application MacDown que j'utilise habituellement: Même en utilisant le bouton droit sur un fichier `.md`, puis  

- Installer l'application *duti* via homebrew :
```
ludo@ludo-mac ~ brew install duti
==> Downloading https://homebrew.bintray.com/bottles/duti-1.5.3.high_sierra.bottle.2.tar.gz
######################################################################## 100.0%
==> Pouring duti-1.5.3.high_sierra.bottle.2.tar.gz
🍺  /usr/local/Cellar/duti/1.5.3: 6 files, 41.7KB
```

- Si l'application est déjà liée
ludo@ludo-mac  ~  osascript -e 'id of app "MacDown"'
com.uranusjr.macdown
 ludo@ludo-mac  ~  duti -s com.uranusjr.macdown .md all
 ludo@ludo-mac  ~  duti -x md
MacDown.app
/Applications/MacDown.app
com.uranusjr.macdown

duti -s com.apple.dt.Xcode .md all