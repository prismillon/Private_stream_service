## Guide d'installation d'un serveur de streaming
#
## Prérequis :
- Une machine qui est sous ``Ubuntu server 21.10`` avec un accès root
- Une connection à internet haut débit
- Un controle sur le par feu du router de la machine si il y en a un

## Installation :
Installation du serveur web qui dans notre cas est ``nginx``
```
sudo apt install nginx
```
Installation de la librairie qui permet à nginx d'utiliser le protocole rtmp
```
sudo apt install libnginx-mod-rtmp
```

Il va maintenant falloir toucher a la configuration d'nginx, utilisez votre éditeur de text favoris

```
sudo vim /etc/nginx/nginx.conf
```
Il faudra y inserer la [configuration nginx](./nginx.conf)

#
Preview :
```
[...]
rtmp {
    server {
        listen 1935; # Port rtmp par défaut

        application live {
            live on; # Active la possibilité de live en rtmp

[...]
```
#
Maintenant que nginx est correctement configuré, il faut recharger la configuration pour appliquer les changements

```
sudo systemctl reload nginx.service
```

Ensuite vous avez besoin de deux ports pour faire marcher votre service, le port 80 pour le moment pour le http et un port pour le rtmp qui part défaut est 1935, il va falloir ouvrir ces ports pour pouvoir y acceder depuis l'exterieur.

Pour Ubuntu server si votre firewall est actif il faudra faire la commande 

```
sudo ufw allow 80/tcp
```
pour le port 80 et celui choisi pour le flux rtmp

Enfin il va falloir faire une page web qui va afficher notre stream

voici un exemple possible, attention il faut remplacer par l'ip de votre machine pour acceder au fichier vidéo à afficher
```
sudo vim /var/www/html/index.html
```
voici un [exemple](./index.html) de lecteur vidéo

Une fois tout cela réalisé votre service de streaming personnel est pret a l'emplois, vous pouvez via un logiciel de stream comme OBS envoyer votre flux a l'adresse ``rtmp://votre.ip.ici:port/live`` avec comme clé de stream ``test``

pour plus de précision, ``/live`` est le nom de l'application dans la configuration nginx, et la clé de stream ``test`` est le nom a faire correspondre dans la configuration de la page html, biensur tout cela est à changer pour plus de sécurité.