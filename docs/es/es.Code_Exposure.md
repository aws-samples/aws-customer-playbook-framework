Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para identificar a los propietarios de los recursos de código, determinar cómo se expusieron, quiénes pueden haber accedido al código expuesto, determinar el impacto de la exposición, acciones correctivas necesarias y determinar la causa raíz de la exposición al código.

## Indicadores potenciales de compromiso
- Alertas de prevención de pérdida de datos (DLP)
- Publicación o exposición de código conocido
- Registros sospechosos CloudTrail

## Conclusiones potenciales de AWS GuardDuty
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Exfiltration:S3/MaliciousIPCaller
- Exfiltration:S3/ObjectRead.Unusual
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- Persistence:IAMUser/AnomalousBehavior
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.InsideAWS
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS

### Objetivos
Durante la ejecución del libro de jugadas, concéntrese en los resultados _***deseados***_, tomando notas para mejorar las capacidades de respuesta a incidentes.

#### Determine:
* **Copias, transferencias y publicación**
* **Exposición de credenciales**
* **Vulnerabilidades expuestas **
* **Inteligencia medioambiental expuesta **
* **Herramientas usadas**
* **Información de configuración**
* **Intención del actor**
* **Atribución del actor**
* **Otros daños causados al medio ambiente y a la empresa**
* **Riesgo creado para el medio ambiente y la empresa**

#### Recuperar:
* **Caduca o restablece cualquier credencial de autenticación expuesta**
* **Enumerar los riesgos de los posibles vectores de ataque basados en la inteligencia ambiental expuesta **
* **Minimizar o eliminar los riesgos del código expuso**

