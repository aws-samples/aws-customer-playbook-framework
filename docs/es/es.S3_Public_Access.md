# Guía de respuesta a incidentes: Exposición de recursos públicos - S3
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para identificar a los propietarios de recursos públicos, determinar quiénes pueden haber accedido a ellos mientras estuvieron expuestos, determinar el impacto de revocar el acceso al recurso y determinar la causa raíz de la accesibilidad pública.

## Indicadores potenciales de compromiso
- Advertencia de acceso público desde el panel de servicios de AWS
- CloudTrail `getPublicAccessBlock`, `DeletePubAccessBlock `, `getObjectACL`, `PutObjectAcl`
- Notificación del investigador de seguridad sobre el acceso público a los recursos
- Eliminación de recursos de la dirección de protocolo público de Internet (IP)

## Conclusiones potenciales de AWS GuardDuty
- Descubrimiento: S3/Llamador IPP malicioso
- Descubrimiento: S3/Malicious IPCaller.Personalizado
- Descubrimiento: llamador S3/TORIP
- Exfiltración: S3/Llamador IPP malicioso
- Exfiltración: S3/lectura de objetos. Inusual
- Impacto: S3/Llamador IPP malicioso
- Pentest: S3/Kalilinux
- Pentest: S3/Parrot Linux
- Lápiz: S3/Pentoolinux
- Política: S3/Account Block Acceso público deshabilitado
- Política: S3/Bucket Acceso anónimo concedido
- Política: S3/Bucket Block Acceso público deshabilitado
- Política: S3/Bucket Acceso público concedido
- Stealth: S3/Registro de acceso al servidor deshabilitado
- Acceso no autorizado: S3/Malicious IPCaller.Custom
- Acceso no autorizado: llamador S3/TO IPP

### Objetivos
Durante la ejecución del libro de jugadas, concéntrate en los resultados _***deseados***_, tomando notas para mejorar las capacidades de respuesta a incidentes.

#### Determine:
* **Vulnerabilidades explotadas**
* **Exploits y herramientas observadas**
* **Intención del actor**
* **Atribución del actor**
* **Daños infligidos al medio ambiente y a la empresa**

#### Recuperar:
* **Volver a la configuración original y endurecida**

#### Mejorar los componentes de la perspectiva de seguridad de CAF:
[Perspectiva de seguridad de AWS Cloud Adoption Framework] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directiva**
* **Detectivo**
* **Responsivo**
* **Preventivo**

! [Imagen] (/images/aws_caf.png)
* * *

### Pasos de respuesta
1. [**PREPARACIÓN**] Crear un inventario de activos de cuenta
2. [**PREPARACION**] Crear un inventario de bucket de S3
3. [**PREPARACION**] Habilitar el registro según corresponda
4. [**PREPARACION**] Identificar qué tipo de datos hay en cada bucket
5. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
6. [**PREPARACION**] Implementar capacitación para abordar los ataques DO/DDoS
7. [**DETECCIÓN Y ANALÍSIS**] Realizar comprobaciones de bucket de S3
8. [**DETECCIÓN Y ANALISIS**] Revisión de CloudTrail: Bucket público de S3
9. [**DETECCIÓN Y ANALISIS**] Revisión de CloudTrail: Objeto público de S3
10. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros de flujo de VPC
11. [**DETECCIÓN Y ANALÍSIS**] Revisar endpoint /basado en host
12. [**CONTENCIÓN**] S3 Bloquear el acceso público
13. [**ERRADICACION**] S3 Eliminar objetos no reconocidos o no autorizados
14. [**RECOVERY**] Realizar procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [Publicación especial de NIST 800-61r2 Guía de manejo de incidentes de seguridad informática] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: AWS Service Public Access
* **Categoría**: Acceso público
* **Recurso**: S3
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: registros de servidor de S3, registros de acceso de S3, CloudTrail, CloudWatch
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
Identificar todos los recursos existentes y tener una lista de inventario de activos actualizada junto con quién posee cada uno

