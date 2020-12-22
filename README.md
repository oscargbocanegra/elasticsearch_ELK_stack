# MANUAL ELASTICSEARCH

## Tabla de contenido.
1. [Elasticsearch](#Elasticsearch)
   1. [Arquitetura Elasticsearch](##Arquitetura-Elasticsearch)
   2. [Despliegue de Elasticsearch](##Despliegue-de-Elasticsearch)
   3. [Busquedas Combinadas](##Busquedas-Combinadas)
   4. [Prueba](##Prueba)
   5. [Prueba](##Prueba)
   
2. [Kibana](#Kibana)
3. [Logstash](#Logstash)
4. [Beats](#Beats)
---

# Elasticsearch

* Es una base de datos de busqueda distribuida en formato no relacional.
* Contiene una api que permite su consumo mediante protocolos  http.
* Elasticsearch usa una puntuacion (score) para la relevancia en la busqueda.
* Los principales usos son:
    * Analisis de Logs.
    * Metricas.
    * Analisis de seguridad 
    * Busquedas optimizadas.
--- 

 ## Arquitetura Elasticsearch.

 * Su arquitectura consta de los siguientes componentes:
    * Beats para realizar el envio de informacion a los diferentes servidores.
    * Logstahs como un componente que realiza los procesos ETL.
    * Elasticsearch Como base de datos no relacional, escalable.
    * Kibana Es un componente que permite realizar visualizar los datos y realizar procesos de analitica.
---

## Instalando Elasticsearch

* Para descargar Elasticsearch se puede realizar desde aqui [descarga](https://www.elastic.co/es/start) pagina oficial de Elastic
  
* Estructura y configuracion de archivos.
    * elasticsearch.yml
        + Configurando variable `cluster.name`
        + Configurando variable `node.name`
        + Configurando variable path.data
        + Configurando variable http.port
        + Configurando variable discovery-seed_hosts modo cluster.
        + Configurando variable seguridad con xpack.security.enabled
            + Activando elasticsearch-setup-passwords interactive.

    * jvm.options
        + Xms2G Definiendo memoria RAM
    * Comunicaciones Elasticsearch.
        + Puerto 9200
        + Puerto 9300 
---
## Desplegando Kibana

* Para descargar kibana se hace desde la url del producto que es la siguiente:
* Configuracion kibana.yml
    + Configurando seguridad con xpack.security.enabled
        + Activando elasticsearch-setup-passwords interactive
---

## CRUD en Elasticsearch.

* Documentos en Elasticsearch.
    + Indice
        + Que es un Indice
        + Como crear un Indice.  
    + Shard.
    + Replicas.
    + Segmentos.
    + Documentos.
        + ***Que es un Documento :*** es un objeto en formato JSON se almacena en Elasticsearch asignando un unico ID este puede ser asignado por el usuario o autogenerado.
        + Como actualizar un Documento.
            + POST Se usa para actualizar parcialmente los campos 
            + PUT  Se usa para actualizar el documento completo.
        + Como leer un Documento.
        + Como Leer un documento.

* POST
    + ***Metodo bulk :***
        ```bash
        curl -XPOST -u {user}:{pass} localhost:9200/_bulk -H "Content-type: application/json" --data-binary @restaurantes_es.json
        ```

* PUT
* DELETE
* GET
    + GET {index}/_doc/# documento
    + GET {indix}/_search/
---

## Consutas con Elasticsearch

  * _`Match query :`_ Es la consulta basica sobre elasticsearch. Tiene como ventaja las busquedas en campos tipo texto que tienen como fuente (numeros o fechas).

      - Ejemplo sin fiiltros. Por defecto el operator es `"OR"`
      ```JSON
      {
      "query":{
          "match":{
              "DESCRIPCION":"cocina tradicional"
              }
          }
      }
      ```

      - Ejemplo con filtro `"AND"` para minimizar la busqueda
      ```JSON
      {
      "query":{
          "match":{
              "DESCRIPCION":{
                  "query":"cocina tradicional",
                  "operator": "and"
                  }
              }
          }
      }
      ```

      - Ejemplo con filtro `"MINIMUM_SHOULD_MATCH : 2"` para buscar palabras que coincidan con el campo DESCRIPTION debe aparecer como minimo 2 de las palabras mencionadas en el query
      ```JSON
      {
      "query":{
          "match":{
              "DESCRIPCION":{
                  "query":"cocina tradicional",
                  "minimum_should_match": 2
                  }
              }
          }
      }
      ```

      - Ejemplo con filtro `"MATCH_PHRASE"` para buscar la frase completa mencionada en el campo DESCRIPCION
      ```JSON
      {
      "query":{
          "match":{
              "DESCRIPCION":{
                  "query":"cocina tradicional",
                  "minimum_should_match": 2
                  }
              }
          }
      }
      ```

      - Ejemplo con filtro `"SLOP:1"` para indicar cuantas palabras internmedias puede contener entre cocina tradicional {intermedio} mencionada en el campo QUERY
      ```JSON
      {
      "query":{
          "match":{
              "DESCRIPCION":{
                  "query":"cocina tradicional",
                  "minimum_should_match": 2
                  }
              }
          }
      }
      ```

      - Ejemplo de un Resultado
      ```JSON
      {
          "_index": "restaurantes",
          "_type": "doc",
          "_id": "T2QZiHYB4s6UTyM2WD54",
          "_score": 2.9040456,
          "_source": {
              "ID": 91,
              "FECHA_MODIFICACION": "2020-08-08T23:11:17.816Z",
              "NOMBRE": "Bodegón Lagunetas",
              "DESCRIPCION": " El Bodegón Lagunetas está situado en la zona comercial de la Calle Triana. Su especialidad es la cocina tradicional canaria.",
              "DIRECCION": "Constantino 16 35002 Las Palmas de Gran Canaria",
              "WEB": null,
              "TELEFONO": "+34 928363094",
              "PRECIO": "Económico",
              "POIS": "28.1058202, -15.416995300000053",
              "LATITUD": 28.1058202,
              "LONGITUD": -15.4169953,
              "HORARIO": "De lunes a viernes de 07:30 a 23:30 horas. Sábados de 07:30 a 17:00 horas. El primer domingo de cada mes.",
              "ESPECIALIDAD": null,
              "uri": "http://datosabiertos.laspalmasgc.es/api/datos/restaurantes/91.csv"
          }
      }
      ```

  * **Range query :** Es un query en formato consulta que se utiliza para busquedas de tipo fecha o numero. 

      - Las consultas de tipo fecha se pueden buscar por String o date math.
      - Date math = Lenguaje user friendly

      - Ejemplo con filtro `"range: {"gte":"valor", "lte":"valor"}"` donde `gte` indica el valor >= y `lte` <= 

          ```JSON
              {
              "query":{
                  "match":{
                      "DESCRIPCION":{
                          "query":"cocina tradicional",
                          "minimum_should_match": 2
                          }
                      }
                  }
              }
          ```

      - Expresiones `"Date Math"`

          | Formato | Valor |
          | -- | -- | -- |
          |y|años|
          |M|meses|
          |w|semanas|
          |d|days|
          |h o H|horas|
          |m|minutos|
          |s|segundos|


## Busquedas combinadas.

* Para utilizar busquedas combinadas se usa la funcion `"BOOL QUERY"`, esta funcion cuenta con las siguientes clausulas. `"MUST, SHOULD & FILTER"`.
  
  * `"must :"` Restringe la busqueda dejando como obligatorio el retorno de un valor.
    ```JSON
    {
        "query": {
            "bool": {
                "must": [
                    {
                        "match_phrase":{
                            "DESCRIPCION": "cocina tradicional"
                        }
                    }
                ],
                "filter":[
                    {
                        "range": {
                            "FECHA_MODIFICACION":{
                                "gte": "2020-08-01",
                                "lte": "now"
                            }
                        }
                    }
                ]
            }
        }
    }
    ```

  * `"must_not :"` Es lo contrario al must donde se excluyen documentos que no sean necesarios esten en la busqueda.
    ```JSON
    prueba
    ```
      
  * `"should : "` Es la clausula que  cuenta con mas permisos, No excluye documentos.
    ```JSON
        {
        "query": {
            "bool": {
                "must": [
                    {
                        "match":{
                            "HORARIO": "lunes"
                        }
                    }
                ],
                "should":[
                    {
                        "match":{
                            "DESCRIPCION": "canaria"
                        }
                    },
                    {
                        "match":{
                            "DESCRIPCION": "portuguesa"
                        }
                    }
                ], 
                "minimum_should_match": 1
            }
        }
    }
    ```

  * `"filter :"` Perminte hacer filtros por diferentes rangos.
    ```JSON
    ```
    



        




    








# Recopilando comandos de consulta.

+ http://localhost:9200/
+ http://localhost:9200/
+ http://localhost:9200/
+ http://localhost:9200/
+ http://localhost:9200/

