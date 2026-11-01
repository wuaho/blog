---
title: "Hacer software es como…  Lecciones de 'Code That Fits In Your Head'"
date: "2026-01-11 15:29:19 +0100"
tags: ["ingeniería de software", "libros"]
show-preview: true
thumbnail-img: "/assets/img/posts/code-that-fits-in-your-head/cover.jpeg"
share-img: "/assets/img/posts/code-that-fits-in-your-head/cover.jpeg"
share-description: '¿Puede tu código caber en tu cabeza? Mis notas sobre "Code That Fits in Your Head": por qué el software no es como construir una casa ni cuidar un jardín'
---

Poco antes de terminar 2025, alcancé la cifra de 14 libros leídos, de los cuales 4 eran de desarrollo de software. Casi todos se centraban en arquitectura y diseño de alto nivel, así que buscaba algo más práctico a la hora de escribir código y con una perspectiva reciente.

**Code That Fits In Your Head** salió en 2021 y, como sabemos, nuestra industria cambia a pasos de gigante cada año. Este terminó siendo mi libro número 15 del año, las ideas que propone me parecieron tan interesantes que decidí que no podían quedarse solo en mi libreta de notas, así que ahí va este blog post.

🔗 Link al libro: [https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head](https://www.goodreads.com/book/show/57345272-code-that-fits-in-your-head)

📢 **RECOMENDACIÓN PARA EL LECTOR:** si esta leyendo esto, te sugiero que te leas los puntos 1,2,3 y 5 (como mucho). Si te gustan las ideas que contiene, prueba a leer el libro.

---

## 1\. La realidad de nuestra industria (capítulos 1-2)

El libro comienza rompiendo mitos sobre qué es realmente el desarrollo de software.

- ⭐ **Hacer software no es ni como construir una casa ni cuidar un jardín:** Cuando la gente compara hacer software con construir una casa o cuidar un jardín, esas metáforas son muy lejanas a la realidad. La industria del desarrollo del software no se asemeja a ninguna disciplina en la ingeniería. Aún no tenemos metodologías replicables que puedan ofrecer resultados de manera consistente.

  <img src="{{ '/assets/img/posts/code-that-fits-in-your-head/cover.jpeg' | relative_url }}" alt="Unas tijeras podando un arbusto" style="max-width: 60%;">

- ⭐ “**A mí me funciona cuando hago las cosas así”:** En el desarrollo de software actual, el 'cada maestro tiene su librillo' se ha convertido en una excusa para el subjetivismo. A diferencia de otras ingenierías, parecemos atrapados en un ciclo de experiencias individuales y contextos aislados. Este libro nace para buscar las leyes fundamentales que rigen nuestra disciplina, transformando la intuición personal en una ingeniería basada en principios (en lo que sea posible).
- **_El futuro ya está aquí, solo que no está distribuido de manera uniforme:_** Hay unas pocas empresas que están haciendo muy buen software y la mayoría, que siguen haciendo churros.
- 📝 **Warnings como errores:** Trata los warnings del linter o compilador como errores. Al principio frustra, pero mejora radicalmente la calidad de tu código y te hace mejor programador.

---

## 2\. El código es para ser leído y mantenido (capítulo 3)

Uno de los pilares del libro es la **legibilidad** y **sostenibilidad de tu código**.

- Un ordenador no es como un cerebro. Con tu cerebro puedes tener como 7 cosas en contexto en la cabeza, la RAM de un ordenador puede tener miles.
- ⭐️ **Sostenibilidad:** El software engineering debería de hacer el proceso de desarrollo de software más uniforme. Debería de mantener a la organización que hay detrás. Algunos programadores nunca consideran si el código que están escribiendo tiene valor o no. Otros tienen dificultades viendo resultados cuantificables que no sean inmediatos. La sostenibilidad es un punto intermedio.
- ⭐ **Optimiza para la lectura:** Pasas más tiempo leyendo el código que escribiéndolo. Cuando escribes código, tienes todo el contexto en la cabeza. Piensa que quien lee tu código no lo tiene, tienes que hacer que su lectura sea lo más fácil y comprensible posible.
- ⭐ **¿Cómo escribo código legible?:** No puedes confiar en tu intuición. Necesitas algo que puedas poner en práctica como el uso de heurísticas y checklists, eso es software engineering.

---

## 3\. Más allá de hacer código que funciona (capítulo 4)

- ⭐️ **La documentación debería de priorizar explicar el por qué una decisión se ha tomado**. Antes que la decisión que se ha tomado.
- ⭐️ **El objetivo no es escribir código rápido**. El objetivo es crear software fácil de mantener.
- ⭐️ **No utilices encapsulamiento con los DTOs:** El rol de un DTO es capturar los datos que llegan a una estructurar de datos, o ayudarnos a transformar una estructuras de datos a output. No deberías de utilizarlo para ninguna otra cosa.
- Está muy bien el análisis de código estático pero los falsos positivos vienen con dicho territorio. Está bien deshabilitar o suprimir avisos específicos pero no lo hagas a la ligera. Al menos, documenta por qué lo has hecho y si puedes, obtén feedback respecto a la decisión
- **⭐️ Smoke tests:** Aunque debas de estar a favor de crear tests automáticos, no deberías de olvidar los tests manuales. De vez en cuando, levanta el sistema y mira si echa fuego.
- 📝 Cuando el código de tus tests es muy grande, eso es un code smell.

---

## 4\. ¿Es el TDD una ciencia? (capítulos 5 y 6)

- Al escribir tests, utilizamos el método **Red - Green - Refactor**:
  - 🔴 **Red**: escribe un test que falle
  - 🟢 **Green**: escribe el mínimo código posible para que el test pase
  - 🛠️ **Refactor**: mejora el código sin cambiar su comportamiento
- **¿Por qué TDD es lo más parecido que tenemos a una ciencia?**
  - En el método científico, primero formulas una hipótesis en la forma de una predicción de un resultado no demostrado. Entonces realizas un experimento y mides el resultado. Finalmente, compruebas el resultado con la predicción que hiciste.
  - ¿No te recuerda a patrón **Arrange - Act - Assert** de los tests en programación?
- **⭐ Los tests te avisan cuando el sistema está funcionando como no queremos:** Cuando haces modificaciones al código, ¿cómo sabes que no estas cambiando el comportamiento externo? Es muy difícil probar que todos los casos posibles funcionan igual. Pero es muy fácil probar que un caso ya no funciona bien.Si un test automático que has hecho deja de funcionar después de un cambio, ya sabes que has roto algo.
- Cuantos más tests añadas, mejor describes el sistema que estás probando. Es como ir añadiendo puntos de coordenadas en un mapa para determinar la ubicación del objetivo. Pero para que funcione, tienes que cambiar tu perspectiva con cada medida.
- **⭐ La encapsulación es la idea de que deberías poder interactuar con un objeto sin tener conocimiento de sus detalles de implementación.** Esto te permite hacer cambios en la implementación, es decir, hacer refactor y pensar en el objeto de una forma abstracta.
  - **⭐ La noción más importante es que un objeto debería de garantizar que nunca va a estar en un estado inválido. Cuya responsabilidad no es de quien lo llama, sino del mismo objeto**, pues es él mismo quien mejor sabe lo que significa que sea válido y cómo garantizarlo
- ️**⭐ La ley de Postel**: **Cuando diseñas la interacción con un objeto, tienes que ser flexible con lo que recibes y conservador con lo que devuelves.** Tienes:
  - Pre-condiciones: estas son las condiciones que el cliente debe de cumplir antes de interactuar con el objeto. Son sus obligaciones.
  - Post-condiciones: estas son las garantías que el objeto ofrece tras haber interactuado con él. Cuanto mejores sean, mejor.

---

## 5\. La regla del 7 (capítulo 7)

- ⭐️ **La regla del 7:** Ante la premisa de que los humanos solo pueden tener 7 cosas en la cabeza, es decir, en contexto, deberíamos crear piezas de código que no hagan más de 7 cosas. Decimos 7, pero podrían ser 10 o el número que sea, siempre que se utilice el mismo número.
  - Así pues, podríamos acordar una complejidad ciclomática (cyclomatic complexity) de 7, cada vez que un cambio la exceda, lo rechazas.
- ⭐ **Cuantas más lineas de código, peor es el código:** Cuantas más líneas de código añadas, más líneas de código tendrán otras personas que ser capaces de leer y entender.
- **¿Qué es la cohesión en OOP?:** Las cosas que cambian con la misma frecuencia deberían ir juntas. Las cosas que cambian con diferente frecuencia van separadas.
  - Piensa en los campos de una instancia de una clase. La mayor cohesión se consigue cuando todos los métodos de la clase utilizan todos los campos de la instancia
- Se presenta un ejemplo donde un metodo `Validate` convierte un dto en una entidad `Reservation`. El método `Validate` es quien utiliza todo el contexto del dto para checkear (más bien parsear) dicho objeto y si todo va bien, te devuelve un `Reservation`, una representación más fuerte de los mismos datos

---

## 6\. Diseño de APIs y cómo evitar comentarios (capítulo 8)

- ⭐️ **Diseña APIs de tal forma que sea difícil usarlas mal:** No diseñes una navaja suiza.
- ⭐️ **No digas algo con un comentario cuando lo puedes decir con el nombre de un método:**
  - A ser posible, reemplaza un comentario con un método con un nombre que sea útil.
  - Al igual que los comentarios se vuelven obsoletos rápidamente, también lo hacen los nombres de los métodos. Cuando modifiques la implementación de un método, no olvides actualizar el nombre si es necesario.
- ⭐️ **No digas algo con el nombre de un método cuando lo puedes decir con el tipado:** Los nombres de los métodos son útiles pero no tienes que repetir lo que ya te dice el tipado del método.
  - - `getReservationOrNull(): Reservation?` ❌ \* `getReservation(): Reservation?` ✅

- ️⭐️ **Hay casos en los que un buen nombre de un método no va a ser suficiente para comunicar toda la intención**
  - **Utiliza comentarios** si quieres incluir por qué has decidido implementar el código de una forma en concreto.
  - **Utiliza el mensaje del commit** si quieres incluir las consideraciones que has tenido al realizar un cambio en el código.
  - **Utiliza documentación o el README** si quieres incluir cómo hacer set up del entorno de desarrollo o la misión que tiene dicho repositorio.
- 🚬 Cualquier tonto puede escribir código que una computadora pueda entender. Los buenos programadores escriben código que los humanos puedan entender.
- ⭐️ Hay una forma de hacer código que es **_Command Query Separation (CQS)_** que se basa en mantener separados tus Comandos y tus Queries. Así, no devuelves datos en los métodos que tienen side effects y no causas side effects en tus métodos que devuelven datos.

---

## 7\. Mejores commits y las pull requests (capítulo 9)

- ⭐️ **Los mensajes de commit son el mejor sitio para explicar el porqué un cambio se ha hecho o por qué se ha hecho así.**
  - Pon el mensaje del commit en imperativo.
  - Deja una línea en blanco y pon el resto en la descripción.
  - Regla 50/72: dejar el _summary_ de 50 caracteres y cada línea de la descripción en 72 caracteres.
  - Ejemplo:

```plaintext
Introduce TimeOfDay struct

This makes the roles of the constructor parameters to MaitreD clearer.
A large part of the TimeOfDay type was autogenerated by Visual Studio
```

- La pregunta principal a la que tiene que contestar una pull request es esta: **¿me parecerá bien tener que mantener estos cambios?**
- ⭐️ **Rechaza pull requests grandes.**
- ⭐️ **Revisar una pull request entre autor y reviewer no es una buena práctica, porque el reviewer debería ser capaz de leer el código y entenderlo sin ayuda alguna.**
- ⭐️ Se puede trabajar tanto en pair programming (2 personas) como en mob programming (3 o más). Esto hace que el código que escribís sea código con el que más de una persona del equipo está de acuerdo, y no reduce la productividad.

---

## 8\. Versionado e integración (capítulo 10)

- ⭐️ **Hazte un favor y leete la especificación el Semantic Versioning Specification:** Si, entero. No tardas más de 5 minutos. En resumen, utiliza el esquema `major.minor.patch`.
  - Solo incrementa la `major` cuando introduzcas _breaking changes_.
  - Incrementa la `minor` para indicar que has introducido una nueva _feature_.
  - Incrementar la `patch` indica que has arreglado un bug.
- Si estás trabajando en una _feature_ que toma mucho tiempo implementar, te verás tentado a desarrollarla en una _feature branch_.
  - No hagas eso, acabarás teniendo lo que se llama un _merge hell_ 🔥
  - En vez de eso, esconde la _feature_ tras un _feature flag_ 🚩 e integra frecuentemente

---

## 9\. Técnicas para arreglar bugs (capítulos 11-12)

- Hacer refactor del código en tests es más difícil que hacer refactor del código en producción. Con el código en producción sabes que tus tests cubren la lógica de negocio. Al cambiar los tests tienes que llevar más cuidado porque estas cambiando el comportamiento de tu sistema (sin tener tests que te protejan).
- ⭐️ **Para entender por qué ocurre un problema hay diferentes técnicas:**
  - **Utiliza un pato de goma:** explica el problema a un patito. Al explicarlo, quizás encuentras la solución. O mejor aún, pregunta en Stack Overflow.
  - **Date un descanso, ve a por café o a por aire fresco**. Muchas veces ese reseteo despeja la mente y vuelves con un enfoque fresco.
- En el desarrollo de software, "más tarde" significa nunca. Cuando aparece un bug, identificarlo debería ser tu prioridad. Para lo que estés haciendo y arréglalo.
- ⭐️ **Utiliza la técnica de bisection para arreglar bugs:**
  1. Encuentra una manera de detectar o reproducir el problema.
  2. Quita la mitad del código.
  3. Si el problema aún está presente, repite el paso 2
  4. Si el problema desaparece, restaura el código que has quitado.
- ⭐️ `git bisect`: Comando súper útil al que le das un rango (`git bad` y `git good`) e itera en los commits intermedios para localizar el error.
- ⭐️ **Aprende y utiliza estas prácticas universales, ya que no puedes utilizar herramientas de debugging en el entorno de producción.**

---

## 10\. CQRS, funciones puras y buenos logs (capítulo 13)

- Cuanto más compones objetos mediante anidamiento, menor parte de esta composición cabe en tu cerebro al mismo tiempo.
  ![]({{ '/assets/img/posts/code-that-fits-in-your-head/c7ade05a-6f45-4307-8877-0cdd20014fe5.png' | relative_url }})
- ⭐️ **Sigue el principio de Command Query Separation. No dejes que tus queries tengan efectos secundarios. Si tu función es** `TrySave`, lo cual indica que es un comando, no dejes que devuelva un boolean "por la cara".
- ⭐️ **Los métodos deterministas son aquellos en los que puedes calcular el resultado con solo saber el input (funciones puras).**
  - Tienen propiedades muy deseadas: puedes abstraerte de lo que hace la función y solo preocuparte del resultado cuando leas el código.
  - ⭐️ **Para funciones puras, la programación orientada a funciones es el rey.**
- ⭐️ **Mantén las queries no-deterministas y con efectos colaterales cerca de la frontera de tu sistema y haz uso de funciones puras para escribir lógica compleja.**
- ⭐️ **En cuanto a los logs:**
  - **Loguea los detalles de cada petición HTTP y su respuesta.** También toda interacción con la base de datos, incluyendo los inputs y el resultado entero.
  - **Loguea todas las funciones impuras pero nada más.** Loguea todo aquello que no puedas reproducir: fechas, hora local, números aleatorios, lectura de ficheros, etc.vi

---

## 11\. Las dailys, los descansos y escribir documentación (capítulo 14)

- **Las dailys deberían ser:** qué hice ayer, qué voy a hacer hoy y si tengo algún bloqueo.
- ⭐️ **Trabaja en intervalos de tiempo delimitados como de 25 minutos con descansos de 5.** No, no es exactamente el método pomodoro 🍅
  - Es importante que al tomarte estos descansos te levantes, des una vuelta si puedes o lo que sea
  - Las ideas o los eureka! Suelen pasar cuando estas haciendo deporte, corriendo o fregando los platos. No suelen ocurrir delante de la pantalla, sino cuando estás de pie
- ⭐️ **Las reuniones no escalan, la documentación sí:** En vez de poner una reunión cada vez que tengas que transferir conocimiento, escríbelo en un documento.
- El autor recomienda actualizar las dependencias regularmente, y así evitar actualizaciones grandes que tengan muchos breaking changes.
- Ley de Conway: cualquier organización que diseñe un sistema producirá inevitablemente un diseño que es copia de la estructura de comunicación de la organización.

---

## 12\. Seguridad y property-based testing (capítulo 15)

- ⭐️ **La seguridad en el software es como los seguros:** no quieres pagar por uno hasta que te arrepientes de no tenerlo.
- Usa el acrónimo **STRIDE** como checklist: _Spoofing, Tampering, Repudiation, Information disclosure, Denial of service y Elevation of privilege_.
- ⭐️ **Property-based testing:** testear campos por sus propiedades y no por valores concretos.
  - Si quieres incluir un test donde se pruebe con un valor negativo, ¿por qué utilizar el valor -1 en concreto?
  - ¿Qué pasaría si hubiese un framework que pudiese producir números negativos arbitrarios?
  - Eso es el property-based testing (e.g: FsCheck lib)
- ⭐ **Behavioural Code Analysis:** extrae información de Git para identificar patrones y problemas visibles solo con el transcurso del tiempo.

---

## 13\. Organización de directorios en un proyecto (capítulo 16)

- ⭐️ **¿Cómo debería organizar los ficheros?**
  - ➡️ **Simplemente pon todos los ficheros en una carpeta.**
  - Utiliza subcarpetas sencillamente para organizar el código
  - Los sistemas de ficheros son jerarquías (árboles) donde cada nodo solo puede tener un padre. Si pones algo en `Controllers`, no puede estar a la vez en `Calendar`.
- Si quieres encontrar ficheros, utiliza las funcionalidades de navegación de tu IDE.

---

## Conclusión personal

No espero que este sea el post más entretenido que he escrito jamás, es más, ¡espero que nadie esté perdiendo el tiempo leyendo esto! Aunque aún me falta mucha experiencia por ganar en el desarrollo del software, hay ideas en este libro que me han cautivado por completo:

- **El código como documento:** Enfocarlo todo a una buena lectura tiene todo el sentido del mundo. Al fin y al cabo, el código es un documento que alguien va a leer y debe entender qué está pasando sin esfuerzo.
- **La regla de las 7 cosas:** Esta limitación cognitiva me parece una de las lecciones más potentes. Resuena mucho con lo que se enseña en otras áreas, por ejemplo, en bibliografía sobre cómo mejorar presentaciones, se recomienda no mostrar más de 4 elementos en pantalla para no saturar a la audiencia. En el código, igual.
- **El** **Property-based testing:** había escuchado sobre este pero nunca lo he visto en los proyectos en los que he trabajado. Tengo ganas de probarlo en algún repo que tenga por ahí.

Las ideas de Mark Seemann resuenan con mi manera de entender el software y creo que de ahí nace mi entusiasmo por este libro. Si algo de esto te ha hecho clic, te invito a que le des una oportunidad a la lectura completa.

Si me quieres comentar cualquier cosa o conectar conmigo: [https://www.linkedin.com/in/juanjorequena/](https://www.linkedin.com/in/juanjorequena/)
