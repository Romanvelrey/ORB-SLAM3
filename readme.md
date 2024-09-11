# ORB-SLAM3 en Dron Tello Ryze
El objetivo de este repositorio es explicar los pasos a realizar para que el dron (DJI Tello) 
implementando la biblioteca de ORB-SLAM3 para que pueda crear un mapa de nube de puntos en distintos
entornos desconocidos, apartir de la información de la cámara monocular que incorpora.
# 1. Instalación de ORB-SLAM3.
El repositorio y los pasos utilizados, basado en la biblioteca oficial de GitHub para 
[ORB-SLAM3](https://github.com/aliaxam153/ORB_SLAM3?tab=readme-ov-file).

El repositorio utiliza un script instalador de las dependencias para un SO de ubuntu 20.04 incluyendo
ROS en su versión noetic, verifica los siguientes pre-requisitos, en lo contrario hace su instalación:
- Compilador C++11
- OpenCV 4.4.0
- Pangolin
- ROS: Noetic

# 2. Pruebas en conjunto de datos para validar su funcionamiento.
2.1 Descargar conjuntos de datos de prueba
- EuRoC dataset recogidos a bordo de un Micro Vehículo Aéreo (MAV). Los conjuntos de datos contienen imágenes estéreo, mediciones IMU sinterreno. Descargar una secuencia (formato ASL) desde [Aqui](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets) y descomprima.
- TUM-VI dataset proporciona imagenes con resolución de 1024x1024 a 20 Hz y datos precisos de movimiento y estructura sobre el tró con dos cámaras ojo de pez y un sensor inercial. Descargar una secuencia desde [Aqui](https://cvg.cit.tum.de/data/datasets/visual-inertial-dataset) y descomprimela.
- KITTI dataset: Incluye una variedad de datos capturados en el tráfico real, specialmente en el contexto de vehículos autónomos. Descargar una secuencia desde [Aqui](https://www.cvlibs.net/datasets/kitti/eval_odometry.php) y descomprimela.

2.2 Estructura para ejecutar ORB-SLAM en EuRoC:
``` python
./Examples/TIPO_DE_CAMARA/TIPO_DE_CAMARA             #Dirección del modo de cámara a evaluar
./Vocabulary/ORBvoc.txt                              #Dirección del vocabulario 
./Examples/TIPÓ_DE_CAMARA/EuRoC.yaml                 #Dirección de la configuración de la cámara
~/Datasets/NOMBRE_DE_LA_CARPETA/MH01                 #Dirección de las secuencias del conjunto de pruebas
./Examples/TIPO_DE_CAMARA/EuRoc_TimeStamps/MH01.txt  #Dirección del orden de las secuencias
dataset-MH01_mono                                    #Nombre del archivo de salida (keyframe)
```
2.3 Pruebas con conjunto de datos (ejecutar simulación desde biblioteca desde la dirección ~./dev/ORB-SLAM3)

Modos principales a ejecutar:

2.3.1 EuRoC Mono:
``` python
./Examples/Monocular/mono_euroc ./Vocabulary/ORBvoc.txt ./Examples/Monocular/EuRoC.yaml ~/datasets/EuRoC/MH_01_easy ./Examples/Monocular/EuRoC_TimeStamps/MH01.txt dataset-MH01_mono
```
2.3.2 EuRoC Mono-inercial
``` python
./Examples/Monocular-Inertial/mono_inertial_euroc ./Vocabulary/ORBvoc.txt ./Examples/Monocular-Inertial/EuRoC.yaml ~/datasets/EuRoC/MH_01_easy ./Examples/Monocular-Inertial/EuRoC_TimeStamps/MH01.txt dataset-MH01_monoi
```
2.4 Estructura para ejecutar ORB-SLAM en TUM:
``` python
./Example/Monocular/mono_tum                         #Dirección del modo de cámara a evaluar
./Vocabulary/ORBvoc.txt                              #Dirección del vocabulario 
./Examples/TIPO_DE_CAMARA/TUMX.yaml                  #Dirección de la configuración de la cámara
~/Datasets/NOMBRE_DE_LA_CARPETA/MH01                 #Dirección de las secuencias del conjunto de pruebas
```
2.4.1 TUM Mono
``` python
./Examples/Monocular/mono_tum ./Vocabulary/ORBvoc.txt ./Examples/Monocular/TUM1.yaml ~/datasets/rgbd_dataset_freiburg1_xyz/
```
# 4. Calibración de Webcam 
# 5. Procesando secuencias personalizadas
# 6. Integración de ORB_SLAM3 como nodo en ROS
El [repositorio](https://github.com/aliaxam153/ORB_SLAM3?tab=readme-ov-file), contiene un script
que instala los nodos para su incorporación con ROS.
3.1 Instalación
``` python
chmod +x build_ros.sh
./build_ros.sh
```
3.2 Descargar archivo rosbag (e.g. V1_02_medium.bag) de [Aqui](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets) 

3.2 Ejecución
Abrir 3 terminales 
Terminal 1: Inicializar ROS
``` python
roscore
```
Terminal 2: Correr conjunto de datos
``` python
rosbag play --pause ~/MH_01_easy.bag /cam0/image_raw:=/camera/left/image_raw /cam1/image_raw:=/camera/right/image_raw /imu0:=/imu
```
Terminal 3: 
``` python
cd ~/dev/ORB_SLAM3/
rosrun ORB_SLAM3 Stereo_Inertial ~/dev/ORB_SLAM3/Vocabulary/ORBvoc.txt ~/dev/ORB_SLAM3/Examples/Stereo-Inertial/EuRoC.yaml true
```
# 7. Evaluación en tiempo real con Webcam y ROS
7.1 Ir a la siguiente dirección:
``` python
cd ~/dev/ORB_SLAM3/Examples/ROS/ORB_SLAM3/src/ros_mono.cc
```
7.2 Remplazar
``` python
    ros::Subscriber sub = nodeHandler.subscribe("/camera/image_raw", 1, ...
 ```
con
``` python
    ros::Subscriber sub = nodeHandler.subscribe("/usb_cam/image_raw", 1, ...
 ```
7.3 Actualizar el paquete
``` python
#Ir a la carpeta build
cd ~/dev/ORB_SLAM3/Examples/ROS/ORB_SLAM3/build
make
 ```
7.4 Ejecución
Abrir 3 terminales 
Terminal 1: Inicializar ROS
``` python
roscore
```
Terminal 2: Launch del nodo ros_webcam
``` python
roslaunch usb_cam usb_cam-test.launch
```
Terminal 3: Espere hasta que inicialice la cámara y transmita su imagen en la pantalla, luego ejecute ORB_SLAM3 con ROS (cambiar archivo .yaml con los de la webcam):
``` python
cd ~/dev/ORB_SLAM3/
rosrun ORB_SLAM3 Mono Vocabulary/ORBvoc.txt Examples/Monocular/EuRoC.yaml
```
