# Libro de jugadas: Proceso de creación de libros de jugadas/runbook
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## PUNTOS DE CONTACTO

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha aprobada: 25/3/2021

## Fase NIST 800-61r2

Preparación

## Resumen ejecutivo
Este libro de jugadas describe el proceso para crear un nuevo libro de jugadas/runbook, la transición del documento de BORRADOR a APROBADO y los requisitos de revalidación mediante GitLab.

Para obtener una creación más avanzada de libros de jugadas, consulte el [Taller de guías de respuesta a incidentes de AWS] (https://gitlab.aws.dev/fredski/aws-incident-response-playbooks-workshop/-/blob/main/playbooks/crypto_mining/EC2_crypto_mining.md)

## Orientación

> Es importante mantener nuestros runbooks para profundizar y ofrecer resultados rápidamente a los clientes. Según Matthew Helmke «Los runbooks nos ayudan como parte de un conjunto más amplio de prácticas, todas diseñadas para generar fiabilidad. Mantener actualizados los runbooks es una parte vital de la ingeniería de confiabilidad del sitio».
(< https://www.gremlin.com/blog/ensuring-runbooks-are-up-to-date/ >)

Nuestro equipo utiliza Github para administrar la documentación. Mira [Referencias] (.. /Playbook_creation_process.md/ #references) para ver algunos pasos abreviados de nuestro flujo de procesos.

## Nuevo libro de jugadas/runbook

### Procedimiento GUI

* Crear un problema para el seguimiento de procesos
* Crear una nueva rama de origen para trabajar en
* Navega hasta la raíz de tu rama
* Seleccione el botón «IDE web»
* Para crear un elemento, resalta la carpeta del margen izquierdo y selecciona la flecha desplegable para ver las opciones
* Seleccione «Nuevo archivo»
* Nota: Puede ser útil copiar el contenido de un libro de jugadas existente para formatearlo
* Nuestros documentos utilizan [Markdown con sabor a GitHub] (https://github.github.com/gfm/)

* Asegúrese de seguir la convención de nomenclatura estándar para nuestra biblioteca
* Libros de jugadas: `# Libro de jugadas/Runbook`
* Guías de herramientas: `# Herramientas: nombreherramienta `
* Esta es la única vez que se utiliza el encabezado 1 (H1) dentro del documento

* Crear un enlace al problema asociado
* Ejemplo: `Problema asociado: (. /problemas/1) `

* Una vez que hayas completado el documento:
* Agrega un enlace a tu documento en el número como comentario
* Añada la etiqueta «REVIEW» en el número
* Publica un mensaje con un enlace a tu problema en el `método de correspondencia del cliente' y solicita una reseña/comentario.

* Para que se apruebe un libro de jugadas, al menos dos (2) miembros del equipo deben haber revisado y agregado sus nombres y comentarios en el número asociado

* Actualizar e implementar cualquier recomendación o corrección del documento

* Una vez que se haya revisado el documento y se hayan realizado todas las correcciones, envíelo para que finalice el proceso de aprobación.

* Entra en el problema y cambia al cesionario a un aprobador
* Entra en el problema
* Añade la etiqueta ENVIADA PARA APROBACIÓN a tu problema
* En el margen derecho junto a «Cesionario», selecciona «Editar»
* Cambiar el cesionario a `aprobador`
* Enviar una nota al «aprobador» para informarle que el documento está listo para su revisión final
* Tras la aprobación, agregue un enlace al documento/libro de jugadas al archivo Léame
* Ejemplo: `[Proceso de creación de libros de jugadas] (. /docs/Playbook_creation_process.md) `
* Enviar una solicitud de fusión

* Nota: Recuerde que todos los pasos previos a la solicitud de fusión DEBEN completarse dentro de su rama de trabajo

* El SLA para esta tarea es de 7 días naturales desde el momento en que envía el documento.
* Si no se ha recibido respuesta dentro del período de SLA para su aprobación, escalar enviando una nota de timbre al `usuario A`, `usuario B`, `usuario C`.

### Procedimiento CLI

POR DETERMINAR

## Actualizaciones de Playbook/Runbook

### Procedimiento

* Crear un problema para el seguimiento de procesos
* Crear una nueva rama de origen para trabajar
* Navega hasta la raíz de tu rama
* Seleccione el botón «IDE web»
* Abra un nuevo número para realizar un seguimiento del progreso de los cambios
* Eliminar todas las etiquetas existentes
* Añadir la etiqueta DRAFT
* Crear un enlace al problema asociado
* Puedes anexar tus problemas a la lista
* Ejemplo: (número1), (número1), (número3)

* Una vez que hayas completado el documento:
* Agrega un enlace a tu documento en el número como comentario
* Añada la etiqueta «REVIEW» en el número
* Publica un mensaje con un enlace a tu problema en `mecanismo de correspondencia de clientes' y solicita una reseña/comentario.

* Para que se apruebe un libro de jugadas, al menos dos (2) miembros del equipo deben haber revisado y agregado sus nombres y comentarios en el número asociado

* Actualizar e implementar cualquier recomendación o corrección del documento

* Una vez que se haya revisado el documento y se hayan realizado todas las correcciones, envíelo para que finalice el proceso de aprobación.

* Entra en el problema y cambia al cesionario a un aprobador
* Entra en el problema
* Añade la etiqueta ENVIADA PARA APROBACIÓN a tu problema
* En el margen derecho junto a «Cesionario», selecciona «Editar»
* Cambiar el cesionario a `aprobador`
* Enviar una nota en Chime al «aprobador» para informarles de que el documento está listo para su revisión final
* Tras la aprobación, agregue un enlace al documento/libro de jugadas al archivo Léame
* Ejemplo: `[Proceso de creación de libros de jugadas] (. /docs/Playbook_creation_process.md) `
* Enviar una solicitud de fusión

* Nota: Recuerde que todos los pasos previos a la solicitud de fusión DEBEN completarse dentro de su rama de trabajo

* El SLA para esta tarea es de 7 días naturales desde el momento en que envía el documento.
* Si no se ha recibido respuesta dentro del período de SLA para su aprobación, escalar enviando una nota al `usuario A`, `usuario B`, `usuario C`

### Procedimiento CLI

POR DETERMINAR

## Proceso de aprobación

### Propietario de la tarea:

propietarios de equipos

### Acuerdo de nivel de servicio

- 7 días naturales a partir de la presentación del autor

### Procedimiento

* Revisar el documento y el problema asociado en GitHub

* Eliminar REVISIÓN del título del documento

* Eliminar la declaración THIS IS A BORRADOR DEL LIBRO DE JUGADAS del documento

* En Puntos de contacto
- Añadir/actualizar su nombre a Aprobador
- Añadir/actualizar `Última fecha aprobada` como AAAA/MM/DD para cuando aprueba el documento

* Volver a asignar el problema al solicitante
* Agrega un comentario en el problema de que se aprueba el libro de jugadas y para continuar con una solicitud de fusión

## Referencias

### Uso de GitHub: Crea un problema

Utilizamos problemas para realizar un seguimiento del trabajo en curso y finalizado. Además, esto ayuda al equipo a realizar la transición a través de nuestro proceso de revisión y aprobación. Esto también permite que los comentarios de los revisores aborden cualquier cosa de los objetos del repositorio como parte de nuestro proceso de aumento de barras de documentación.

* Interfaz gráfica de usuario: la GUI te permite interactuar con GitHub desde un navegador web para interactuar con este repositorio
1. Navega hasta tu repositorio de GitHub utilizando tu navegador web favorito
1. Abra un problema seleccionando Problemas en el menú superior
1. Selecciona «Nuevo número»
1. Añade un título y una descripción de lo que estás trabajando
1. Asignarse a sí mismo (esto permitirá el seguimiento en pasos posteriores)
1. En Milestone, seleccione `hito del cliente aquí`
1. En Etiquetas, selecciona las que sean apropiadas para tu problema
* Para documentos nuevos, asegúrese de seleccionar BORRADOR
1. Selecciona «Enviar problema»
1. En la nueva página Problema, en el margen derecho Bloquear problema > Edición > Bloquear

* Interfaz de línea de comandos: la CLI te permite interactuar con GitHub desde la línea de comandos

### Uso de GitHub: Crea una sucursal en la que trabajar

Trabajamos en ramas individuales, por lo que el trabajo en curso no interfiere con lo que otros pueden estar haciendo en tiempo real.

* Interfaz gráfica de usuario: la GUI te permite interactuar con [GitHub desde un navegador web para interactuar con este repositorio] (https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-and-deleting-branches-within-your-repository)

* Interfaz de línea de comandos: la CLI te permite interactuar con GitHub desde la línea de comandos

### Uso de GitHub: Crear una solicitud de fusión

Las solicitudes de fusión permiten revisar el aumento de barras secundario antes de fusionarse con el repositorio principal.

* [Interfaz gráfica de usuario] (https://docs.github.com/en/github/collaborating-with-pull-requests/incorporating-changes-from-a-pull-request/about-pull-request-merges)

* Interfaz de línea de comandos: la CLI te permite interactuar con GitHub desde la línea de comandos

## Artículos atrasados