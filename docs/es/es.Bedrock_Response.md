# Manual de seguridad para responder a los eventos de seguridad de Amazon Bedrock
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) en la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información de este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, declaración, compromiso contractual, condición o garantía por parte de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y obligaciones de AWS con sus clientes están reguladas por los acuerdos de AWS, y este documento no forma parte de ningún acuerdo entre AWS y sus clientes ni lo modifica.

© 2024 Amazon Web Services, Inc. o sus filiales. Todos los derechos reservados. Esta obra está bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponible en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha de aprobación:

## Resumen ejecutivo

Como parte de nuestro compromiso continuo con los clientes, AWS ofrece este
manual de respuesta a incidentes de seguridad que describe los pasos necesarios para
investigue los eventos de seguridad en los que Amazon Bedrock sea la fuente o el objetivo del uso no autorizado en sus cuentas de AWS. El objetivo de este documento es proporcionar una guía prescriptiva sobre las medidas que debe tomar si sospecha que se ha producido un incidente de seguridad.

! [Imagen] (/images/nist_life_cycle.png)

*Aspectos de la respuesta a incidentes de AWS*

## Preparación

Para ejecutar de forma rápida y eficaz las actividades de respuesta a incidentes, es fundamental preparar a las personas, los procesos y la tecnología de su organización. Consulte la [*Guía de respuesta a incidentes de seguridad de AWS*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html) e implemente los pasos necesarios para garantizar la preparación de los tres dominios.

## Cómo solicitar asistencia al soporte de AWS

Es importante que notifique a AWS tan pronto como sospeche que hay credenciales comprometidas en su cuenta u organización de AWS. Estos son los pasos para contratar a AWS Support:

### Abrir un caso de AWS Support

- Inicie sesión en su cuenta de AWS:
- Esta es la primera cuenta de AWS que se vio afectada por el evento de seguridad, para validar la propiedad de la cuenta de AWS.
- Nota: Si no puede acceder a la cuenta, utilice [este formulario] (https://support.aws.amazon.com/#/contacts/aws-account-support/) para enviar una solicitud de soporte.
- Selecciona «*Servicios*», «*Centro de asistencia*», «*Crear caso*».
- Selecciona el tipo de problema «*Cuenta y facturación» *.
- Seleccione el servicio y la categoría afectados:
- Ejemplo:
- Servicio: cuenta
- Categoría: Seguridad
- Elige una gravedad:
- Clientes de Enterprise Support o On-Ramp: *Pregunta crítica sobre los riesgos empresariales.
- Clientes de asistencia empresarial: *Pregunta urgente sobre riesgos comerciales*.
- Describa su pregunta o problema:
- Proporcione una descripción detallada del problema de seguridad al que se enfrenta, los recursos afectados y el impacto en la empresa.
- Hora en que se reconoció por primera vez el incidente de seguridad.
- Resumen de la cronología del evento hasta el momento.
- Los artefactos que has recopilado (capturas de pantalla, archivos de registro).
- ARN de los usuarios o roles de IAM que sospeche que están comprometidos.
- Descripción de los recursos afectados y del impacto empresarial.
- Nivel de participación en su organización (por ejemplo: «Este evento de seguridad cuenta con la visibilidad del director ejecutivo y del consejo de administración»).
- Opcional: añada contactos adicionales al caso.
- Selecciona «*Contáctenos*».
- Idioma de contacto preferido (puede estar sujeto a disponibilidad)
- Método de contacto preferido: web, teléfono o chat (recomendado)
- Opcional: contactos de caja adicionales
- *Haga clic en «Enviar» *

- **Escalaciones**: notifique a su equipo de cuentas de AWS lo antes posible para que puedan contratar los recursos necesarios y escalar según sea necesario.

**Nota: ** Es muy importante comprobar que su «contacto de seguridad alternativo» esté definido para cada cuenta de AWS. Para obtener más información, consulte [esto
artículo] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).


## Amazon Bedrock

Amazon Bedrock está comprometido con varios componentes o servicios:

* Modelos: pueden ser modelos básicos de las principales empresas emergentes de IA o modelos personalizados
* Inferencia: proceso de generar un resultado a partir de una entrada proporcionada a un modelo
* Base de conocimientos: le permite acumular fuentes de datos en un repositorio de información
* Agentes: ayudan a los usuarios finales a realizar acciones en función de los datos de la organización y las aportaciones de los usuarios

