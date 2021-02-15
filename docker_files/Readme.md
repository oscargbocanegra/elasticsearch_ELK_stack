
# Comandos.

- Comando para visualizar los logs de los contenedores 
    ´´´docker logs --tail {Cantidad lineas} -f {contenedor}´´´ 
- Compartir carpetas en docker con **Bind mounts**
    ´´´docker run -d --name {nombre image} -v <path origen mi maquina>:<path destino el contenedor(/data/carpeta)>´´´ 
- Compartir carpetas en docker con **Volumes**
  - ´´´
    - **1. docker volume** = Consulta los volumenes acturales
    - **2. docker volume create {name volume}** = Crea un volumen para ser usado.
    - **3. docker run -d --name {name_volume} --mount src={name_volume},{dst=/data/db}** = Montar un volumen en un contenedor.
   ´´´ 
- Consultar la descripción completa del contenedor **docker inspect {contenedor}**
- Copiar archivos desde mi terminal al contenedor.
  - **docker cp {ruta_origen_local}/{archivo} {contenedor}:{ruta_destino}/nombre_archivo** Copiando Archivos
  - **docker cp {contenedor}:{ruta_contenedor}/nombre_archivo {ruta_origen_local}** Copiando Carpetas
  - 