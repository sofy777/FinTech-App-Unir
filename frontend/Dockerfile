# React requiere Node.js y npm para el proceso de build. Se usa node:18-alpine por su tamaño reducido
# y compatibilidad con react-scripts.
FROM node:18-alpine AS build

# Establecer directorio de trabajo
WORKDIR /app

# Copiar dependencias primero y aprovechar la caché de Docker para acelerar reconstrucciones cuando solo cambia el código fuente.
COPY package*.json ./

# Instalar todas las dependencias
RUN npm ci

# Copiar código fuente después de dependencias para optimizar caché.
COPY . .

# Ejecutar el build
RUN npm run build

# Usar Nginx para servir los archivos estáticos
FROM nginx:alpine

# Copiar archivos construidos desde la etapa anterior
COPY --from=build /app/build /usr/share/nginx/html

# Exponer puerto 80
EXPOSE 80

# Ejecutar Nginx en primer plano (no daemon) para que Docker pueda gestionar el proceso correctamente.
# 'daemon off' previene que Nginx se ejecute en background, asegurando que el contenedor no termine prematuramente.
CMD ["nginx", "-g", "daemon off;"]