Cuando un usuario de una cuenta de AWS (autorizado o no autorizado) accede a Bedrock, en última instancia lo hace a través de un modelo, una base de conocimientos o un agente. Todas las solicitudes emitidas y sus respuestas se publicarán en un registro de invocación modelo, siempre que se haya configurado previamente (https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html). Para obtener más información, consulte la guía del usuario de Amazon Bedrock aquí: https://docs.aws.amazon.com/bedrock/

La siguiente imagen muestra la relación entre Prompts, [Agents] (https://docs.aws.amazon.com/bedrock/latest/userguide/agents-how.html) y Knowledge Bases, y puede ser una buena forma de visualizar el flujo de datos (extraída de la [Guía del usuario] (https://docs.aws.amazon.com/bedrock/latest/userguide/what-is-bedrock.html))

! [Construcción del agente durante las operaciones de la API en tiempo de compilación] (/images/bedrock-01.png)


## Detección

### Notas y consideraciones

* Bedrock no está disponible en todas las regiones (4 de junio de 2022). Consulte [enlace] (https://docs.aws.amazon.com/bedrock/latest/userguide/bedrock-regions.html) para conocer la disponibilidad actual en la región
* Se requiere el registro de invocaciones de modelos para ver las solicitudes y respuestas enviadas a los modelos. Se trata de una configuración a nivel regional y no es la predeterminada
* Para CloudTrail, EventSource = bedrock.amazonaws.com
* La API ***InvokeModel*** es un evento**de solo lectura**

### Nombres de eventos

Los nombres de los eventos que aparecen en la tabla siguiente son una referencia rápida para los tipos de eventos que suelen encontrarse en CloudTrail durante la investigación de un evento de seguridad que involucra a Bedrock. De los que se muestran en la tabla, los nombres de eventos más comunes que se registran durante el uso normal del modelo son:

* `getFoundationModelAvailability`
* `InvokeModel`
* `Invoque Model con ResponseStream`

| **Nombres de eventos** | **Solo lectura** | **Descripción** |
|: -----------------|: -----------------|: ------------------------ |
| ***Eventos relacionados con los modelos*** | --- | --- |
| `getFoundationModelAvailability` | TRUE | Recupera la disponibilidad de un modelo básico |
| `getUseCaseForModelAccess` | TRUE | Recupera un caso de uso de un modelo especificado |
| `InvokeModel` | TRUE | Invoca el modelo Bedrock especificado para ejecutar la inferencia utilizando la entrada proporcionada en el cuerpo de la solicitud |
| `InvokeModelWithResponseStream` | TRUE | Invoca el modelo Bedrock especificado para ejecutar la inferencia utilizando la entrada proporcionada en el cuerpo de la solicitud mediante la fragmentación |
| `listCustomModels` | TRUE | Muestra los modelos personalizados de Bedrock que se han creado |
| `ListFoundationModels` | TRUE | Muestra los modelos básicos que se pueden usar |
| `ListProvisionedModelThroughputs` | TRUE | Muestra las capacidades aprovisionadas para los modelos |
| --- | --- | --- |
| ***Eventos relacionados con el registro de invocaciones de modelos*** | --- | --- |
| `DeleteModelInvocationLoggingConfiguration` | FALSE | Elimina una configuración de registro de invocaciones existente |
| `getModelInvocationLoggingConfiguration` | TRUE | Recupera la configuración de registro de invocaciones existente |
| `PutModelInvocationLoggingConfiguration` | FALSE | Crea una configuración de registro de invocaciones existente |
| --- | --- | --- |
| ***Eventos relacionados con las bases de conocimiento*** | --- | --- |
| `AssociateAgentKnowledgeBase` | FALSE | Esta acción registra la asociación de una base de conocimientos durante la creación del agente o después de crearlo |
| `CreateDataSource` | FALSE | Crea una fuente de datos |
| `CreateKnowledgeBase` | FALSE | Crea una base de conocimiento |
| `deleteKnowledgeBase` | FALSE | Elimina una base de conocimientos |
| `getDataSource` | TRUE | Obtiene información sobre una fuente de datos |
| `getKnowledgeBase` | TRUE | Recupera una base de conocimientos existente |
| `ListDataSources` | TRUE | Muestra las fuentes de datos disponibles |
| `ListKnowledgeBases` | TRUE | Enumera las bases de conocimiento existentes |
| --- | --- | --- |
| ***Eventos relacionados con los agentes*** | --- | --- |
| `CreateAgent` | FALSE | Crea un nuevo agente y un alias de agente de prueba que apunta a la versión del agente DRAFT |
| `CreateAgentActionGroup` | FALSE | Crea un grupo de acciones para un agente. Un grupo de acciones representa las acciones que un agente puede llevar a cabo para el cliente al definir las API a las que puede llamar un agente y la lógica para llamarlas |
| `CreateAgentAlias` | FALSE | Crea un nuevo alias para un agente |
| `deleteAgent` | FALSE | Elimina un agente creado anteriormente |
| `GetAgent` | TRUE | Recupera un agente existente |
| `GetAgentAlias` | TRUE | Recupera un alias existente |
| `InvokeAgent` | FALSE | Envía un mensaje para que el agente lo procese y responda |
| `ListAgentActionGroups` | TRUE | Muestra los grupos de acciones de un agente e información sobre cada uno de ellos |
| `ListAgentAliases` | TRUE | Muestra los alias de un agente y la información sobre cada uno de ellos |
| `ListAgentKnowledgeBases` | TRUE | Muestra las bases de conocimiento asociadas a un agente y la información sobre cada una de ellas |
| `ListAgents` | TRUE | Muestra los agentes existentes |
| `ListAgentVersions` | TRUE | Muestra las versiones de un agente e información sobre cada una de ellas |
| `PrepareAgent` | FALSE | Prepara un Amazon Bedrock Agent existente para recibir solicitudes en tiempo de ejecución |
| --- | --- | --- |
| ***Eventos relacionados con la ingesta de empleo*** | --- | --- |
| `GetIngestionJob` | TRUE | Obtiene información sobre un trabajo de ingestión, en el que se añade una fuente de datos a una base de conocimientos |
| `ListingestionJobs` | TRUE | Muestra los trabajos de ingestión de una fuente de datos e información sobre cada uno de ellos |
| `StartingEstionJob` | FALSE | Comienza un trabajo de ingestión, en el que se añade una fuente de datos a una base de conocimientos |
| --- | --- | --- |
| ***Eventos relacionados con RAG*** | --- | --- |
| `Recuperar` | TRUE | Recupera los datos ingeridos de una base de conocimientos (se consideran eventos de datos y no se muestran en CloudTrail) |
| `RetrieveAndGenerate` | FALSE | Envía la entrada del usuario para realizar la recuperación y la generación (se consideran eventos de datos y no se muestran en CloudTrail) |

### Capturas de pantalla sobre el uso de Bedrock

Las siguientes capturas de pantalla pueden proporcionar una ayuda visual al personal de respuesta a incidentes a la hora de interpretar los hechos detectados durante una investigación. Cada imagen que aparece a continuación representa las acciones realizadas que coinciden con el nombre del evento que se registra

---

**Obtenga la disponibilidad del modelo Foundation**
<details>
<summary>Expandir captura de pantalla</summary>
-
Este evento se registra por primera vez al navegar a la página principal de Amazon Bedrock

! [Obtenga la disponibilidad del modelo de base] (/images/bedrock-02.png)

</details>

---

**ListFoundationModels** y **ListCustomModels**
<details>
<summary>Expandir captura de pantalla</summary>
-
Estos eventos se registran al navegar por las páginas «Modelos básicos» y «Modelos personalizados»

! [ListFoundationModels y ListCustomModels] (/images/bedrock-03.png)

</details>

---

**Invoke Modelo**
<details>
<summary>Expandir capturas de pantalla</summary>
-
Este evento se registra al invocar el modelo.

! [Ejemplo de invocación de modelo] (/images/bedrock-04.png)

Ejemplo de registro de eventos de CloudTrail para la invocación de modelos:

! [CloudTrail para InvokeModel] (/images/bedrock-05.png)

</details>

---

**InvokeModel**: ejemplos adicionales
<details>
<summary>Expandir capturas de pantalla</summary>
-
Las siguientes imágenes proporcionan ejemplos adicionales de cómo se registra el nombre del evento `InvokeModel`.
La primera imagen muestra las diferencias entre los distintos métodos de invocación de modelos:
(a) Modele la invocación utilizando una base de conocimientos
(b) Invocación directa del modelo
(c) Modele la invocación mediante un agente

Tenga en cuenta que el *Nombre de usuario* que se muestra es diferente según la forma en que se invoque el modelo

! [Invocación del modelo de agente y base de conocimiento] (/images/bedrock-06.png)

Esta diferencia se resalta mediante dos capturas de pantalla simultáneas del registro del evento `InvokeModel` en CloudTrail.
La imagen de la izquierda = Invocación directa del modelo
La imagen de la derecha = Invocación del modelo mediante una base de conocimientos

! [CloudTrail para InvokeModel] (/images/bedrock-07.png)


</details>

---

**InvokeModel** o **InvokeModelWithResponseStream**: solicitudes y respuesta
<details>
<summary>Expandir captura de pantalla</summary>
-
Si bien los eventos `InvokeModel` e `InvokeModelWithResponseStream` aparecen en CloudTrail, los registros de invocación del modelo (cuando están configurados) también muestran la solicitud y la respuesta que corresponden a cada evento `InvokeModel`. La siguiente imagen está tomada de los registros de invocación del modelo que se enviaron a S3; contienen la solicitud utilizada, así como el resultado o la respuesta del modelo (a continuación encontrará un DDL de Athena):

! [Ejemplo de invocación del modelo S3] (/images/bedrock-09.png)

</details>

---

**Configuración de registro de invocaciones de PutModel**
<details>
<summary>Expandir captura de pantalla</summary>
-
Este evento se registra cuando el registro de invocación de modelos se configura en una cuenta de AWS.

! [Configuración del registro de invocación de modelos] (/images/bedrock-10.png)

</details>

---

**Crear base de conocimiento**
<details>
<summary>Expandir captura de pantalla</summary>
-
El siguiente registro de eventos de CloudTrail se registra cuando se crea una base de conocimientos. El registro del evento incluye la identidad que emitió la solicitud y el nombre de la base de conocimientos. Se puede hacer referencia al nombre de la base de conocimientos en los registros posteriores que capturan las invocaciones de modelos mediante `InvokeModel`

! [Creación de una base de conocimientos] (/images/bedrock-11.png)

</details>

---

**Crear agente**
<details>
<summary>Expandir captura de pantalla</summary>
-
La siguiente imagen muestra la creación de un agente mediante la consola. El nombre del evento resultante que se registra es `CreateAgent`

! [Creación de agentes] (/images/bedrock-12.png)

</details>

---

**Recuperar** y **Recuperar y generar**
<details>
<summary>Expandir captura de pantalla</summary>
-
En la imagen siguiente se muestra un ejemplo de cómo se muestran los nombres de los eventos `Retrieve` y `RetrieveAndGenerate` al probar una base de conocimientos. Tenga en cuenta que `Retrieve` solo prueba la recuperación de datos de la base de conocimientos, y `RetrieveAndGenerate` prueba la recuperación de datos de la base de conocimientos y la generación de una respuesta:

! [Recuperar, recuperar y generar] (/images/bedrock-13.png)

</details>

---

### Athena DDL

Utilice el siguiente código DDL para crear una tabla en Athena a partir de los registros de invocación de modelos almacenados en un bucket de Amazon S3. Asegúrese de utilizar el URI de S3 correcto para la ubicación del depósito al final del fragmento de código:

```
CREAR TABLA EXTERNA bedrock_model_invocation_metadata_unfiltered (
Cadena SchemaType,
cadena SchemaVersion,
cadena de fecha y hora,
cadena AccountID,
<arn: string>estructura de identidad,
cadena de región,
cadena RequestID,
cadena de operación,
cadena ModelID,
cadena ErrorCode,
estructura de entrada <inputBodyJSON: cadena,
inputBodyS3Path: cadena,
Tipo de contenido de entrada: cadena,
InputTokenCount: int>,
estructura de salida <outputBodyJSON: cadena,
OutputBodyS3Path: cadena,
Tipo de contenido de salida: cadena,
OutputTokenCount: int,
Tamaño de paso de la imagen agrupada en cubos: int,
Altura de la imagen de salida: int,
Ancho de la imagen de salida: int
>
)
FORMATO DE FILA SERDE 'org.openX.data.jsonSerde.JsonSerde'
<insert_prefix>UBICACIÓN 'S3<insert_S3_bucket>:////'
```

## Elementos pendientes resueltos
- Como personal de respuesta a incidentes, necesito poder monitorear todos los eventos críticos de Bedrock
- Como personal de respuesta a incidentes, necesito un manual sobre cómo consultar los eventos de Bedrock Cloudtrail a gran escala

## Elementos pendientes actuales