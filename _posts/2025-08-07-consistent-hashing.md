---
title: "⚖️¿Qué es Consistent Hashing?"
subtitle: "La técnica que utilizan Cassandra y DynamoDB para rebalancear datos en sistemas distribuidos"
date: "2025-08-07 07:55:46 +0200"
tags: ["consistent hashing", "rebalancing", "distributed system"]
post-cover: "/assets/img/posts/consistent-hashing/cover.png"
thumbnail-img: "/assets/img/posts/consistent-hashing/cover.png"
share-img: "/assets/img/posts/consistent-hashing/cover.png"
share-description: "Descubre el algoritmo de consistent hashing para redistribuir eficientemente la carga en sistemas distribuidos."
share-title: "Que es el consistent hashing"
---
### Un poco de contexto

Conforme pasa el tiempo, surgen problemas que afrontar en nuestra base de datos:

* La cantidad de queries por segundo aumenta, con lo que necesitas añadir más CPUs para poder aguantar la carga.
    
* La cantidad de datos aumenta, con lo que necesitas añadir más almacenamiento en discos o RAM.
    
* Cuando un servidor falla y es necesario que otros servidores asuman sus responsabilidades. Para ello tendremos que mover todos esos datos y peticiones de un nodo a otro nuevo.
    

➡️ **El proceso de mover toda esta carga de un nodo a otro dentro del clúster se conoce como rebalanceo.**

No importa el esquema que hayas utilizado partición de la base de datos, hay una serie de requisitos mínimos que se buscan al hacer dicho rebalanceo:

* Después del rebalanceo, la carga (datos, peticiones de lectura y escritura) debe de ser distribuida de manera equitativa entre los nodos del clúster.
    
* Mientras ocurre el rebalanceo, la base de datos debe de seguir aceptando peticiones de lectura y escritura.
    
* **No deberían de moverse más datos de los necesarios al realizar el balanceo. Así conseguiremos un rebalanceo más rápido y minimizaremos la carga por operaciones de E/S en disco y de red.**
    

Sabiendo que estas son las cualidades que se buscan para rebalancear, vamos ahora a introducir un ejemplo.

Imaginemos que tienes un sistema montado con tu cliente, servidor y base de datos. Pero tu plataforma se ha hecho muy popular y tus servidores pueden escalar horizontalmente tranquilamente, pero tu base de datos ya no da abasto. Tienes que distribuir los datos que había en la base de datos entre diferentes nodos, vamos, lo que se conoce como sharding.

Este sistema se vería tal que así:

![]({{ '/assets/img/posts/consistent-hashing/d232b752-5315-4b27-9dd5-ef6b1ee62710.png' | relative_url }})

Lo que tenía antes en una sola instancia de mi base de datos, ahora lo tengo que repartir en 3 nodos separados. ¿Cómo lo hago? ¿Qué criterio utilizo para saber en qué base de datos tengo que guardar un dato u otro? Es decir:

## ¿Qué estrategia de rebalanceo utilizo?

### Un primer intento: módulo N

El método más simple sería calcular el módulo (%) del hash de la key. Por ejemplo, si nuestra función para calcular el hash de una key devuelve un número en base 10, podríamos calcular el módulo N de ese número, siendo N el número de nodos que tenemos en total.

El algoritmo se vería tal que así:

* Primero, cogemos el id de la entidad y lo pasamos por la función hash que tenemos, lo cual lo convierte a un número.
    
* Ahora, cogemos ese número y calculamos el módulo (%) según el número de nodos que tengamos en nuestra base de datos.
    
* El resultado nos indica donde guardar dicho dato.
    

Un ejemplo donde queremos calcular donde van 3 keys y tenemos 3 bases de datos sería:

```plaintext
ID #15→ hash(15)→ 32→ 32 % 3 = 2 → Database 2
ID #99→ hash(99)→ 27→ 27 % 3 = 1 → Database 1
ID #66→ hash(66)→ 69→ 69 % 3 = 0 → Database 0
```

El problema con esta manera de particionar la base de datos es que si el número de nodos cambia, es decir, si la N cambia, vamos a tener que mover la mayor parte de las keys.

