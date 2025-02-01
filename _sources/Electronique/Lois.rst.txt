Introduction
============

En électricité, il existe plusieurs lois fondamentales importantes à connaître pour dimensionner des composants
électroniques.

Loi d'Ohm
=========

Elle dit que la tension aux bornes d'une résistance vaut le produit de sa valeur multiplié par le courant qui
la traverse.

.. code-block:: bash

    U = R * I

Avec U en volts, R en ohms et I en ampères.

.. image:: images/lois/loi_ohm.png
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

Loi des mailles
===============

Elle est l'une des lois de Kirchhoff et dit que la somme des tensions dans une maille est nulle.

.. image:: images/lois/loi_mailles.png
	   :scale: 100 %
	   :align: center
	   :class: with_shadow

Loi des nœuds
=============

Elle est l'une des lois de Kirchhoff et dit que la somme des courants entrants est égal à la somme des courants sortants.

.. image:: images/lois/loi_noeuds.jpeg
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

Pont diviseur de tension
========================

Il permet de réduire le niveau de tension à l'aide de résistances.

.. image:: images/lois/pont_diviseur.png
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

.. warning::

    Le pont diviseur n'est valable que si aucun courant ne traverse les résistances.

Résistances en série
====================

Lorsque des résistances sont mises en série, la résistance équivalente vaut la somme des résistances.

.. image:: images/lois/resistance_serie.png
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

Résistances en parallèle
========================

Lorsque des résistances sont mises en parallèle, la résistance équivalente vaut la somme des inverses des résistances.

.. image:: images/lois/resistance_parallele.png
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

Théorème de Millman
===================

Il permet de déterminer le potentiel à un nœud connaissant le potentiel au bout de chacune des branches connectée
à ce nœud.

.. image:: images/lois/millman.png
	   :scale: 80 %
	   :align: center
	   :class: with_shadow

Puissance
=========

La puissance aux bornes d'un dipôle vaut le produit entre la tension à ses bornes et le courant le traversant.

.. code-block:: bash

    P = U * I

Avec P en watts, U en volts et I en ampères.

Et dans le cas où le dipôle est une résistance, on a, d'après la loi d'Ohm :

.. code-block:: bash

    P = R * I^2

Avec P en watts, R en ohms et I en ampères.

Tension efficace
================

Dans le cas d'une tension alternative, la tension efficace vaut :

.. image:: images/lois/tension_efficace.png
	   :scale: 100 %
	   :align: center
	   :class: with_shadow

Autres théorèmes
================

Il existe d'autres théorèmes dont l'utilisation est moins fréquente mais qui sont importants à connaître.

Le théorème de Norton permet de remplacer un dipôle par un modèle équivalent. Il est mis en relation avec le modèle de
Thévenin.
