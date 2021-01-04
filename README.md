[//]: # (Image References)

[image1]: ./assets/gazebo_1.png "Gazebo"
[image2]: ./assets/gazebo_2.png "Gazebo"

[image3]: ./assets/vcxsrv_1.png "VcXsrv"
[image4]: ./assets/vcxsrv_2.png "VcXsrv"
[image5]: ./assets/vcxsrv_3.png "VcXsrv"
[image6]: ./assets/rqt_1.png "rqt"
[image7]: ./assets/rqt_2.png "rqt"
[image8]: ./assets/rqt_3.png "rqt"
[image9]: ./assets/rqt_4.png "rqt" 


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

## Building editor


xxx

---

# RViz

xxx

---


# URDF fájl

## Egyszerű URDF készítése

## URDF megnyitása az RVizben

## URDF betöltése Gazebo-ba

xxx

---








