# **mise en place d'une communicaiton entre deux réseaux**

dans un premier temps nous réalison les aspects purement physique à savoir connecter avec des connectiques RJ45 les terminaux personnels aux switchs. Il y aura donc 2 switch qui feront le lien entre deux porte de travail et un routeur. Ces deux configurations formeront deux réseaux locaux distincts. Les routeurs seront, eux, reliés au réseaux de la salle via des connectiques RJ45 aussi.<br><br>
une fois les branchements effectués, il nous faut alors configurer les routeurs afin qu'ils puissent échanger. Pour se faire, on branche un PC sur le port 1 du routeur, dit port console. On lance un dhclient puis une fois celui-ci executé, on regarde ne route afin de connaître l'IP du routeur. On lance alors une fenêtre de navigateur et on entre l'adresse ip trouvée. Par se fait, on se connecte à la page d'administration graphique du switch. Il devient alors possible de chnager certains paramètres.
<br> <br>
Il est important de mettre d'abord l'équipement en mode routeur. Ensuite, de préciser que son IP sera statique puis de la préciser. afin d'assurer le bon fonctionnement des échange et la manipulation se réalisant en salle 202 dont l'adresse réseau est 10.202.0.0/16, on attribuera un ip dans cette portée. Le routeur 1 sera donc 10.202.75.254. Le troisième bit est à 75 pour éviter toute superposition et les conflits avec des postes déjà existants.
Le quatrième bit est à 254 car telle est la convention pour les routeur d'utiliser la dernière adresse valide(les adresse ip ayant une portée de 0-255 et 255 étant un adresse de diffusion). 
<br>
on précise ensuite le masque de la salle et sa passerelle(/16 pour le masque comme vue pour l'adresse réseau et 10.202.255.254 pour la passerelle de la salle)
<br> <br>
enfin on cofigure le réseau local en 192.168.13.0/24 qui est donc une adresse de classe C qui nous servira à configurer notre LAN.
<br><br>
on configure donc l'ip de la passerelle côté LAN à la dernière IP valide: 192.121.13.254
ainsi que son masque en valeur totale(non octale).
<br><br>
on a alors la fenètre suivante:![image](/home/s4uc3/Bureau/SAE12/interface_routeur.jpg)<br>
on peut alors mettre en place les ip et les routes des postes de travails dans le LAN en 192.168.13.252 et 192.168.13.253<br>
![image](/home/s4uc3/Bureau/SAE12/ip_a_192.169.jpg)<br><br>
nous allons maintenant réaliser la même chose pour notre deuxième réseau. Brancher le pc, exécuter dhclient, se connecter à la page d'administration et configurer le réseau externe et le LAN. pour ce deuxième réseau, l'ip du routeur sera 10.202.71.254(en suivant les même règles), le LAN sera lui en 172.121.8.0/24.<br><br>

on configure ensuite les route des machines à l'intérieur du LAN pour qu'elles puissent joindre le routeur en paramétrant la route par défaut.![image](/home/s4uc3/Bureau/SAE12/ip_a_172.121.jpg)
<br><br>
il faudra ensuite ajouter une route statique entre les routeurs afin que la communication entre les postes client puisse se faire. Ainsi sur les deux routeurs, on passe en CLI(Command Line Interface) puis on rentre la commande suivante :
```
ip route add dst-address=%ip%/%masque% gateway=%ip%
```
-ip: permet d'invoquer l'utilitaire ip<br>
-route: permet de modifier les route<br>
-add: ajoute<br>
-dst-adress permet de préciser l'adresse de destination<br>
-%ip% précise l'ip que l'on essaye de joindre. Il faut donc préciser l'ip de la machine sur son LAN<br>
-%masque% la masque en valeur octale<br>
-gateway précise la gateway à atteindre pour joindre le LAN de la machine ciblée<br>
-%ip% précise l'ip de la passerelle entre le LAN ciblé et le réseau inter routeurs.<br> <br>
une fois que cette commande est entrée dans les deux sens, il devient alors possible de faire communiquer les deux machines alors qu'elles sont sur des réseaux différents. 
