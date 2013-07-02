% DONE 6.3.7. Integración Continua  

% DONE 6.5. Pruebas y Validación 

8. Desarrollo de un proyecto 79
8.1. Estructura de un proyecto . . . . . . . . . . . . . . . . . . . . . . . . . . . . 79
8.1.1. Código Fuente . . . . . . . . . . . . . . . . . . . . . . . . . . . . 79
8.1.2. Tareas . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 79
8.1.3. Binarios . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 79
8.2. Crear un proyecto . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.2.1. Proyecto en redmine . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.2.2. Repositorio git . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.2.3. Jobs en Jenkins . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.2.4. Repositorios Archiva . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.3. Alta usuarios . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 80
8.4. Proceso de desarrollo basado en ramas .  



What is Jenkins?
In a nutshell Jenkins CI is the leading open-source continuous integration server. Built with Java, it provides over 400 plugins to support building and testing virtually any project.


h1. Proceso de Desarrollo

Proceso de desarollo integrado en la forja **SidelabCodeStack**.

* "Introducción":001-git-introduccion.html: @Git@
* Visión general del "proceso de desarrollo con @Git@":002-proceso-git.html
* Gestión de repositorios "@Git@ con @Gerrit@":003-gerrit-git.html
* Proceso de desarrollo con "@Eclipse IDE@":003-eclipse.html
* "Integración continua":004-jenkins.html @Jenkins@

h1. Git

!images/git-logo.png!

**Git** es un SCM distribuido (DSCM) en donde cada desarrollador tiene una copia del repositorio.

p. No hay concepto de repositorio centralizado pero al final suele haberlo.

h2. Características

h3. Snapshots

En el repositorio **Git** no se guardan diferencias se guardan snapshots en comparación con Subversion.

!images/svn-git-comparison.png!

h3. Integridad

Los commits se identifican por un @hash sha1@

* @Svn@: _rev 33_
* @Git@: _d025a7b3217f05110ebbf48065b8d02a0ad22ae3_

O más amigablemente: _d025a7b_

Los ficheros también se identifican por su sha1 de esta forma si un fichero se corrompe durante la transmisión por la red se detecta inmediatamente

h3. Los 3 estados

Los ficheros en git pueden estar en tres estados:

* @Modificado@: el fichero ha cambiado desde el último checkout
* @Staged@: un fichero modificado ha sido marcado para ser añadido en el próximo commit
* @Committed@: el fichero se encuentra en la base de datos de git

Hay un 4º estado: **untracked**.

h3. Las 3 áreas de un proyecto git

# El directorio git (git directory):
** Contiene los metadatos y la base de datos de git
** Es lo que se copia cuando se clona un repositorio
** Normalmente es una carpeta .git en algún directorio
# La carpeta de trabajo (working directory):
** Es un checkout de una versión específica del proyecto
** Se extrae del directorio git
** Es el espacio donde modificamos los ficheros
# Staging area:
** Fichero en el directorio git que indica qué cambios van en el próximo commit

!(center)images/git-3-areas.png!

h3. La identidad

Git necesita conocer algunos datos del desarrollador (aparecen en los commits para identificar al autor)

* Nombre
* Email

Si no están correctamente configurados pueden aparecer varios problemas:

* Los commits **fallan** porque el usuario no está autorizado
* Commits del mismo usuario _"físico"_ **no son considerados como del mismo usuario** porque el nombre _"lógico"_ cambia.

Por lo que se ha de tener muy en cuenta este apartado y configurar correctamente en el archivo @~/.gitconfig@.

<pre><code class="shell">$ patxi@patxi-PORTEGE-R830:~$ cat .gitconfig 
[user]
    name = patxigortazar
    email = patxi.gortazar@gmail.com
$ git config --global user.name “patxigortazar”
$ git config --global user.email “patxi.gortazar@gmail.com”
$ cd [path-to-gitrepo]
[path-to-gitrepo]$ git config user.name “patxigortazar”
[path-to-gitrepo]$ git config user.email “patxi.gortazar@gmail.com”
</code></pre>

bq. "Follow The Yellow Brick Road":http://git-scm.com/book/en/Customizing-Git-Git-Configuration

