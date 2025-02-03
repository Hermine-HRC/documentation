Principe
========

*GitHub* est un service de gestion de code en ligne. Il permet de gérer le code source de l'équipe et de le partager
avec les autres membres de l'équipe.

Il est possible de cloner le code source sur son propre ordinateur afin de travailler sur le code.

Une fois le code cloné, il est possible de faire des modifications et les soumettre au dépôt GitHub.

Les modifications sont effectuées en local via *Git* en *commit* et les *commits* sont stockés dans un *repository*.

Un *repository* est un ensemble de *commits* et de fichiers.

Les *commits* sont identifiés par un *hash* qui permet de les retrouver facilement.

Le *hash* est généré automatiquement lors de la création d'un *commit*.

Les *commits* sont affectés à une *branch* afin de pouvoir travailler sur plusieurs branches simultanément.

Une fois que le code est prêt, il est possible de *push* le *commit* et/ou la branche sur le dépôt GitHub afin de le
partager avec les autres.

Pour fusionner les modifications, il faut faire une *pull request* sur le dépôt GitHub.

.. note::

   La branche :code:`master` est la branche par défaut et elle est protégée. C'est-à-dire que les modifications ne
   peuvent pas être faites sur cette branche. Il faut procéder à une *pull request* afin de fusionner les
   modifications.

Exemple d'utilisation
=====================

Voici un exemple d'utilisation pour travailler sur le code source. Ici, les étapes sont détaillées en ligne de commande.
Il est cependant possible d'utiliser un logiciel de gestion de code comme *GitHub Desktop* ou des IDE comme
*Visual Studio Code* afin de réaliser ces étapes.

.. warning::

   Pour pouvoir réaliser toutes ces étapes, il faut avoir été ajouté en tant que *collaborateur* sur le dépôt GitHub.
   Pour cela, se référer à l'administrateur en lui donnant votre pseudo GitHub.

.. warning::

   **En ligne de commande**, Git va demander de se connecter à GtiHub. Rentrer le nom d'utilisateur et le mot de passe
   qui est un *Personal Access Token* créé sur le dépôt GitHub. Sur GitHub, se rendre dans :code:`Settings -> Developer
   settings -> Personal access tokens -> Generate new token`. Puis coller le token dans la ligne de commande.


1. Cloner le code source sur son propre ordinateur

.. code-block:: bash

   git clone https://github.com/Hermine-HRC/cdfr.git

2. Créer une branche de travail

.. code-block:: bash

   git switch -c my_branch

3. Faire des modifications dans le code

4. Ajouter les modifications à *Git*

.. code-block:: bash

   git add . # '.' signifie tous les fichiers dans le dossier courant et les sous-dossiers

5. Commiter les modifications et écrire un message dans un éditeur de texte

.. code-block:: bash

   git commit

6. Pousser les modifications sur le dépôt GitHub

.. code-block:: bash

   git push

7. Faire une *pull request* sur le dépôt GitHub

   - Se connecter sur le dépôt GitHub
   - Cliquer sur le bouton *Compare & pull request*
   - Sélectionner la branche de base
   - Cliquer sur *Create pull request*

8. Fusionner la *pull request*

   Depuis la *pull request*, précédemment créée, si c'est indiqué qu'elle peut être automatique fusionnée,
   cliquer sur *Merge pull request*. Sinon, cela signifie qu'il y'a des conflits pour fusionner les branches. Dans ce
   cas, il faut procéder à un *rebase*.

9. Récupérer les modifications sur la branche sur laquelle la fusion a été effectuée (ici :code:`master`)

.. code-block:: bash

   git switch master
   git pull --ff-only # Le paramètre '--ff-only' permet de s'assurer qu'il n'y a pas de divergence avec le code local

GitHub Actions
==============

Les GitHub Actions sont des outils qui permettent notamment de gérer les tests et le déploiement automatiquement.
Ils gèrent notamment l'exécution des tests, ROS et la mise en ligne de cette documentation.

Ils sont définis dans les fichiers au format :code:`yml` du dossier :code:`.github/workflows/`.

Commandes Git utiles
====================

.. code-block:: bash

   git status # Affiche les fichiers non commités
   git log # Affiche les commits
   git diff # Affiche les différences entre le dernier commit et le commit courant
   git diff --staged # Affiche les différences entre le dernier commit et le commit courant pour les fichiers commités
   git add . # Ajoute tous les fichiers dans le dossier courant et les sous-dossiers
   git commit # Crée un commit
   git commit --amend # Fusionne le dernier commit avec les modifications non commitées
   git push # Envoie les modifications sur le dépôt GitHub
   git push --force-with-lease # Force le push (utile quand l'historique local est modifié par rapport à celui sur GitHub) À ÉVITER car ça casse l'historique
   git pull # Met à jour le dépôt local
   git checkout <branche> # Change le dépôt local pour la branche spécifiée
   git switch <branche> # Change le dépôt local pour la branche spécifiée
   git switch -c <branche> # Change le dépôt local pour la branche spécifiée et crée une nouvelle branche
   git branch # Affiche les branches
   git branch -d <branche> # Supprime la branche spécifiée
   git restore . # Restaure tous les fichiers dans le dossier courant et les sous-dossiers
   git stash # Sauvegarde les modifications non commitées
   git stash pop # Restaure les modifications non commitées
   git blame <fichier> # Affiche quel commit a modifier quelles lignes dans un fichier

Pour *rebase* une branche, la fonction :code:`grb` peut être utilisée.

.. code-block:: bash

   # Get the current active branch
   get_branch () {
       set -e #Stop on any errors

       # Get the current branch
       current_branch=$(git symbolic-ref -q HEAD)
       current_branch=${current_branch##refs/heads/}
       current_branch=${current_branch:-HEAD}

       echo $current_branch
   }

   # Branch rebasing on master
   grb () {
       if [ "$#" == 0 ];
       then
           read -r current_branch <<< $(get_branch)
       elif [ "$#" == 1 ];
       then
          current_branch=$1
       else
          echo "function requires 0 or 1 argument"
          return 1
       fi

       main_branch=master

       git switch $main_branch
       if [[ ! $(git pull --ff-only) ]]; then
         echo "Pull of master failed"
         return 1
       fi
       git switch $current_branch

       git rebase $main_branch
   }

Quelques commandes de configuration :

.. code-block:: bash

   git config --local user.name "Hermine"
   git config --local user.email "hermine.hrc@gmail.com"
   git config --local core.editor vim # définir l'éditeur de texte
   git config --local color.ui true # définir si les couleurs sont activées

.. note::

   L'argument :code:`--local` définit les options pour le dépôt local. Remplacer par :code:`--global` pour modifier dans
   tous les dépôts.

.. note::

   Pour voir la valeur affectée à une option, il suffit de ne pas affecter de valeur.
   Par exemple :code:`git config --local user.name` affichera la valeur de l'option :code:`user.name`.
   Ou encore, pour afficher toutes les options :code:`git config --local --list`.
