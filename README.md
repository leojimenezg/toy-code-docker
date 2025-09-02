# Docker

* [Docker Documentation](https://docs.docker.com/)
* [Docker Get Started](https://docs.docker.com/get-started/)

## What is Docker?
Docker es una plataforma abierta para desarrollar, desplegar y ejecutar aplicaciones. Permite separar las aplicaciones de la infraestructura al empaquetar todo lo necesario de una aplicación dentro de un contenedor.

Docker permite crear aplicaciones independientes del sistema operativo, versiones y dependencias del host. Los contenedores son ambientes aislados y ligeros que contienen todo lo necesario para ejecutar una aplicación sin preocuparse por tener instaladas las dependencias en el host.

Los contenedores se pueden compartir para asegurar que todos trabajen con las mismas tecnologías y configuraciones. Múltiples contenedores pueden ejecutarse en un mismo host de manera eficiente.

Una de las ventajas de Docker es que al ser ligero y portable, permite ejecutar múltiples aplicaciones (contenedores) en un mismo sistema operativo y permite que se comuniquen entre ellas, haciendo un mejor y más efectivo uso de los recursos disponibles.

## Docker architecture
Docker utiliza la arquitectura Cliente-Servidor. El Docker Client se comunica con el Docker Daemon (servidor) que se encarga de los pesados procesos de construir, ejecutar y distribuir los contenedores. El cliente y el servidor pueden correr en el mismo sistema o el cliente puede usar un servidor remoto, estos se pueden comunicar mediante una API REST, UNIX Sockets o una interfaz de red. Otro cliente importante es Docker Compose, que permite trabajar con aplicaciones formadas por múltiples contenedores.

### The Docker daemon
El Docker daemon (`dockerd`) es el servidor que se encarga de todos los procesos pesados y principales, se encuentra esperando por peticiones de Docker API y gestiona todos los objetos Docker como imágenes, contenedores, redes y volúmenes. Un daemon puede comunicarse con otros daemon para gestionar servicios Docker.

### The Docker client
El Docker client (`docker`) es la forma principal para interactuar con Docker. Al usar comandos docker, el cliente envía los comandos al servidor para que los procese y ejecute. El comando `docker` utiliza la Docker API, incluso, un mismo cliente puede comunicarse con múltiples daemons.

### Docker registries
Docker registry es el concepto de almacenar, buscar, subir y descargar Docker images. Docker Hub es una implementación específica de este concepto. Por defecto, Docker utiliza Docker Hub como registry principal para buscar y descargar imágenes. Los registries pueden ser públicos (accesibles para todos) o privados (para uso interno de organizaciones).

### Docker objects
#### Images
Una imagen es una plantilla de solo lectura con todo lo necesario para crear contenedores. Generalmente, una imagen está basada en otra imagen pero con personalizaciones y detalles adicionales.

Es posible crear imágenes propias o usar imágenes creadas por otros usuarios publicadas en algún registro. Para crear una imagen se debe crear un Dockerfile que define los pasos necesarios para construir la imagen. Cada instrucción del Dockerfile crea una capa en la imagen, por lo que al reconstruir solo se modifican las capas que cambiaron. Este sistema de capas es lo que hace que las Docker images sean ligeras, pequeñas y rápidas.

#### Containers
Un contenedor es un instancia completamente ejecutable generada a partir de una Docker image. Es posible crear, iniciar, detener, mover o eliminar un contenedor mediante Docker API o CLI, además, se puede conectar un contenedor a múltiples redes, asignar almacenamiento específico e inluso generar nuevas imágenes basadas en el estado actual de un contenedor.

Por defecto, un contenedor está relativamente aislado de otros contenedores y del propio host, pero es posible controlar qué tan aislado está un contenedor de los aspectos con los que interactúa.

Un contenedor es definido por la imagen de la que parte y por configuraciones adicionales dadas al momento de crearlo. Cuando un contendor es eliminado, sus cambios y estado que no han sido guardados serán completamente eliminados.

## What is a container?
De forma simple, los contenedores son procesos aislados que ejecutan tareas específicas. Un contenedor contiene todo lo que necesita para funcionar sin depender de lo instalado en el sistema host (versiones, plataformas, dependencias). Están completamente aislados de otros contenedores, aunque pueden comunicarse entre ellos, lo que los convierte en procesos independientes, seguros y portables.

### Containers VS Virtual Machines
Una máquina virtual incluye un sistema operativo completo con su propio kernel, drivers y programas. Un contenedor es simplemente un proceso aislado con los componentes necesarios para funcionar. Por lo tanto, múltiples contenedores comparten el mismo kernel del sistema operativo host, haciéndolos más eficientes que las VMs.

## What is an image?
Una imagen es un paquete estandarizado que contiene todos los archivos, binarios, librerías y configuraciones necesarias para ejecutar un contenedor (proceso). Las imágenes tienen dos características fundamentales:
* **Las imágenes son inmutables:** una vez que una imagen ha sido creada no puede modificarse, pero, sí se pueden crear nuevas imágenes con los nuevos cambios usando como base la imagen previa.
* **Las imágenes se componen de capas:** cada capa de una imagen representa un conjunto de modificaciones en el sistema de archivos que agregan, eliminan o modifican archivos.

### Finding images
[Docker Hub](https://hub.docker.com/) es el registry oficial y global para almacenar y distribuir Docker images, con más de 100,000 imágenes de diferentes proveedores avalados por Docker.

## What is a registry?
Un registry es una ubicación centralizada para almacenar y distribuir Docker images. Pueden ser públicos o privados. Docker Hub es el registry por defecto, pero no es la única opción.

### Registry VS Repository
Un registry es una ubicación centralizada que almacena y gestiona imágenes; mientras que un repository es una colección de imágenes relacionadas dentro de un registry. Por lo tanto, un Registry es un concepto más amplio que gestiona todo el ecosistema de Docker images, y un Repository simplemente agrupa imágenes relacionadas entre sí.

## What is Docker Compose?
Docker Compose es una herramienta que permite definir y gestionar aplicaciones multi-contenedor mediante un archivo de configuración `.yml`. Con este archivo, ejecutar aplicaciones que requieren múltiples contenedores se vuelve mucho más sencillo.

Es importante entender que Compose es una herramienta declarativa, donde simplemente se define qué contenedores se necesitan y cómo deben configurarse, y la herramienta se encarga de gestionar todo el ciclo de vida.

Esta herramienta se puede confundir con Dockerfile, pero Dockerfile define las instrucciones para construir una Docker image, mientras que Compose define cómo orquestar múltiples contenedores. Generalmente ambas herramientas se usan juntas.

## Image layers
Las imágenes son planos para construir contenedores, que contienen todo lo necesario para ejecutar de forma independiente dicho contenedor. Todas las imágenes son inmutables una vez creadas y se componen de distintas capas, donde cada capa contiene un conjunto de cambios en el sistema de archivos: agregar, eliminar o modificar archivos.

Las capas de las imágenes permiten la reutilización, agregando solamente las nuevas capas necesarias a una nueva imagen y reduciendo la cantidad de instrucciones y contenido duplicado.

### Stacking layers
Usar distintas capas como un conjunto es posible debido al almacenamiento de contenido direccionable (content-addressable storage). Así funciona:
* Una vez descargado el contenido de una capa, es extraído y puesto en su propio directorio en el sistema de archivos
* Cuando se ejecuta el contenedor, se crea un sistema de archivos unificado donde las capas son apiladas una sobre otra, resultando en una vista unificada
* Al iniciar el contenedor, su directorio raíz corresponde al sistema de archivos unificado (`chroot`)

Además de las capas de imagen, se crea una capa de escritura específica para el contenedor, permitiendo que haga cambios sin afectar la imagen original. Esto permite ejecutar múltiples contenedores de la misma imagen.
