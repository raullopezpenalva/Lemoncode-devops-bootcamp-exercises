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