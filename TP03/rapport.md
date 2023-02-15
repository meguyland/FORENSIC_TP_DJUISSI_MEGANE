
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

## 2- Adresses IP

Pour connaitre les afifférentes adresses des différents hotes de la machine, nous avons accédé au 
fichier hosts qui les contient en entrant les commandes `cat /etc/hosts`qui va aller dans le dossier
**etc** reccuperer le fichier **hosts** et nous l'afficher avec la commande **cat**

![alt text](https://github.com/meguyland/FORENSIC_TP_DJUISSI_MEGANE/blob/main/TP03/img/hosts.png)

Comme on peut le voir nous avons les adresses :

**-127.0.0.1** : qui est tout simplement l'adresse associée au **localhost**. c'est cette adresse que
l'on utilise lorsqu'on souhaite accéder à la page web du serveur depuis le navigateur

**-172.18.0.2** : associé à l'hote **bosch-cyber**

