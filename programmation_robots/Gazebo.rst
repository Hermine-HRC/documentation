Le monde
========

Toute simulation Gazebo est basée sur un monde. Ce monde contient des objets et des robots. Il est possible de
définir des règles pour les objets et les robots afin de les manipuler.

Le fichier est au format :code:`.sdf`, qui a une syntaxe *XML*. Il contient au minimum les informations suivantes :

.. code-block:: xml

   <sdf version='1.9'>
       <world name='default'>
           <scene>
               <ambient>0.4 0.4 0.4 1</ambient>
               <background>0.6 0.6 0.6 1</background>
               <shadows>false</shadows>
           </scene>
           <physics type='ignored'>
               <max_step_size>0.02</max_step_size>
               <real_time_factor>1</real_time_factor>
               <max_contacts>10</max_contacts>
           </physics>
           <plugin name='gz::sim::systems::Physics' filename='libgz-sim-physics-system.so'/>
           <plugin name='gz::sim::systems::UserCommands' filename='libgz-sim-user-commands-system.so'/>
           <plugin name='gz::sim::systems::SceneBroadcaster' filename='libgz-sim-scene-broadcaster-system.so'/>
           <plugin name='gz::sim::systems::Imu' filename='libgz-sim-imu-system'/>
           <plugin name='gz::sim::systems::Sensors' filename='libgz-sim-sensors-system'>
               <render_engine>ogre2</render_engine>
           </plugin>
           <gravity>0 0 -9.8</gravity>
           <magnetic_field>6e-06 2.3e-05 -4.2e-05</magnetic_field>
           <atmosphere type='adiabatic'/>
       </world>
   </sdf>

Le monde est ensuite composé de plusieurs éléments appelés *models*. Ils peuvent être directement définis dans le world
entre les balises :code:`world`.

.. code-block:: xml

   <model name='box_model'>
       <static>1</static> <!-- 0 pour un model mobile, 1 pour un model statique -->
       <pose>0 0 0.5 0 0 0</pose> <!-- Position du model (x y z rx ry rz) -->
       <link name='link'>
           <visual name='visual'>
               <geometry>
                   <box>
                       <size>1 1 0.1</size>
                   </box>
               </geometry>
               <material> <!-- Couleurs -->
                   <ambient>1 1 1 1</ambient>
                   <diffuse>1 1 1 1</diffuse>
                   <specular>1 1 1 1</specular>
                   <emissive>1 1 1 1</emissive>
               </material>
           </visual>
       </link>
   </model>

Ils peuvent aussi être définis dans un fichier spécifique. Il faut alors utiliser la balise :code:`include` pour
inclure le fichier.

.. code-block:: xml

   <include>
      <name>box_model_name_modified</name>
      <pose>0 0 1 0 0 0</pose> <!-- Position du model (x y z rx ry rz) -->
      <uri>model://box_model</uri>
   </include>

Gazebo peut aussi avoir un fichier de configuration pour définir son environnement. Voici quelques configurations :

.. code-block:: xml

   <plugin filename="MinimalScene" name="3D View">
       <gz-gui>
           <title>3D View</title>
           <property type="bool" key="showTitleBar">false</property>
           <property type="string" key="state">docked</property>
       </gz-gui>

       <engine>ogre2</engine>
       <scene>scene</scene>
       <ambient_light>0.4 0.4 0.4</ambient_light>
       <background_color>0.7 0.7 0.7</background_color>
       <camera_pose>0 -4.5 5.5 0 0.8 1.6</camera_pose>
   </plugin>

   <plugin filename="GzSceneManager" name="Scene Manager">
       <gz-gui>
           <property key="resizable" type="bool">false</property>
           <property key="width" type="double">5</property>
           <property key="height" type="double">5</property>
           <property key="state" type="string">floating</property>
           <property key="showTitleBar" type="bool">false</property>
       </gz-gui>
   </plugin>

Les modèles
===========

Les modèles sont des objets 3D. Ils peuvent être des robots, des objets ou des scènes. Ils sont définis dans un fichier
:code:`.sdf` au format *XML*.

Chaque modèle est composé de plusieurs éléments appelés *links*. Un *link* est un ensemble de *visuals* et de
*collisions*. Entre chaque *link*, il y a un *joint* qui permet de lier les deux *links*.

Si on prend l'exemple d'une jambe, le pied est un *link*, la cheville est un *joint*, le tibia est un *link*, le genou
est un *joint*, le fémur est un *link*, etc.

Les géométries, que ce soit les visuels ou les collision, peuvent provenir d'une forme par défaut comme une *box* ou
un cylindre ou bien d'un modèle 3D importé depuis un autre fichier, *mesh*. Le format du fichier importé est soit
:code:`.stl` soit :code:`.dae`. Le :code:`DAE` à l'avantage de pouvoir utiliser des textures mais à l'inconvénient de
ne pas être supporté par tous les logiciels de modélisation.

Je conseille d'exporter les modèles au format :code:`.stl` depuis le logiciel de modélisation puis d'ouvrir le fichier
dans :code:`Blender`, d'y replacer l'objet par rapport à l'origine puis de l'exporter  en :code:`.stl` si un couleur
unie suffit sinon en :code:`.dae`.

.. tip::

   Cette vidéo montre comment appliquer une texture image à un objet 3D : https://www.youtube.com/watch?v=Rx-aOHCfTOw