#### Inventario de cubos de S3
- Utilice la API de AWS [list-buckets] (https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html) para mostrar los nombres de todos los buckets de Amazon S3 (en todas las regiones): `aws s3api list-buckets —consulta «Buckets [] .Nombre"`

#### Activar registro
- Compruebe si los buckets de S3 tienen habilitado el registro de nivel de servidor en CloudTrail: `. /merodeador -c extra718`
- Compruebe si los buckets de S3 tienen habilitado el registro a nivel de objeto en CloudTrail: `. /merodeador -c extra725`

#### Identifica qué tipo de datos hay en cada bucket
**Opción A**
- Para ver todos los archivos de un bucket de S3 use el comando: `aws s3 ls s3: //your_bucket_name —recursive`
- **NOTA** Esta llamada a la API está limitada a las primeras 1000 coincidencias y no incluirá objetos por encima de ese umbral
- Categorizar manualmente qué cubos y objetos son importantes para la empresa
- Añada etiquetas a depósitos críticos para que existan metadatos como referencia futura

**Opción B**
- [Amazon Macie] (https://aws.amazon.com/macie/) es un servicio de seguridad de datos y privacidad de datos totalmente administrado que utiliza aprendizaje automático y coincidencia de patrones para descubrir y proteger sus datos confidenciales en AWS. Amazon Macie utiliza el aprendizaje automático y la coincidencia de patrones para descubrir de forma rentable datos confidenciales a escala.

### Capacitación
- `¿ Qué formación está impartida para que los analistas de la empresa se familiaricen con la API de AWS (entorno de línea de comandos), S3, RDS y otros servicios de AWS? `
>>>
Las oportunidades aquí para la detección de amenazas y la respuesta a incidentes incluyen:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Evaluación de seguridad de autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `¿ Qué funciones pueden realizar cambios en los servicios de su cuenta? `
- `¿ Qué usuarios tienen asignados esos roles? ¿Se siguen los privilegios mínimos o existen usuarios de superadministrador? `
- `Se ha realizado una evaluación de seguridad en su entorno, ¿tiene una línea de base conocida para detectar cosas «nuevas» o «sospechosas»? `

### Tecnología de la comunicación
- `¿ Qué tecnología se utiliza dentro del equipo/empresa para comunicar problemas? ¿Hay algo automatizado? `
>>>
Teléfono\
Correo electrónico\
AS SES\
AWS SNS\
Slack\
Campanilla\
¿Otro?
>>>

## Detección

### Comprobaciones de depósito de S3
- Asegúrese de que el registro de acceso a bucket de S3 esté habilitado en el bucket de CloudTrail S3: `. /merodeador -c check 26`
- Asegúrese de que no haya depósitos de S3 abiertos para todos o cualquier usuario de AWS: `. /merodeador -c extra73`
- Identifique los recursos de su organización y cuentas; como buckets de Amazon S3 o roles de IAM; que se comparten con una entidad externa: `. /merodeador -c extra769`
- Buscar recursos expuestos a Internet: `. /merodeador -g grupo17`

## Procedimientos de escalada
- `¿ Quién monitorea los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Análisis
Es muy recomendable exportar registros a una solución de administración de eventos de incidentes de seguridad (SIEM) (como Splunk, pila ELK, etc.) para facilitar la visualización y el análisis de una variedad de registros para un análisis de cronograma de ataque más completo.

### CloudTrail: cubo público de S3
De forma predeterminada, CloudTrail registra las llamadas a la API realizadas en los últimos 90 días, pero no las solicitudes de registro realizadas a objetos. Puede ver los eventos a nivel de depósito en la consola de CloudTrail. Sin embargo, no puede ver los eventos de datos (llamadas a nivel de objeto de Amazon S3) allí; debe analizar o consultar los registros de CloudTrail para ellos.

1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail de los nombres de eventos `getPublicAccessBlock` y `DeletePublicAccessBlock`

### CloudTrail: objeto S3 público
También puede obtener registros de CloudTrail para acciones de Amazon S3 a nivel de objeto. Para ello, habilita los eventos de datos para tu bucket de S3 o todos los depósitos de tu cuenta. Cuando se produce una acción a nivel de objeto en su cuenta, CloudTrail evalúa la configuración de seguimiento. Si el evento coincide con el objeto que especificó en un rastro, se registra el evento.

1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail para ver los nombres de eventos `getObjectACL` y `putObjectACL`

### Registros de flujo de VPC
Los registros de flujo de VPC es una característica que le permite capturar información sobre el tráfico IP que entra y sale de las interfaces de red de la VPC. Esto puede resultar útil para las direcciones IP detectadas en CloudTrail para determinar los tipos de conexiones externas a cualquier recurso público.

Para obtener más información y pasos, incluida la consulta con Athena, consulte la [Documentación de AWS para registros de flujo de VPC] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Se recomienda que el análisis de Atenea se incluya en un libro de jugadas separado y se vincule a otros elementos relevantes.

### Endpoint/Basado en host
1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise CloudTrail para buscar solicitudes `putObject` y `DeleteObject` de direcciones IP públicas

- Revise los registros de aplicaciones y sistemas operativos EC2 para detectar inicios de sesión inapropiados, instalación de software desconocido o la presencia de archivos no reconocidos.

- Se recomienda encarecidamente contar con una solución de sistema de detección de intrusiones (HIDS) basado en host de terceros (como OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), otros)

## Contención

### S3 Bloquear el acceso público
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration «blockpublicacls=true, ignorepublicaCls=true, blockPublicPolicy=True, restrictPublicBuckets=True»

También puedes consultar [Bloquear el acceso público a tu almacenamiento de Amazon S3] (https://aws.amazon.com/s3/features/block-public-access/) para obtener más información sobre cómo bloquear el acceso público de S3 en toda tu cuenta.

## Erradicación

### S3 Eliminar objetos no reconocidos o no autorizados
Eliminar los objetos no reconocidos de los depósitos

1. Inicie sesión en AWS Management Console y abra la consola de Amazon S3 en https://console.aws.amazon.com/s3/
1. En la lista Nombre del bucket, elija el nombre del bucket del que desea eliminar un objeto.
1. Elija el nombre del objeto que desea eliminar.
1. Para eliminar la versión actual del objeto, elija Versión más reciente y elija el icono de papelera.
1. Para eliminar una versión anterior del objeto, elija Versión más reciente y elija el icono de papelera junto a la versión que desea eliminar.

## Recuperación
Los mismos procedimientos que los enumerados para la erradicación

## Acciones preventivas

Cheques de merodeador ### S3
**Encriptación**
- Compruebe si los depósitos de S3 tienen habilitado el cifrado predeterminado (SSE): `. /merodeador -c extra734`

**Recuperación ante catástros**
- Compruebe si los buckets de S3 tienen habilitado el versionado de objetos: `. /merodeador -c extra763`

### Acciones de servicio de S3
[Impedir que los usuarios modifiquen la configuración de acceso público de bloques de S3] (https://asecure.cloud/a/scp_s3_block_public_access/)

Revise periódicamente el acceso a los bucket y las políticas mensualmente y utilice [CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) o Security Hub para detectar automatizadas

[Uso del control de versiones en buckets de S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) para mitigar la eliminación accidental o intencional de objetos de nivel superior

[Administración del acceso con ACL] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) para limitar el acceso no autorizado a los recursos a nivel de bucket y objeto

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) tiene varias reglas automatizadas para protegerse contra el acceso público, incluidas [s3-bucket-level-public-access-prohibidas] ( https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html).

### Postura general de seguridad
Ejecute una [Evaluación de seguridad de autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook sobre cómo mitigar los recursos que se hacen públicos incorrectamente
- Como Respondedor de Incidentes, necesito poder detectar recursos públicos (AMI, volúmenes de EBS, repositorios de ECR, etc.)
- Como Respondedor de incidentes, necesito saber qué roles son capaces de realizar cambios críticos en AWS.
- Como Respondedor de Incidentes, necesito un libro de jugadas para mitigar la exposición al cubo público y los puntos de escalada necesarios
- Como Respondedor de Incidentes, necesito documentación sobre los registros necesarios para diferentes clasificaciones de bucket

## Elementos atrasados actuales