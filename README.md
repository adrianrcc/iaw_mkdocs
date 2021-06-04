# Implantación de Aplicaciones Web - Práctica Mkdocs

Adrián Ramírez-Cruzado Carmona

2º CFGS Admón. de Sistemas Informáticos en Red

IES Celia Viñas (Almería) - 2020/2021

# Repositorio del proyecto

- [https://github.com/adrianrcc/iaw_mkdocs](https://github.com/adrianrcc/iaw_mkdocs)

# Práctica: Creación de blogs con Mkdocs y GitHub Pages

MkDocs es un generador de sitios web estáticos que nos permite crear de forma sencilla un sitio web para documentar un proyecto. El contenido del sitio web está escrito en texto plano en formato Markdown y se configura con un único archivo de configuración en formato YAML.

# Descripción del proceso

## Crear un repositorio de páginas estáticas de Github

Creamos un repositorio con el formato "nombre".github.io para almacenar el contenido del blog.

## Preparar una máquina para Mkdocs

Usaremos una instancia de Amazon con los siguientes puertos abiertos en el grupo de seguridad:

- SSH 22
- HTTP 80
- 8000

Instalamos Docker:

~~~
apt update

apt install docker 

systemctl enable docker
systemctl start docker
~~~

Clonamos el contenido del repositorio github.io:

~~~
sudo git clone https://github.com/adrianrcc/adrianrcc.git
~~~

## Creación de un contenedor Docker con Mkdocs


### Comando: new

En primer lugar vamos a situarnos en el directorio donde queremos crear nuestro proyecto. En nuestro caso será la raíz de nuestro repositorio.

Para **crear la estructura de archivos** del proyecto MkDocs podemos hacer uso del comando new, como se muestra en el siguiente ejemplo.

~~~
docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material new .
~~~

**Nota: Vamos a crear el contenedor desde el directorio principal el proyecto porque necesitamos crear un volumen de tipo bind mount entre nuestra máquina y el contenedor Docker.**


Todos los archivos con el contenido en Markdown tienen que estar dentro del directorio docs. Nuestro proyecto quedará con la siguiente estructura de archivos:

~~~
.
└── repositorio
    ├── docs
    │   └── index.md
    └── mkdocs.yml
~~~

Dentro del directorio de nuestro proyecto deberemos crear el archivo de configuración YAML mkdocs.yml.

Por ejemplo, en el siguiente archivo configuración mkdocs.yml estamos indicando el **nombre del sitio** web que estamos creando (site_name), **los enlaces** a las páginas que van a aparecer en el menú de navegación (nav) y el **theme** que vamos a utilizar (theme).
~~~
site_name: adrianrcc

nav:
    - Principal: index.md

theme: material
~~~

### Comando: serve

Una vez que hemos creado la estructura del proyecto podemos empezar el **desarrollo de nuestro sitio** iniciando un contenedor Docker **con MkDocs y el theme Material**.

**Nota: Vamos a crear el contenedor desde el directorio principal el proyecto porque necesitamos crear un volumen de tipo bind mount entre nuestra máquina y el contenedor Docker.**

~~~
docker run --rm -it -p 8000:8000 -v "$PWD":/docs squidfunk/mkdocs-material
~~~

Una vez iniciado el contenedor podemos acceder a la URL **http://localhost:8000** desde un navegador web para ver el estado actual del sitio web que estamos creando.

Ahora podemos editar nuestros archivos Markdown y ver cómo se va generando el sitio web de forma inmediata.

![](https://raw.githubusercontent.com/adrianrcc/iaw_mkdocs/main/images/Captura%20de%20pantalla%202021-06-03%20215011.png)

### Comando: build

También es posible **generar directamente el sitio web** sin tener que iniciar un servidor local de desarrollo. Para generar el sitio web automáticamente podemos ejecutar el siguiente comando:

~~~
docker run --rm -it -v "$PWD":/docs squidfunk/mkdocs-material build
~~~

El comando anterior **creará un directorio llamado site** donde guarda el sitio web que se ha generado.

### Comando: gh-deploy

Es posible **publicar la el sitio web en GitHub Pages** con el siguiente comando:

~~~
docker run --rm -it -v ~/.ssh:/root/.ssh -v "$PWD":/docs squidfunk/mkdocs-material gh-deploy
~~~

Esto creará un branch llamado **"gh-pages"** con el contenido de nuestro sitio.

### Configurar GitHub Pages

Debemos ir a "Settings">"Pages" y seleccionar como "Source" el nuevo branch "gh-pages" y el directorio "/docs". También podríamos alojarlo en la raíz de este branch o incluso en el main, ya sea en un directorio "/docs" o en la raíz, según nos convenga.

![](https://raw.githubusercontent.com/adrianrcc/iaw_mkdocs/main/images/Captura%20de%20pantalla%202021-06-04%20182951.png)

### Usar un dominio propio

Es posible utilizar un dominio propio para alojar nuestro sitio. Para ello, en "Settings">"Pages">"Custom domain" debemos indicar el nombre de nuestro dominio:

![]()

## Sitio 

https://adrianramirez.tk/

## Fuentes

- https://josejuansanchez.org/iaw/practica-mkdocs/index.html
- https://richpauloo.github.io/2019-11-17-Linking-a-Custom-Domain-to-Github-Pages/