Por ejemplo, digamos que hash(12345)= 6. Si tenemos 3 nodos como en nuestro ejemplo, la key se asignará a la base de datos número 0 (porque 6 mod 3 = 0). Cuando añadas una base de datos más y tengas 4, tendrás que mover dicha key al nodo 2 (6 mod 4 = 2). Pero es que cuando añadas otra instancia más y tengas 5 nodos, tendrás que moverla al nodo 1 (6 mod 5 = 1). Y no es solo esa key, tendrás que mover de un lado a otro las keys de diferentes nodos cada vez que añadas o quites una base de datos.

Esto hace que el rebalanceo sea muy costoso y hace que movamos más datos de los necesarios durante el rebalanceo.

### Tener un número fijo de particiones

Una solución común es crear más particiones que número de nodos, y asignar varias particiones a cada nodo. Por ejemplo, nuestro clúster de 3 nodos podríamos dividirlo en 300 particiones y así se le asignarían 100 a cada nodo. Esto va a hacer que sea mucho más fácil rebalancear los datos cuando se añadan o eliminen nodos.

Cuando añadas un nodo, dicho nodo va a simplemente robar unas cuantas particiones del cluster. Y cuando quites un nodo, pues lo mismo pero al contrario. Tienes que redistribuir las particiones entre los nodos existentes.

![]({{ '/assets/img/posts/consistent-hashing/541a426f-9b11-45c0-9022-387071afb393.png' | relative_url }})

Solo las particiones enteras van a ser movidas entre nodos. El número de particiones no cambia, como tampoco cambia la partición que tiene asignada cada key. Mientras se hace la migración, las operaciones funcionan como en la configuración antigua hasta que se finaliza el cambio.

Aunque existe la posibilidad de dividir o combinar particiones, en la práctica muchas bases de datos eligen tener un número de particiones fijas desde que montas el sistema. Lo complicado es saber el número de particiones que vas a necesitar cuando tu sistema crezca. Pocas y largas particiones van a dificultar el balanceo, muchas particiones cortas van a generar mucho overhead. Lo ideal claro está, es encontrar un punto intermedio intentando predecir cómo va a crecer tu sistema.

Esta estrategia de balanceo es la que utilizan sistemas como Elasticsearch y Couchbase.

## Consistent hashing

La última estrategia que vamos a presentar y en la que nos vamos a centrar se denomina **consistent hashing**. La clave principal es que vamos a distribuir nuestros datos y las bases de datos que disponemos en un anillo circular.

1. Supongamos que, al aplicar el hashing a nuestras keys, los valores resultantes puede ir del 0 al 100.
    

![]({{ '/assets/img/posts/consistent-hashing/21948f4d-b55f-49cf-8ad5-f7fd6c8e4cef.png' | relative_url }})

2. Cuando ponemos dichos valores en un anillo circular, donde la posición final va justo antes de la primera posición, nos quedaría algo así.
    

![]({{ '/assets/img/posts/consistent-hashing/15558848-0489-4d99-8716-3a49b8ddf192.png' | relative_url }})

3. Ahora, distribuimos nuestros nodos de bases de datos a lo largo del anillo. Como tenemos 3 nodos, vamos a colocarlos en las posiciones 0, 33 y 66.
    

![]({{ '/assets/img/posts/consistent-hashing/b157cf17-580a-43cd-b421-253f45f783fb.png' | relative_url }})

4. Y finalmente, solo nos queda distribuir nuestros datos. Para saber en qué nodo van nuestros datos, tenemos que calcular el hash de dicho dato y, en vez de hacer el mod 3 de dicho resultado, buscaremos el valor de dicho hash en el anillo y nos movemos siguiendo el sentido de las agujas del reloj hasta que encontremos un nodo. Si tenemos una key = 115 y hash(115) = 93, el nodo donde irá dicho dato es la base de datos 1.
    

![]({{ '/assets/img/posts/consistent-hashing/b750965f-317a-40cf-8634-a246fec66213.png' | relative_url }})

### Cómo añadir y quitar nodos

Vimos anteriormente que con la estrategia módulo N, al quitar o añadir nodos tendríamos que mover la mayoría de keys de sitio. Ahora que utilizamos consistent hashing, esto no va a ser así. Dado que cada servidor va a ser responsable de un arco de la circunferencia, añadir un servidor va a ser tan simple como mover ligeramente cada servidor en el anillo. De esta manera, solo un pequeño subconjunto de keys van serán reasignadas a un servidor diferente del que estaban.

