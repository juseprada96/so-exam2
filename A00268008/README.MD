### Examen 2
**Universidad ICESI**  
**Curso:** Sistemas Operativos  
**Estudiante:** Juan Sebastián Prada.  
**Tema:** Namespaces, CGroups, LXC
**Correo:** juan.prada1 at correo.icesi.edu.co

### Objetivos
* Comprender los fundamentos que dan origen a las tecnologías de contenedores virtuales
* Conocer y emplear funcionalidades del sistema operativo para asignar recursos a procesos
* Conocer y emplear capacidades de CentOS7 para la virtualización

### Prerrequisitos
* Virtualbox o WMWare
* Máquina virtual con sistema operativo CentOS7

### Descripción
El segundo parcial del curso sistemas operativos trata sobre el manejo de namespaces, cgroups y virtualización por medio de LXC/LXD

### Actividades
1. Realice una prueba de concepto empleando systemd y el recurso de control CPUQuota teniendo en cuenta los requerimientos que se describen a cotinuación. Incluya evidencias del funcionamiento de lo solicitado (30%):
 * Las pruebas se realizaran sobre un solo núcleo de la CPU
 * Se deben ejecutar dos procesos
 * Cada proceso debe poder acceder solo al 50% de la CPU
 * Cuando uno de los procesos se cancela, el que continua ejecutándose no debe acceder a mas del 50% de la CPU
 
* Para realizar la pruena de concepto se crearan dos scripts a los cuales se les asignara posteriormente un demonio y un mecanismo de control de CPUQuota.
 
 Ambos scripts se crearon en el directorio: /home/parcial-2/scripts/
 
 **Creación de los dos scripts:**
 
![][1]:

**Creación de los demons:**

Para crear los demons nos dirigimos a la siguiente carpeta

``
cd /etc/systemd/system/
``
y creamos los demons con los nombres

*nombreservicio*.service

y con el siguiente codigo:

``
[Unit]

Description= Descripción del servicio

[Service]

User= usuario sobre el cual se ejecuta el proceso

ExecStart= ubicación del script

Restart= tipo de restart

[Install]

WantedBy= Usuarios interesados
``
![][2]:
![][3]:

**Una vez creados los demons utilizamos systemctl para iniciar los serivicios.**

``
systemctl enable *nombreservicio*
``
![][4]

Configuramos la CPUQuota

``
systemctl set-property *nombreservicio* CPUQuota=50%
``

![][5]

Iniciamos el servicio

``
systemctl start *nombreservicio*
``
![][6]

Y verificamos el estado de los servicios

``
systemctl status *nombreservicio*
``

![][7]

Para verificar que estamos usando un unico procesador y observar sus caracteristicas
usamos el siguiente comando:

``
cat /proc/cpuinfo
``
![][8]

Para observar ambos procesos podemos usar el comando top.

``
top
``
Para salir presionamos q.

![][9]

Como podemos ver ningun proceso supera el CPUQuota.


Ahora paramos alguno de los dos procesos:

``
systemctl stop *nombreservicio*
``
![][10]

Como podemos ver el proceso restante no supera el CPUQuota.

![][11]
 
2.  Realice una prueba de concepto empleando systemd y el recurso de control CPUShares teniendo en cuenta los requerimientos que se describen a continuación. Incluya evidencias del funcionamiento de lo solicitado (30%):
 * Las pruebas se realizaran sobre un solo núcleo de la CPU
 * Se deben ejecutar dos procesos
 * Uno de los procesos tendrá el 25% de la CPU mientras que el otro tendrá el 75% de la CPU
 * Cuando uno de los procesos se cancela, el que continua ejecutándose debe poder llegar al 100% de la CPU
3. Por medio de las evidencias obtenidas en los puntos anteriores y de fuentes de consulta en Internet, elabore las definiciones para los grupos de control CPUQuota y CPUShares, además concluya acerca de cuando es preferible usar un recurso de control sobre otro (20%)



### Referencias
https://github.com/ICESI/so-containers

[1]:Imagenes/Screenshot_1.png
[2]:Imagenes/Screenshot_2.png
[3]:Imagenes/Screenshot_3.png
[4]:Imagenes/Screenshot_4.png
[5]:Imagenes/Screenshot_5.png
[6]:Imagenes/Screenshot_6.png
[7]:Imagenes/Screenshot_7.png
[8]:Imagenes/Screenshot_8.png
[9]:Imagenes/Screenshot_9.png
[10]:Imagenes/Screenshot_10.png
[11]:Imagenes/Screenshot_11.png
