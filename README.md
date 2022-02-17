# BLINDA
Es un proyecto que utiliza la inteligencia artificial para ayudar a las personas ciegas o con baja visión. El proyecto consiste en, con una cámara detectar el objeto que tienes delante y a través de un auricular o altavoz escuchar qué objeto se está detectando. Todo este proceso se realiza a través de inteligencia artificial.

Cabe destacar que BLINDA de momento sólo está disponible en idioma inglés, pero se espera que en un futuro integre más idiomas.

En esta guía describiremos paso a paso todo lo que debes de hacer para poder recrear BLINDA.
### Tabla de contenido
- Materiales
- Detección de objetos
- Speak
- Fuente de alimentación
- Puesta en marcha

### MATERIALES
Los materiales utilizados son los siguientes:

- Jetson Nano 2GB o 4GB (En nuestro caso se ha usado la de 2GB)
- Cámara ArduCam 12MP IMX477
- TECNOIOT LTC1871 DC DC Step Up Booster Converter
- MOSFET IRFP9240
- Transistor BD139
- Resitencia 10 Ohmios
- Resistencia 2k2 Ohmios
- Potenciometro 5k Ohmios
- Resistencia 100k Ohmios
- TL431
- Condensador 1nF
- Bateria de Li-Po 3.7V 3000mAh
- Switch
- Conectores DC Jack
- Diodo tipo Schottky 1N5818
- Pines para PCB

### DETECCIÓN DE OBJETOS

Usaremos un repositorio que utiliza NVIDIA TensorRT para implementar eficientemente las redes neuronales en la plataforma de la Jetson Nano, mejorando el rendimiento y la eficiencia energética mediante optimizaciones de gráficos, fusión de kernel y precisión FP16/INT8.
Este repositorio viene con una serie de redes pre-entrenadas, que se pueden cargar en la jetson nano, que se descargan cuando descargas el repositorio

Para ello hacemos uso de los siguientes comando:

#### Clonar el repositorio
Primero asegúrese de que git y cmake estén instalados:
```
$ sudo apt-get update
$ sudo apt-get install git cmake
```
Una vez comprobado clonamos el repositorio:
```
$ git clone https://github.com/dusty-nv/jetson-inference
$ cd jetson-inference
$ git submodule update --init
```
#### Paquete de desarrollo de python
La funcionalidad de Python de este proyecto se implementa a través de módulos de extensión de Python que proporcionan enlaces al código nativo de C++ mediante la API de Python C. Mientras configura el proyecto, el repositorio busca versiones de Python que tengan paquetes de desarrollo instalados en el sistema y luego creará los enlaces para cada versión de Python que esté presente (por ejemplo, Python 2.7, 3.6 y 3.7). También creará enlaces numpy para las versiones de numpy que están instaladas.

Estos paquetes de desarrollo son necesarios para que los enlaces se creen mediante la API de Python C.
Entonces, para que el proyecto cree enlaces para Python 3.6, instale estos paquetes antes de continuar:

`$ sudo apt-get install libpython3-dev python3-numpy`

La instalación de estos paquetes adicionales permitirá que el repositorio cree los enlaces de extensión para Python 3.6, además de Python 2.7 (que ya está preinstalado). Luego, después del proceso de compilación, los paquetes jetson.inferencey jetson.utilsestarán disponibles para usar dentro de sus entornos de Python.

#### Configuración con CMake
```
$ cd jetson-inference    # omitir si ya se encuentra en el directorio jetson-inference/
$ mkdir build
$ cd build
$ cmake ../
```
#### Descarga de modelos
El proyecto viene con muchas redes pre-entrenadas que puede descargar e instalar a través de la herramienta Model Downloader (download-models.sh). De forma predeterminada, no todos los modelos se seleccionan inicialmente para descargar para ahorrar espacio en disco. Puede seleccionar los modelos que desee o volver a ejecutar la herramienta más tarde para descargar más modelos en otro momento. Al ejecutar `cmake ../` se abrirá una ventana donde puede descargar todos los modelos necesarios:

!(https://raw.githubusercontent.com/dusty-nv/jetson-inference/python/docs/images/download-models.jpg)



en el caso de BLINDA se ha dejado los modelos que vienen por defecto.
