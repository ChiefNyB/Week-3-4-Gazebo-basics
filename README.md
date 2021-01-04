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

![alt text][image6]

![alt text][image5]

## Building editor

![alt text][image7]

![alt text][image8]

![alt text][image9]

![alt text][image10]

![alt text][image11]

![alt text][image12]

gazebo MyWorld.world





## Launchfile

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

https://github.com/andreasBihlmaier/pysdf

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

rosrun pysdf sdf2urdf.py /home/david/model_editor_models/simple_model/model.sdf /home/david/model_editor_models/simple_model/simple_model.urdf

xxx

---

## URDF megnyitása az RViz-ben


## Egyszerű URDF / Xacro készítése


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








