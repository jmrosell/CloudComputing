﻿Introducción a la infraestructura virtual: concepto y soporte físico
====================================================================

## Ejercicio 1
_Consultar en el catálogo de alguna tienda de informática el precio de un 
ordenador tipo servidor y calcular su coste de amortización a cuatro y siete años. 
Consultar [este artículo en Infoautónomos sobre el tema](http://www.infoautonomos.com/consultas-a-la-comunidad/988/)._

- [Servidor torre PowerEdge T620](http://www.dell.com/es/empresas/p/poweredge-t620/pd)
- 579€ Sin IVA
- Amortización a cuatro años: 144.75€ por año.
- Amortización a siete años: 82.72€ por año.

## Ejercicio 2
_Usando las tablas de precios de servicios de alojamiento en Internet y de proveedores de servicios en la nube, 
comparar el coste durante un año de un ordenador con un procesador estándar (escogerlo de forma que sea el mismo 
tipo de procesador en los dos vendedores) y con el resto de las características similares (tamaño de disco duro 
equivalente a transferencia de disco duro) si la infraestructura comprada se usa sólo el 1% o el 10% del tiempo._

Un año son _24 * 365 = 8760 horas_.

Se han elegido los servicios de [Microsoft Azure](http://azure.microsoft.com/es-es/pricing/details/cloud-services/) y [Google Compute Engine](https://cloud.google.com/compute/#pricing),
ambos alojados en Europa, en concreto, los paquetes __A5__ y __n1-highmem-2__, respectivamente.

Para un __1%__ de uso:
- __Microsoft Azure__: 0.01 * 8760 * _0,35$/h_ = __30.66 $__
- __Google Compute Engine__: 0.01 * 8760 * _0,18$/h_ = __15.768 $__

Para un __10%__ de uso:
- __Microsoft Azure__: 0.1 * 8760 * _0,35$/h_ = __306.6 $__
- __Google Compute Engine__: 0.1 * 8760 * _0,18$/h_ = __157.68 $__

## Ejercicio 3
_¿Qué tipo de virtualización usarías en cada caso?_

- __Alojamiento de varios clientes en un servidor__: Utilizaría una __virtualización a nivel de sistema operativo__ para que los clientes compartieran el _SO_ a la vez que cada uno estuviera aislado de los demás.
- __Sistema web + middleware + BD__: En este caso utilizaría una __virtualización plena__ para separar este sistema completamente del resto.
- __Sistema de prueba de software e integración continua__: Para este supuesto elegiría una __virtualización de entornos de desarrollo__ para reproducir de la manera más precisa posible entornos concretos.

## Ejercicio 4
_Hacer el [tutorial de línea de órdenes de docker](https://www.docker.com/tryit/) para comprender cómo funciona._

1. `~$ docker version`
2. `~$ docker search tutorial`
3. `~$ docker pull learn/tutorial`
4. `~$ docker run learn/tutorial echo "hello world"`
5. `~$ docker run learn/tutorial apt-get install -y ping`
6. 
  + `~$ docker ps -l`
  + `~$ docker commit 698 learn/ping`
7. `~$ docker run learn/ping ping www.google.com`
8. `~$ docker inspect efe`
9. `~$ docker push learn/ping`

## Ejercicio 5
_Instala el sistema de gestión de fuentes **git**._

En Ubuntu se instala con la orden `sudo apt-get install git`.

## Ejercicio 6
1. _Crear un proyecto y descargárselo con git. Al crearlo se marca la opción de incluir el fichero README._
2. _Modificar el readme y subir el fichero modificado._

-  Creamos el proyecto desde la web de GitHub y pinchamos en la casilla de que nos dice si queremos incluir un README.
   A continuación, en nuestro ordenador, ejecutamos `git clone https://github.com/antorof/nombre-del-proyecto.git`.
-  Ya en nuestra máquina podemos modificar el README con un editor de texto. Despues confirmamos los cambios con 
`git commit -a .m "mensaje del commit"` y los subimos con `git push origin master`.

## Ejercicio 7
_Comprobar si en la instalación hecha se ha instalado cgroups y en qué punto está montado, así como qué contiene._

En mi instalación de linux (Ubuntu 14.04) se encuentra en `/sys/fs/cgroup` y contiene una sola carpeta llamada `systemd`.

## Ejercicio 8
1. _Crear diferentes grupos de control sobre un sistema operativo Linux._ 
   _Ejecutar en uno de ellos el navegador, en otro un procesador de textos y en uno último cualquier otro proceso._      _Comparar el uso de recursos de unos y otros durante un tiempo determinado._
2. _Calcular el coste real de uso de recursos de un ordenador teniendo en cuenta sus costes de amortización._
   _Añadir los costes eléctricos correspondientes._

- Voy a la carpeta de `cgroup` (`/sys/fs/cgroup/`) y creo los grupos con `mkdir grupoff`, `mkdir grupotxt	`, `mkdir grupoaux`.
   _Se ve que no se crean los grupos y no me sale nada en las carpetas.
   Tampoco me deja lanzar los programas con `cgexec`_.
- _No he podido realizarlo, al igual que el apartado anterior_.

## Ejercicio 9
1. ~~Discutir diferentes escenarios de limitación de uso de recursos o de asignación de los mismos a una u otra CPU.~~
2. Implementar usando el fichero de configuración de `cgcreate` una política que dé menos prioridad a los procesos de usuario que 
a los procesos del sistema (o viceversa).
3. Usar un programa que muestre en tiempo real la carga del sistema tal como `htop` y comprobar los efectos de la migración en tiempo 
real de una tarea pesada de un procesador a otro (si se tiene dos núcleos en el sistema).
4. Configurar un servidor para que el servidor web que se ejecute reciba mayor prioridad de entrada/salida que el resto de los usuarios.

- ### Apartado 2
  > [DavidGSola](https://github.com/DavidGSola) dijo: 
  
  > Te pongo un enlace por si te ayuda para hacer este ejercicio. [Link](http://docs.oracle.com/cd/E37670_01/E37355/html/ol_use_cases_cgroups.html)
  
  Con la ayuda del [enlace](http://docs.oracle.com/cd/E37670_01/E37355/html/ol_use_cases_cgroups.html), que son unos **casos de uso** de *Oracle*, asignamos baja prioridad al grupo "usuarios".
  
  ```
  mount {
  	cpu = /sys/fs/cgroup/cpu
  }
  
  group usuarios {
  	cpu {
  		cpu.shares="250";
  	}
  }
  
  group sistema {
  	cpu {
  		cpu.shares="750";
  	}
  }
  ```

## Ejercicio 10
_Comprobar si el procesador o procesadores instalados tienen estos_ flags. _¿Qué modelo de procesador es? ¿Qué aparece como salida de esa orden?_

- Procesador: __Intel(R) Core(TM) i3-3110M CPU @ 2.40GHz__ (máquina virtual)
- Salida de `flags`: `fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts mmx fxsr sse sse2 ss ht syscall nx rdtscp lm constant_tsc arch_perfmon pebs bts nopl xtopology tsc_reliable nonstop_tsc aperfmperf eagerfpu pni pclmulqdq ssse3 cx16 pcid sse4_1 sse4_2 x2apic popcnt xsave avx f16c hypervisor lahf_lm arat epb xsaveopt pln pts dtherm fsgsbase smep`

## Ejercicio 11
_Comprobar si el núcleo instalado en tu ordenador contiene este módulo del kernel usando la orden `kvm-ok`._

El comando `kvm-ok` devuelve: 

```
INFO: Your CPU does not support KVM extensions
INFO: For more detailed results, you should run this as root
HINT:   sudo /usr/sbin/kvm-ok
```

## Ejercicio 12
_Comentar diferentes soluciones de Software as a Service de uso habitual_

Son aplicaciones a las que accedemos por internet, y en la actualizad utilizamos Saas a diario, muchas veces sin tener constancia, ya que lo vemos cada vez más normal, pudiendo empezar por Gmail. 
Existen muchas otras, como los editores de texto y de hojas de cálculo de Google Drive, Google calendar, Outlook.com, Yahoo! Mail, Hangouts, PDFscape (editor online de pdf), Autocad360...






Creando aplicaciones en la nube: Uso de PaaS y SaaS
===================================================

## Ejercicio 1
_Instalar un entorno virtual para tu lenguaje de programación favorito (uno de los mencionados arriba, obviamente)._

He elegido instalar un entorno virtual para __python__. 

Para poder hacerlo he tenido que instalar primero _pip_. Se instala en Ubuntu con `sudo apt-get install python-pip`.

Después instalo el entorno: `sudo pip install virtualenv`

También he instalado _nodeenv_ para __NodeJS__: `sudo pip install nodeenv`

## Ejercicio 2
_Darse de alta en algún servicio PaaS tal como Heroku, Nodejitsu u OpenShift._

Registrado en _Openshift_ y _Heroku_.

## Ejercicio 3
_Crear una aplicación en OpenShift y dentro de ella instalar WordPress._

Creada y en marcha en https://wordpress-tfgos2.rhcloud.com/

## Ejercicio 4
_Crear un script para un documento Google y cambiarle el nombre con el que aparece en el menú, así como la función a la que llama._

Script creado: https://script.google.com/d/1CDot9JH09oTQa2WqSPx0kZmVN3wZ3TT8-ZZfCfMier6Gnh9sk9gdaQVK/edit?usp=sharing

## Ejercicio 5
_Buscar un sistema de automatización de la construcción para el lenguaje de programación y entorno de desarrollo que usemos habitualmente._

Aunque ya no lo empleo habitualmente, he utilizado **make** para C/C++. [Makefile](http://mrbook.org/tutorials/make/)

## Ejercicio 6
_Identificar, dentro del PaaS elegido o cualquier otro en el que se dé uno de alta, cuál es el fichero de automatización de 
construcción e indicar qué herramienta usa para la construcción y el proceso que sigue en la misma._

(En _OpenShift_ se puede configurar el proceso de construcción de las aplicaciones mediante _Jenkins_.
El proceso de construcción comienza cuando el usuario realiza un `git push` hacia el repositorio de la aplicación
en _OpenShift_. Después, se realiza la construcción de la aplicación (que se puede personalizar con _Jenkins_).

Se puede ver de manera resumida en el [workflow de OpenShift](https://www.openshift.com/walkthrough/developer-workflow), y 
más explicada [aquí](https://www.openshift.com/products/architecture).) <- Esto no es

## Ejercicio 7
_Buscar un entorno de pruebas para el lenguaje de programación y entorno de desarrollo que usemos habitualmente._

Para __NodeJS__ se pueden realizar test unitarios con [__Nodeunit__](https://github.com/caolan/nodeunit). También se pueden utilizar
una serie de herramientas, para realizar muchos tipos de pruebas, que están muy bien descritas [aquí](http://www.clock.co.uk/blog/tools-for-unit-testing-and-quality-assurance-in-node-js)

Para __python__ se puede utilizar el módulo __unittest__. [Aquí](http://www.openp2p.com/pub/a/python/2004/12/02/tdd_pyunit.html) se
puede ver un tutorial de TDD con __unittest__.

Para __Java__ existe __JUnit__, que nos permite la realización de test unitarios.
