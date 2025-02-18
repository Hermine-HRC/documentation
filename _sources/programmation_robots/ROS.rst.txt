Installation
============

La version de ROS utilisée est `ROS2 Jazzy <https://docs.ros.org/en/jazzy/index.html>`_ prévu pour un fonctionnement
sur Ubuntu 24.04. Une grande partie du code utilise et est inspiré par `Nav2 <https://docs.nav2.org>`_.

Un guide pest disponible dans la documentation officielle de
`ROS2 Jazzy pour l'installation <https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html>`_.

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

Il faut sourcer le workspace pour utiliser les packages créés à l'aide de la commande suivante :

.. code-block:: bash

   source install/setup.bash

Fonctionnement code
===================

Plusieurs packages ont été créés, chacun ayant un rôle différent, afin de répondre à différents besoins.
Leur usage est détaillé respectivement dans les fichiers :code:`README.md` du dossier correspondant.

Certains packages contiennent du code C++ et Python. Le code C++ est contenu dans les dossiers
:code:`src/` et :code:`include/`. Le code Python quant à lui est contenu dans le dossier du même nom que le package.

.. important::

   Dans les packages :code:`cmake` (avec un fichier :code:`CMakeLists.txt`), les exécutables Python (fichiers
   avec un :code:`main`) doivent avoir un shebang :code:`#!/usr/bin/env python3` en haut du fichier.

Le package :code:`bringup` contient les fichiers de launch regroupant tous les packages.

.. tip::

   Pour avoir une coloration des logs, exécuter :code:`export RCUTILS_COLORIZED_OUTPUT=1` dans le terminal avant de
   lancer le launch.

Les nœuds peuvent tous être configurés via un fichier de configuration au format YAML. Par conséquent, il est préférable
d'utiliser un fichier de configuration pour les paramètres de nœuds plutôt que mettre ces paramètres en brut dans le
code.

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

Des *linters*, logiciels qui vérifient la forme du code, sont utilisés afin de vérifier que le code respecte les
conventions de programmation :

- :code:`flake8` et :code:`pep257` pour le code Python
- :code:`uncrustify` pour le code C++

Les règles sont détaillées dans la partie :doc:`/Informatique_Programmation/Tree-Code_conventions`.

Exécuter les tests
------------------

Pour exécuter les tests, il faut d'abord compiler le code puis exécuter les tests.

.. code-block:: bash

   colcon build
   colcon test

Les tests ne mettent que quelques secondes à s'exécuter.

Les détails des tests peuvent être affichés avec la commande suivante :

.. code-block:: bash

   colcon test-result --all --verbose

.. note::

   Pour toutes les erreurs de linter, il suffit de corriger de la façon qui est proposée (sauf cas exceptionnel).

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

Une fois que les tests sont passés, une couverture de code est générée. Le rapport est accessible sur le site
`codecov <https://app.codecov.io/github/Hermine-HRC/cdfr>`_ ou en cliquant sur le badge :code:`codecov`
dans le *README*.

Tests à ajouter
---------------

Autant que possible, il faut ajouter des tests afin de vérifier que le code fonctionne correctement.
Soit avec le framework :code:`gtest` pour les tests C++ ou avec :code:`pytest` pour les tests Python.

À ça s'ajoute aussi des tests pour vérifier que le code est conforme aux conventions de programmation.

C++
***

test_foo.cpp

.. code-block:: cpp

   #include <gtest/gtest.h>
   #include "foo.hpp"

   class Tester : public ::testing::Test
   {
   public:
       Tester() {foo_ = std::make_shared<Foo>();}
   protected:
       std::shared_ptr<Foo> foo_;
   };

   TEST(Tester, test_foo)
   {
       ASSERT_EQ(foo_->baz(), 1);
   }

   int main(int argc, char** argv)
   {
       // Initialize the system
       testing::InitGoogleTest(&argc, argv);
       rclcpp::init(argc, argv);
       // Actual testing
       bool test_result = RUN_ALL_TESTS();
       // Shutdown
       rclcpp::shutdown();
       return test_result;
   }

test_uncrustify.py

.. code-block:: python

   from ament_index_python.packages import get_package_share_directory
   from ament_uncrustify.main import main
   import os
   import pytest

   @pytest.mark.linter
   def test_uncrustify():
       cfg_file = os.path.join(get_package_share_directory("herminebot_bringup"), "config", "ament_code_style.cfg")
       rc = main(argv=[f"-c{cfg_file}"])
       assert rc == 0, "Found uncrustify errors"


CMakeLists.txt

