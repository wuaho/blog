---
title: "Motores de bases de datos: OLTP vs OLAP"
date: "2025-06-01 05:41:33 +0200"
tags: ["bases de datos"]
thumbnail-img: "/assets/img/posts/motores-de-bases-de-datos/cover.jpg"
share-img: "/assets/img/posts/motores-de-bases-de-datos/cover.jpg"
share-description: "Vamos a hablar de algo un poco más amplio: los motores de bases datos. Los podemos dividir en dos grandes categorías: OLTP y OLAP."
share-title: "Motores de bases de datos: oltp vs olap"
---

Hoy vamos a hablar de algo un poco más amplio: los motores de bases de datos. A grandes rasgos, los podemos dividir en dos grandes categorías:

- **OLTP** (procesamiento de transacciones online): los que están **optimizados para procesar transacciones**.
- **OLAP** (procesamiento analítico online): los que están **optimizados para analíticas**.

Y sí, los patrones de acceso a datos en uno y otro son muy diferentes.

## OLTP: lo que maneja el usuario

Los sistemas OLTP suelen ser los que están de cara al usuario. Tienen que manejar muchas peticiones a la vez, así que las queries suelen pedir pocos registros, casi siempre por clave, para que el motor pueda usar un índice y responder rápido.

Aquí el cuello de botella suele estar en **el tiempo que tarda el disco en buscar el dato exacto**.

<img src="{{ '/assets/img/posts/motores-de-bases-de-datos/cover.jpg' | relative_url }}" alt="Interior de un disco duro" style="max-width: 60%;">

Dentro de este mundo OLTP hay dos formas comunes de organizar el almacenamiento:

- **Estructura tipo log**: no se sobreescriben ficheros, solo se añaden. Luego se eliminan ficheros obsoletos. Ejemplos: **SSTables, LSM-trees, LevelDB, Cassandra, HBase, Lucene**...
- **Update-in-place**: se sobreescriben páginas concretas en disco. El ejemplo clásico: **B-trees**, usados en la mayoría de bases de datos relacionales y muchas no relacionales también.

La ventaja de las estructuras tipo log es que convierten escrituras aleatorias en **escrituras secuenciales**, lo que es mucho más eficiente para escrituras en discos y SSDs. Por eso han ido ganando fama en los últimos años.

**Si la estructura tipo log va a ofrecer un mejor rendimiento en las escrituras, la estructura con update-in-place va a hacerlo en lecturas.**

## OLAP: lo que manejan los analistas

Aquí ya no estamos hablando de miles de usuarios haciendo clics, sino de almacenes de datos o _warehouses_ y otros sistemas de analíticas menos conocidos que utilizan los analistas de negocios. Los sistemas OLAP manejan muchas menos queries, pero cada una escanea millones de filas.

En estos casos el cuello de botella no es el tiempo de búsqueda, sino el **ancho de banda del disco**: cuántos datos puedes leer por segundo.

### ¿Y qué pasa con los índices?

Cuando tus queries requieren escanear secuencialmente una gran cantidad de filas, los índices dejan de ser tan relevantes. En cambio, se convierte mucho más valioso el poder **codificar los datos de forma compacta** y así poder minimizar la cantidad de datos que necesita la query leer del disco.

Por eso, en estos sistemas ha triunfado el **almacenamiento por columnas**. Permite leer solo las columnas necesarias, y además compactar los datos. Dado que en una misma columna van a haber muchos registros con el mismo valor, es mucho más facil compactar dichos datos. Esto hace que las queries vayan mucho más rápido.

Así quedaría una base de datos en base a columnas en vez de filas:

![]({{ '/assets/img/posts/motores-de-bases-de-datos/7f9b8a67-8651-4a44-9047-ead809519788.png' | relative_url }})

Y así quedaría comprimida utilizando **bitmap encoding** para una sola columna:

![]({{ '/assets/img/posts/motores-de-bases-de-datos/86a384c5-d139-4d2d-bd4f-5f56d67f7028.png' | relative_url }})

## Tabla resumen

| Característica             | OLTP                                       | OLAP                                         |
| -------------------------- | ------------------------------------------ | -------------------------------------------- |
| Casos de uso               | Apps de usuario, web, móviles              | Análisis de negocio, reporting               |
| Nº de queries              | Muy alto                                   | Bajo                                         |
| Datos por query            | Pocos registros                            | Millones de registros                        |
| Patrón de acceso           | Lectura por clave (índice)                 | Escaneo secuencial                           |
| Cuello de botella          | Latencia de búsqueda en disco              | Ancho de banda de disco                      |
| Almacenamiento recomendado | Por filas                                  | Por columnas                                 |
| Ejemplos de motor          | B-trees, LSM trees, LevelDB, PostgreSQL... | ClickHouse, Redshift, BigQuery, Snowflake... |
