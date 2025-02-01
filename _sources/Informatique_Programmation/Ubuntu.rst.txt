Introduction
============



Hotspot WIFI et IP fixes
========================

Nous allons maintenant configurer un hotspot wifi afin de se connecter sur la Pi en ssh et de permettre aux robots
et aux autres périphérique de communiquer sur un même réseau.

Installons les outils de réseau qui ne sont pas encore présents après la mise à jour.

.. code-block:: bash

	sudo apt install network-manager
	sudo apt install wpasupplicant
	sudo apt install ifupdown

Désactivation de cloud init :

.. code-block:: bash

	sudo bash -c "echo 'network: {config: disabled}' > /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg"

Nous allons maintenant modifier le fichier de configuration du réseau, nous en profiterons pour ajouter des IP fixes
pour faciliter les accès en SSH avec ces dernières.

.. code-block:: bash
	
	sudo nano /etc/netplan/*.yaml

Puis rentrer le texte suivant en prenant soin de modifier "yourssid" et "yourpassword" par le nom du réseau wifi
et le mot de passe que vous voulez.

.. code-block:: bash

	network:
	  version: 2
	  renderer: NetworkManager
	  ethernets:
	    eth0:
	      dhcp4: true
	      optional: true
	      addresses: [192.168.2.10/24]
	  wifis:
	    wlan0:
	      dhcp4: true
	      optional: true
	      addresses: [192.168.1.10/24]
	      access-points:
	        "yourssid":
	          password: "yourpassword"
	          mode: ap
	          networkmanager:
	            passthrough:
	              wifi-security.proto: "rsn" # WPA2 only

Rappel : **ctrl + x** puis **y** pour quitter le fichier

Désactivons l'ipv6 qui ne nous sera pas nécessaire, dans le fichier suivant : 

.. code-block:: bash

	sudo nano /etc/sysctl.conf

Rajouter la ligne suivante à la fin du fichier :

.. code-block:: bash

	net.ipv6.conf.all.disable_ipv6=1

Avant d'appliquer cette configuration nous devons impérativement oublier tous les réseaux wifi sur lequel la Pi
s'est auparavant connectée. En effet, cette dernière ne peut pas émettre de réseau en même temps qu'elle est connectée
à un autre. Nous ne voulons donc pas qu'elle se connecte à un ancien réseau wifi par mégarde.

Une fois les wifi oublié, nous pouvons maintenant appliquer notre configuration et redémarrer.

.. code-block:: bash

	sudo sysctl -p
	sudo netplan generate
	sudo netplan apply
	sudo reboot

Un réseau wifi devrais maintenant être disponible dès que la Pi aura redémarré.

.. image:: images/ubuntu/wifi.png
   :scale: 100 %
   :align: center


Noter que vous pourrez toujours connecter la Pi à un réseau wifi (par exemple pour l'installation de ros2),
mais cette dernière ne pourra pas émettre son réseau et il faudra penser à lui faire oublier
la dernière connexion wifi par sécurité.

Commandes utiles
****************

Quelques commandes utiles concernant le wifi avec le terminal :

Désactivation et réactivation du hotspot.

.. code-block:: bash
	
	nmcli radio wwan off

Lister les wifi disponibles.

.. code-block:: bash

	nmcli dev wifi list

Se connecter à un wifi.

.. code-block:: bash

	sudo nmcli dev wifi connect network-ssid password "network-password"

En remplaçant network-ssid par le nom du wifi présent dans la liste et "network-password" par le mot de passe
(garder les guillemets)

Pour oublier un réseau :
Commencez par trouver le réseau que vous voulez oublier dans la liste avec la commande suivante :

.. code-block:: bash

	nmcli -t -f TYPE,UUID,NAME con 

Vous devriez obtenir un résultat du genre : *802-11-wireless:12345678-31d1-51e7-a60e-3a52e52b4495:YourWifiName*,
copie la suite de chiffres et lettres pour l'ajouter dans la commande ci-dessous

.. code-block:: bash

	sudo nmcli c delete choosedUUID

Exemple :

.. code-block:: bash

	sudo nmcli c delete 12345678-31d1-51e7-a60e-3a52e52b4495


SSH
===

Pour se connecter en SSH il faut utiliser la commande suivante sur votre PC :

.. code-block:: bash

	ssh utilisateur@addressip

À partir de ce que nous avons mis en place, nous avons donc :

**TODO: image missing**

wlan0
*****

Sur l'interface wlan0 après s'être connecté au réseau wifi émit par la Pi, rentrer la commande suivante dans un terminal.

.. code-block:: bash

	ssh crubs@192.168.1.10

eth0
****

Sur l'interface eth0 après avoir connecté un câble éthernet :

Brancher le câble éthernet puis direction le panneau de contrôle Windows (touche Win puis rechercher panel).
*Réseau et Internet > Centre de résau* et *partage > Ethernet > Propriété >* cocher puis doucle cliquer
Protocole Internet version 4 (TCP/IPv4)

.. image:: images/ubuntu/setup_eth0_windows_1.png
   :scale: 25 %
   :align: center

.. image:: images/ubuntu/setup_eth0_windows_2.png
   :scale: 25 %
   :align: center

Renseigner maintenant une adresse IP sur le même réseau. Ici par exemple 192.168.2.5 avec le même masque 255.255.255.0.
Je recommande vivement d'enlever ces changements dès la manipulation finie. En effet, vous risquez d'avoir
de gros problèmes dès que vous vous connecterez à un autre réseau éthernet.

Enfin, vous pouvez retourner dans un terminal pour rentrer la commande suivante.

.. code-block:: bash

	ssh crubs@192.168.2.10

Dépannage
*********

L'erreur ci-dessous vous empêchant de vous connecter en SSH peut subvenir sur votre PC.

.. image:: images/ubuntu/error_ssh.png
   :scale: 75 %
   :align: center

Rentrez simplement la commande suivante puis réessayer la connexion SSH en acceptant le message avec **y**.

.. code-block:: bash

	ssh-keygen -R 192.168.2.10
	ssh-keygen -R 192.168.1.10


Fixer le nom des ports USB
==========================

Afin de piloter le robot, deux cartes Arduino sont utilisées ce qui amène à un problème d'identification
de ces dernières par les codes. En effet au démarrage, le pi attribue un nom de périphérique à chaque appareil
en fonction de la vitesse de démarrage des cartes, ce qui est aléatoire. Nous devons donc faire en sorte
d'attribuer un nom fixe en fonction de l'appareil connecté.

Commencez par débrancher tous les périphériques de la Pi et rallumer la.

Pour tous cette série d'étapes, nous devons passer en super utilisateur.

.. code-block:: bash

	sudo su -

Brancher une première carte puis identifier son nom actuel :

.. code-block:: bash

	ls -l /dev/ttyACM*

**TODO: image missing**

.. image

Une fois le port identifié, ici ttyACM1, nous devons récupérer les données de la carte

.. code-block:: bash

	udevadm info --name=/dev/ttyACM0 --attribute-walk

**TODO: image missing**

.. image de ce qui faut recup

Repérer les premiers idProduct et idVendor à apparaître et notez les.

Toujours en tant que super utilisateur, nous devons maintenant créer une nouvelle règle.

.. code-block:: bash

	cd etc/udev/rules.d/
	sudo nano 10-usb-serial.rules

Ajouter la ligne suivante avec les paramètre idVendor et idProduct obtenus

.. code-block:: bash

	SUBSYSTEM=="tty", ATTRS{idVendor}=="2341", ATTRS{idProduct}=="0042", SYMLINK+="NouveauNom"

**TODO: image missing**

.. image

Vous pouvez appliquer la règle fraîchement créé avec la commande suivante, mais je vous recommande de redémarrer quand-même.

.. code-block:: bash

	sudo udevadm trigger
	sudo reboot

Après redémarrage, vous pouvez vérifier de la manière suivante.

.. code-block:: bash

	ls -l /dev/NouveauNom

Et vous devriez obtenir le résultat suivant.

**TODO: image missing**

.. image

Vous pouvez appliquer de nouveau la même méthode en changeant la carte à nommer et en suivant les étapes précédentes.
Tuto suivis : https://dominoc925.blogspot.com/2019/11/fix-usb-serial-adapters-to-static.html


Autres commandes 
================

Interface graphique
*******************

Désactivation de l'interface graphique :

.. code-block:: bash

	sudo systemctl set-default multi-user.target

Réactivation de l'interface graphique (nécessite la bonne version de l'OS) :

.. code-block:: bash

	sudo systemctl set-default graphical.target


Activation des interfaces des différents bus
============================================

I2C
***

Pour utiliser de l'I2C sur Ubuntu, commencez par vérifier si il n'est pas déjà activé.

.. code-block:: bash

	sudo raspi-config nonint get_i2c

Si ça renvoie un 1 il n'est pas activé, il faut alors utiliser :

.. code-block:: bash

	sudo raspi-config nonint do_i2c 0

Ensuite, nous allons vérifier si nous avons les permissions :

.. code-block:: bash

	ls -l /dev/i2c*

Voilà un exemple de sortie possible :

.. code-block:: bash

	crw------- 1 root dialout 89, 1 Sep 19 18:59 /dev/i2c-1

Dans ce cas, il y a 2 groupes qui ont la permission d'utiliser l'I2C il peut donc s'ajouter au groupe dialout avec :

.. code-block:: bash

	sudo usermod -aG dialout crubs

En remplacent *crubs* par le nom de la session.

Nous pouvons maintenant installer un outil permettant de détecter les adresses des composants présents sur le bus.

.. code-block:: bash

	sudo apt install i2c-tools
	
Vous pouvez maintenant utiliser la commande suivante pour lancer la recherche de composants présents sur le bus.

.. code-block:: bash

	i2cdetect -1 y

SPI
***


CAN
***





















