[//]: # (Image References)

[image1]: ./assets/gazebo_1.png "Gazebo"
[image2]: ./assets/gazebo_2.png "Gazebo"
[image3]: ./assets/gazebo_3.png "Gazebo"
[image4]: ./assets/gazebo_4.png "Gazebo"
[image5]: ./assets/gazebo_5.png "Gazebo"
[image6]: ./assets/gazebo_6.png "Gazebo"
[image7]: ./assets/gazebo_7.png "Gazebo"
[image8]: ./assets/gazebo_8.png "Gazebo"
[image9]: ./assets/gazebo_9.png "Gazebo" 
[image10]: ./assets/gazebo_10.png "Gazebo" 
[image11]: ./assets/gazebo_11.png "Gazebo" 
[image12]: ./assets/gazebo_12.png "Gazebo" 
[image13]: ./assets/gazebo_13.png "Gazebo" 
[image14]: ./assets/gazebo_14.png "Gazebo" 


# 3. hét - Gazebo és URDF


# Gazebo

A Gazebo egy önálló fizikai szimulációs környezet, nem a ROS része, azonban rengeteg csomag segíti az integrációját a ROS-hoz. Jelenleg a 11-es verziónál tart. Melyik verziót használjuk?

| ROS     | Gazebo |
|---------|--------|
| Melodic | 9.0    |
| Noetic  | 11.1   |

Ha nincs még telepítve akkor a `sudo apt install gazebo9` vagy `gazebo11` paranccsal tudjuk telepíteni.  
A Gazebo-t a `gazebo` paranccsal tudjuk elindítani. És a következő képernyővel indul:  
![alt text][image1]

Hivatalos Gazebo tutorialok: [link](http://gazebosim.org/tutorials).

A `gazebo` parancs a `gzserver` és a `gzclient`-et foglalja össze. A `gzserver` a gazebo backendje, ez végzi a fizikai szimulációt, viszont nincs grafikus felülete. A `gzclient` adja a Gazebo grafikus felületét.

## Gazebo UI
A Gazebo-t megnyitva a következő képernyőt látjuk:
![alt text][image2]

Részletes angol tutorial a UI-ról: [link](http://gazebosim.org/tutorials?cat=guided_b&tut=guided_b2)

A bal oldali panel (zöld) mutatja a szimulációban lévő objektumok hierarchiáját.
Fent (piros) az eszköztár, mozgatás, forgatás, kamera beállítások, stb.
Alul (barna) a szimuláció megállítása, elindítása, real-time faktor és időbélyegek.
Középen (kék) a szimuláció 3D-s világa.

## Egyszerű modellek, modell library

Egyszerű gyakorlatok:
- helyezzünk egy kockát a szimulációba
- állítsuk meg a fizikai szimulációt, emeljük fel és forgassuk el a kockát
- engedélyezzük újra a szimulációt, hagyjuk leesni a kockát
- adjunk adott nagyságú erőt a kockára

![alt text][image3]
![alt text][image4]

## Model editor

Először hozzunk létre egy ROS csomagot a leckének:  
`catkin_create_pkg bme_gazebo_basics roscpp rospy std_msgs actionlib actionlib_msgs`

Ide fogjuk menteni a létrehozott fájlokat.

![alt text][image6]

![alt text][image5]

Mentsük el a ~/catkin_ws/src/bme_gazebo_basics/urdf alá simple model néven.

## Building editor

Nyissuk meg a building editort.

Adjuk hozzá a foorplant a ~/catkin_ws/src/bme_gazebo_basics/worlds/floorplan.png fájlból az import gomb segítségével.




![alt text][image7]
Állítsuk be a fal hosszakat a next gomb megnyomása után.

![alt text][image8]

Húzzuk be a falakat a floorplan alapján. Érdemes figyelni arra, hogy "záródjanak" a fal kontúrjai.

![alt text][image9]

Ajtókat és ablakokat hozzá tudunk adni a window és door toolokkal, viszont praktikus szempontból az ajtókat nem ajánlom, mert ha térképet akartok generálni az épületről, akkor az ajtókat és ablakokat falként fogja kezelni.

![alt text][image10]

Elmenthetjük az épületet sdf fájlként, és visszatérhetünk a Gazebohoz. Fontos, hogy mentés után többet az épület nem szerkeszthető!

![alt text][image11]

Opcionális: tehetünk bele pár objektumot a gazebo model libraryből:

![alt text][image12]

Mentsük el a gazebo világot world_modified.world néven, ezt fogjuk betölteni Gazebo-ba a szimuláció során.

Ki is tudjuk próbálni:  
gazebo world_modified.world





## Launchfile

Készítsünk egy launchfile-t, ami megnyitja a Gazebot és betölti az elmentett világunkat.

```xml
<?xml version="1.0" encoding="UTF-8"?>

<launch>

  <!-- World File -->
  <arg name="world_file" default="$(find bme_gazebo_basics)/worlds/world_modified.world"/>

  <!-- Launch Gazebo World -->
  <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="use_sim_time" value="true"/>
    <arg name="debug" value="false"/>
    <arg name="gui" value="true" />
    <arg name="world_name" value="$(arg world_file)"/>
  </include>

</launch>
```

roslaunch bme_gazebo_basics world.launch

---

# URDF fájl

## URDF konvertálás

Az előbb elmentett simple_model sdf formátumban van, ami a Gazebonak megfelel, azonban a ROS-nak nem. A ROS-nak URDF fájlra van szüksége.
Megpróbálhatjuk átkonvertálni az sdf fájlokat, de csak a baj lesz vele. Érdemes a robotokat urdf/xacro fájlként modellezni, ezeket a ROS és a Gazebo is tudja kezelni. A háttérben a Gazebo egyébként sdf-fé konvertálja az URDF-jeinket, de ezzel nem kell foglalkoznunk.

Itt egy konvertáló tool, ha mégis meg szeretnénk próbálni:


https://github.com/MOGI-ROS/pysdf

git clone a catkin_ws/src-be
catkin make
source devel/setup.bash

```console
david@DavidsLenovoX1:~/bme_catkin_ws$ rosrun pysdf sdf2urdf.py -h
usage: sdf2urdf.py [-h] [-p PLOT] [--no-prefix] sdf urdf

positional arguments:
  sdf                   SDF file to convert
  urdf                  Resulting URDF file to be written

optional arguments:
  -h, --help            show this help message and exit
  -p PLOT, --plot PLOT  Plot SDF to file
  --no-prefix           Do not use model name as name prefix
```

rosrun pysdf sdf2urdf.py /home/david/bme_catkin_ws/src/Week-3-Gazebo-basics/bme_gazebo_basics/urdf/simple_model/model.sdf /home/david/bme_catkin_ws/src/Week-3-Gazebo-basics/bme_gazebo_basics/urdf/simple_model.urdf

```xml
<?xml version="1.0" ?>
<robot name="simple_model">
  <joint name="simple_model__link_1_JOINT_0" type="revolute">
...
```

```xml
<?xml version="1.0" ?>
<robot name="simple_model">
  <joint name="simple_model__link_1_JOINT_0" type="continuous">
...
```

xxx

---

## URDF megnyitása az RViz-ben

roslaunch urdf_tutorial display.launch model:='$(find bme_gazebo_basics)/urdf/simple_model.urdf'

Saját URDF launch fájl:

```xml
<?xml version="1.0" encoding="UTF-8"?>

<launch>

  <arg name="model" default="$(find bme_gazebo_basics)/urdf/simple_model.urdf"/>
  <arg name="gui" default="true" />
  <arg name="rvizconfig" default="$(find bme_gazebo_basics)/rviz/simple_model.rviz" />

  <param name="robot_description" command="$(find xacro)/xacro.py $(arg model)" />
  <param name="use_gui" value="$(arg gui)"/>

  <node name="joint_state_publisher_gui" pkg="joint_state_publisher_gui" type="joint_state_publisher_gui" />
  <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" />
  <node name="rviz" pkg="rviz" type="rviz" args="-d $(arg rvizconfig)" required="true" />

</launch>
```

Launch file elemzése!

roslaunch bme_gazebo_basics check_urdf.launch


## URDF / Xacro készítése



```xml
<?xml version='1.0'?>

<robot name="mogi_bot" xmlns:xacro="http://www.ros.org/wiki/xacro">

  <link name="base_footprint"></link>

  <joint name="base_footprint_joint" type="fixed">
    <origin xyz="0 0 0" rpy="0 0 0" />
    <parent link="base_footprint"/>
    <child link="base_link" />
  </joint>

  <link name='base_link'>
    <pose>0 0 0.1 0 0 0</pose>

    <inertial>
      <mass value="15.0"/>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <inertia
          ixx="0.1" ixy="0" ixz="0"
          iyy="0.1" iyz="0"
          izz="0.1"
      />
    </inertial>

    <collision name='collision'>
      <origin xyz="0 0 0" rpy="0 0 0"/> 
      <geometry>
        <box size=".4 .2 .1"/>
      </geometry>
    </collision>

    <visual name='base_link_visual'>
      <origin xyz="0 0 0" rpy="0 0 0"/>
      <geometry>
        <box size=".4 .2 .1"/>
      </geometry>
    </visual>

    <collision name='rear_caster_collision'>
      <origin xyz="-0.15 0 -0.05" rpy="0 0 0"/>
      <geometry>
        <sphere radius="0.0499"/>
      </geometry>
    </collision>

    <visual name='rear_caster_visual'>
      <origin xyz="-0.15 0 -0.05" rpy="0 0 0"/>
      <geometry>
        <sphere radius="0.05"/>
      </geometry>
    </visual>

    <collision name='front_caster_collision'>
      <origin xyz="0.15 0 -0.05" rpy="0 0 0"/>
      <geometry>
        <sphere radius="0.0499"/>
      </geometry>
    </collision>

    <visual name='front_caster_visual'>
      <origin xyz="0.15 0 -0.05" rpy="0 0 0"/>
      <geometry>
        <sphere radius="0.05"/>
      </geometry>
    </visual>

  </link>

</robot>
```

roslaunch bme_gazebo_basics check_urdf.launch model:='$(find bme_gazebo_basics)/urdf/mogi_bot.xacro' rvizconfig:='$(find bme_gazebo_basics)/rviz/mogi_bot.rviz'

Adjunk hozzá 2 kereket is a `</robot>` tag elé:

```xml
  <joint type="continuous" name="left_wheel_joint">
    <origin xyz="0 0.15 0" rpy="0 0 0"/>
    <child link="left_wheel"/>
    <parent link="base_link"/>
    <axis xyz="0 1 0" rpy="0 0 0"/>
    <limit effort="10000" velocity="1000"/>
    <dynamics damping="1.0" friction="1.0"/>
  </joint>

  <link name='left_wheel'>
    <inertial>
      <mass value="5.0"/>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/>
      <inertia
          ixx="0.1" ixy="0" ixz="0"
          iyy="0.1" iyz="0"
          izz="0.1"
      />
    </inertial>

    <collision name='collision'>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/> 
      <geometry>
        <cylinder radius=".1" length=".05"/>
      </geometry>
    </collision>

    <visual name='left_wheel_visual'>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/>
      <geometry>
        <cylinder radius=".1" length=".05"/>
      </geometry>
    </visual>
  </link>

  <joint type="continuous" name="right_wheel_joint">
    <origin xyz="0 -0.15 0" rpy="0 0 0"/>
    <child link="right_wheel"/>
    <parent link="base_link"/>
    <axis xyz="0 1 0" rpy="0 0 0"/>
    <limit effort="10000" velocity="1000"/>
    <dynamics damping="1.0" friction="1.0"/>
  </joint>

  <link name='right_wheel'>
    <inertial>
      <mass value="5.0"/>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/>
      <inertia
          ixx="0.1" ixy="0" ixz="0"
          iyy="0.1" iyz="0"
          izz="0.1"
      />
    </inertial>

    <collision name='collision'>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/> 
      <geometry>
        <cylinder radius=".1" length=".05"/>
      </geometry>
    </collision>

    <visual name='right_wheel_visual'>
      <origin xyz="0 0 0" rpy="0 1.5707 1.5707"/>
      <geometry>
        <cylinder radius=".1" length=".05"/>
      </geometry>
    </visual>
  </link>
```

roslaunch bme_gazebo_basics check_urdf.launch model:='$(find bme_gazebo_basics)/urdf/mogi_bot.xacro' rvizconfig:='$(find bme_gazebo_basics)/rviz/mogi_bot.rviz'


## URDF betöltése (spawn) Gazebo-ba és RViz-be



## Plugin

### differenciálhajtás

### Skid steer

# Odometria a ROS-ban

# ROS Graph

# TF és TF tree

# Keyboard teleop



xxx

---








