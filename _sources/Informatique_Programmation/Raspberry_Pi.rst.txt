Introduction
============
Les Raspberry Pi sont des "mini-ordinateurs". Ces derniers peuvent recevoir diffèrents systèmes d'exploitation
d'une déclinaison Linux telle que `Ubuntu 22 <https://releases.ubuntu.com/jammy/>`_ que nous utilisons pour les robots.
Une distribution plus légère est RaspiOS aussi appeler Raspbian.

- :doc:`/Informatique_Programmation/Tree-Ubuntu`
- :doc:`/Informatique_Programmation/Tree-Raspbian`

Ci-dessous une photo d'une Raspberry Pi 3.

.. image:: images/raspi/raspi3.png
	:scale: 40 %
	:align: center

Démmarage
=========

Installation
************
 
Afin d'installer un OS sur une carte SD, vous pouvez utiliser `Raspberry Pi Imager <https://www.raspberrypi.com/software/>`_
qui est un logiciel vous permettant de sélectionner l'OS de votre choix.

.. image:: images/raspi/raspi_imager.png
	:scale: 75 %
	:align: center


Mises à jour
************

Je recommande d'utiliser un terminal pour ces premières étapes.


Commençons par mettre à jour la Pi.

.. code-block:: bash

	cd
	sudo apt update
	sudo apt-get update
	sudo apt upgrade
	sudo apt-get upgrade

Ensuite, il va falloir installer le service de ssh.

.. code-block:: bash

	sudo apt-get install openssh-client
	sudo apt-get install openssh-server
	sudo systemctl enable ssh
	sudo systemctl start ssh
	sudo systemctl status ssh

Vérifions que le service ssh s'est bien installé, vous devriez le voir actif comme ci-dessous.

.. image:: images/raspi/ssh_systemctl.png
   :scale: 75 %
   :align: center




