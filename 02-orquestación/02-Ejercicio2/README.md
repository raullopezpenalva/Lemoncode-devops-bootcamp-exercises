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

Ejecuto el comando `docker build` con el tag `classes-api:DockerChallenge` en el directorio donde está el Dockerfile y el repositorio de node-backend.

```bash
docker build --tag classes-api:DockerChallenge .
```
**NOTA IMPORTANTE**: He creado un archivo [.dockerignore](./lemoncode-challenge/node-stack/backend/.dockerignore) que cuando hace `COPY` en el `Dockerfile` ignora esos directorios/archivos y no los mete dentro del contenedor de forma innecesaria.

## 3- Arrancar el contenedor con el backend

Descripción de los argumentos:

**-d** -> para arrancar el contenedor sin que se quede en terminal la linea de logs del contenedor.

**--name classes-api** -> especifico el nombre del contenedor.

**--network lemoncode-challenge** -> nombre de la red tipo bridge donde también está el contenedor de Mongo.

**-p 5000:5000** -> publica el puerto 5000 en el host para poder acceder desde fuera del contenedor al puerto 5000 interno del contenedor.

**-e DATABASE_URL=mongodb://some-mongo:27017** -> varible de entorno que especifica donde conectarse a la base de datos, `some-mongo` es el nombre del contenedor de  mongo y como la red docker tiene su propio servicio DNS se traduce a la IP asignada al contendor de Mongo en la red docker.

**classes-api:DockerChallenge** -> nombre de la imagen a ejecutar.

```bash
docker run -d \
--name classes-api \
--network lemoncode-challenge \
-p 5000:5000 \
-e DATABASE_URL=mongodb://some-mongo:27017 \
classes-api:DockerChallenge
```
## 4- Comprobación

Ejecuto con REST client: `GET {{host}}/api/classes`

**Response:**
```
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 2084
ETag: W/"824-zZeuJS9gMLuiKc1iTjm2tTFArxM"
Date: Sun, 23 Nov 2025 17:19:20 GMT
Connection: close

[
  {
    "_id": "692322e5246f1a2728746baf",
    "name": "Contenedores I",
    "instructor": "Gisela Torres",
    "startDate": "2025-10-17T18:00:00Z",
    "endDate": "2025-10-17T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69232338246f1a2728746bb0",
    "name": "Contenedores II",
    "instructor": "Gisela Torres",
    "startDate": "2025-10-24T18:00:00Z",
    "endDate": "2025-10-24T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69232f59246f1a2728746bb1",
    "name": "Contenedores III",
    "instructor": "Gisela Torres",
    "startDate": "2025-10-31T18:00:00Z",
    "endDate": "2025-10-31T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69232fa0246f1a2728746bb2",
    "name": "Contenedores IV",
    "instructor": "Gisela Torres",
    "startDate": "2025-11-07T18:00:00Z",
    "endDate": "2025-11-07T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69233027246f1a2728746bb3",
    "name": "Contenedores V",
    "instructor": "Gisela Torres",
    "startDate": "2025-11-14T18:00:00Z",
    "endDate": "2025-11-14T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "6923306e246f1a2728746bb4",
    "name": "Contenedores VI",
    "instructor": "Gisela Torres",
    "startDate": "2025-11-21T18:00:00Z",
    "endDate": "2025-11-21T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "6923375c246f1a2728746bb5",
    "name": "Azure Web Services I",
    "instructor": "Gisela Torres",
    "startDate": "2026-02-20T18:00:00Z",
    "endDate": "2026-02-20T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "692337a5246f1a2728746bb6",
    "name": "Azure Web Services II",
    "instructor": "Gisela Torres",
    "startDate": "2026-02-27T18:00:00Z",
    "endDate": "2026-02-27T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69233807246f1a2728746bb7",
    "name": "Kubernetes AKS",
    "instructor": "Gisela Torres",
    "startDate": "2026-03-13T18:00:00Z",
    "endDate": "2026-03-13T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "692338ed246f1a2728746bb8",
    "name": "SESIÓN IA I",
    "instructor": "Gisela Torres",
    "startDate": "2026-04-17T18:00:00Z",
    "endDate": "2026-04-17T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  },
  {
    "_id": "69233916246f1a2728746bb9",
    "name": "SESIÓN IA II",
    "instructor": "Gisela Torres",
    "startDate": "2026-04-24T18:00:00Z",
    "endDate": "2026-04-24T20:00:00Z",
    "duration": 2,
    "level": "Beginner"
  }
]
```
Podemos comprobar que funciona el contenedor con la imagen creada.

Que el contenedor ha mapeado bien el puerto 5000 en el host para poder acceder desde el host o desde LAN. 

Que ha aplicado correctamente las variables de entorno porque el backend ha comunicado correctamente con el contenedor de Mongo por la red Bridge usando el nobre de dominio "some-mongo".

Tambien utilizo el comando `docker logs` para poder ver los logs del contenedor.

```bash
docker logs classes-api
```
Se puede observar en los logs que se conecta a mongo por la red docker correctamente

![logs](./images/comprobacion-reto2.png)
