# **introduction**
les réseaux informatiques sont moyens de faire
passer de la donnée à travers un canale numérique.
Ils font maintenant partie intégrante de notre vie
et il semble donc pertinent de faire un simple guide
de dépanage basique qui permettrait à tout un chacun
d'essayer, par lui même, quelques manipulations
simples de débugage réseau. Nous aborderons ici
les problèmes en se basant sur les couches du
modèle TCP-IP qui est le modèle pratique d'envoi
de message à travers internet. Nous n'aborderons pas
la couche transport car à notre connaissance il
n'existe pas de moyen de débuger cette couche.<br>
par ailleur, nous n'utiliserons ici que l'interface
en ligne de commande et non l'interface graphique
pour des soucis de sobriété et de compréghension.<br>
en effet il est facile de cliquer sur des boutons,
il est moins évident de comprendre les actions effectuées
derrière. pour se faire voici quelques informations utiles.
### **windows**
ouvrez simplement l'invité de commande en tapant
cmd dans la barre de recherche et lançant celle-ci
en mode administrateur ou appuyez
sur les touches Win+R. Une fenêtre nommée executer
devrait souvrir, entrez CMD.
### **sur linux**
ouvrez le terminal via la barre de recherche ou dans
le menu déroulant de votre bureau.<br>
la plus part des commandes que nous allons éffectuer
requierent les droits administrateurs. Pour des
questions de sécurité, il vous est conseillé de
passer en mode root(administrateur) avant l'exécution
de chacune d'elle. puis de quitter le mode root
après la bonne exécution de celle-ci.
Pour passer en mode root utilisez:
```
sudo su
```
pour le quitter faites simplement:
```
exit
```
### **avant de commencer**
voici un tableau de commandes usuelles sur les deux
Système d'exploitation

|linux|windows|
|:----|------:|
| cd | cd |
| ls | dir |
|clear | cls |
| rm | rm |
| mv | mv |

nous rappellerons enfin le modème TCP-IP comme suivant :
|couches|
|:-------:|
|application|
|transport|
|internet|
|physique|

ainsi que la principale architecture d'un réseau:


# **couche physique**<br>

Il est tout d'abord intéressant de se pencher sur l'éventualité d'un problème
très simple: une carte réseau éteinte. 

## **linux**<br>
pour ce faire il existe deux utilitaires sur
les systèmes linux: Ethool et mii-tool. Ces deux utilitaires peuvent être invoqués
très simplement en les nommant 'ethool' pour ethool et mii-tool pour mii-tool
suivit de la carte que nous souhaitons interroger. Si vous êtes connectés en filaire
l'interface pourrait être eno1 ou eth0.
```
mii-tool eno1
```

ou
```
ethool eno1
```
![image](/home/s4uc3/Bureau/SAE12/Screen_mii-tool_eth0.png)<br>
ou
<br>
![image](/home/s4uc3/Bureau/SAE12/screen_ethtool_wlan0.png)<br>
note: dans le premier cas, eth0 n'est pas connecté<br> <br>
Si aucun de ces deux ne fonctionnent il est
possible que le système d'exploitation ne fasse pas la traduction entre le nom original
de la carte et son utilisation. il est donc possible qu'elle utilise un nom alternatif
comme 'enp0s1'. Vous pouvez aussi l'utiliser sur l'interface wlan0 (l'interface wireless
ou wifi)(note: l'observation d'une interface sans fil ne peut se faire qu'avec ethool). 
Vous pourez donc voir si le link est considéré comme OK ou non. C'est à dire
si votre carte est bien connecté ou pas. Si ce n'est pas le cas:
-dans le cas ou vous êtes branchés en filaire (RJ45) vérifiez que le câble est bien
branché à l'arrière de votre PC, qu'il n'y a pas de faux contacts. Si vous avez des
doutes sur la fiabilité de celui-ce et que vous en avez la capacité, demandez à changer
de câble. Si vous y avez accès, allez regader sur la baie de brassage et sur le switch si
le câble qui vous relie au switch est bien branché à celui ci et si sur la prise en question
vous observez que les petites led de part et d'autre de la prise s'allume ou clignote.
si c'est le cas cela veut dire qu'il y a bien de l'information qui circule et que donc le
problème ne vient pas de l'interface physique.
-revérifiez avec Ethool ou mii-tool si le problème est résolut. Si il ne l'est
 pas c'est peut être que votre carte réseau est éteinte. utilisez alors la commande
 ```
 ip link set up dev <le nom de l'interface>
 ```
 Ceci devrait alors avoir pour effet
 d'allumer votre carte. Il va de soi qu'il faut remplacer la zone nom de l'interface
 par l'interface en question (eno1, eth0,wlan0,enp0s1,...). Retournez vérifier la baie
 de brassage. Normalement les diodes doivent s'allumer.

 ![image](/home/s4uc3/Bureau/SAE12/wlan0_down.png)
ici on peut voir que l'interface wlan0 est "down" -> éteinte<br>
pour l'allumer celle ci on utilise
la commande précisement citée:<br>
 ![image](/home/s4uc3/Bureau/SAE12/ip_link_set_up.png)<br>
 et on a donc l'output suivant:
![image](/home/s4uc3/Bureau/SAE12/wlan0_up.png)
on voit ici que la carte est "UP"-> allumée


## **windows**<br>
pour ce qui est de windows, le kernel gère beaucoup plus de choses. Il vient d'ailleurs
avec un panel d'utilitaire qui fournissent déjà des fonctions complètes.
sur windows, la commande "netstat" permet de vérifier dans les trois premières lignes
si le lien est créé et utilisable ou non.
```
netstat
```
si tout est ok un scan devrait suivre des connetion qui ont put être établies
sur le réseau local.<br>
cela vous permettras alors de vérifier par la même occasion que ce n'est pas un problème
d'ip ou de route car si toutes les machines peuvent vous répondres à l'intérieur de votre
réseau local, cela veut dire que le problème ne vient pas de la passerelle qui gèrerait mal
celui-ci.


# **couche réseau<br>**
si la partie sur la couche physique n'a pas résolut votre problème il est possible que le
problème vienne alors de de la couche réseau. il existe pour cela, sur linux un outil multiple
se nomant "ip". Il peut être invoqué avec la simple commande "ip" suivie d'arguments. Tout
d'abord il est important de connaître le réseau sur lequel vous êtes censé êtres. Pour se faire
demandez à votre Administrateur l'adresse ip de votre poste. Pour rappel, une adresse ip est une suite de 4 octets séparés par des points.
les quatres valeurs sont comprises entre 0 et 255 ex: 192.168.1.2, 10.214.213.4,...). Par exemple une
adresse de réseau pourra être 10.213.4.1 ou 192.168.1.6. Vous lui demanderez également le masque de sous
réseau qui permet alors de séparer la partie réseau de la partie hôte d'une adresse IPv4(la partie hôte
est celle qui nous intéressera ici car c'est la seule qui nous seront apte à configurer). Vous obitiendrez
alors une deuxième suite de 4 chiffre 255 ou 0 séparés comme des points. Vous allez alors compter le nombre
de 255 qui se suivent et vous le multipliez par 8 (ex: 255.255.0.0 nous donne 2 qui devient alors 2*8=16).

