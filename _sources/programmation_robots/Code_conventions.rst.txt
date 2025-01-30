Messages de commit et PR
========================

Les messages de commit sont la justification de la modification. Ils doivent être clairs et précis. Il est peu utile
de dire ce qui est modifié, ça se voit facilement dans le code. En revanche, on ne voit pas le pourquoi de la
modification. C'est ici tout l'intérêt du message de commit.

Ils doivent respecter les conventions suivantes:

- Ils doivent être en anglais
- Ils doivent commencer par une phrase courte résumant les modifications
- La phrase courte doit commencer par un verbe et doit être suivie d'une ligne vide
- La suite du message doit expliqués pourquoi les modifications ont été faites. Il ne faut pas hésiter à détailler
- Dans le cas d'un commit pour résoudre un problème, il faut préciser le problème et le résultat ainsi que les étapes à suivre pour reproduire le problème

Exemples :

.. code-block::

   implement a publisher in the head node to publish the score

   This allows to display the score in the screen managed by another node.

.. code-block::

   fix robot stuck after stop by collision monitor

   Problem: Once the robot is stopped by the collision monitor, it can't move again.
   Steps to reproduce:
   - Launch the bringup
   - Add an obstacle
   - Make the robot to move towards the obstacle
   - The robot stops near the obstacle
   - Make the robot to go in the opposite direction
   - The robot is stuck
   Cause: The collision monitor is not correctly configured

Le message de *pull request*, lui, résume les modifications apportées par les différents commits qui la constituent.
Tout comme le message de commit, il doit exprimer pourquoi les modifications ont été apportées.

Langages de programmation
=========================

Les langages de programmation ont leurs propres conventions adaptées à leur spécificité. Les conventions ne sont pas
une règle à toute épreuve et peuvent être adaptées à la situation.

Commun
------

- Tout le code est en anglais
- Les noms de classes sont en :code:`UpperCamelCase`
- Les noms des variables et attributs sont en :code:`snake_case`
- Les noms des fichiers sont en :code:`snake_case` (exceptions pour certains fichiers où la convention est autre)
- Les variables globales sont en :code:`ALL_CAPS`
- Les tabulations sont de 4 espaces (exception des packages copiés de nav2 ou elles sont de 2 espaces)
- Les fonctions et méthodes sont documentées avec des docstrings
- Tous les opérateurs arithmétiques et d'assignation sont entourés d'espaces (ex : :code:`x = x + 1`)
- Il est inutile de mettre des commentaires à tout bout de champ car ça alourdit le code. Ils doivent être utilisés de manière pertinente
- Les lignes de code sont limitées à 120 caractères (exception pour les packages copiés de nav2 où elles sont de 100)

.. tip::

   Dans VSCode, il est possible d'ajouter une ligne pour indiquer la limite.
   Dans :code:`.vscode/settings.json`, ajouter la ligne suivante : :code:`"editor.rulers": [120]`


C++
---

Les conventions du C++ sont gérées dans un `fichier de configuration
<https://github.com/Hermine-HRC/cdfr/blob/master/robot/ros_ws/src/herminebot_bringup/config/ament_code_style.cfg>`_.

Voici une liste non exhaustive des règles (en complément des conventions communes à tous les langages) :

- Les noms des fonctions et des méthodes sont en :code:`lowerCamelCase`
- Les fonctions et méthodes ont une docstring de la forme ci-dessous et sont placées dans le header (s'il existe) :

.. code-block:: cpp

   /**
    * @brief Calculate the sum of two numbers
    *
    * @param bar First operand
    * @param baz Second operand
    * @return The sum of the two operands
    */
   double foo(int bar, double baz)
   {
       return bar + baz;
   }

- Aucune ligne ne doit être terminée par un espace

.. tip::

   Dans VSCode, il est possible d'afficher les espaces.
   Dans :code:`.vscode/settings.json`, ajouter la ligne suivante : :code:`"editor.renderWhitespace": "all"`

- Les attributs de classe sont en :code:`snake_case` et se terminent par un underscore (ex: :code:`my_attribute_`)
- Les accolades des fonctions et méthodes sont placées à la ligne suivante (voir exemple de *foo* au dessus)
- Les accolades des conditions et boucles sont placées à la suite (sauf si la condition est placée sur plusieurs lignes)
- Les *main* sont placées dans un fichier séparé de la définition de la classe

Python
------

Les conventions du Python sont celles de la `PEP 8 <https://www.python.org/dev/peps/pep-0008/>`_, à quelques
exceptions près.
Voici une liste non exhaustive des règles (en complément des conventions communes à tous les langages) :

- Les noms des fonctions et des méthodes sont en :code:`snake_case`
- Les imports sont sur des lignes distinctes et triés par ordre alphabétique
- Les strings sont entourées de guillemets simples
- Les docstrings sont de la forme suivante :

.. code-block:: python

   """
   Calculate the sum of two numbers

   :param bar: First operand
   :param baz: Second operand
   :return: The sum of the two operands
   """
   def foo(bar: float | int, baz: float | int) -> float | int:
      return bar + baz

- Les fonctions et méthodes indiquent, autant que possible, le type des arguments et le type de retour (voir exemple de *foo* au dessus)
