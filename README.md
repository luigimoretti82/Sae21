# **Journal de bord Sae21, Luigi MORETTI**

* **24 et 25 mars -> Découverte du sujet et répartition des tâches**

Je dois m'occuper de la partie virtuelle sur GNS3.

*  **08/04 -> Configuration DHCP + Nat partie virtuelle:**

J'ai configuré le DHCP et le NAT sur le routeur du réseau.
Pour cela il suffit d'entrer les commandes suivantes dans le routeur.

-conf t

-interface fastEthernet 0/0

-ip address 192.168.1.254 255.255.255.0

-no sh

-no shutdown

-exit

-ip dhcp pool toto

-network 192.168.1.0 255.255.255.0

-default-router 192.168.1.254

-dns-server 192.168.1.254

-class tata

-address range 192.168.1.2 192.168.1.252

Ensuite, on peut attribué une configuration a chaque pc automatiquement en entrant la commande dhclient sur chaque pc.

Maintenant on configure le NAT sur le routeur, pour cela on entre les commandes suivantes:

-conf t

-interface fastEthernet 0/0

-ip nat inside

-exit

-interface fastEthernet 0/1

-ip nat outside

-exit

-access-list 1 permit 192.168.1.0 0.0.0.255

-ip nat pool MY_POOL 10.213.0.0 10.213.255.254 netmask 255.255.0.0

-ip nat inside source list 1 pool MY_POOL

-exit

-write memory


* **15/04 -> Début configuration DNS (partie physique)**

Après réorganisation du groupe je dois maintenant m'occuper de la partie DNS et du serveur WEB pour la partie physique du projet.

* **21/04 -> Suite configuration DNS + serveur Web**

Tout d'abord on commence par installer bind9 avec la commande : apt install bind9 dnsutils.<br>
Pour lancer/arrêter le service il suffit de faire : <br>
systemctl start/stop bind9 <br>

Pour connaître l’état du service : <br>
systemctl status bind9 <br>

Après chaque modification il faut relancer le serveur avec :<br>
systemctl restart bind9<br>
 
**Après avoir installer bind9, je m'occupe de la configuration des fichiers suivant pour le DNS :**

* etc/bind/named.conf.local

On accède a ce fichier en entrant nano /etc/bind/named.conf.local. <br>
Grâce à ce fichier, on créé notre zone ici donc, DockerCoorporation.fr qui est le nom de notre entreprise. <br>
De plus ce fichier permet de déterminer si notre serveur DNS doit être master ou slave, on finit ensuite par indiquer où trouver le fichier de configuration de notre zone.<br>

![img_reseau](named_conf_localv1.png)

* etc/bind/named.conf.options

Après avoir configurer le fichier précédent on passe désormais au fichier named.conf.options, pour y accèder on tappe nano /etc/bind/named.conf.options. <br>
Ici on va simplement changer le forwarders en entrant 8.8.8.8. <br>
Grâce a cela la résolution des noms symboliques va d'abord passer par notre DNS et si elle n'y arrive pas elle sera redirigée vers le résolver de google qui est un résolver public, comme il est indiqué dans le cahier des charges.

![img_reseau](named_conf_optionsv1.png)

* etc/bind/db.DockerCoorporation.fr

Enfin, je finis par configurer le fichier db.DockerCoorporation.fr créé précédemment. <br>
Il est accèssible avec, nano /etc/bind/db.DockerCoorporation.fr <br>


![img_reseau](db_dockercoorporationv1.png)
