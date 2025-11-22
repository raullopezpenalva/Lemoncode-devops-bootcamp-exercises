# Reto 1: Creación del contenedor MongoDB y conexion con el backend

## 1- Comando para crear la Docker network para el challenge

Aquí he creado una red con el nombre indicado para el challenge y de tipo driver. De forma automatica se establece el rango de subred y mascara.

```bash
docker network create --driver bridge lemoncode-challenge
```

## 2- Creamos el volumen persistente para el MongoDB

```bash
docker volume create some-mongoDB
```

## 3- Comando para ejecutar el contenedor MongoDB

Ejecuto el comando para hacer pull de la imagen MongoDB oficial con el tag :noble (por similitud de que mi host es un Ubuntu noble, pero he visto que el tag :latest es el mismo que el noble para amd64).

Pongo de nombre del contenedor some-mongo que es el que exige el challenge y ademas es el mismo que usan en la documentacion de la imagen oficial.

Expongo el puerto 27017 que es el por default del mongo y el que exige el challenge para así poder conectarse el backend que corre en el host local.

Enlazo el volumen some-mongoDB que he creando anteriormente con la ruta /data/db del contenedor.

Ordeno que se conecte en la red Docker "lemoncode-challenge".

```bash
docker run --name some-mongo -p 27017:27017 -v some-mongoDB:/data/db --network lemoncode-challenge -d mongo:noble
```
## 4- Arrancar backend (node.js) en local

Voy a la carpeta donde estan el package.json del backend y ejecuto el comando:
```bash
npm install
```
Con esto instalo las dependencias necesarias para poder correr el backend

Despues ejecuto el backend con:
```bash
npm start
```
## 5- Comprobaciones del CRUD

A continuacion muestro las pruebas realizadas con Rest client.

### 5.1 - GET 


GET http://localhost:5000/api/topics HTTP/1.1


**Response:**

```json
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 98
ETag: W/"62-oQGJnQHXgkLtl0c2vQ0ptYf5Cfg"
Date: Sat, 22 Nov 2025 17:30:21 GMT
Connection: close

[
  {
    "Name": "Devops",
    "id": "6921f00b67275baba1f12985"
  },
  {
    "Name": "K8s",
    "id": "6921f04b67275baba1f12986"
  }
]
```
### 5.2 - POST 1

POST http://localhost:5000/api/topics HTTP/1.1
content-type: application/json

{
    "TopicName": "Docker images"    
}

**Response:**
```JSON
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 62
ETag: W/"3e-tU9O736bDh/J3jTmuZzH1ZuQHs0"
Date: Sat, 22 Nov 2025 17:35:38 GMT
Connection: close

{
  "TopicName": "Docker images",
  "_id": "6921f46a67275baba1f12987"
}
```
### 5.3 - POST 2

POST http://localhost:5000/api/topics HTTP/1.1
content-type: application/json

{
    "TopicName": "Docker volumes"    
}

**Response:**
```JSON
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 63
ETag: W/"3f-9ebB0dVRAlXbdFN7bjlTM7o/Iss"
Date: Sat, 22 Nov 2025 17:53:22 GMT
Connection: close

{
  "TopicName": "Docker volumes",
  "_id": "6921f89267275baba1f12988"
}
```
### 5.4 - POST 3

POST http://localhost:5000/api/topics HTTP/1.1
content-type: application/json

{
    "TopicName": "Docker networking"    
}

**Response:**
```JSON
HTTP/1.1 200 OK
X-Powered-By: Express
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=utf-8
Content-Length: 66
ETag: W/"42-pBN5vQh7fnV++R3jnK5N6vlfqqg"
Date: Sat, 22 Nov 2025 17:55:05 GMT
Connection: close

{
  "TopicName": "Docker networking",
  "_id": "6921f8f967275baba1f12989"
}
```




