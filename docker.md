# Docker

[https://github.com/docker/getting-started](https://github.com/docker/getting-started)

#### Imagen
La imagen esta definida con el dockerfile. Contiene todo lo que queremos montar (SO, comandos iniciales, etc).

Para poder usarla necesitamos primero buildear la imagen.
``` docker build -t getting-started .``` 
Solo hay que especificar donde esta el dockerfile, en este caso "." refiere que estamos en el directorio y el nombre de la imagen "getting-started"

Con eso aparece la imagen en docker desktop.

### Container
Para generar un container basado en la imagen hay que correrlo.

`docker run -dp 3000:3000 getting-started`

### Volumen
El container per se es una instancia nueva de cada imagen. Cada vez que corremos se crea un container independiente y cualquier modificación hecha en uno no se ve reflejada en otro.
Una solución es generar un volumen que guarda en disco todos los archivos y próximos containers recuperan esa data.

Crear named volumen
`docker volume create todo-db`

Correr referenciando el volumen e indicando donde se va a montar? No entiendo bien, porque creo que no es donde se guarda ne disco.
`docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started`

Listo. Cambios que hagamos, por ejemplo en la db, van a quedar guardados para el próximo container.

### Bind Mounts
Revisar. No terminé de entender. Permitía cambios sin tener que rebuildear (?)
Es otra manera de persistir data. Parece igual que usar volumes es mas straightforward y recomendado.

### Multi Container Apps

> If two containers are on the same network, they can talk to each other. If they aren't, they can't.

1.  Create the network.
    
    `docker network create todo-app`
	
2. Creamos un container con MySQL y lo agregamos a la network.

`docker run -d  
	--network todo-app --network-alias mysql 
	-v todo-mysql-data:/var/lib/mysql 
	-e MYSQL_ROOT_PASSWORD=secret 
	-e MYSQL_DATABASE=todos 
	mysql:5.7 `
	
En el tutorial muestra como chequear que esta up accediendo.


3.  Conectando MySql
Usa esto :[nicolaka/netshoot](https://github.com/nicolaka/netshoot) que sirve para resolver temas de networking.

container con esa imagen en la misma network:

`docker run -it --network todo-app nicolaka/netshoot`

`dig mysql` -> buscar IP del hostname mysql

Esto termina explicando que solo tenemos que conectar nuestra app a un host llamado mysql y se van a comunicar. 

4. Creando nuestro container con bind mount creo, y pasando las variables para que conecte a la base mysql.

`docker run -dp 3000:3000 \
	-w /app -v "%cd%:/app" \ 
	--network todo-app \
	-e MYSQL_HOST=mysql \ 
	-e MYSQL_USER=root \ 
	-e MYSQL_PASSWORD=secret \
	-e MYSQL_DB=todos \
	node:12-alpine \ 
	sh -c "yarn install && yarn run dev"`

Ahora si agregar productos en la app y vas a mysql podes ver como se van escribiendo.


### Docker Compose

> [Docker Compose](https://docs.docker.com/compose/) is a tool that was developed to help define and share multi-container applications. With Compose, we can create a YAML file to define the services and with a single command, can spin everything up or tear it all down.

El docker-compose.yml permite definir varias imagenes (services) a levantar, la red que los conecta, etc. Todo lo previo, en un solo paso para que sea sencillo.

En services uno va listando las imagenes. Acá no termino de entender si hace falta lo interno del dockerfile o queda suplantado por esto.
Le pasas todos los parametros que haga falta a cada uno. En el ejemplo por un lado tenés la app y por el otro la base.  
Parece que hay que tener un apartado de volumes tambien.

Una vez todo configurado se corre con
`docker-compose up -d`


### Ejemplo

Dockerfile del proyecto de MySportsFeed.

```Docker
# set base image (host OS)
FROM python:3.7.4   #imagen Python
RUN pip install --upgrade pip   # pip
RUN apt-get -y install git   # GIT, creo que es al pedo

  

# set the working directory in the container
# comentado porque no termine de ver cómo impacta
# https://stackoverflow.com/questions/55108649/what-is-app-working-directory-for-a-dockerfile
# WORKDIR /app

  

# copy the dependencies file to the working directory
COPY requirements.txt .

  

# install dependencies
RUN pip install -r requirements.txt


# copy the code and files to the working directory. Hace falta si no cambio el workdir?
COPY data/ get_data/ model/ online_results/ parse_data/ pinnacle/ utils/ *.py *.ipynb .gitignore .git README.md ./

# entiendo que con esto pones ROOT (/./) como punto de partida cuando importas modulos y archivos. Todo relativo a root
ENV PYTHONPATH "${PYTHONPATH}:/./"
```