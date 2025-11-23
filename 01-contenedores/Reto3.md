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

Ejecuto el comando docker build con el tag `frontend-node:DockerChallenge` en directorio donde está el Dockerfile y el repositorio de node-frontend.

```bash
docker build --tag frontend-node:DockerChallenge .
```

**NOTA**: También hay un [.dockerignore](./lemoncode-challenge/node-stack/frontend/.dockerignore) para ignorar cosas innecesarias.

## 3- Arrancar el contenedor con el frontend

Descripción de los argumentos:

**-d** -> para arrancar el contenedor sin que se quede en terminal la linea de logs del contenedor.

**--name topics-frontend** -> especifico el nombre del contenedor.

**--network lemoncode-challenge** -> nombre de la red tipo bridge donde también está el contenedor de Mongo.

**-p 3000:3000** -> publica el puerto 3000 en el host para poder acceder desde fuera del contenedor al puerto 3000 interno del contenedor.

**--env-file .env** -> archivo [.env](./lemoncode-challenge/node-stack/frontend/.env) que está en el directorio de [frontend](./lemoncode-challenge/node-stack/frontend/), **NOTA IMPORTANTE: para que el comando tenga existo con esta sintaxis debes estar en el directorio donde se encuentra el arcivho `.env`, por el contrario no cogerá las variables de entorno, si no estas en el directorio correcto puedes usar ruta absoluta o relativa desde donde estés.**

**frontend-node:DockerChallenge** -> nombre de la imagen a ejecutar.

```bash
docker run -d \
--name topics-frontend \
--network lemoncode-challenge \
-p 3000:3000 \
--env-file .env \
frontend-node:DockerChallenge
```