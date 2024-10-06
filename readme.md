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
# 4. Operar el Dron Tello DJI con Keyboard/mando de xbox s
Documentación y métodología:
# 5. Calibración de Webcam 
Documentación teórica y métodología de la obtención de los parámetros extrínsecos e intrı́nsecos del archivo .yaml para su enfoque
e implementación en ORB-SLAM3.
Documentación:
Video calibración Webcam (Método por ROS):
Video calibración Drone DJI Tello Ryze (Método por Matlab):

# 6. Procesando secuencias personalizadas con Drone DJI Tello Ryze**
Video explicación:

# 7. Integración de ORB_SLAM3 como nodo en ROS
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
# 8. Evaluación en tiempo real de Webcam con ROS
Video de calibración e implementación: 

# 9. Evaluación en tiempo real del Drone DJI Tello con ROS