## **sur linux** <br>
Tout d'abord vous allez vérifier qu'il n'y ai pas de service DHCP qui est lancé sur votre machine. Pour se 
faire vous allez entrer 
```
ps aux | grep dhc 
```
![image](/home/s4uc3/Bureau/SAE12/dhcp_actif.png)<br>
si il n'y a qu'une seule ligne qui s'affiche, il n'y aura pas de
problèmes avec le dhcp car il n'est pas lancé. Vous pouvez donc passer à l'étape suivante<br>
![image](/home/s4uc3/Bureau/SAE12/pas_dhcp.png)

si ce n'est pas le cas, notez le chiffre sous la section PID ######
puis executez la commande 
```
kill <numéro de PID>
```
réexecutez la commande 
```
ps aux | grep dhc
```
et vérifiez que
le processus soit bien arrêté. si ce n'est pas le cas passez à l'étape suivante.

Sinon executez la commande 
```
kill -9 <numéro de PID>
```
et le problème sera réglé, passez à l'étape suivante.

Vous allez ensuite commencer par supprimer toutes les configuration déjà existantes sur l'interface
réseau que vous utilisez. Pour se faire utilisez la commande 
```
ip address flush dev <nom de l'interface>
```
puis 
```
ip route flush dev <nom de l'interface>
```
 Bien maintenant entre simplement ip a (racourcis de ip address)
et vous devriez obtenir quelque chose comme ça : ![image](/home/s4uc3/Bureau/SAE12/ip_a_ip_r_vide.png)
 
Bien, maintenant que la configuration réseau est vierge, nous allons pouvoir l'éditer. Commencer par vous munir de votre adresse IP et de votre valeur octale de masque(8,16,24,32) et entrez 
```
ip a a <adresse ip>/<masque en valeur octale> dev <interface réseau>
```
-ip sert à invoquer l'utilitaire ip
-a (address)sert à préciser que l'on va modifier l'adresse
-a (add) sert à expliquer que l'on va ajouter une nouvelle adresse
-dev (device) sert à préciser sur qu'elle interface on l'ajoute

vérifier avec "ip a" que l'adresse à bien été ajoutée. Si ce n'est pas le cas, recommencez la manipulation
depuis le début.

Vérifiez ensuite votre route. il n'est censé y avoir que 2 routes. dont une avec l'adresse que vous venez
de rentrer####

vous allez donc ajouter une route par défaut. Cette route permettra d'envoyer tous les paquets qui ne
sont pas dans les 2 routes précédentes (la plupart finalement). Il vous faudra alors demander à votre
administrateur l'adresse ip de votre passerelle. Vous pourez alors executer la commande
```
ip r a default via <adresse ip de la passerelle>
```
-ip permet d'invoquer ip<br>
-r (route) précise que la route va être modifiée<br>
-a (add) précise que l'on va ajouter une nouvelle route<br>
-default précise que ce sera une route par defaut<br>
-via précise l'ip a suivre