h4. Comprobar la identidad

Who am I?

<pre><code class="shell">$ git config --list
user.name=patxigortazar
user.email=patxi.gortazar@gmail.com
</code></pre>

h3. Clientes git

* En Eclipse
** Egit (viene por defecto en las últimas versiones)
* CLI Linux client
** sudo apt-get install git
* Windows
** Msysgit: http://code.google.com/p/msysgit/downloads/
** Tortoise Git (requiere msysgit): http://code.google.com/p/tortoisegit/wiki/Download
* Mac
** SourceTree: http://www.sourcetreeapp.com/
** Gitbox (simple): http://www.gitboxapp.com/

h1. Visión general del proceso de desarrollo con Git

El proceso de Desarrollo se basa en canales. Aprovechando las facilidades que aporta un repositorio distribuido podemos gestionar diferentes ramas e intercambio de información entre las mismas de forma eficiente.

h2. Desarrollo en canales

Partiendo de 2 ramas de forma continua:

* **master:** Desarrollo limpio. Sólo versiones estables.
* **develop:** El desarrollo inicial de la versión actual tiene lugar aquí.

Gestión de las ramas para estabilización de las versiones:

* **release-0.1**,**release-0.2**; una rama de estabilización cada vez

h3. Proceso de estabilización

El proceso de estabilización se gestiona a través de las _ramas de estabilización_:

* Estabilización del código (_RC release candidates_)
* Arreglar bugs (hotfixes)
* Cuando la versión se considera estable se procede al siguiente paso:
** Tag
** Mezclar (merge) con development
** Mezclar (merge) con master
* Si surgen nuevos bugs se vuelve a repetir el **proceso de estabilización**:
** Se arreglan en la misma rama (release-0.1)
** Nuevo tag y mezcla

h3. Releasing

Para crear una Release se define un proceso de gestión a través las ramas:
 
* Checkout del tag
* Build (Jenkins)
* Deploy (Jenkins)

h3. Diagrama de desarrollo

El flujo de trabajo a través de las ramas se representa en este diagrama:

!images/flujo-desarrollo-git-000.png!

h1. Gestión de repositorios Git con Gerrit

El primer usuario que se loguea obtiene privilegios de administrador. Esto es importante si el primero que se loguea lo hace desde el ldap. Idealmente, debería hacerlo el usuario gerritadmin (ver pasos previos).
Gerrit tiene el concepto de grupo, de forma que si asigno permisos a un grupo para un repositorio, todos los miembros del grupo adquieren esos permisos.

Los repositorios en jerga Gerrit se denominan proyectos. Pueden crearse vacíos o con algunas ramas ya preparadas. En nuestro caso todo proyecto "nacerá" por simplicidad con dos ramas:

* @master@
* @develop@
 