.. code-block:: cmake

   ament_add_gtest(test_foo test_foo.cpp)
   target_link_libraries(test_foo
       foo_lib
   )

   set(python_tests
       test_uncrustify.py
   )
   foreach(python_test ${python_tests})
       string(REPLACE "/" "_" python_test_name ${python_test})
       ament_add_pytest_test(${python_test_name} ${python_test}
           APPEND_ENV PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}
           TIMEOUT 60
           WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
       )
   endforeach()

Python
******

test_foo.py

.. code-block:: python

   import foo
   import pytest
   import rclpy


   def test_demo():
       rclpy.init()
       try:
           node = foo.Foo()
           assert node.baz() == 1
       finally:
           rclpy.shutdown()

   if __name__ == '__main__':
      pytest.main(['-v'])

test_flake8.py

.. code-block:: python

   from ament_flake8.main import main_with_errors
   import pytest


   @pytest.mark.flake8
   @pytest.mark.linter
   def test_flake8():
       rc, errors = main_with_errors(argv=['--linelength=120', '--exclude=$pkg_name$/__init__.py'])
       assert rc == 0, \
           'Found %d code style errors / warnings:\n' % len(errors) + \
           '\n'.join(errors)

test_pep257.py

.. code-block:: python

   from ament_pep257.main import main
   import pytest


   @pytest.mark.linter
   @pytest.mark.pep257
   def test_pep257():
       rc = main(argv=['.', 'test'])
       assert rc == 0, 'Found code style errors / warnings'

.. important::

   Si le code se situe dans un package cmake, il faut modifier le fichier :code:`CMakeLists.txt` pour ajouter les tests.

   .. code-block:: cmake

      set(python_tests
          test_foo.py
          test_flake8.py
          test_pep257.py
      )
      foreach(python_test ${python_tests})
         ament_add_pytest_test(${python_test} ${python_test}
             APPEND_ENV PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}
             TIMEOUT 60
             WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
         )
      endforeach()

Créer des plugins Nav2
======================

Nav2 est la base du projet. La stack est utilisée pour permettre au robot de se déplacer de manière autonome.
Des plugins ont été écrits et continueront d'être écrits afin de s'adapter au cas d'utilisation.

Dans les sous parties suivantes est décrit le cas d'utilisation des principaux plugins qui devraient être écrits
pour répondre aux besoins. Pour les autres, se rendre dans la documentation officielle de Nav2.

Ceux plugins qui vont être cités sont des `actions dans le sens de ROS
<https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html>`_.

Navigateur
----------

Le navigateur est un plugin de Nav2 qui permet de faire interface entre une action et un arbre de comportement.
Un arbre de comportement  définit les différentes actions possibles et leur priorités afin de réaliser une action
plus globale. Par exemple, c'est un arbre de comportement qui permet au robot de se déplacer en générant via un planner
dans un premier temps le parcours à parcourir puis fait suivre le parcours via un contrôleur.

Un navigateur est un plugin qui est géré par le nœud :code:`bt_navigator`.

Pour créer un nouveau navigateur, suivre le
`tutoriel de Nav2 sur les navigateurs <https://docs.nav2.org/plugin_tutorials/docs/writing_new_navigator_plugin.html>`_.

Plugin d'arbre de comportement
------------------------------

Un plugin d'arbre de comportement est un plugin qui fait le lien entre un composant d'arbre de comportement et une
action. C'est notamment le plugin qui va lancer l'action de génération de chemin.

Pour créer un nouveau plugin d'arbre de comportement, suivre le `tutoriel de Nav2 sur les arbres de comportement
<https://docs.nav2.org/plugin_tutorials/docs/writing_new_bt_plugin.html>`_.

Comportement
------------

Un comportement est un plugin qui fait va exécuter une action. Il peut être appelé depuis un arbre de comportement ou
depuis un appel d'action. La génération de chemin est un exemple d'action qui est exécuté par un comportement.

Un comportement est géré par le nœud :code:`behavior_server`.

Pour créer un nouveau comportement, suivre le
`tutoriel de Nav2 sur les comportements <https://docs.nav2.org/plugin_tutorials/docs/writing_new_behavior_plugin.html>`_.

Écrire de nouveaux messages
===========================

Les messages sont des structures de données qui sont utilisés pour communiquer entre les nœuds. Ils sont de 3 types :

- actions
- services
- messages

Pour plus d'informations, se rendre dans la documentation officielle de ROS2 qui explique le `concept des interfaces
<https://docs.ros.org/en/jazzy/Concepts/Basic/About-Interfaces.html>`_.

Dans le projet, les messages créés le sont dans le package :code:`hrc_interfaces`.

Pour créer un nouveau message, se référer au `tutoriel sur les messages
<https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html>`_ de ROS2.