vérifiez que la route à bien été ajoutée avec 
```
ip r
```
qui affiche vos routes.
Si la route est bien affichée, essayez de vous connecter à internet. Si tout à fonctionné mais que
vous n'arrivez pas à joindre internet il vous reste à faire quelque test.

Vous allez utiliser la commande ping qui permet d'envoyer des requête ICMP à une adresse.
Le protocole ICMP ou Internet Control Message Protocol permet de déceler des problèmes
lors de l'envoi de messages sur internet.

vous allez donc effectuer un ping vers la passerelle. Pour se faire vous allez effectuer
```
ping <ip de la passerelle>
```
et vérifiez que vous reçevez des réponses <br>
![image](/home/s4uc3/Bureau/SAE12/ping_passerelle.png)

si vous n'en reçevez pas c'est que la passerelle est peut être hors services ou que vous avez mal rentré votre ip ou votre route

dans le cas contraire, la passerelle répond: donc c'est qu'elle est en service
essayez alors de ping un camarade avec la commande
```
ping <ip d'une autre machine du réseau> 
```
vérifiez qu'il y ait bien des réponses
si ce n'est pas le cas, l'erreur vient de la passerelle
qui ne parvient pas à faire la liaison dans le réseau local

enfin essayez de joindre le serveur de test de google avec un ping

```
ping 8.8.8.8
```
si vous reçevez des réponses mais que vous n'arrivez pas
à vous connecter via l'url
c'est que le problème viens du DNS. 
Si ce n'est pas le cas, cela veut dire que votre
passerelle ne fais pas le lien entre le réseau LAN
et Internet.

## **windows**<br>
### **statique**
il est possible de résoudre les problèmes de la couche internet avec windows en utilisant l'utiliaire netsh.
vous utiliserez alors la commande
```
netsh interface ip set address "Description" static %adresse% %netmask% %gateway% %metric%
```
-netsh-invoque netsh<br>
-interface-passe en mode interface(paramétrage)<br>
-ip set address-précise que l'on va mettre en place un IP<br>
-"Description" description de l'adresse(nom)<br>
-static-précise que l'ip sera statique (en opposition avec Dynamique obtenue avec un DHCP)<br>
-%addresse% votre adresse IP<br>
-%netmask% masqe de sous réseau en valeur complète (non octale) ex 255.255.0.0 pour /16<br>
-%gateway% IP de la passerelle<br>
-%metric% métric de la carte réseau (généralement 1)<br>

ainsi une ip statique sera ajoutée sur votre carte réseau. La route sera aussi définie du fait
de la spécification de l'adresse ip de la passerelle dans la commande
### **dynamique**
si vous voulez simplement utiliser le DHCP c'est à dire le service qui permet d'attribuer des addresses
vous pouvez utiliser ipconfig.
```
ipconfig /release
```
Cela à pour effet derompre le bail avec le DHCP(serveur) et relache donc l'ip qu'il lui était attribué<br><br>
puis:
```
ipconfig /renew
```
Permettra de rétablir la connexion en créant un nouveau bail avec le DHCP


# **couche application**<br>

## **linux**<br>

vous allez maintenant vérifiez que le DNS
de site que vous essayez de joindre
est correct. 


Pour se faire, utiliser l'utilitaire
dig avec la syntaxe:
```
dig <url>
```
![image](/home/s4uc3/Bureau/SAE12/dig_base.png)
<br>
maintenant observez la zone en ligne 
```
status: 
```
si le status est :
```
NOERROR
```
c'est qu'il n'y a pas de problème et que le
problème viens donc d'ailleur.<br>
![image](/home/s4uc3/Bureau/SAE12/dig_NOERROR.png)

si le status est :
```
SERVFAIL
```
c'est que l'url que vous saisissez est valide mais
qu'il y a un problème dans la résoltion DNS

si le status est :
```
NXDOMAIN
```
cela veut dire que l'url que vous avez entré est invalide
et que il n'y a donc pas de serveur relié au nom
que vous essayez de joindre. Essayez sur un autre
url.<br>
![image](/home/s4uc3/Bureau/SAE12/dig_NXDOMAIN.png)

si le message est:
```
REFUSED
```
c'est que le serveur requière une authentification
et que vous ne l'êtes pas. Essayez de joindre
l'administrateur du site en question et essayez
de trouver une solution avec lui.



## **Windows**<br>
il est possible (comme sur linux) d'utiliser l'utilitaire "nslookup"
afin d'effectuer des tests DNS. nslookup va, comme dig, renvoyer un 
status (NXDOMAIN, NOERROR). Vous pouvez donc tester
```
nslookup google.com
```
si il y a un problème:
-soit vous avez mal rentre l'url
-soit il y a un problème de DNS avec votre machine. Vous pouvez alors utiliser la commande
```
ipconfig /flushdns
```
pour renouveler votre cache DNS qui pourrait être à l'origine de certaines erreurs.


Vous pouvez aussi utiliser
```
ipconfig /registerdns
```
qui renouvèlera alors les baux avec le DHCP et mettra à jour le nom DNS.