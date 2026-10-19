# Practica servidor web

**Estudiante:** Juan Bravo · **GitHub:** [Jbra420](https://github.com/Jbra420)
**Asignatura:** Software Aplicativo · **Tarea autónoma:** Semana 1 · **Fecha:** octubre 2026

## 1. Titulo

**Creación y personalización de dos servidores web con Nginx usando contenedores Docker.**

## 2. Tiempo de duración

60 minutos (aprox.)

| Actividad | Tiempo |
|---|---|
| Instalación / verificación de Docker | 10 min |
| Creación de contenedores nginx1 y nginx2 | 5 min |
| Copia y edición de los archivos `index.html` | 25 min |
| Pruebas en el navegador y capturas | 10 min |
| Elaboración del informe | 10 min |

## 3. Fundamentos:

**Docker** es una plataforma de código abierto que permite empaquetar una aplicación junto con todas sus dependencias (librerías, configuraciones y sistema de archivos) dentro de una unidad llamada **contenedor**. A diferencia de una máquina virtual, un contenedor no necesita un sistema operativo invitado completo: comparte el kernel del sistema anfitrión y aísla únicamente los procesos, la red y el sistema de archivos. Por eso los contenedores son livianos, arrancan en segundos y se comportan igual en cualquier computador donde exista Docker, lo que elimina el típico problema de "en mi máquina sí funciona".

Una **imagen** es una plantilla de solo lectura a partir de la cual se crean contenedores. Las imágenes se descargan de un registro público llamado **Docker Hub**; en esta práctica se usa la imagen oficial `nginx`. Un **contenedor** es una instancia en ejecución de esa imagen: a partir de una misma imagen se pueden crear varios contenedores independientes, cada uno con su propio nombre, estado y archivos. Esto es justamente lo que se aprovecha aquí al crear `nginx1` y `nginx2` desde la misma imagen pero con contenido diferente.

**Nginx** (pronunciado "engine-x") es un servidor web de alto rendimiento, también utilizado como proxy inverso y balanceador de carga. Escucha peticiones HTTP, por defecto en el puerto **80**, y responde entregando archivos estáticos. En la imagen oficial de Docker, la página de inicio se encuentra en la ruta `/usr/share/nginx/html/index.html`; al modificar ese archivo cambiamos lo que el visitante ve en el navegador.

Como cada contenedor tiene su propia red aislada, su puerto 80 no es accesible directamente desde el anfitrión. Para resolverlo se usa el **mapeo de puertos** con la opción `-p PUERTO_ANFITRION:PUERTO_CONTENEDOR`. Con `-p 8089:80` todo lo que llega al puerto 8089 de la computadora se redirige al puerto 80 del contenedor `nginx1`, y con `-p 8090:80` el puerto 8090 se redirige a `nginx2`. Así dos servidores web conviven en la misma máquina sin conflicto.

Finalmente, el comando **`docker cp`** permite copiar archivos entre el sistema anfitrión y un contenedor en ambas direcciones. Gracias a él podemos extraer el `index.html` original, editarlo cómodamente con un editor de texto como `nano` o `vi`, y volver a colocarlo dentro del contenedor sin necesidad de reconstruir la imagen. Otros comandos útiles son `docker ps` (lista contenedores activos), `docker stop`/`docker start` (detiene o inicia) y `docker rm` (elimina un contenedor).

<p align="center">
  <img src="img/diagrama-contenedores.svg" alt="Diagrama de contenedores" width="800">
</p>

**Figura 3-1.** Diagrama de la práctica: el navegador accede a dos contenedores Nginx a través de los puertos 8089 y 8090.

<p align="center">
  <img src="https://docs.docker.com/get-started/images/docker-architecture.webp" alt="Arquitectura de Docker" width="700">
</p>

**Figura 3-2.** Arquitectura de Docker: cliente, demonio (host), imágenes, contenedores y registro (Docker Docs).

## 4. Conocimientos previos.

Para realizar esta practica el estudiante necesita tener claro los siguientes temas:

- Comandos básicos de Linux/terminal (`cd`, `ls`, `pwd`, `nano`, `vi`).
- Conceptos básicos de Docker: imagen, contenedor, puerto.
- Estructura básica de un documento HTML.
- Manejo del navegador web (acceso a `localhost:puerto`).
- Uso de Git y GitHub para subir el informe.

## 5. Objetivos a alcanzar

- Implementar dos contenedores con Nginx a partir de la misma imagen oficial.
- Exponer cada servidor en un puerto distinto del anfitrión (8089 y 8090).
- Manipular archivos entre el anfitrión y los contenedores usando `docker cp`.
- Personalizar el `index.html` de cada servidor: información institucional en `nginx1` e información personal en `nginx2`.
- Documentar la práctica en formato Markdown y publicarla en GitHub.

## 6. Equipo necesario:

- Computador MacBook Air 2020 con macOS (también es posible en Windows o Linux).
- Docker Desktop para Mac (Docker Engine 27.x o superior) — alternativamente, una cuenta en [Play with Docker](https://labs.play-with-docker.com/).
- Editor de texto en terminal: `nano` o `vi`.
- Navegador web (Chrome, Safari o Firefox).
- Cuenta de GitHub y Git instalado.
- Conexión a Internet para descargar la imagen `nginx`.

## 7. Material de apoyo.

- Documentación oficial de Docker: <https://docs.docker.com>
- Imagen oficial de Nginx en Docker Hub: <https://hub.docker.com/_/nginx>
- Guía de la asignatura y video guía rápida de la semana.
- Cheat sheet de comandos Linux y de Docker CLI.
- Guía de sintaxis Markdown de GitHub.

## 8. Procedimiento

**Paso 1: Verificar la instalación de Docker.**

```bash
docker --version
docker info
```

![Figura 8-1](img/paso1-docker-version.png)

**Figura 8-1.** Versión de Docker instalada.

**Paso 2: Crear el primer contenedor `nginx1` en el puerto 8089.**

```bash
docker run -d --name nginx1 -p 8089:80 nginx
```

`-d` ejecuta el contenedor en segundo plano, `--name` le asigna un nombre y `-p` mapea el puerto 8089 del anfitrión al 80 del contenedor. Si la imagen no existe localmente, Docker la descarga de Docker Hub.

**Paso 3: Crear el segundo contenedor `nginx2` en el puerto 8090.**

```bash
docker run -d --name nginx2 -p 8090:80 nginx
```

**Paso 4: Comprobar que ambos contenedores están en ejecución.**

```bash
docker ps
```

![Figura 8-2](img/paso4-docker-ps.png)

**Figura 8-2.** Contenedores `nginx1` y `nginx2` en ejecución.

**Paso 5: Copiar el `index.html` de `nginx1` al anfitrión.**

```bash
docker cp nginx1:/usr/share/nginx/html/index.html ./index1.html
```

**Paso 6: Editar `index1.html` con la información del instituto.**

```bash
nano index1.html
```

Se reemplazó el contenido por defecto ("Welcome to nginx!") con el nombre del instituto, misión, visión, carreras y datos de contacto. En `nano` se guarda con `Ctrl + O`, `Enter`, y se sale con `Ctrl + X`. El archivo final se encuentra en este repositorio: [index1.html](index1.html).

![Figura 8-3](img/paso6-nano-index1.png)

**Figura 8-3.** Edición de `index1.html` con nano.

**Paso 7: Copiar el archivo editado de vuelta a `nginx1`.**

```bash
docker cp index1.html nginx1:/usr/share/nginx/html/index.html
```

**Paso 8: Repetir el proceso para `nginx2` con información personal.**

```bash
docker cp nginx2:/usr/share/nginx/html/index.html ./index2.html
nano index2.html
docker cp index2.html nginx2:/usr/share/nginx/html/index.html
```

El archivo final se encuentra en este repositorio: [index2.html](index2.html).

**Paso 9: Verificar en el navegador.**

- Servidor institucional: <http://localhost:8089>
- Servidor personal: <http://localhost:8090>

**Paso 10 (opcional): Detener y eliminar los contenedores al terminar.**

```bash
docker stop nginx1 nginx2
docker rm nginx1 nginx2
```

## 9. Resultados esperados:

Al finalizar la práctica se obtienen dos servidores web independientes ejecutándose en la misma computadora, creados a partir de la misma imagen `nginx` pero con contenido distinto:

- En `http://localhost:8089` el contenedor **nginx1** muestra la página con la información institucional (nombre, misión, visión, carreras y contacto).
- En `http://localhost:8090` el contenedor **nginx2** muestra la página personal del estudiante (nombre, carrera, intereses y enlace a GitHub).

![Figura 9-1](img/resultado-nginx1.png)

**Figura 9-1.** Página institucional servida por `nginx1` (puerto 8089).

![Figura 9-2](img/resultado-nginx2.png)

**Figura 9-2.** Página personal servida por `nginx2` (puerto 8090).

Se comprobó que los cambios hechos en un contenedor no afectan al otro, lo que demuestra el aislamiento que ofrecen los contenedores, y que `docker cp` permite actualizar el contenido de un servidor sin reconstruir la imagen.

## 10. Bibliografía

- Docker Inc. (2026). *Docker overview*. Docker Docs. https://docs.docker.com/get-started/docker-overview/
- Docker Inc. (2026). *docker container cp*. Docker Docs. https://docs.docker.com/reference/cli/docker/container/cp/
- Docker Inc. (2026). *nginx — Docker Official Image*. Docker Hub. https://hub.docker.com/_/nginx
- F5, Inc. (2026). *nginx documentation*. https://nginx.org/en/docs/
- GitHub. (2026). *Basic writing and formatting syntax*. GitHub Docs. https://docs.github.com/es/get-started/writing-on-github
