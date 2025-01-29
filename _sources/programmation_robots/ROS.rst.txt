Installation
============

La version de ROS utilisée est ROS2 Jazzy prévu pour un fonctionnement sur Ubuntu 24.04.

Un guide pour l'installation est disponible dans la documentation officielle de
`ROS2 Jazzy <https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html>`_.

Une fois ROS installé, le dépôt GitHub doit être cloné si cela n'est pas déjà fait.

.. code-block:: bash

   git clone https://github.com/Hermine-HRC/cdfr.git

Ensuite, se rendre dans le dossier :code:`robot/ros_ws/`. Le fichier :code:`README.md` contient les instructions
pour installer les dépendances.

Le fichier contient les packages ROS à installer, la version de Gazebo à utiliser pour les simulations ainsi que
divers utilitaires nécessaires au développement.

La compilation du code peut être effectuée avec la commande suivante :

.. code-block:: bash

   colcon build

.. warning::

   La compilation peut prendre plusieurs minutes.

Pour sourcer le workspace pour utiliser les packages créés :

.. code-block:: bash

   source install/setup.bash

Fonctionnement code
===================

Plusieurs packages ont été créés, chacun ayant un rôle différent, afin de répondre à différents besoins.
Leur usage est détaillé respectivement dans les fichiers :code:`README.md` du dossier correspondant.

Certains packages contiennent du code C++ et Python. Le code C++ est contenu dans les dossiers
:code:`src/` et :code:`include/`. Le code Python quant à lui est contenu dans le dossier du même nom que le package.

Le package :code:`bringup` contient les fichiers de launch regroupant tous les packages.

Les fichiers *.erb*
-------------------

Ces fichiers sont des templates ERB. Ils sont utilisés pour générer les fichiers de données à partir de code en Ruby.
Ils permettent notamment de réaliser des boucles afin d'éviter de dupliquer des informations.
Ils sont convertis en fichiers de données à la compilation suivant les règles données dans le ficher
:code:`CMakeLists.txt`.

Herminebot ou HRC
-----------------

Tous les packages sont préfixés :code:`herminebot_` ou :code:`hrc_`.
Ceux qui commencent par :code:`hrc_` sont des packages qui ont pour vocation d'être utilisés en tant que bibliothèque
alors que ceux qui commencent par :code:`herminebot_` sont des packages qui ont pour vocation d'être utilisés
pour le fonctionnement de l'herminebot.

Fonctionnement tests
====================

Les tests permettent de vérifier que le code est correct et qu'il respecte les conventions de programmation.
Ils sont contenus dans le dossier :code:`test/` de chaque package.

Les frameworks utilisés sont :

- :code:`ament_cmake_pytest` pour le code Python
- :code:`ament_cmake_gtest` pour le code C++

Des linters sont utilisés afin de vérifier que le code respecte les conventions de programmation :

- :code:`flake8` et :code:`pep257` pour le code Python
- :code:`uncrustify` pour le code C++

Les règles sont détaillées dans la partie :doc:`/programmation_robots/Tree-Code_conventions`.

Exécuter les tests
------------------

Pour exécuter les tests, il faut d'abord compiler le code puis exécuter les tests.

.. code-block:: bash

   colcon build
   colcon test

Les tests ne mettent que quelques secondes à exécuter.

Les détails des tests peuvent être affichés avec la commande suivante :

.. code-block:: bash

   colcon test-result --all --verbose

Exécution sur GitHub
--------------------

Les tests sont exécutés sur GitHub, afin de vérifier que le code est correct et qu'il respecte les conventions de
programmation, à chaque *push* sur la branche :code:`master` ou dans une *pull request*.

.. warning::

   Il faut plus de 10 minutes pour exécuter les tests sur GitHub car toute l'installation puis la compilation
   sont réalisées.

Si les tests échouent, il faut vérifier en local si ça échoue aussi et la cas échéant corriger le code.
Si en local les tests passent, c'est parce que les tests échouent parfois de manière intermittente. Dans ce cas,
il faut relancer les tests.