![]({{ '/assets/img/posts/consistent-hashing/82abd3d0-3164-464d-8ee1-e78fd00b1fd0.png' | relative_url }})

Así, conseguiremos reducir el número de datos que se mueven durante el rebalanceo y, por lo tanto, disminuirán el número de cache misses durante este. Quitar un nodo del anillo será la misma operación que añadir un nodo pero a la inversa.

Esta solución ya es mucho mejor a lo que teníamos con la estrategia de modulo N. Pero hay una manera incluso mejor de repartir los nodos, una forma que al realizar el rebalanceo, consigue una redistribución más uniforme de las keys y generar un menor impacto en nuestro anillo.

Y esa manera es:

### Virtual nodes

Los nodos virtuales (virtual nodes) son una extensión del consistent hashing que hace el sistema más resistente a distribuciones desequilibradas de datos en el anillo. La idea consiste en que cada base de datos va a ser representada por más de un nodo en el anillo. Esto lo podríamos conseguir con cualquiera de las siguientes 2 maneras:

* Añadiendo un prefijo a la base de datos al calcular su hash. Así, sin nodos virtuales estábamos calculando hash(DB 1), pero ahora tendríamos que calcular hash(DB 1\_0), hash(DB 1\_1), hash(DB 1\_2), etc.
    
* Calcular la posición de los nodos de cada base de datos aplicando más de una función hash. Es decir, para saber la posición “\*\_0” utilizaríamos una función hash, para la posición “\*\_1” utilizaríamos otra función diferente y así.
    

![]({{ '/assets/img/posts/consistent-hashing/08765a23-403b-4b4d-a901-a4a21864bd08.png' | relative_url }})

En el caso de que la base de datos 4 falle:

* Las keys en “BD 4\_1” serán distribuidas al nodo “2\_2” así que irán a la base de datos 2.
    
* Las keys en “BD 4\_3” serán distribuidas al nodo “3\_2” así que irán a la base de datos 3.
    
* Las keys en “BD 4\_2” serán distribuidas al nodo “1\_3” así que irán a la base de datos 1.
    
* Las keys en “BD 4\_0” serán distribuidas al nodo “1\_2” así que irán a la base de datos 1.
    

Como vemos, esto hace que la carga de una base de datos caída se distribuya de manera más equitativa entre los diferentes nodos físicos. Cuantos más nodos virtuales uses por base de datos, más uniforme será la redistribución de la carga.

## Conclusión

Con la llegada de los sistemas distribuidos, el consistent hashing es uno de esos algoritmos que solucionó un problema fundamental de manera elegante: cómo distribuir las carga entre bases de datos para luego minimizar la cantidad de datos que se redistribuyen al cambiar el número de servidores. Y todo esto simplemente poniendo los servidores en un anillo y siguiendo el sentido de las agujas del reloj.

Este algoritmo está integrado a día de hoy en tecnologías que utilizamos como Cassandra, DynamoDB o CDNs. Quizás no tendremos que implementarlo en los sistemas que construyamos en nuestro día a día, pero saber cómo funciona por adentro nos ayuda a tomar mejores decisiones cuando diseñemos sistemas distribuidos donde necesitemos particionar los datos.

### Fuentes

* [https://www.hellointerview.com/learn/system-design/deep-dives/consistent-hashing](https://www.hellointerview.com/learn/system-design/deep-dives/consistent-hashing)
    
* [Artur Ejsmont. *Web Scalability for Startup Engineers : Tips & Techniques for Scaling Your Web Application*. New York, Mcgraw-Hill Education, 2015.](https://www.goodreads.com/book/show/23615147-web-scalability-for-startup-engineers)
    
* [Xu, Alex. System Design Interview : An Insider’s Guide. 2020.](https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide)
    
* [Kleppmann, Martin. *Designing Data-Intensive Applications : The Big Ideas behind Reliable, Scalable, and Maintainable Systems*. Sebastopol, CA, O’Reilly Media, 2017.](https://www.goodreads.com/book/show/23463279-designing-data-intensive-applications)
