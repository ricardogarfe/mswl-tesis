1. Introducción

3. Objetivos 17

¿ Que necesitamos ?

Constancia, metodología y facilidad de uso para para crear software de calidad a través del uso de las herramientas.

¿ Que aúna el proceso de desarrollo de software ?

Puntos en común

* Proyecto, usuarios, permisos y roles
* Carpetas compartidas
* ITS
* Repositorio de código
* Gestión de librerías
* Interoperabilidad

Metodologías y Calidad a aplicar

* TDD Test Driven Development
* CI - Integración Continua
* CD - Despliegues continuos

4. Procesos de desarrollo

Como proceso de desarrollo que se elige Iterativo e Incremental a través de TDD Integración Continuos y Despliegues Continuos adaptado a nuestras necesidades.

Software de Calidad . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 21
Proceso iterativo . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 23
Proceso incremental . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 25
Iterativo e Incremental . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 27
Fases del proceso . . . . . . . . . . . . . . . . . . . . . . . . . . . . 29
Desarrollo Iteración . . . . . . . . . . . . . . . . . . . . . . . . . . . 29

4.5. Gestión de tareas . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 32
4.6. Código versionado . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 35
4.6.1. Desarrollo por Ramas . . . . . . . . . . . . . . . . . . . . . . . . . . 36
4.7. TDD y CI . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 37
4.7.1. TDD . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 37
4.7.2. Integración Continua . . . . . . . . . . . . . . . . . . . . . . . . . . 40

5. ALM Tools - Forjas de desarrollo

¡ Aparecen las ALM Tools !

¿ Pero que es esto ? ¿ otro nombre ? ¿ más mareo ?

5.1. Historia . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 43
5.2. Qué es una forja . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 45

* Facilitan la obtención de los requerimientos y el desarrollo de un proyecto.
* Seguimiento del estado de un proyecto.

5.4. Estado del arte de forjas . . . . . . . . . . . . . . . . . . . . . . . . . . . . 49

* Nube de forjas

Problemas en algunas forjas . . . . . . . . . . . . . . . . . . . . . . 55
Conclusiones del estudio de forjas . . . . . . . . . . . . . . . . . . . . . . . 56

*Necesitamos una nueva forja que se ha de adaptar a nuestros requerimientos*

6. SCStack

¡Bienvendio SCStack! ¿ Reinventar la rueda ?

Facilitar la rueda mediante software libre

Integrantes del equipo: 

Con el 1
6.1. Arquitectura . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 58

	Consola de Administración . . . . . . . . . . . . . . . . . . . . . . . 58
	Servicio web REST . . . . . . . . . . . . . . . . . . . . . . . . . . . 59
	API . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 61

Con el 2
6.3. Componentes . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 63
	6.3.1. Usuarios, roles y grupos . . . . . . . . . . . . . . . . . . . . . . . . 64
	6.3.2. API OpenLDAP . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 64
	6.3.3. Gestión de Requisitos ITS . . . . . . . . . . . . . . . . . . . . . . . 65
	6.3.4. Plugins . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 67
	6.3.5. Repositorios de Código . . . . . . . . . . . . . . . . . . . . . . . . . 68
	6.3.6. Revisión de Código . . . . . . . . . . . . . . . . . . . . . . . . . . . 73
	6.3.7. Integración Continua . . . . . . . . . . . . . . . . . . . . . . . . . . 74
	6.3.8. Gestión de distribuciones y dependencias . . . . . . . . . . . . . . . 79ÍNDICE GENERAL

Con el 3
6.3.9. Desarrollador: Eclipse y Maven . . . . . . . . . . . . . . . . . . . . . 81

Nada nuevo hasta ahora... 
Con el 4
6.2. Aprovisionamiento (Puppet)

Con el 5
6.4. Pruebas y Validación Vagrant
	Pruebas instalación

7. Desarrollo de un proyecto

¿ Como lo hacemos ? ¿ los objetivos se consiguen ?

Crear un proyecto en 5 pasos transparentes:

*Pruebe a hacerlo en casa o donde quiera.*

* Alta usuarios
* Crear un proyecto
* Proceso de desarrollo basado en ramas
  * Release the kraken
* Releasing

8. Epílogo

Conclusiones

Crear un marco de trabajo para Integrar el uso de la metodología Iterativa e Incremental a través de las herramientas necesarias.

¿ Hemos llegado ? 

Se ha conseguido integrar de forma satisfactoria el proceso de desarrollo Iterativo e Incremental en el marco de trabajo de SCStack además de dotar a la forja de un potente sistema de inicialización y configuración a través de Puppet, permitiendo futuras incorporaciones, actualizaciones o sustituciones de módulos que pueden aportar mejoras a la herramienta.

Lecciones aprendidas

1. Este proyecto no hubiera sido posible sin el uso de soluciones FLOSS ya que se trata de un proyecto de integración y comunicación de diferentes tecnologías por lo que se ha tenido que investigar a través de cada módulo involucrado

2. Este desarrollo ha supuesto una inclusión en la virtualización de recursos y configuración de los mismos a través de Vagrant y Puppet respectivamente.

*Otro paso importante es el referente a la publicidad de la herramienta y sus casos de éxito, partiendo como ejemplo el uso de la misma en un entorno de producción en la empresa TSCompany*

3. Hemos disfrutado aprendiendo.

’Progress, far from consisting in change, depends on retentiveness. When change is absolute there remains no being to improve and no direction is set for possible improvement: and when experience is not retained, as among savages, infancy is perpetual. Those who cannot remember the past are condemned to repeat it.’

Trabajo Futuro

* Integración de una herramienta para el análisis de código,
* facilitando así la refactorización.
* Instalación modular de los componentes para no hacer un harakiri con un auto vendor-locking.
* Publicitar la comunidad y los casos de éxito.


