# Reto 3: Contenerizacion del frontend Node.js y conexion con el backend por red docker

## 1 - Creación Dockerfile

He creado un [Dockerfile](./lemoncode-challenge/node-stack/frontend/Dockerfile) dentro del directorio raíz del repo de frontend en Node.js

```Dockerfile
# Imagen base de alpone con Node.js 20
FROM node:20-alpine

# Directorio de trabajo dentro del contenedor
WORKDIR /app

# Copiar archivos de dependencias
COPY package.json package-lock.json ./

# Instalar dependencias
RUN npm install

# Copiar el resto de la aplicación
COPY . .

# Exponer el puerto 3000
EXPOSE 3000

# Comando para iniciar la aplicación
CMD ["npm", "start"]
```

## 2- Creación de la imagen

Ejecuto el comando docker build con el tag `frontend-node:DockerChallenge` en el directorio donde está el Dockerfile y el repositorio de node-frontend.

```bash
docker build --tag frontend-node:DockerChallenge .
```

**NOTA**: También hay un [.dockerignore](./lemoncode-challenge/node-stack/frontend/.dockerignore) para ignorar cosas innecesarias.

## 3- Arrancar el contenedor con el frontend

Descripción de los argumentos:

**-d** -> para arrancar el contenedor sin que se quede en terminal la linea de logs del contenedor.

**--name classes-frontend** -> especifico el nombre del contenedor.

**--network lemoncode-challenge** -> nombre de la red tipo bridge donde también está el contenedor de Mongo.

**-p 3000:3000** -> publica el puerto 3000 en el host para poder acceder desde fuera del contenedor al puerto 3000 interno del contenedor.

**--env-file .env** -> archivo [.env](./lemoncode-challenge/node-stack/frontend/.env) que está en el directorio de [frontend](./lemoncode-challenge/node-stack/frontend/), **NOTA IMPORTANTE: para que el comando tenga existo con esta sintaxis debes estar en el directorio donde se encuentra el archivo `.env`, por el contrario no cogerá las variables de entorno, si no estas en el directorio correcto puedes usar ruta absoluta o relativa desde donde estés.**

**frontend-node:DockerChallenge** -> nombre de la imagen a ejecutar.

```bash
docker run -d \
--name classes-frontend \
--network lemoncode-challenge \
-p 3000:3000 \
--env-file .env \
frontend-node:DockerChallenge
```

## 4- Comprobaciones

### 4.1 - Primera comprobación

Entro por browser a http://localhost:3000

![comprobacion1](./images/comprobacion1-reto3.png)

El frontend carga y vemos que sale el mensaje `Backend conectado (11 clases cargadas)`.

### 4.2 - Segunda comprobación

Miro los logs del frontend `docker logs classes-frontend`.

![comprobacion2](./images/comprobacion2-reto3.png)

Los logs demuestran que ha cogido las variables de entorno del archivo `.env` y la URL de la API es la correcta.

### 4.3 - Tercera comprobación

![comprobación3](./images/comprobacion3-reto3.png)

Podemos observar que solo se han copiado los archivos necesarios para el contenedor.

