# ENTREGA DEL LABORATORIO DE KUBERNETES by Raúl López Penalva

## Introducción

En este directorio del repositorio podemos encontrar mi laboratorio del modulo de orquestación con Kubernetes. Es un laboratorio que se divide en 3 ejercicios en los que consiste en poder orquestar una app desde un monolito sin memoria persistente a una app distribuida con memoria persistente.


## Estructura del repositorio

Principalmente la entrega se centra en los archivos denominados `README.md` dentro del directorio de cada ejercicio. Son archivos Markdown donde redacto todo el proceso y tiene los enlaces a los archivos que se piden por ejercicio.
```markdown
02-orquestación/
├── 01-Ejercicio1/
│   ├── README.md
│   ├── deploy-todo-app.yaml
│   ├── svc-todo-app.yaml
│   ├── cgm-todo-app.yaml
│   └── images/
├── 02-Ejercicio2/
├── 04-Ejercicio3/
└── README.md
```
## Entrega de los ejercicios

### Ejercicio 1 - Monolito en memoria

Este ejercicio consiste en hacer un deploy de la app `todo-app` y exponerlo en un servicio *LoadBalancer*

La entrega de este ejercicio es este fichero [Ejercicio 1](./01-Ejercicio1/README.md)

### Ejercicio 2 - Monolito con persistencia de datos

Este ejercicio consiste en hacer un deploy de la app `todo-app` pero tiene persistencia de datos por una base de datos Postgres, esta debe usar un `PersistanceVolumenClaim` declarado con un StatefulSet.

La entrega de este ejercicio es este fichero [Ejercicio 2](./02-Ejercicio2/README.md)

### Ejercicio 3- Ingress: App distribuida

Este ejercicio consiste en hacer deploy de la app de forma distriuida y configurar un ingress para que re-dirigi al frontend o a la API dependiendo de cierta ruta o condiciones.

La entrega de este ejercicio es este fichero [Ejercicio 3](./03-Ejercicio3/README.md)


