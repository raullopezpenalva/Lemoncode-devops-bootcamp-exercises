# ENTREGA DEL DOCKER CHALLENGE by Raúl López Penalva

## Introducción

En este este directorio del repositorio podemos encontrar mi laboratorio del modulo de contenedores. Es un reto que se divide en 4 sub-retos en los que consiste en poder crear contenedores desde una imagen del repositiorio oficial de DockeHub pasando por crear tus propias imagenes para contenerizar los repos aportados para el laboratorio con un reto final de orquestar todo con `docker compose`.

## Lemoncode challenge directory

El directorio *lemoncode-challenge* es copiado del repositorio del Bootcamp DevOps para poder usar el codigo necesario para poder llevar a cabo el laboratorio.

La app que tiene el directorio se compone de un frontend y un backend.

El directorio tiene dos stacks propuestos para el backend, uno en Node.js y otro en .NET. Para esta entrega yo solamente he usado el repositorio Node.js.

Dentro del propio directorio hay archivos añadidos por mi para poder llevar a cabo el reto.

## Estructura del repositorio

Principalmente la entrega se centra en los archivos denominados `Reto$.md`. Son archivos Markdown donde redacto todo el reto y tiene los enlaces a los archivos que se piden por reto.
```markdown
01-contenedores/
├─ Reto1.md
├─ Reto2.md
├─ Reto3.md
└─ Reto4.md
```
Los demás archivos que se piden están en:
```text
01-contenedores/
├─ compose.yml
└─ lemoncode-challenge/
    └─ node-stack/
        ├─ backend/
        │  ├─ .env
        │  ├─ Dockerfile
        │  └─ .dockerignore
        └─ frontend/
            ├─ .env
            ├─ Dockerfile
            └─ .dockerignore
```


## Entrega de los retos

### Reto 1 - Crear contenedor de MongoDB

Este reto consiste en crear un contenedor con mongo y que el backend que se ejecuta el local pueda conectarse.

La entrega de este reto es este fichero [Reto1](Reto1.md)

### Reto 2 - Crear imagen y correr contenedor del backend

Este reto consiste en crear una imagen en base al codigo del backend y despues ejecutar un contenedor y que este se conecte al contenedor de mongo por una red docker.

La entrega de este reto es este fichero [Reto2](Reto2.md)

### Reto 3- Crear imagen y correr contenedor del frontend

Este reto consiste en crear una imagen en base al codigo del frontend y despues ejecutar un contenedor y que este se conecte al contenedor del backend y use su API.

La entrega de este reto es este fichero [Reto3](Reto3.md)

### Reto 4 - Orquestar todo con Docker compose

Este reto consiste en hacer todo lo anterior pero desde docker compose.

La entrega de este reto es este fichero [Reto4](Reto4.md)