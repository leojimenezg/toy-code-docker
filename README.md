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

## Dockerfile
Un archivo llamado Dockerfile es un documento de texto usado para crear una imagen, pues provee las instrucciones necesarias para construirla.

Las siguientes instrucciones son algunas de las más comunes que un Dockerfile incluye:
* `FROM <image>`: Especifica la imagen base de la cual se va a extender
* `WORKDIR <path>`: Establece el directorio de trabajo donde se ejecutarán los comandos y se copiarán archivos
* `COPY <host-path> <image-path>`: Copia archivos del host hacia la imagen
* `RUN <command>`: Ejecuta un comando durante la construcción de la imagen
* `ENV <name> <value>`: Establece una variable de entorno disponible para el contenedor
* `EXPOSE <port-number>`: Documenta qué puerto usará la aplicación (no lo abre automáticamente)
* `USER <user-or-uid>`: Establece el usuario por defecto para las siguientes instrucciones
* `CMD ["<command>", "<arg1>"]`: Define el comando por defecto que ejecutará el contenedor al iniciarse

Para más comandos está la [Dockerfile reference](https://docs.docker.com/reference/dockerfile/).

## Build, tag and publish an image
Este es el flujo de trabajo fundamental para distribuir aplicaciones en contenedores. El proceso consiste en crear una imagen local, asignarle un nombre y versión, y finalmente publicarla en un registro para su distribución.

### Building images
Las imágenes se construyen a partir de un archivo de configuración denominado Dockerfile mediante el comando docker build. Este proceso lee las instrucciones del Dockerfile, obtiene la imagen base requerida y ejecuta cada paso para ensamblar las capas de la imagen final.

El comando en su forma más básica es:
```bash
docker build .
```

Al ejecutarlo, Docker busca un Dockerfile en el directorio actual `.`. El resultado es una imagen sin etiqueta (conocida como "dangling image"), la cual solo es identificable por su ID único.

### Tagging images
El etiquetado es el proceso de asignar un nombre legible y versionado a una imagen, lo cual es indispensable para su gestión y publicación. La estructura estándar de una etiqueta es la siguiente:
* `[HOST[:PORT_NUMBER]/]PATH[:TAG]`, donde:
    * `HOST`: El nombre del registro donde se encuentra la imagen (opcional). Si no es especificado, Docker busca en el registro público `docker.io` por defecto
    * `PORT_NUMBER`: El número de puerto del registro, si es proporcionado
    * `PATH`: El path de la imagen construida por componentes separados por `/`. En Docker Hub, se debe seguir la estructura `[NAMESPACE/]REPOSITORY`, donde NAMESPACE es el nombre de usuario u organización. Sino es especificado, Docker usa `library`, que es el namespace para Docker Official Images
    * `TAG`: Un identificador personalizable usado generalmente para identificar diferentes versiones o variantes de una imagen. Si no es especificado, de una `latest` por defecto

El etiquetado se puede realizar de dos formas:
* Durante la construcción, usando la bandera `-t` o `--tag`:
    ```bash
    docker build -t mi-usuario/mi-app:v1.0.0
    ```
* Después de la construcción, sobre una imagen existente mediante su ID:
    ```bash
    docker tag <ID> mi-usuario/mi-app:v1.0.0
    ```

### Publishing images
Una vez que una imagen está construida y correctamente etiquetada con su `NAMESPACE`, está lista para ser compartida en un registro.

Para publicarla, se utiliza el comando `docker push`:
```bash
docker push mi-usuario/mi-app:v1.0.0
```

## Build cache
Cuando se usa el comando `docker build` para crear una imagen, Docker ejecuta cada instrucción del Dockerfile creando una capa por instrucción en orden en que se presentan. Sin embargo, Docker verifica si puede reutilizar cada instrucción desde construcciones previas. Si detecta que ya ejecutó una instrucción idéntica anteriormente, la reutiliza desde el caché en lugar de ejecutarla nuevamente.

Usar el build cache efectivamente acelera la construcción de imágenes al reutilizar resultados previos, evitando trabajo innecesario. Para usarlo efectivamente, es necesario entender cuándo se invalida el caché:
* **Cambios en `RUN`**: Cualquier modificación en comandos `RUN` invalida esa capa
* **Cambios en archivos copiados**: Modificaciones en archivos usados por `COPY` o `ADD` invalidan esas capas  
* **Invalidación en cascada**: Una vez que una capa se invalida, todas las capas siguientes también se invalidan automáticamente

Docker invalida capas dependientes para mantener consistencia y prevenir inconsistencias en la imagen resultante. Por lo tanto, es importante considerar el build cache al escribir Dockerfiles para lograr construcciones más rápidas y eficientes.

## Multi-stage builds
En la construcción tradicional de imágenes, todas las instrucciones son ejecutadas secuencialmente para construir un único contenedor al: descargar dependencias, compilar código, empaquetar la aplicación, etc. Todas esas capas son contenidas en la imagen resultante. Esta forma de trabajar funciona, pero puede producir imágenes pesadas con contenido innecesario e incluso generar vulnerabilidades de seguridad. Aquí es donde entra el multi-stage build.

Esta técnica introduce múltiples fases (stages) en un Dockerfile, cada una con un propósito específico. Es como ejecutar diferentes partes de múltiples builds en un mismo proceso, permitiendo separar el entorno de construcción del entorno de ejecución. Es decir, separar el entorno donde está todo lo necesario para construir el proyecto, del entorno donde está únicamente lo que se necesita para ejecutar la aplicación.

Utilizar multi-stage builds es recomendado para todo tipo de aplicaciones:
* **Lenguajes interpretados** (JavaScript, Python, Ruby): construir y minimizar código en una fase, luego copiar solo los archivos necesarios para ejecución
* **Lenguajes compilados** (Go, Rust, C++): compilar en una fase, luego copiar solo los binarios resultantes

## Publish and expose ports
Los contenedores ejecutan aplicaciones de forma completamente aislada de otros procesos. Sin embargo, aunque este aislamiento proporciona seguridad y facilita el manejo de aplicaciones, también significa que **no es posible** acceder directamente a los procesos desde el exterior.

Aquí es donde la publicación de puertos es fundamental.

### Publish port
Publicar un puerto crea un puente de comunicación entre el proceso aislado y el entorno externo al establecer reglas de acceso directo. La publicación ocurre al ejecutar un contenedor usando la flag `-p` o `--publish`:
```bash
docker run -d -p HOST_PORT:CONTAINER_PORT nginx
```
* `HOST_PORT`: Puerto de la máquina local (host) donde se recibirá el tráfico
* `CONTAINER_PORT`: Puerto del contenedor donde la aplicación escucha

Es importante saber que cuando un puerto es publicado, se hace accesible desde todas las interfaces de red por defecto.

### Publish to ephemeral ports
Si no importa qué puerto del host usar, Docker puede elegir automáticamente omitiendo el `HOST_PORT`:
```bash
docker run -d -p :80 nginx   # Docker asigna puerto del host automáticamente
```

Los puertos elegidos por Docker son generalmente mayores a 1024 para evitar conflictos con puertos del sistema.

### Publish to all ports
La instrucción `EXPOSE` en el Dockerfile indica qué puertos usará el contenedor, pero no los publica automáticamente. Con `-P` o `--publish-all` se publican todos los puertos expuestos usando ephemeral ports:
```bash
docker run -P nginx
```

Esto es útil para evitar conflictos de puertos al dirigir tráfico a contenedores.

## Override container defaults
Todos los contenedores tienen configuraciones por defecto que funcionan para casos generales, pero es posible personalizarlas según necesidades específicas.

El comando `docker run` permite sobrescribir configuraciones por defecto mediante flags, incluyendo pero no limitado:
* **Comando de inicio**: Cambiar qué ejecuta el contenedor al iniciarse
* **Variables de entorno**: Configurar la aplicación sin reconstruir la imagen
* **Puertos**: Publicar puertos diferentes a los definidos por defecto
* **Volúmenes**: Montar directorios del host al contenedor
* **Nombre del contenedor**: Asignar nombres específicos en lugar de aleatorios
* **Recursos**: Limitar CPU, memoria y otros recursos

**Ejemplos comunes:**
```bash
docker run ubuntu ls -la # Sobrescribir comando por defecto

docker run -e NODE_ENV=production mi-app # Configurar variables de entorno

docker run --name mi-contenedor nginx # Cambiar nombre del contenedor
```

Esta flexibilidad hace que Docker sea adaptable a diferentes entornos y casos de uso sin necesidad de crear múltiples imágenes.

## Persist container data
Cuando un contenedor es ejecutado utiliza únicamente los archivos contenidos en la imagen de la cual fue construido. Los contenedores pueden crear, modificar o eliminar archivos sin afectar a otros contenedores. Sin embargo, una vez que el contenedor es eliminado, todos los cambios realizados se pierden. Esta característica efímera de los contenedores es útil, pero introduce el desafío de guardar información que debe persistir.

### Container volumes
Un volumen es un mecanismo que permite almacenar información que persiste más allá del ciclo de vida individual de un contenedor.

Un volumen es básicamente un directorio en el host independiente del contenedor, pero accesible por él. Cuando el contenedor que use ese volumen sea eliminado, la información persiste en el host y otros contenedores pueden usar ese mismo volumen.

Además, cuando se asigna un volumen a un contenedor y este no existe, Docker automáticamente crea el volumen. Un mismo volumen puede ser compartido por múltiples contenedores.

### Manage volumes
Los volúmenes tienen su propio ciclo de vida independiente de los contenedores, por lo que pueden crecer significativamente. Los siguientes comandos permiten gestionarlos:
* `docker volume ls`: Lista todos los volúmenes existentes
* `docker volume rm <volume-name-or-id>`: Elimina un volumen específico (solo si no está en uso)
* `docker volume prune`: Elimina todos los volúmenes no utilizados

## Share local files with containers
Los contenedores tienen todo lo necesario para funcionar independientemente del host. Debido a que se ejecutan como procesos aislados, obtienen muchos beneficios de seguridad y portabilidad. Sin embargo, esto también significa que no pueden acceder a archivos del host por defecto.

Para resolver esto, Docker proporciona dos mecanismos para persistir información y compartir archivos: **volumes** y **bind mounts**.

### Volume VS Bind mounts
* **Volume**: Mejor opción cuando se requiere asegurar que información generada por un contenedor persista después de que el contenedor se elimine
* **Bind mount**: Mejor opción para compartir archivos específicos del host que no deben estar dentro del contenedor (configuraciones, credenciales)

### Share files between host and container
Las flags `-v` (o `--volume`) y `--mount` en el comando `docker run` permiten compartir archivos entre host y contenedor:

**Flag `-v` (simple):**
```bash
docker run -v /HOST/PATH:/CONTAINER/PATH nginx
```
* Más simple para operaciones básicas
* Crea automáticamente directorios/archivos si no existen

**Flag `--mount` (avanzada):**
```bash
docker run --mount type=bind,source=/HOST/PATH,target=/CONTAINER/PATH,readonly nginx
```
* Mayor control y opciones granulares
* Falla si el directorio/archivo no existe (más seguro)
* Recomendada por Docker para producción

### File permissions
Con `--mount` es importante asegurar que Docker tenga los permisos necesarios:
* `:rw`: Lectura y escritura
* `:ro`: Solo lectura

Docker recomienda `--mount` por su especificidad y control, evitando problemas potenciales.

## Multi-container applications
