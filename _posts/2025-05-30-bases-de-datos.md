---
title: "🛠️¿Qué base de datos debería usar?"
date: "2025-05-30 16:59:15 +0200"
tags: ["Databases", "NoSQL", "SQL"]
post-cover: "/assets/img/posts/bases-de-datos/cover.jpg"
thumbnail-img: "/assets/img/posts/bases-de-datos/cover.jpg"
share-img: "/assets/img/posts/bases-de-datos/cover.jpg"
share-description: "En este post voy a comparar brevemente bases de datos relacionales y basadas en documentos (aunque también mencionaré grafos)."
share-title: "¿Qué base de datos debería usar?"
---
<div class="callout"><span class="callout-emoji">⚠</span><div class="callout-text"><strong>ATENCIÓN:</strong> este artículo está muy desactualizado. No representa realmente todas las opciones que disponemos de bases de datos NoSQL actualmente y los casos de uso han cambiado un poco. No recomiendo leer este documento como guía.</div></div>

Estaba leyendo [*Designing Data-Intensive Applications*](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321) de Martin Kleppmann y me acordé de cuando, en mi última empresa, teníamos que elegir una base de datos. Básicamente era: **"vale, a buscar en internet qué base de datos escoger"**.

Pero creo que, en ese momento, me faltaban nociones básicas. Especialmente sobre cómo en el mundo del desarrollo de software hemos llegado hasta los distintos modelos de base de datos que existen ahora. Entender el **por qué** surgieron SQL y NoSQL ayuda bastante a saber **qué decisiones** toman los programadores al elegir una base de datos para su sistema.

En este post voy a comparar brevemente bases de datos **relacionales** y **basadas en documentos** (aunque también mencionaré grafos).

### Un repaso rápido al modelo relacional

Creo que todos conocemos el modelo relacional. Cuando sabes los requisitos del sistema, lo típico es empezar a normalizar los datos: es decir, **organizar los datos en tablas**, cada una con sus filas (registros) y columnas (atributos), y definir relaciones entre ellas usando *primary keys* y *foreign keys*.

Para interactuar con este tipo de base de datos se utiliza SQL (*Structured Query Language*), lo cual te permite hacer consultas declarativas como si trabajaras con conjuntos matemáticos.

### ¿Qué pasa con NoSQL?

El "rollo NoSQL" surgió por dos tendencias muy distintas:

* Bases de datos **basadas en documentos**, donde los datos vienen enteros en un solo documento y las relaciones entre documentos son poco frecuentes.
    
* Bases de datos **de grafos**, que van justo al otro extremo: intentan modelar casos donde todo puede estar relacionado con todo.
    

A día de hoy, los tres modelos (relacional, documentos y grafos) se usan bastante y **cada uno tiene sentido en su propio dominio**.

### ¿Qué hace especial al modelo basado en documentos?

Una base de datos basada en documentos permite **guardar estructuras anidadas** dentro del mismo documento padre. Por ejemplo, imagina un documento `User` que incluye un campo `Schools`, con una lista de colegios a los que fue ese usuario. Ahí estaríamos modelando una relación 1 a N directamente dentro del mismo documento.

En una base de datos relacional, eso serían dos tablas (`User` y `School`) relacionadas con una *foreign key*.

Cuando tienes relaciones simples (1 a 1, o incluso algunas N a N), **ambos modelos pueden funcionar**. La diferencia empieza a notarse cuando necesitas hacer muchas *joins* o cuando los datos están muy conectados.

### Pros y contras de los modelos

He de decir que, tras los años, las bases de datos SQL se intentan parecer más a las bases de datos basadas en documentos (permitiendo el uso de JSON en columnas, entre otros), y las bases de datos basadas en documentos se intentan parecer más a las SQL, así ambas consiguen mayor flexibilidad para sus casos de uso.

Aquí te dejo una mini tabla para que te hagas una idea rápida:

| Característica | Relacional | Documentos | Grafos |
| --- | --- | --- | --- |
| Flexibilidad del esquema | ❌ Baja | ✅ Alta | ✅ Alta |
| Relaciones complejas (N a N) | ✅ Natural con joins | ⚠️ Requiere lógica en la aplicación | ✅ Muy natural |
| Modelo cercano a tu caso de uso en app | ❌ Menos | ✅ Más cercano | ⚠️ Depende del uso |
| Escalabilidad horizontal | ⚠️ Más difícil | ✅ Más fácil | ⚠️ Complejo |

### Cosas que debes saber sobre las bases de datos basadas en documentos

* **Flexibilidad no significa caos**: aunque no tengas un esquema rígido, sigue siendo tu responsabilidad controlar qué estás metiendo en la base de datos. Se suele decir que en relacional se valida al escribir y en documentos al leer.
    
* **Evita documentos enormes**: aunque puedes guardar estructuras complejas (JSON, BSON, XML...), eso no significa que debas abusar. Si solo necesitas una parte del documento, la base de datos probablemente cargue todo. Además, cuando actualizas un documento, muchas veces se reescribe entero. Por eso suele recomendarse mantener los documentos **pequeños y bien pensados**.
    
* **Para relaciones N a N**, las bases de datos de documentos no son lo ideal. Puedes denormalizar, pero entonces mantener la consistencia se vuelve más complicado. Y si usas código en la aplicación para simular *joins*, el rendimiento baja y la complejidad del código sube.
    

### Conclusión rápida

* ¿Tu modelo tiene **datos muy conectados**? → Relacional o grafos.
    
* ¿Necesitas **flexibilidad**, escalabilidad y acceso rápido a una estructura de datos entera y concreta para tu caso de uso? → Modelo basado en documentos.
    
* ¿Tienes muchas relaciones complejas N a N? → El modelo en documentos se te va a quedar corto.
    

Y como resumen simple:  
👉 Para datos **muy interconectados**, el modelo en documento es 💩, el relacional es aceptable, y los grafos son lo más natural.
