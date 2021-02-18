# COMANDOS CON DOCKER.

## Como hacer limpieza en docker.

1. docker container prune => Elimina los contenedores detenidos.
2. docker rm -f {docker ps -aq} => Elimina todos los contenedores encendidos y apagados.
3. docker system prune => Elimina (contenedores, networks, imagenes, volumenes) que no se estan  usando.

## Comandos de administraciòn docker.

1. docker stats => Muestra cuanto consume de recursos el contenedor.
2. docker inspect {contenedor} => Inspecciona como configurado el componente.
3. .dockerignore => Es un archivo que evita subir archivos o carpetas en la imagen.
_____

## como definir un "stage" o fase llamada builder.
  Esta fase permite hacer las pruebas y crear imagenes productivas optimizadas.

  ```docker
  FROM node:12 as builder
  # copiando archivos necesarios
  COPY ["package.json", "package-lock.json", "/usr/src/"]

  # Ubicaciòn de ruta en la imagen
  WORKDIR /usr/src

  # Instalaciòn de dependencias definicidas para producciòn en archivo package.json
  RUN npm install --only=production

  # Copiando archivos desde maquina local a ruta destino 
  COPY [".", "/usr/src/"]

  # Instalaciòn dependencias ambiente desarrollo
  RUN npm install --only=development

  # Ejecutando tests
  RUN npm run test

  ## Esta imagen esta creada solo para pasar los tests.

  # Productive image
  FROM node:12

  # Copiando archivos seleccionados desde maquina local a ruta destino 
  COPY ["package.json", "package-lock.json", "/usr/src/"]

  # Ubicaciòn de ruta en la imagen
  WORKDIR /usr/src

  # Instalaciòn dependencias ambiente producciòn
  RUN npm install --only=production

  # Copiar  el fichero de la imagen anterior reutilizando los stage de las capas iguales.
  COPY --from=builder ["/usr/src/index.js", "/usr/src/"]

  # Exponer el puerto seleccionado
  EXPOSE 3000

  CMD ["node", "index.js"]
  ### En tiempo de build en caso de que algún paso falle, el build se detendrá por completo
  ```
## compartir carpetas o archivos en docker con Bind mounts
  
  ```bash
  docker run -d --name {nombre image} -v <path origen mi maquina>:<path destino el contenedor(/data/carpeta)>
  ``` 
## compartir carpetas o archivos en docker con Volumes
  ```
    - **1. docker volume** = Consulta los volumenes acturales
    - **2. docker volume create {name volume}** = Crea un volumen para ser usado.
    - **3. docker run -d --name {name_volume} --mount src={name_volume},{dst=/data/db}** = Montar un volumen en un contenedor.
  ```




___
## Comandos.

- Comando para visualizar los logs de los contenedores 
    ´´´docker logs --tail {Cantidad lineas} -f {contenedor}´´´ 

- Consultar la descripción completa del contenedor **docker inspect {contenedor}**
- Copiar archivos desde mi terminal al contenedor.
  - **docker cp {ruta_origen_local}/{archivo} {contenedor}:{ruta_destino}/nombre_archivo** Copiando Archivos
  - **docker cp {contenedor}:{ruta_contenedor}/nombre_archivo {ruta_origen_local}** Copiando Carpetas
  - REDES CON DOCKER
    - docker network create --attachable {nombre_red} => Crear red para conectar contenedores.
    - docker network inspect {nombre_red} => Inspeccionar elementos de la red
    - docker network connect {nombre_red} {contenedor}