Estos repositorios definen los permisos de acceso en base a referencias (@refs/heads/develop@, por ejempo, es la referencia de la rama *develop*) que se pueden definir con wildcards como @refs/heads/*@ (todas las referencias dentro del directorio @refs/heads@). Hay diferentes tipos de permisos, más abajo se explica cómo crear un proyecto con unos permisos razonables para poder funcionar.

h2. Pasos previos

El primer usuario que accede a Gerrit obtiene privilegios de administrador. Al instalar la forja, se recomienda crear un usuario **"gerritadmin"** y password **"t0rc0zu310"** y acceder con este usuario a Gerrit. Este usuario se convertirá en administrador automáticamente al hacer login. A partir de este momento, este será el usuario con el que crear los grupos y proyectos (repositorios) en Gerrit.

Obtener la clave pública del servidor para asignarla al usuario **gerritadmin**:

# _Settings -> SSH Public Keys -> Add_
# Copiar la clave del fichero @/opt/ssh-keys/gerritadmin_rsa.pub@.

Configurar permisos para creación de proyectos:

# Acceder a _Projects -> List -> All-Projects_.
# Seleccionar _Access_:
# _Editar_ -> _Add Group_ -> _Group name_ en:
** **refs/** añadir _Push_ el grupo _Administrators_.
** **refs/meta/config** añadir a _Read_ el grupo _Administrators_.
# Save Changes.

h2. Crear un proyecto

La Consola de Administración es la encargada de gestionar los repositorios @Git@ mediante @gerrit@. Por lo que la creación de un proyecto Git va asociada a la creación de un nuevo proyecto en la consola.

* Se ha de elegir un nuevo proyecto asociado a un usuario _administrador_.
* Marcar _proyecto con repositorio_.
* Elegir repositorio de tipo **Git**.
* Grupo de _Usuarios_ del proyecto.
* **Crear proyecto**.

h2. Comenzar a trabajar en un proyecto

Asegurarse del mail que se utiliza para la identificación de la autoría de los commits

bq. <pre><code class="shell">git config --global user.email "usuario@<dominio-mail.com>"
git config --global user.name "usuario"
git clone ssh://usuario@<dominio.sidelabcode>:29418/<projectname>
cd <projectname>
echo “hello git” > greetings.txt
git add greetings.txt
git commit - "mensaje de commit obligatorio."
</code></pre>

Hasta aquí hemos hecho un commit en el repo local. Para subir este commit al repo remoto (el que hemos clonado y que se identifica como origin):

bq. <pre><code class="shell">git push
</code></pre>

h1. Proceso de desarrollo con Eclipse

Prerequisitos:

Se recomienda utilizar la versión de Eclipse **Spring Tools Suite 3.1** de Spring:

* http://www.springsource.org/downloads/sts-ggts

Se ha de escoger la opción basada en **Eclipse 3.8.1** ya que incluye la instalación de las herramientas relacionadas con @Git@ como es el caso del plugin "Egit":http://www.eclipse.org/egit/.

Para usar los ejemplos de la línea de comandos se han de instalar los clientes especificados para cada plataforma que se definieron en el apartado [Clientes git]

h2. Paso 1 - Generación de claves

Se han de generar las claves para el acceso al repositorio remoto

**Ubuntu:**

pre. $ ssh-keygen -t rsa

* Copiar el contenido del fichero @~/.ssh/id_rsa.pub@ en el depósito de claves de la cuenta personal de **Gerrit**.

**Windows:**

pre. Git bash
ssh-keygen.exe

* Copiar el contenido del fichero @c:/documents and settings/<usuario>/.ssh/id_rsa.pub@ en el depósito de claves de la cuenta personal de **Gerrit**.

"Generating SSH Keys":https://help.github.com/articles/generating-ssh-keys

h2. Paso 2 - Clonar el repositorio

El administrador del proyecto ha creado un repositorio en **Gerrit** con dos ramas:

* master
* development

h3. Situación del repositorio

Clonar el repositorio remoto tiene consecuencias:

* El repositorio local guarda localmente información sobre el repositorio remoto (llamado por defecto “origin”)
* Esto permite _subir/bajar_ cambios _al/desde_ repositorio remoto
* Las ramas refs/heads/* del repositorio remoto se almacenan en el repositorio local como @refs/remotes/origin/*@.
* Ver @.git/config@

h3. Eclipse

A través de Eclipse se han seguir los siguiente pasos:

* Perspectiva Git repository exploring: @Window -> Open perspective -> Others -> Git Repository Exploring@
* Añadir la **URI** (la URI es de prueba, se ha de sustituir por valores reales) del repositorio y se autocompletan los demás campos: 
** ssh://patxigortazar@192.168.33.10:29418/filetransfer
* Branch selection: dejar ambas seleccionadas
* Initial branch: seleccionar **development**

!images/eclipse-clone-repo.png!

h3. Cliente Git

Clonar el repositorio remoto a través de la consola:

pre. $ git clone ssh://patxigortazar@192.168.33.10:29418/filetransfer

**Git** hace @checkout del master@ por defecto (porque es donde se encuentra el _HEAD_ posicionado en el repositorio remoto). Se ha de cambiar a la rama **development** para empezar a desarrollar:

pre. $ cd filetransfer
$ git checkout development
Branch development set up to track remote branch development from origin.
Switched to a new branch 'development'

h2. Paso 3 - crear el proyecto

# Se ha de crear un proyecto Java:
** @org.filetransfer@
# Crear un fichero de versión en la raíz
** Version.txt <- 0.1
# Crear un fichero @SFTPTransfer@ en el paquete @org.filetransfer@.
# Añadir el proyecto al repositorio git del proyecto @org.filetransfer@.
## Eclipse:
### @Team > Share project > Git@
### @Repository: filetransfer@
### Añadir los ficheros para que Eclipse haga tracking de los mismos
### @Team > Add to index@
## Cliente Git:
*** @$ git status@
*** @$ git add <file>@
# **Commit!**

h3. Commit

Eclipse:

# Sobre el proyecto > Team > Commit
# El comentario es obligatorio
# Chequear
## Que el autor es el correcto
## Que están marcados los ficheros adecuados
## Que *no está marcada* la casilla _"Push the changes to upstream"_.

Cliente Git:

pre. git commit

h2. Paso 4 - desarrollo de la versión actual

Añadir algún método más a la clase @SFTPTransfer@ y ejecutar el comando @git status@:

pre. $ git status
# On branch develop
# Your branch is ahead of 'origin/develop' by 1 commit.
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   modified:   SFTPTransfer.java
#
no changes added to commit (use "git add" and/or "git commit -a")

Hay ficheros no añadidos al staging area por lo que no se hará commit de ellos.

!images/eclipse-git-console-stage.png!

h3. git add

Añadir algún método más a la clase @SFTPTransfer@ y ejecutar el comando @git status@:

pre. git status

Hay ficheros no añadidos al staging area no se hará commit de ellos

pre. git add SFTPTransfer.java

Para añadirlos al staging area y que vayan en el próximo commit

pre. $ git status
# On branch develop
# Your branch is ahead of 'origin/develop' by 1 commit.
#
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   modified:   SFTPTransfer.java
#

En Eclipse este proceso se hace automáticamente al hacer **commit**.
En consola se puede forzar con el parámetro **-a** al ejecutar:

pre. git commit -a

h2. Paso 5 - subir cambios al repositorio remoto (push)

En este momento el repositorio local se encuentra “a 2 commits” del repositorio remoto.

!images/repo-2-commits-ahead.png!

Subir lo que hay a la rama develop de origin (el repositorio remoto)

* Eclipse: @Sobre el proyecto > Team > Push to upstream@
* Cliente git consola: @git push origin@

h2. Paso 6 - estabilización de la versión 0.1

Se ha de seguir el proceso para unificar los desarrollos del proyecto en una rama estable y crear el respectivo tag.

h3. Rama de Versión

Crear un branch para la versión.

* Eclipse:
** Sobre el @proyecto > Team > Switch to > New branch@
*** From: refs/heads/development
*** Branch name: release-0.1
*** Asegurarse de que checkout new branch está activado
* Cliente git:
** @git checkout -b release-0.1 --track@
** @git push -u origin release-0.1@

El código del workspace señala ahora la versión **release-0.1**.
* Hacer algún cambio
* Commit  

h3. Actualizar desarrollo

* Cambiar en la rama development la **versión a 0.2**.
* Sobre el @proyecto > Team > Switch to > development@.
* Modificar el fichero @version.txt@.
* @Commit@
* @Push to upstream@

h3. Crear el Tag

Preparar la RC1 (_Release Candidate 1_).

* Sobre el @proyecto > Team > Switch to > release-0.1@
* Cambiar el nombre de la versión a **0.1.0-RC1**.
** @Commit@
** @Push to upstream@

Hacer el tag.

* Eclipse:
** @Team > Advanced > Tag > 0.1.0-RC1@
** @Team > Remote > Push... > Next > Add all tags spec@
* Cliente Git:
** @git tag -a 0.1.0-RC1 -m 'Release 0.1.0-RC1‘@
** @git push origin 0.1.0-RC1@

Ejecutar el ciclo de **Build/test/deploy...**

h2. Paso 7 - publicar la versión 0.1.0

Si el desarrollo, las pruebas y los despliegues son correctos en la rama **RCx**:

* Cambiar el nombre de la versión a **0.1.0**.
** Commit
** Push to upstream
Hacer el tag.
* Eclipse:
** @Team > Advanced > Tag > 0.1.0@
** @Team > Remote > Push... > Next > Add all tags spec@
** Marcar _"Save in origin"_ para añadirlo a la configuración del repositorio
* Cliente git:
** @git tag -a 0.1.0 -m 'Version 0.1.0‘@
** @git push origin 0.1.0@

Volver a ejecutar el ciclo de **Build/test/deploy...**

h2. Paso 8 - integrar los cambios en development

Se han de integrar los cambios de la nueva version en la rama de desarrollo
 **development**. Esta acción se conoce como **merge**.

Eclipse
* @Team > Switch to > deve lopment@
* @Team > Merge > Tags > 0.1.0@

**Conflictos en el fichero version.txt!!**

* Abrir el fichero version.txt
* Dejar la versión 0.2 y quitar todo lo demás

Para indicar que los conflictos han sido resueltos:

* Eclipse: Sobre el fichero > Team > Add to index
* Cliente git: @git add <fichero>@
* Commit

h3. Si todo va mal en un merge

Se ha de hacer un **Reset**.

Así se actualiza el índice (base de datos de git), el HEAD y la carpeta de trabajo con la versión que le digamos

* Eclipse: @Team > Reset... > elegir un branch > Marcar HARD@
* Cliente git: @git reset --hard <branch>@

h2. Paso 9 - bug fixing

El proceso para arreglar **bugs** y **hotfixes** para integrarlo en el desarrollo mediante ramas.

* Eclipse: @Team > Switch to > release-0.1@
* Cliente git: @$ git checkout development@
** Cambiar la versión a 0.1.1-RC1
** Arreglar el bug
** Mismo proceso que con la 0.1.0-RCx
** Eclipse: @Team > Merge > Tags > 0.1.1-RC1@

Volver a repetir el cicloe de **Release** y **Tag**:

* Cuando se considere suficientemente estable: release
* Cambiar versión a 0.1.1
* Tag 0.1.1
* Mezclar con development
* Mezclar con master

h2. Paso 10 - obtener cambios del repositorio remoto (pull)

Representar el escenario de programación en pareja con dos roles y las distintas acciones sobre los ficheros.

* Rol A
* Rol B

# @A@ modifica el fichero java:
* Hacer @commit@ de los cambios.
* @Push to upstream@
# @B@ obtiene los cambios:
* Sobre el @proyecto > Team > Fetch from upstream@ para obtener el índice de cambios
* Sobre el @proyecto > Team > Pull@

h3. Resolución de conflictos

¿Qué pasa si otro desarrollador subió cambios que entran en conflicto con los míos?

* A modifica el constructor
* B modifica el constructor de otra manera diferente
* A y B hacen push del repositorio

Obtener los cambios: 

* @Team > Fetch from upstream@
* @Team > Pull@

Los cambios se mezclan y git marca los conflictos para resolver.
Se han de corregir _manualmente_ los conflictos y añadir al _upstream_.

* @git add <fichero>@
* @git commit@
* @git push origin <rama>@

h2. Comandos útiles

Git ofrece una lista de acciones relacionadas con el repositorio distribuido para el desarrollo basado en ramas ya que hay mucha información y nos ofrece una forma de gestionarla. 

h3. Commit Log

El histórico de _commits_ en git se conoce como log. Debido a que los comentarios asociados a los commits son **obligatorios** tenemos un historial bastante potente.

* Información de los commits: @git log@
* Información de lo que ha cambiado en los últimos dos commits: @git log -p -2@
* Información de los commits enlazando la estrucutra de padres e hijos y ramas @git log --graph@

h3. Deshacer acciones

Deshacer commits en git se podría comparar @subversion revert@ pero funcional.
Nos permite añadir nuevos ficheros, actualizar los comentarios, eliminar ficheros que no deberían entrar en ese commit, es decir rehacer el _último commit_. 

* @git commit --amend@

h1. Integración continua

h2. Cómo vamos a hacer CI

!images/jenkins_logo.png!

Usaremos un servidor de CI: "Jenkins":http://jenkins-ci.org/ Jenkins realizará determinadas tareas de forma automática:
* Tags
* Construir las versiones vivas

También proveerá tareas para ser ejecutadas manualmente
* Branches de releases
* Desplegar una versión específica con un clic

Las versiones vivas vivirán en su propia máquina virtual

!images/jenkins-git.png!

h3. Objetivos

Se mantendrán diferentes versiones vivas a la vez para cumplir unos objetivos: 
* Asegurar la calidad
* Hacer el despliegue ágil
* Minimizar el riesgo

bq. "Release early, release often"

h3. Proceso

El repositorio remoto debería contener versiones más o menos estables

!images/integracion-co-test.png!
!images/integracion-tag-deploy.png!
!images/despliegue-produccion.png!

**Develop**
* Pueden fallar algunos tests, no es problema.
* La versión del último commit se despliega en _"local"_

**Release-X**
* Los tests deberían pasar, eventualmente se podrían desactivar.
* La versión del _último commit_ se despliega en _"local"_.

**Nightly builds**
* Construcciones que comprueban la _"salud"_ del proyecto.
* Se hacen sobre los branches _development_ y _release-X_ (siendo X la versión más reciente).
* Se ejecutan los tests.
* Se despliegan dos versiones por cada rama:
** @Limpia@: una bbdd nueva.
** @Migración@: con actualización de bbdd ya existente sobre la bbdd que ya hubiera para ese despliegue.

**Preproducción**
* Las versiones que van a preproducción son aquellas de las que se ha hecho @tag@.
* Pasan los tests.
* El entorno de pre puede estar en _local_ o en _Amazon_.
* Se despliegan dos versiones.
** @Limpia@.
** @Migración@.

**Producción**
* Las versiones que van a producción son aquellas de las que se ha hecho @tag@.
* Pasan los tests
* El despliegue se realiza en _Amazon_.
* Se despliegan dos versiones:
** @Limpia@.
** @Migración@.

!images/diagrama-integracion-continua.png!

h2. Configuración de Jenkins

Configurar _Jenkins_ para acceder a múltiples repositorios git. Para esto se han de tener en cuenta los siguientes requerimientos:

* _Jenkins_ construye diferentes proyectos en donde en proyecto puede tener su propio repositorio git.
* _Jenkins_ debe tener permisos de lectura o lectura/escritura a todos los repositorios de aquellos proyectos que vaya a construir.
* Por defecto _Jenkins_ usa la clave del usuario en @~/.ssh@ para autenticarse
* ¿Con qué usuario se ejecuta Jenkins? con el usuario **tomcat**.

h3. Opciones

**Opción I**: Un usuario @jenkinsci@ con acceso de _lectura/escritura_ a **todos** los repositorios.
* **Pros**:
** _Simplicidad_: sólo hay que gestionar un usuario.
** Una **única** clave ssh @/home/tomcat/.ssh/id_rsa.pub@.
* **Contras**:
** Un usuario para dominarlos a todos.
** Cualquier error en un job para el proyecto _X_ puede afectar a los repositorios del proyecto _Y_.
**Opción II**: Un usuario @jenkinsci@ con acceso de _lectura/escritura_ a todos los repositorios y otro @jenkinsci_read@ con acceso sólo de lectura
* **Pros**:
** Sólo hay que gestionar _dos usuarios_: basta con asociarlos a **todos** los proyectos.
* **Contras**:
** Seguimos teniendo un usuario para dominarlos a todo: jenkinsci
** Cualquier error en un job para el proyecto _X_ puede afectar a los repositorios del proyecto _Y_.
** Hay que gestionar dos claves ssh... no es trivial
**Opción III**: Un **usuario por proyecto** para integración continua.
* **Pros**:
** El usuario de ci de un proyecto no tiene acceso a los repos de otro proyecto.
* **Contras**:
** Hay que gestionar múltiples @claves ssh@.

**Utilizaremos la Opción III**

h3. Configurar Opción III

Configurar _Jenkins_ para acceso a _múltiples repositorios git_ siguiendo los siguientes pasos:
* Crear los usuarios necesarios en la forja.
!images/gestion-jenkins-usuarios-000.png!
* Generar un par de claves pública/privada con @ssh-keygen@ para cada usuario en diferentes ficheros.
* Acceder a Gerrit con cada usuario creado.
!images/gestion-jenkins-usuarios-001.png!
* Añadir la clave pública para este usuario.
* Copiar las claves a la máquina de Jenkins.
** Carpeta @/opt/ssh-keys@.

bq. <pre><code class="shell">$ cd /opt/ssh-keys
$ ll
total 24
drwxr-xr-x  2 tomcat tomcat 4096 Jan  4 09:46 ./
drwxr-xr-x 14 root   root   4096 Jan  4 09:42 ../
- rw-------  1 tomcat tomcat 1679 Jan  4 09:46 filetransferci_rsa
- rw-r--r--  1 tomcat tomcat  398 Jan  4 09:46 filetransferci_rsa.pub
- rw-------  1 tomcat tomcat 1679 Jan  4 09:44 samplegitci_rsa
- rw-r--r--  1 tomcat tomcat  396 Jan  4 09:44 samplegitci_rsa.pub
</code></pre>

* Configurar SSH para que utilice la clave correcta en cada caso.
** Crear el fichero @/home/tomcat/.ssh/config@.

bq. <pre><code class="shell">$ cd /home/tomcat/.ssh
$ cat config
Host samplegit.patxi.sidelab.es
    HostName patxi.sidelab.es
    User samplegitci
    IdentityFile /opt/ssh-keys/samplegitci_rsa
Host filetransfer.patxi.sidelab.es
    HostName patxi.sidelab.es
    User filetransferci
    IdentityFile /opt/ssh-keys/filetransferci_rsa
</code></pre>

h2. Configuración de builds

Los builds de _Jenkins_ funcionan a través de la configuración de **jobs**. Por lo que vamos a definir los jobs necesarios para el proceso de integración continua.

Se dividen en tres grupos:

* Jobs de *integración* (read only).
** Descargan el código (checkout).
** Construyen.
** Pasan tests.
** Despliegan la versión construida en "local".
* Jobs de *release* (read/write).
** Realizan los pasos anteriores y además.
** Tag si los tests pasaron.
** Push del tag al repositorio remoto.
* Jobs de *despliegue* (read only).
** Descargan el binario del repositorio de binarios
** Desplegar

h3. Job de integración

* Crear un *job* _#Maven_.
* Configurar el repositorio git
** @ssh://filetransferci@filetransferci.code.tscompany.es/filetransfer@
** Ssh leerá el fichero config y utilizará el fichero de claves correspondiente el host @filetransferci.code.tscompany.es@.
* Añadir las ramas a construir (añadir nuevas ramas con el botón “Add”)
** @development@
** @release-0.1.1@
* Añadir el @user.email@ y @user.name@ que usará _Jenkins_.
!images/jenkins-job-integracion.png!
* Los resultados del build se pueden comprobar en:
** @/opt/jenkins/jobs/filetransfer/workspace@
** Si es un proyecto _Maven_, dentro del proyecto en la carpeta target estará el artefacto generado.
** También se puede acceder vía web.
*** Y descargar el workspace como un *zip*.
** Los *tests* están en la carpeta @surfire-reports@ del proyecto _Maven_.
** También pueden consultarse *vía web* accediendo al build y seleccionando _"Resultado de los tests"_.
!images/jenkins-job-resultados.png!

h2. Maven

_Jenkins_ permite construir proyectos "Maven":http://maven.apache.org/.

En determinadas ocasiones los proyectos requieren configuraciones específicas. La información sensible suele ir en el fichero @settings.xml@ en el @home@ del usuario en su máquina de desarrollo-

* Info de *autenticación para Archiva*.
* *Profiles*

En Jenkins esto se puede gestionar con el plugin _"Config File Provider Plugin"_.

!images/jenkins-config-file-management.png!

Podemos añadir cualquiera de los ficheros creados con _Config File Management_ en un *job*.

!images/jenkisn-config-file-management-settings.png!

Para los deploys, si el certificado es autofirmado es *necesario* generar un *truststore* a partir del certificado generado por el servidor:

* http://www.liferay.com/web/neil.griffin/blog/-/blogs/fixing-suncertpathbuilderexception-caused-by-maven-downloading-from-self-signed-repository

Este truststore debe incluirse en todos los @jdk@ que utilice _Jenkins_ en la ruta indicada en el enlace anterior

