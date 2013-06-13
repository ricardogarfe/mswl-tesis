Te contesto debajo a las dudas que planteabas:

==================

* Procesos de desarrollo (y herramientas relacionadas)
** Introducción -> hablar de proceso iterativo e incremental
** Gestión de tareas: ¿qué hay que hacer? ¿quién tiene que hacerlo? -> Sistemas de gestión de tickets
** Código versionado -> repositorios de código
** TDD -> sistemas de CI para asegurar que los tests se pasan regularmente!!! 

En este apartado ¿ se debería explicar con distintos ejemplos las opciones disponibles de cada una de las herramientas ? es decir como un análisis de requisitos, pero claro está sin excederse. O, por otra parte una descripción de las herramientas elegidas para la forja y el porqué de su elección.

Yo creo que aquí basta con una somera descripción comentando que son las herramientas más ampliamente utilizadas para la gestión del software desde el código hasta las metodologías.

==================

* Forjas de desarrollo
** Qué es una forja
** Objetivos
** Problemática -> administración, costes...

** Problemática -> administración, costes, 
** Algunos ejemplos y sus limitaciones

En este apartado tengo información a partir de un trabajo que hicimos en el máster de análisis de forjas de desarrollo, por lo que estaría bien incluirlo de alguna manera y además de hacer hincapié en las limitaciones hablar sobre las 'bondades'.

Perfecto, si tienes ya cosas inclúyelas.
 
** Forjas online/offline -> ventajas y desventajas, o este apartado en problemática de costes ?

==================
 
* SCStack
** Arquitectura

Este punto sobre la arquitectura de la forja no me queda demasiado claro, es decir, no se definirla.

La arquitectura está definida en el documento de proyecto fin de carrera que te pasé. Básicamente la arquitectura está basada en un LDAP contra el que todas las demás herramientas de la forja se autentican. En tu caso, a esa arquitectura tienes que añadirle un Gerrit que gestiona repositorios git.
 
 
** Provisionamiento (puppet)
** Componentes

Componentes, ¿ aquí se definirían las herramientas a instalar ?

Efectivamente, qué herramientas instala la forja (Se supone que las has descrito en la parte de procesos de desarrollo de forma genérica, aquí se dan nombres concretos). 
 
==================

* Cómo llevar a cabo el desarrollo de un proyecto con SCStack (aquí sería interesante compararlo por ejemplo con github+travis con el que tienes experiencia). Creo que de esta parte estuviste escribiendo en el wiki la presentación que teníamos del proceso de desarrollo

Se podría hacer una comparación en paralelo de ambos procesos en distintas estructuras, incluso en la configuración ya que jenkins y travis se han de configurar para comunicarse con el repositorio en cuestión. Está bien este enfoque.
 
 
** Estructura de un proyecto:
*** Código fuente
*** Tareas
*** Binarios y/o fuentes empaquetados en lenguajes que no generan binarios
** Cómo crear el proyecto:
*** Proyecto en redmine
*** Repo git
*** jobs en jenkins
*** repositorios de archiva para binarios

Esta parte es buena ya que estuve jugando (aprendiendo) con la instalación de librerías de maven en una fase previa al build de travis  (que utiliza un nexus) para que estuviesen disponibles para el proyecto, con archiva y jenkins sería un camino paralelo.

Eso es.
 
** Dar de alta desarrolladores/Project Owners (usuarios de la forja)
** Proceso de desarrollo basado en ramas

Este punto (proceso de desarrollo basado en ramas) habría que ahondar más en el e incluso lo pondría de cabecera o a parte haciendo una comparación con otros sistemas de control de versiones, como lo ves ?

Hombre, si no se te va ya de madre estaría bien. Pero mide el esfuerzo. Si esta parte te mola más, puedes dedicarle más tiempo. Creo que es muy interesante, y era la idea inicial si no se nos hubiera ido tanto tiempo en la parte de provisionamiento... 
 
 
* Conclusiones

Conclusiones: Muchas y buenas ! sin hablar de las versiones de ruby .)