#### Mejorar los componentes de la perspectiva de seguridad de CAF:
[Perspectiva de seguridad de AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directiva**
* **Detectivo**
* **Responsivo**
* **Preventivo**

! [Imagen] (/images/aws_caf.png)
* * *

### Pasos de respuesta
1. [**PREPARACION**] Crear un inventario de activos de cuenta
2. [**PREPARACION**] Crear un inventario de repositorio
3. [**PREPARACION**] Habilitar el registro según corresponda
4. [**PREPARACION**] Identificar qué tipo de datos hay en cada repositorio
5. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
6. [**PREPARACION**] Implementar capacitación para abordar los ataques de exfiltración
7. [**DETECCIÓN Y ANALISIS**] Realizar comprobaciones de exfiltración y DLP
8. [**DETECCIÓN Y ANALÍSIS**] Revisar acciones de lectura y escritura del repositorio (CodeCommit)
9. [**DETECCIÓN Y ANALÍSIS**] Revisar registros DNS
10. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros de flujo de VPC
11. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros de endpoints o basados en host
12. [**CONTENCIÓN**] Bloquear el acceso de las cuentas afectadas
13. [**ERRADICATION**] Eliminar objetos no reconocidos y no autorizados de los repositorios
14. [**RECOVERY**] Realizar procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Exfiltración de datos, Abuso de servicios de AWS
* **Categoría**: Pérdida de datos
* **Recurso**: CodeCommit, S3, EC2
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, CloudWatch Metrics
* **Fuentes de registro**: registros DNS, registros de VPC Flow Logs, CloudTrail, CloudWatch
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Preparación

Este manual hace referencia e integra, siempre que es posible, con [Prowler] (https://github.com/toniblyx/prowler), que es una herramienta de línea de comandos que le ayuda con la evaluación de la seguridad, la auditoría, el refuerzo y la respuesta a incidentes de AWS.

Sigue las directrices del CIS Amazon Web Services Foundations Benchmark (49 comprobaciones) y tiene más de 100 comprobaciones adicionales, incluidas las relacionadas con el RGPD, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 y otros.

Esta herramienta proporciona una instantánea rápida del estado de seguridad actual dentro de un entorno de cliente. Además, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) proporciona un análisis automatizado de conformidad y puede [integrarse con Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventario de activos
Identifique todos los recursos existentes y tenga una lista de inventario de activos actualizada junto con quién es el propietario de cada recurso. El código fuente se puede almacenar en uno de los siguientes activos:
- (CodeCommit) [https://aws.amazon.com/codecommit/] repositorios
- (S3) [https://aws.amazon.com/s3/] almacenamiento de código respaldado
- (EC2) [https://aws.amazon.com/ec2/] mecanismos de almacenamiento de código autohospedado

#### Identifica qué datos se han extraído de cada activo
- Los registros de CloudWatch y CloudTrail almacenados en S3 se pueden consultar mediante (Amazon Athena) [https://aws.amazon.com/athena/] para identificar acciones no deseadas
- (Amazon GuardDuty) [https://aws.amazon.com/guardduty/] puede detectar automáticamente tráfico anómalo. Se puede acceder a esto mediante (AWS Security Hub) [https://aws.amazon.com/security-hub/] o (Amazon Detective) [https://aws.amazon.com/detective/]
- Los registros de aplicaciones e instancias almacenados en S3 también se pueden consultar mediante Athena.

### Formación
- `¿ Qué formación está impartida para que los analistas de la empresa se familiaricen con la AWS API (entorno de línea de comandos), CodeCommit, S3, RDS y otros servicios de AWS? `
>>>
Las oportunidades aquí para la detección de amenazas y la respuesta a incidentes incluyen:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `¿ Qué funciones pueden realizar cambios en los servicios de su cuenta? `
- `¿ Qué usuarios tienen asignados esos roles? ¿Se siguen los privilegios mínimos o existen usuarios de superadministrador? `
- `¿ Se ha realizado una evaluación de seguridad en relación con su entorno? ¿Tiene una línea de base conocida para detectar cosas «nuevas» o «sospechosas»? `

### Tecnología de la comunicación
- `¿ Qué tecnología se utiliza dentro del equipo/empresa para comunicar problemas? ¿Hay algo automatizado? `
>>>
Teléfono\
Correo electrónico\
SMS\
AWS SE UTILIZA\
AWS SNS\
Slack\
Campanilla\
Equipos\
¿Otro?
>>>

### Implementación de DLP

La implementación de una solución de prevención de pérdida de datos (DLP) puede proporcionar alertas y capacidades de detección adicionales. Una solución DLP puede proporcionar el mayor valor en entornos EC2 o evaluar el tráfico de red. Las soluciones DLP se pueden encontrar en [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp).

## Detección

### Registro y comprobaciones de bucket de S3
- Asegúrese de que CloudTrail esté habilitado en todas las regiones: `. /merodeador -c check 21`
- Asegúrese de que la validación de los archivos de registro de CloudTrail esté habilitada /merodeador -c check 22`
- Asegúrese de que el registro de acceso a bucket de S3 esté habilitado en el bucket de CloudTrail S3: `. /merodeador -c check 26`
- Asegúrese de que no haya depósitos de S3 abiertos para todos o cualquier usuario de AWS: `. /merodeador -c extra73`
- Identifique los recursos de su organización y cuentas; como buckets de Amazon S3 o roles de IAM; que se comparten con una entidad externa: `. /merodeador -c extra769`
- Buscar recursos expuestos a Internet: `. /merodeador -g grupo17`

### Registros y eventos de CodeCommit
- [Supervise los eventos de AWS CodeCommit en EventBridge] (https://docs.aws.amazon.com/codecommit/latest/userguide/monitoring-events.html), que proporciona un flujo de datos en tiempo real. Estos eventos son los mismos que los que aparecen en Amazon CloudWatch Events, que ofrece un flujo casi en tiempo real de eventos del sistema que describen los cambios en los recursos de AWS.
- [Cree un rastro de CloudTrail para permitir la entrega continua de eventos de CodeCommit a un bucket de S3] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html). CloudTrail captura todas las llamadas a la API de CodeCommit como eventos.

### Alertas DLP

La implementación de una solución DLP puede proporcionar alertas y capacidades de detección adicionales. Los detalles están disponibles en el proveedor de soluciones DLP.

## Procedimientos de escalada
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Análisis
Es muy recomendable exportar registros a una solución de administración de eventos de incidentes de seguridad (SIEM) (como Splunk, ELK stack, etc.) para ayudar a ver y analizar una variedad de registros para un análisis de cronograma de ataque más completo.

### CloudTrail
CloudTrail proporciona hasta 90 días de registros de eventos para todas las llamadas a la AWS API. Esta información se puede utilizar para identificar y rastrear acciones malintencionadas o anómalas. Hay más información disponible en la [Referencia de eventos de registro de CloudTrail] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html).

### CloudTrail: cubo público de S3
De forma predeterminada, CloudTrail registra las llamadas a la API realizadas en los últimos 90 días, pero no las solicitudes de registro realizadas a objetos. Puede ver los eventos a nivel de depósito en la consola de CloudTrail. Sin embargo, no puede ver los eventos de datos (llamadas a nivel de objeto de Amazon S3) de forma predeterminada; debe habilitar el registro a nivel de objeto antes de que estos eventos aparezcan en CloudTrail.

1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail de los nombres de eventos `getPublicAccessBlock` y `DeletePublicAccessBlock`

### CloudTrail: objeto S3 público
También puede obtener registros de CloudTrail para acciones de Amazon S3 a nivel de objeto. Para ello, [habilita eventos de datos para tu bucket de S3] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) o todos los depósitos de tu cuenta. Cuando se produce una acción a nivel de objeto en su cuenta, CloudTrail evalúa la configuración de seguimiento. Si el evento coincide con el objeto que especificó en un rastro, se registra el evento.

1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail para ver los nombres de eventos `getObjectACL` y `putObjectACL`

### VPC Flow Logs
VPC Flow Logs es una característica que le permite capturar información sobre el tráfico IP que entra y sale de las interfaces de red de la VPC. Esto puede resultar útil para las direcciones IP detectadas en CloudTrail para determinar los tipos de conexiones externas a cualquier recurso público.

Para obtener más información y pasos, incluida la consulta con Athena, consulte la [Documentación de AWS para VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Se recomienda que el análisis de Athena se incluya en un libro de jugadas separado y se vincule a otros elementos relevantes.

### Registros DNS
Los registros de DNS es una característica que le permite capturar información sobre el tráfico DNS que va y sale de las interfaces de red de la VPC. Esto puede ser útil para identificar anomalías o dominios de alto riesgo.

### CloudWatch
Los datos de las instancias de EC2 y otros orígenes se pueden [ingerir en CloudWatch] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html). Estos datos se pueden utilizar para activar alarmas o realizar análisis. CloudWatch también proporciona [detección de anomalías] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) donde está habilitada.

### Endpoint/Basado en host
1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise CloudTrail para buscar solicitudes `putObject` y `DeleteObject` de direcciones IP públicas

- Revise los registros de aplicaciones y sistemas operativos EC2 para detectar inicios de sesión inapropiados, instalación de software desconocido o la presencia de archivos no reconocidos.

- Se recomienda encarecidamente contar con una solución de sistema de detección de intrusiones (HIDS) basado en host de terceros (como OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), otros)

### DLP

Las soluciones DLP pueden detectar y alertar tal como están configuradas. Las soluciones DLP están disponibles en [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp).

## Contención

### S3 Bloquear el acceso público
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration «BlockPublicAcls=true, IgnorePublicAcls=true, BlockPublicPolicy=true, RestrictPublicBuckets=true»

También puedes consultar [Bloquear el acceso público a tu almacenamiento de Amazon S3] (https://aws.amazon.com/s3/features/block-public-access/) para obtener más información sobre cómo bloquear el acceso público de S3 en toda tu cuenta.

### CodeCommit

Control de acceso de auditoría para todos los usuarios con el servicio [AWS Identity and Access Management (IAM) junto con CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html).

Los permisos también se pueden cambiar o restringir con la [referencia de permisos CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-permissions-reference.html).

## Erradicación

### S3 Eliminar objetos no reconocidos o no autorizados
Eliminar los objetos no reconocidos de los depósitos

1. Inicie sesión en AWS Management Console y abra la consola de Amazon S3 en https://console.aws.amazon.com/s3/
1. En la lista Nombre del bucket, elija el nombre del bucket del que desea eliminar un objeto.
1. Elija el nombre del objeto que desea eliminar.
1. Para eliminar la versión actual del objeto, elija Versión más reciente y elija el icono de papelera.
1. Para eliminar una versión anterior del objeto, elija Versión más reciente y elija el icono de papelera junto a la versión que desea eliminar.

### AWS CodeCommit

[Identidades de auditoría y acceso a CodeCommit.] (https://docs.aws.amazon.com/codecommit/latest/userguide/security-iam.html)

### Amazon EC2

Siempre que sea posible:

1. [Lanzar una instancia EC2 de reemplazo] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-launch-snapshot.html) mediante [Snapshots de EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) o [copias de seguridad de Amazon Machine Image (AMI)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) creado a partir de la fuente
1. [Adjuntar un volumen de EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) de la instancia terminada a la nueva instancia EC2.

## Recuperación
Los mismos procedimientos que los enumerados para Erradicación

## Acciones preventivas

### Verificaciones de Prowler de autenticación
- Asegúrese de que la autenticación multifactor (MFA) esté habilitada:
`. /merodeador -c check 12`

Cheques de merodeador ### S3
**Encriptación**
- Compruebe si los depósitos de S3 tienen habilitado el cifrado predeterminado (SSE): `. /merodeador -c extra734`

**Recuperación ante desastres
- Compruebe si los buckets de S3 tienen habilitado el versionado de objetos: `. /merodeador -c extra763`

### Acciones de servicio de S3
[Impedir que los usuarios modifiquen la configuración de acceso público de bloques de S3] (https://asecure.cloud/a/scp_s3_block_public_access/)

Revise periódicamente el acceso a los bucket y las políticas mensualmente y utilice [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) o Security Hub para detectar automatizadas

[Uso del control de versiones en buckets de S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) para mitigar la eliminación accidental o intencional de objetos de nivel superior

[Administración del acceso con ACL] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) para limitar el acceso no autorizado a los recursos a nivel de bucket y objeto

### Amazon EC2

[Utilice la autenticación multifactor (MFA) con cada cuenta.] (https://aws.amazon.com/iam/features/mfa/)

Utilice TLS para comunicarse con los recursos de AWS. Recomendamos TLS 1.2 o posterior. Algunos servicios tienen esta opción habilitada de forma predeterminada, otros deberán implementarse (por ejemplo, en [SDK de JavaScript] (https://docs.aws.amazon.com/sdk-for-script/v2/developer-guide/enforcing-tls.html)).

[Configurar el registro de actividad de los usuarios y las API con AWS CloudTrail.] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitor-with-cloudtrail.html)

[Utilice soluciones de cifrado de AWS como KMS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html), junto con todos los controles de seguridad predeterminados dentro de los servicios de AWS.

[Habilitar instantáneas de EBS] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

### AWS CodeCommit

[Utilice la autenticación multifactor (MFA) con cada cuenta.] (https://aws.amazon.com/iam/features/mfa/)

Utilice TLS para comunicarse con los recursos de AWS. Recomendamos TLS 1.2 o posterior. Algunos servicios tienen esta opción habilitada de forma predeterminada, otros deberán implementarse (por ejemplo, en [SDK de JavaScript] (https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/enforcing-tls.html)).

[Configurar el registro de actividad de los usuarios y las API con AWS CloudTrail.] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)

[Utilice soluciones de cifrado de AWS como KMS] (https://docs.aws.amazon.com/codecommit/latest/userguide/encryption.html), junto con todos los controles de seguridad predeterminados dentro de los servicios de AWS.

[Utilice servicios de seguridad administrados avanzados como Amazon Macie] (https://aws.amazon.com/blogs/compute/discovering-sensitive-data-in-aws-codecommit-with-aws-lambda-2/), que ayuda a descubrir y proteger los datos personales almacenados en Amazon S3.

### Amazon Macie
[Amazon Macie] (https://aws.amazon.com/macie/) puede detectar credenciales almacenadas, claves privadas y otros datos de acceso [utilizando identificadores de datos administrados] (https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html).

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) tiene varias reglas automatizadas para gestionar la exposición del código, incluidas [codebuild-project-envvar-awscred-check] (https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html) a compruebe si las credenciales están almacenadas en código.

### Postura general de seguridad
Ejecute una [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

### Implementación de DLP

La implementación de una solución de prevención de pérdida de datos (DLP) puede proporcionar alertas y capacidades de detección adicionales. Las soluciones DLP se pueden encontrar en [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) y deben configurarse según lo prescrito.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como respuesta a incidentes, necesito un runbook sobre cómo detectar la exposición al código
- Como respuesta a incidentes, necesito un runbook sobre cómo detectar la exfiltración de código
- Como respuesta a incidentes, necesito poder detectar recursos públicos (AMI, volúmenes de EBS, repositorios de ECR, etc.)
- Como respuesta a incidentes, necesito saber qué roles son capaces de realizar cambios críticos en AWS
- Como respuesta a incidentes, necesito un libro de jugadas para mitigar la exposición del código y los puntos de escalado necesarios
- Como respuesta de incidentes, necesito documentación sobre los registros necesarios para diferentes clasificaciones de datos

## Elementos atrasados actuales