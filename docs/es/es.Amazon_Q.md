# Manual de respuesta a incidentes: respuesta a los eventos de seguridad de Amazon Q

Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) en la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información de este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, declaración, compromiso contractual, condición o garantía por parte de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y obligaciones de AWS con sus clientes están reguladas por los acuerdos de AWS, y este documento no forma parte de ningún acuerdo entre AWS y sus clientes ni lo modifica.

© 2024 Amazon Web Services, Inc. o sus filiales. Todos los derechos reservados. Esta obra está bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponible en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha de aprobación:

## Resumen ejecutivo
Este manual describe ejemplos de procesos y consultas para investigar eventos de seguridad relacionados con Amazon Q.

## Preparación
Este manual hace referencia a [Prowler] (https://github.com/toniblyx/prowler) (), una herramienta de línea de comandos que le ayuda a evaluar la seguridad, auditar, reforzar y responder a los incidentes de AWS, siempre que sea posible.

### Objetivos
A lo largo de la ejecución del manual, concéntrese en los _***resultados deseados***_ y tome notas para mejorar las capacidades de respuesta a los incidentes.

#### Determine:
* **Vulnerabilidades explotadas**
* **Explotaciones y herramientas observadas**
* **Intención del actor**
* **Atribución del actor**
* **Daños causados al medio ambiente y a las empresas**

#### Recuperar:
* **Volver a la configuración original y reforzada**

#### Componentes de mejora de la perspectiva de seguridad de CAF:
[Perspectiva de seguridad del AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directiva**
* **Detective**
* **Responsivo**
* **Preventivo**

! [Imagen] (/images/aws_caf.png)
* * *

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Amazon Q
* **Categoría**: Análisis de registros
* **Recurso**: Amazon Q
* **Indicadores**: inteligencia sobre ciberamenazas, aviso de terceros
* **Fuentes de registro**: CloudTrail
* **Equipos**: centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes consta de las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

### Pasos de respuesta
1. [**PREPARACIÓN**]
2. [**PREPARACIÓN**]
3. [**PREPARACIÓN**]
4. [**DETECCIÓN Y ANÁLISIS**] Detecte y analice CloudTrail para detectar eventos de API no reconocidos
5. [**DETECCIÓN Y ANÁLISIS**] Realice una detección y analice CloudWatch para detectar eventos no reconocidos
6. [**CONTENCIÓN Y ERRADICACIÓN**] Eliminar o rotar las claves de usuario de IAM
7. [**CONTENCIÓN Y ERRADICACIÓN**] Eliminar o rotar los recursos no reconocidos

## Preparación
* Evalúe su postura de seguridad para identificar y remediar las brechas de seguridad
* AWS desarrolló una nueva herramienta de código abierto Self-Service Security Assessment que ofrece a los clientes una evaluación puntual para obtener información valiosa sobre el estado de seguridad de su cuenta de AWS.
* Mantenga un inventario de activos completo de todos los recursos, incluidos los servidores, los dispositivos de red, los recursos compartidos de red/archivos y las máquinas de los desarrolladores
* Considere la posibilidad de implementar AWS GuardDuty para monitorear continuamente actividades maliciosas y comportamientos no autorizados a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon SES
* Implemente CIS AWS Foundations, incluida la caducidad de las cuentas y la rotación obligatoria de credenciales
* Aplique la autenticación multifactorial (MFA)
* Haga cumplir los requisitos de complejidad de las contraseñas y establezca períodos de caducidad
* Ejecute un Informe de credenciales de IAM para enumerar todos los usuarios de su cuenta y el estado de sus distintas credenciales, incluidas las contraseñas, las claves de acceso y los dispositivos MFA
* Utilice AWS IAM Access Analyzer para identificar los recursos de su organización y sus cuentas, como las funciones de IAM que se comparten con una entidad externa. Esto le permite identificar el acceso no deseado a sus recursos y datos, lo que constituye un riesgo para la seguridad
* Considere la posibilidad de implementar AWS GuardDuty para monitorear continuamente actividades maliciosas y comportamientos no autorizados a fin de proteger sus cuentas y cargas de trabajo de AWS.

## Procedimientos de escalación
- `Necesito una decisión empresarial sobre cuándo deben realizarse los análisis forenses de EC2`
- `Quién supervisa los registros/alertas, los recibe y actúa en función de cada uno de ellos? `
- `¿ Quién recibe una notificación cuando se descubre una alerta? `
- `¿ Cuándo intervienen las relaciones públicas y el derecho en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Modelo Amazon Q

Amazon Q está comprometido con varios componentes o servicios:

* Chat: Amazon Q responde a las preguntas en lenguaje natural sobre AWS en inglés, incluidas las preguntas sobre la selección de servicios de AWS, el uso de la interfaz de línea de comandos de AWS (AWS CLI), la documentación y las prácticas recomendadas. Amazon Q responde con resúmenes de información o instrucciones paso a paso e incluye enlaces a sus fuentes de información.
* Memoria: Amazon Q utiliza el contexto de la conversación para informar las respuestas futuras mientras dure la conversación.
* Mejoras y consejos sobre el código: dentro de los IDE, Amazon Q puede responder preguntas sobre el desarrollo de software, mejorar el código y generar código nuevo.
* Solución de problemas y asistencia: Amazon Q puede ayudarle a entender los errores en la consola de administración de AWS y le proporciona acceso a agentes de AWS Support activos para responder a sus preguntas y problemas de AWS.
* Comentarios de los clientes: Amazon Q utiliza la información y los comentarios que envías a través de los formularios de comentarios para proporcionar asistencia o ayudar a solucionar problemas técnicos.

## Detección y análisis

### Notas y consideraciones

* Q no está disponible en todas las regiones (22 de mayo de 2022). Consulta [la guía para desarrolladores] (https://docs.aws.amazon.com/amazonq/latest/qdeveloper-ug/regions.html) para ver la disponibilidad actual en cada región
* El registro de invocaciones de modelos no es actualmente una función de Q. Está previsto añadirlo en el futuro. Se trata de una configuración a nivel regional y no es la predeterminada
* Puntos finales:
* Q Business (desarrolladores): qbusiness.amazonaws.com
* Un chat: q.amazonaws.com

### Nombres de eventos

Los nombres de eventos que se muestran en la tabla siguiente son una referencia rápida para los tipos de eventos que suelen encontrarse en CloudTrail durante la investigación de un evento de seguridad que involucra a Q. De los que se muestran en la tabla, los nombres de eventos más comunes que se registran durante el uso normal del modelo son:

* `getConversation`
* `Iniciar conversación`
* `GetIndex`
* `ListRetrievers`
* `Obtener aplicaciones`

| **Nombres de eventos** | **Descripción** |
|: -----------------|: ----------------------------------------- |
| ***Permisos generales*** | --- |
| `getConversation` | Otorga permiso para recibir mensajes individuales asociados a una conversación específica con Amazon Q |
| `GetTroubleShootingResults` | Concede permiso para obtener resultados de solución de problemas con Amazon Q |
| `sendMessage` | Concede permiso para enviar un mensaje a Amazon Q |
| `StartConversation` | Concede permiso para iniciar una conversación con Amazon Q |
| `StartTroubleShootingAnalysis` | StartTroubleShootingAnalysisOtorga permiso para iniciar un análisis de solución de problemas con Amazon Q
| `StartTroubleShootingResolutionExplanation` | Concede permiso para iniciar una explicación de resolución de problemas con Amazon Q|
| --- | --- |
| ***Crear una aplicación*** | -------- |
| `CreateApplication` | Otorga permiso para crear una aplicación |
| `DeleteApplication` | Concede permiso para eliminar una aplicación |
| `getApplication` | Otorga permiso para obtener una aplicación |
| `ListApplications` | Otorga permiso para publicar las aplicaciones|
| `UpdateApplication` | Otorga permiso para actualizar una aplicación |
| --- | --- |
| ***Crear un índice*** | -------- |
| `CreateIndex` | Otorga permiso para crear un índice para una aplicación determinada |
| `DeleteIndex` | Concede permiso para eliminar un índice |
| `getIndex` | Otorga permiso para obtener un índice|
| `ListIndices` | Otorga permiso para enumerar los índices de una aplicación |
| `UpdateIndex` | Otorga permiso para actualizar un índice |
| --- | --- |
| ***Crear un recuperador*** | -------- |
| `CreateRetriever` | Concede permiso para crear un recuperador para una aplicación determinada |
| `DeleteRetriever` | Concede permiso para eliminar un recuperador |
| `GetRetriever` | Otorga permiso para obtener un recuperador |
| `ListRetrievers` | Otorga permiso para enumerar los recuperadores de una aplicación |
| `UpdateRetriever` | Otorga permiso para actualizar un Retriever |
| --- | --- |
| ***Conexión de fuentes de datos*** | -------- |
| `CreateDataSource` | Otorga permiso para crear una fuente de datos para una aplicación e índice determinados |
| `DeleteDataSource` | Otorga permiso para eliminar una fuente de datos |
| `getDataSource` | Otorga permiso para obtener una fuente de datos |
| `ListDataSources` | Otorga permiso para enumerar las fuentes de datos de una aplicación y un índice |
| `UpdateDataSource` | Otorga permiso para actualizar una fuente de datos |
| `StartDataSourceSyncJobs` | Otorga permiso para iniciar el trabajo de sincronización de la fuente de datos |
| `StopDataSourceSyncJobs` | Otorga permiso para detener el trabajo de sincronización de la fuente de datos |
| `ListDataSourceSyncJobs` | Otorga permiso para obtener el historial de trabajos de sincronización de la fuente de datos |
| --- | --- |
| ***Carga de documentos*** | -------- |
| `BatchPutDocument` | Otorga permiso para colocar documentos por lotes |
| `BatchDeleteDocument` | Concede permiso para eliminar documentos por lotes |
| --- | --- |
| ***Gestión de chats y conversaciones*** | -------- |
| `Chat` | Otorga permiso para chatear mediante una aplicación|
| `ChatSync` | Otorga permiso para chatear de forma sincrónica usando una aplicación |
| `DeleteConversation` | Concede permiso para eliminar una conversación |
| `ListConversations` | Otorga permiso para enumerar todas las conversaciones de una aplicación |
| `ListMessages` | Concede permiso para enumerar todos los mensajes |
| --- | --- |
| ***Gestión de usuarios y grupos*** | -------- |
| `CreateUser` | Concede permiso para crear un usuario |
| `DeleteUser` | Otorga permiso para eliminar un usuario |
| `getUser` | Otorga permiso para obtener un usuario |
| `UpdateUser` | Otorga permiso para actualizar un usuario |
| `PutGroup` | Otorga permiso para poner un grupo de usuarios |
| `DeleteGroup` | Concede permiso para eliminar un grupo |
| `getGroup` | Otorga permiso para obtener un grupo |
| `ListGroups` | Otorga permiso para enumerar grupos |
| --- | --- |
| ***Complementos*** | -------- |
| `CreatePlugin` | Concede permiso para crear un complemento para una aplicación determinada |
| `DeletePlugin` | Concede permiso para eliminar un complemento |
| `getPlugin` | Otorga permiso para obtener un complemento |
| `UpdatePlugin` | Otorga permiso para actualizar un complemento |
| --- | --- |
| ***Controles de administración y barandas *** | -------- |
| `UpdateChatControlsConfiguration` | Otorga permiso para actualizar la configuración de los controles de chat de una aplicación |
| `DeleteChatControlsConfiguration` | Concede permiso para eliminar la configuración de los controles de chat de una aplicación |
| `GetChatControlsConfiguration` | Otorga permiso para obtener la configuración de los controles de chat de una aplicación |

### Eventos de planos de datos en CloudTrail

De forma predeterminada, CloudTrail no registra los eventos de datos. A continuación, se muestran las operaciones de la API Amazon Q registradas en CloudTrail como eventos de datos. La columna Tipo de evento de datos (consola) muestra la selección adecuada en la consola de CloudTrail. La columna de tipos de recursos de Amazon Q muestra el valor resources.type que especificaría para registrar los eventos de datos del recurso. Encontrará más información en la [Guía del usuario de Q] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/logging-using-cloudtrail.html).

**Aplicaciones empresariales de Amazon Q**: AWS: :QBusiness: :Aplicación
* Enumere los trabajos de sincronización de fuentes de datos
* Inicie el trabajo de sincronización de la fuente de datos
* Detenga el trabajo de sincronización de la fuente de datos
* Documento de carga por lotes
* Eliminar documento por lotes
* Poner comentarios
* ChatSync
* Eliminar conversación
* Listar conversaciones
* Listar mensajes
* Lista de grupos
* Eliminar grupo
* Obtener grupo
* Poner grupo
* Crear usuario
* Eliminar usuario
* Obtener usuario
* Actualizar usuario
* Listar documentos

**Recurso de datos empresariales de Amazon Q**: AWS: :QBusiness: :DataSource
* Enumere los trabajos de sincronización de fuentes de datos
* Inicie el trabajo de sincronización de la fuente de datos
* Detenga el trabajo de sincronización de la fuente de datos

**Índice Amazon Q Business**: AWS: :QBusiness: :Índice
* Eliminar grupo
* Obtener grupo
* Poner grupo
* Liste los grupos
* Listar documentos
* Documento de carga por lotes
* Eliminar documento por lotes

### Análisis

En el caso de que se produzca un incidente, además de investigar los indicadores de compromiso, el actor de la amenaza, el plazo, etc., estas son algunas preguntas adicionales que se deben tener en cuenta una vez que se haya confirmado que se trata de un incidente relacionado con los recursos de Amazon Q:

1. ¿Qué recursos se crearon y eliminaron?
2. ¿Qué complementos se utilizaron?
3. ¿A qué aplicaciones tuvo acceso Q durante el incidente de seguridad?
4. ¿Q accedió a estas aplicaciones?
5. ¿Se subió algún tipo de archivo a Q?
6. ¿Q tiene acceso a algún entorno IDE?

## Contención y erradicación

* [Eliminar o rotar las claves de usuario de IAM] (https://console.aws.amazon.com/iam/home#users) y [Root User Keys] (https://console.aws.amazon.com/iam/home#security_credential); es posible que desee rotar todas las claves de su cuenta si no puede identificar una o varias claves específicas que hayan quedado expuestas
* [Eliminar usuarios de IAM no autorizados] (https://console.aws.amazon.com/iam/home#users.)
* [Eliminar políticas no autorizadas] (https://console.aws.amazon.com/iam/home#/policies)
* [Eliminar funciones no autorizadas] (https://console.aws.amazon.com/iam/home#/roles)
* [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM.
* NOTA: La eliminación de los usuarios de IAM puede afectar a las cargas de trabajo de producción y debe hacerse con cuidado
* [Rotar las credenciales SMTP] (https://aws.amazon.com/premiumsupport/knowledge-center/ses-create-smtp-credentials/)
* [Cómo eliminar la aplicación Amazon Q] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-app-actions.html)
* [Cómo eliminar Amazon Q Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-native-retriever-actions.html)
* [Cómo eliminar Amazon Q Kenra Retriever] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-kendra-retriever-actions.html)
* [Cómo eliminar los documentos cargados] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/delete-doc-upload.html)
* [Cómo eliminar la fuente de datos de Amazon Q] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-datasource-actions.html)
* [Cómo eliminar Amazon Web Experience] (https://docs.aws.amazon.com/amazonq/latest/qbusiness-ug/supported-exp-actions.html)

## Recuperación

* Cree nuevos usuarios de IAM con políticas de acceso con privilegios mínimos


## Políticas de protección de datos e IAM

### Protección de datos:

* NUNCA coloque información confidencial o delicada, como las direcciones de correo electrónico de sus clientes, en etiquetas o campos de texto de formato libre, como un campo de nombre. Esto incluye cuando trabaja con Amazon Q u otros servicios de AWS mediante la consola, la API, la CLI de AWS o los SDK de AWS. Todos los datos que introduzca en las etiquetas o en los campos de texto de formato libre utilizados para los nombres se pueden utilizar en los registros de facturación o diagnóstico.
* Amazon Q almacena sus preguntas, sus respuestas y el contexto adicional, como los metadatos de la consola y el código en su IDE, para generar respuestas a sus preguntas.
* Amazon Q, los datos se envían y almacenan en una región de EE. UU. Los datos recopilados durante las conversaciones con Amazon Q se almacenan en la región EE.UU. Este (Norte de Virginia). Los datos recopilados durante las sesiones de resolución de errores de la consola se almacenan en la región EE.UU. Oeste (Oregón).

### Permisos de IAM:

* La política gestionada por `AmazonQFullAccess` proporciona acceso total para permitir las interacciones con Amazon Q. Todos los administradores y usuarios avanzados tienen acceso de forma predeterminada, pero en el caso de los demás usuarios y funciones, el cliente deberá adjuntar la política gestionada de AWS Q. Los permisos se pueden restringir en función de las acciones de Amazon Q.
* Para hacer preguntas sobre Amazon Q en la consola de AWS, una identidad de IAM necesita permisos para las siguientes acciones:
* StartConversation [Ejemplo: «Q:StartConversation"]
* Enviar mensaje

* Para solucionar los errores de la consola con Amazon Q, una identidad de IAM necesita permisos para las siguientes acciones:
* Comience el análisis de solución de problemas
* Obtenga resultados de solución de problemas
* Comience el análisis de solución de problemas

* Si una de estas acciones no está permitida explícitamente en una política adjunta, se mostrará un error de permisos de IAM cuando intentes usar Amazon Q.


## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesiten «arreglarse», pero que es importante conocer al ejecutar este manual de estrategias junto con los requisitos operativos y empresariales. `

## Elementos pendientes resueltos

## Elementos pendientes actuales