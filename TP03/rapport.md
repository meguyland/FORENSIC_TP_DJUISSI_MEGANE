
# INTRODUCTION

Le site Bosch-cyber a malheureusement été compromis, l'attaquant lors de ses manœuvres 
aurait exfiltré des outils secrets très dangereux. L’administration a donc pris la 
décision de mettre le site sous maintenance. 

Notre tâche désormais sera de découvrir tout ce que l'attaquant aurait pu exfiltrer 
comme outil. Pour cela nous ferrons une analyse des différents logs ce qui nous 
permettra de voir les activités récentes qui ont été effectué. Cela nous permettra de 
mieux savoir où chercher.


# I- CONTEXTE DU PROJET

Le site du réseau Bosch comme relevé plus haut a été compromis et l'attaquant a pu 
révéler des outils secrets très dangereux dans ses manœuvres. Par conséquent, 
l’administration a décidé d’isoler cette machine afin que nous puissions l’analyser et 
découvrir ce que le pirate a exfiltré comme données.

# II- ETUDE DU PERIMETRE

Une étude préalable nous permettra de savoir quelle est cette machine, à quoi sert-elle, son 
adresse IP etc… c’est juste une analyse préalable qui nous permettra de définir les points forts 
sur lesquels nous allons nous pencher.

## 1- Role de la machine

Etant donné que le role de la machine n'a pas été définit dans la consigne, nous avons donc 
cherché tout d'abord à connaitre son role. Notre étude nous révèle que le serveur est en effet 
un serveur web tournant sur APACHE2.

pour se faire, nous avons entré la commande `cd /etc` puis nous avons fait un `ls` pour lister
contenu comme vous pouvez le constater sur l’image ci-dessous 

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/apache.png)

les fichiers et sous dossiers que ce dossier contient

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/apachelist.png)

nous pouvons meme avoir accès au fichier de configuration html de la page web en tapant la commande
`cd /var/www/html` puis `ls` qui nous a permis de lister les fichier et enfin ̀`cat index.html` pour
afficher son contenu. comme vous le voyez sur cette image
 
![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/pagesite.png)

## 2- Adresses IP et Hotes

Pour connaitre les afifférentes adresses des différents hotes de la machine, nous avons accédé au 
fichier hosts qui les contient en entrant les commandes `cat /etc/hosts`qui va aller dans le dossier
**etc** reccuperer le fichier **hosts** et nous l'afficher avec la commande **cat**

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/hosts.png)

Comme on peut le voir nous avons les adresses :

**- 127.0.0.1** : qui est tout simplement l'adresse associée au **localhost**. c'est cette adresse que
l'on utilise lorsqu'on souhaite accéder à la page web du serveur depuis le navigateur

**- 172.18.0.2** : associé à l'hote **bosch-cyber**


# III- VERIFICATIONS

## 1- Les commandes `w`et `who`

Lorsqu’on pense avoir été piraté, la première étape consiste à vérifier que l'intrus n'est pas connecté 
à notre système, nous pouvons y parvenir en utilisant les commandes `w` ou `who`, la première commande`w` 
contenant des informations supplémentaires :

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/w.png)

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/who.png)

nous constatons qu'il n'y a aucun utilisateur actuellement connecté à part nous avec l'adresse **172.18.0.1**

## 2- La commande `last`

Une autre façon de superviser l'activité des utilisateurs est à travers la commande "last" qui permet de 
lire le fichier wtmp qui contient des informations sur l'accès au login, la source du login, l'heure 
du login, avec des fonctionnalités pour améliorer les événements spécifiques du login :

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/last.png)

malheureusement, nous ne pouvons voir que nos différentes connexions a des horaires différentes 

## 3- La commande `history`

Puisque nous suspectons une activité malveillante de la part d'un utilisateur, nous pouvons vérifier 
l'historique de Bash, Puisque notre utilisateur est le seul auquel nous avons accès, nous allons enquêter 
et lancer l'historique des commandes comme dans l'exemple suivant  avec la commande `history` :

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/history.png)

a partir de cette commande nous pouvons observer que les commandes tapées de la première à la 15e ligne
n'ont pas été effectuées par nous. l'attaquant a donc utilisé ce compte pour exfiltrer les outils dont il 
avait besoin.

pour que cela soit plus compréhensible, nous allons passer à une explication des différentes commandes :

- `id`: commande utilisée pour trouver les noms d'utilisateur et de groupe et les ID numériques 
(UID ou ID de groupe)

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/id.png)

- `cat etc/passwd` : Le fichier passwd définit dans /etc/ comprend 7 champs, séparés par le symbole « : ».
qui ne sont rien d'autre que **les nom de connexion**, **numéro d'utilisateur** et **numéro de groupe**

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/passwd.png)

- `pwd`: qui permet de nous situer sur notre chemin

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/pwd.png)

- `ping 138.66.89.12`: cette commande qui je pense permet à l'attaquant surement de faire un ping vers la machine
sur laquelle il compte exfiltrer les outils

- `crontab -e` : qui permettra à l'attaquant d'exécuter automatiquement des scripts

- `zip -r --password $(cat /tmp/mypassword) bosch_cyber_tools.zip /home/b0sch/bosch_cyber_tools`qu'il a utilisé 
pour compresser un fichier, celui qui contenait tous les outils

- `mkdir /opt/leak`: ensuite il crée ce dossier appelé **leack**

- `mv bosch_cyber_tools.zip /opt/leak`: dans le but de déplacer le zip dans le dossier leak qu'il a créé

- `rm /tmp/mypassword`: ensuite il termine avec cette commande qui lui permettra de supprimer le fichier contenant
les password utilisés pour se connecter


pour voir le fichier zippé, il nous suffit d'accéder au dossier leak dans lequel l'attaquant l'a caché

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/leak.png)

# CONCLUSION

cette analyse avait pour but de nous permettre de voir quels étaient les fichiers que l'attaquant avait exfiltrés
malheureusement, nous n'avons pas pu y arriver mais une étude du périmètre puis une vérification des logs nous a 
permis de comprendre ou qu'il a crée un fichier compressé contenant les tools
