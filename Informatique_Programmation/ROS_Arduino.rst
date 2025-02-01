.. warning::

	OUTDATED

	Nous n'utilisons pas ros1 car ce dernier ne tourne plus a partir des version superieur a 22.04 de Ubuntu 

Installation et Setup
=====================

Commençons par l'installation d'un outil très pratique pour l'utilisation de ROS dans un seul terminal.

.. code-block:: bash
	
	$ sudo apt-get install terminator

Procédons maintenant a l'installation de la version "noetic" de ROS 1 et des packets arduino.

.. code-block:: bash

	$ sudo apt-get install ros-noetic-rosserial-arduino
	$ sudo apt-get install ros-noetic-rosserial

Installons ensuite les drivers de ROS.
L'instruction <ws> correspond au rerpertoire d'installation de catkin_make

.. code-block:: bash
	
	$ cd <ws>/src
	$ git clone https://github.com/ros-drivers/rosserial.git
	$ cd <ws>
	$ catkin_make
	$ catkin_make install

Il nous reste maintenant à installer la librairie arduino de ROS : ros_lib

.. code-block:: bash

	$ cd <sketchbook>/librairies
	$ sudo rm -rf ros_lib
	$ rosrun rosserial_arduino make_librairies.py

Vous pouvez maintenant lancer votre IDE arduino et vous devriez trouvez la librairie de ros dans Fichier>Exemples>Rosserial Arduino Library

Lancement d'une liaison serie ROS - Arduino
===========================================

Pour cela je vous recommande de lancer terminator pour utiliser plusieurs terminals dans une seul fenetre. En effet ROS occupe beaucoup de terminal et l'utilisation de terminator permet de visualer ces dernier dans une seul fenetre pour surveiller les logs.

Dans un premier terminal lancer la commande suivante :

.. code-block:: bash

	$ roscore

Pour ouvrir la liaison serie avec un permipherique utilisez la commande qui suit :

.. code-block:: bash
	
	$ rosrun rosserial_arduino serial_node.py _port:=/dev/ttyUSB0

ttyUSB0 corrspond aux chemin d'acces de votre peripherique. il existe aussi ttyUSB1, ttyACM0, ttyACM1, etc. Si la liaison serie ne s'ouvre pas je vous recommande d'essayer les autres accèes citer precedement.

La liaison serie est maintenant ouverte, vous pourrez desormais publier des messages aux differents noeud de ROS via vos topic.



