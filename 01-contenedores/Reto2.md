# Reto 2: Contenerizacion del backend Node.js y conexion con MongoDB por red docker

## 1- Creación Dockerfile

He creado un [Dockerfile](./lemoncode-challenge/node-stack/backend/Dockerfile) dentro del directorio raíz del repo de backend en Node.js

```Dockerfile
# Imagen base de Alpine con Node:20 pre instalado
FROM node:20-alpine

# Establecer el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar los archivos de dependencias y ejecutar npm install
COPY package*.json ./
RUN npm install

# Copiar el resto de los archivos de la aplicación al contenedor
COPY . .

# Exponer el puerto en el que la aplicación escuchará
EXPOSE 5000

# Comando para iniciar la aplicación
CMD ["npm", "start"]
```

## 2- Creación de la imagen

Ejecuto el comando `docker build` con el tag `topics-api:DockerChallenge` en directorio donde está el Dockerfile y el repositorio de node-backend.

```bash
docker build --tag topics-api:DockerChallenge .
```
**NOTA IMPORTANTE**: He creado un archivo [.dockerignore](./lemoncode-challenge/node-stack/backend/.dockerignore) que cuando hace `COPY` en el `Dockerfile` ignora esos directorios/archivos y no los mete dentro del contenedor de forma innecesaria.

## 3- Arrancar el contenedor con el backend

Descripción de los argumentos:

**-d** -> para arrancar el contenedor sin que se quede en terminal la linea de logs del contenedor.

**--name topics-api** -> especifico el nombre del contenedor.

**--network lemoncode-challenge** -> nombre de la red tipo bridge donde también está el contenedor de Mongo.

**-p 5000:5000** -> publica el puerto 5000 en el host para poder acceder desde fuera del contenedor al puerto 5000 interno del contenedor.

**-e DATABASE_URL=mongodb://some-mongo:27017** -> varible de entorno que especifica donde conectarse a la base de datos, `some-mongo` es el nombre del contenedor de  mongo y como la red docker tiene su propio servicio DNS se traduce a la IP asignada al contendor de Mongo en la red docker.

**-e HOST=0.0.0.0** -> Especificamos a la app que pueda recibir peticiones de cualquier IP, por default tiene solo localhost.

**topics-api:DockerChallenge** -> nombre de la imagen a ejecutar.

```bash
docker run -d \
--name topics-api \
--network lemoncode-challenge \
-p 5000:5000 \
-e DATABASE_URL=mongodb://some-mongo:27017 \
-e HOST=0.0.0.0 \
topics-api:DockerChallenge
```
## 4- Comprobación

Ejecuto con REST client: `GET http://localhost:5000/api/topics HTTP/1.1`

**Response:**
```JSON
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 415
ETag: W/"19f-LCO+ZJz2ASSPEzlbbxjSuyiRpiA"
Date: Sat, 22 Nov 2025 19:10:46 GMT
Connection: close

[
  {
    "Name": "Devops",
    "id": "6921f00b67275baba1f12985"
  },
  {
    "Name": "K8s",
    "id": "6921f04b67275baba1f12986"
  },
  {
    "TopicName": "Docker images",
    "id": "6921f46a67275baba1f12987"
  },
  {
    "TopicName": "Docker volumes",
    "id": "6921f89267275baba1f12988"
  },
  {
    "TopicName": "Docker networking",
    "id": "6921f8f967275baba1f12989"
  },
  {
    "TopicName": "Docker volumes",
    "id": "69220a851bbb8bc66b46ef23"
  },
  {
    "TopicName": "Docker volumes",
    "id": "69220ab31bbb8bc66b46ef24"
  }
]
```
Podemos comprobar que funciona el contenedor con la imagen creada.

Que el contenedor ha mapeado bien el puerto 5000 en el host para poder acceder desde el host o desde LAN. 

Que ha aplicado correctamente las variables de entorno porque el backend ha comunicado correctamente con el contenedor de Mongo por la red Bridge usando el nobre de dominio "some-mongo" y he podido acceder a la app desde el host por cambiar la variable HOST a 0.0.0.0

