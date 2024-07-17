# Guía de respuesta a incidentes: Exposición de recursos públicos - RDS
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para identificar a los propietarios de recursos públicos, determinar quiénes pueden haber accedido a ellos mientras estuvieron expuestos, determinar el impacto de revocar el acceso al recurso y determinar la causa raíz de la accesibilidad pública.

## Indicadores potenciales de compromiso
- Advertencia de acceso público desde el panel de servicios de AWS
- Nombre del evento CloudTrail `PubliclyAccessible`
- Notificación del investigador de seguridad sobre el acceso público a los recursos
- Eliminación de recursos de la dirección de protocolo público de Internet (IP)

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
[Perspectiva de seguridad de AWS Cloud Adoption Framework] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **Directiva**
* **Detectivo**
* **Responsivo**
* **Preventivo**

! [Imagen] (/images/aws_caf.png)
* * *

### Pasos de respuesta
1. [**PREPARACIÓN**] Crear un inventario de activos
2. [**PREPARATION**] Crear un inventario de instancias de RDS
3. [**PREPARATION**] Establecer comprobaciones de seguridad y registro de RDS
4. [**PREPARACION**] Implementar un programa de formación para identificar y evaluar el acceso a RDS y el análisis de registros
5. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado [**DETECCIÓN Y ANALÍSIS**]
6. [**DETECCIÓN Y ANALÍSIS**] Realizar comprobaciones de instancias
7. [**DETECCIÓN Y ANALISIS**] Revise CloudTrail para RDS Public Resources
8. [**DETECCIÓN Y ANALÍSIS**] Revisar VPC Flow Logs
9. [**DETECCIÓN Y ANALÍSIS**] Revisar registros de endpoint RDS /basados en host
10. [**CONTENCIÓN**] Contener la exposición pública de RDS
11. [**ERRADICACION**] Eliminar las instantáneas o bases de datos públicas no reconocidas o no autorizadas
12. [**PREPARATION**] Acciones preventivas adicionales: comprobaciones de seguridad de RDS
13. [**PREPARATION**] Acciones preventivas adicionales: Política de control de seguridad - Cifrado RDS
14. [**PREPARATION**] Acciones preventivas adicionales: AWS Config
15. [**PREPARACIÓN**] Acciones preventivas adicionales: postura general de seguridad

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: AWS Service Public Access
* **Categoría**: Acceso público
* **Recurso**: RDS
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: RDS Database Log Files, CloudTrail, CloudWatch
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

Esta herramienta proporciona una instantánea rápida del estado de seguridad actual dentro de un entorno de cliente. Como alternativa, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) proporciona un escaneo automatizado de conformidad y puede [integrarse con Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventario de activos
Identificar todos los recursos existentes y tener una lista de inventario de activos actualizada junto con quién posee cada uno

#### Inventario de instancias de RDS
- Para inventar instancias de RDS, utilice la AWS API [describe-db-instances] (https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html) para enumerar los nombres de todas las instancias de una región determinada: `aws rds describe-db-instances —region us-east-1 —query 'dbInstances [*]. [DBInstanceIdentifier, identificadores de instancia de réplica de lectura] '`

#### Comprobaciones de seguridad y registro de RDS
- Compruebe si el almacenamiento de instancias RDS está cifrado: `. /merodeador -c extra735`
- Compruebe si las instancias RDS tienen habilitada la copia de seguridad: `. /merodeador -c extra739`
- Compruebe si las instancias de RDS están integradas con CloudWatch Logs: `. /merodeador -c extra747`
- Asegúrese de que las instancias de RDS tengan habilitada la actualización de versión secundaria: ` /merodeador -c extra7131`
- Compruebe si las instancias de RDS han habilitado [supervisión mejorada] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html): `. /merodeador -c extra7132`
- Comprobaciones de seguridad de RDS: `. /merodeador -g grupo13`

### Formación
- `¿ Qué formación está impartida para que los analistas de la empresa se familiaricen con la AWS API (entorno de línea de comandos), S3, RDS y otros servicios de AWS? `
>>>
Las oportunidades aquí para la detección de amenazas y la respuesta a incidentes incluyen:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

- `¿ Qué funciones pueden realizar cambios en los servicios de su cuenta? `
- `¿ Qué usuarios tienen asignados esos roles? ¿Se siguen los privilegios mínimos o existen usuarios de superadministrador? `
- `Se ha realizado una evaluación de seguridad en su entorno, ¿tiene una línea de base conocida para detectar cosas «nuevas» o «sospechosas»? `

### Tecnología de la comunicación
- `¿ Qué tecnología se utiliza dentro del equipo/empresa para comunicar problemas? ¿Hay algo automatizado? `
>>>
Teléfono\
Correo electrónico\
AWS SE UTILIZA\
AWS SNS\
Slack\
Campanilla\
¿Otro?
>>>

## Detección

### Comprobaciones de instancias de RDS

#### AWS Config
AWS Config tiene varias [reglas administradas para evaluar sus instancias de RDS] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* rds-automatic-minor-version-upgrade-enabled
* rds-cluster-deletion-protection-enabled
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-plan
* rds-logging-enabled
* rds-multi-az-support
* rds-resources-protected-by-backup-plan
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted
#### merodeador
- Asegúrese de que no haya instancias de RDS de acceso público: `. /merodeador -c extra78`
- Compruebe si las instantáneas de RDS y las instantáneas de clúster son públicas: `. /merodeador -c extra723`
- Identifique los recursos de su organización y cuentas; como buckets de Amazon S3 o roles de IAM; que se comparten con una entidad externa: `. /merodeador -c extra769`
- Buscar recursos expuestos a Internet: `. /merodeador -g grupo17`

## Procedimientos de escalada
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Análisis
Es muy recomendable exportar registros a una solución de administración de eventos de incidentes de seguridad (SIEM) (como Splunk, ELK stack, etc.) para facilitar la visualización y el análisis de una variedad de registros para un análisis de cronograma de ataque más completo.

### CloudTrail: RDS Público
1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail para ver el nombre del evento `ModifyDBInstance`, `ModifyDBSnapshotAttribute` o `ModifyDBClusterSnapshotAttribute` y busque el valor de los eventos `PubliclyAccessible` de direcciones IP públicas

### VPC Flow Logs
VPC Flow Logs es una característica que le permite capturar información sobre el tráfico IP que entra y sale de las interfaces de red de la VPC. Esto puede resultar útil para las direcciones IP detectadas en CloudTrail para determinar los tipos de conexiones externas a cualquier recurso público.

Para obtener más información y pasos, incluida la consulta con Athena, consulte la [Documentación de AWS para VPC Flow Logs] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Se recomienda que el análisis de Athena se incluya en un libro de jugadas separado y se vincule a otros elementos relevantes.

### Endpoint/Basado en host
- Revise los registros de aplicaciones y sistemas operativos EC2 para detectar inicios de sesión inapropiados, instalación de software desconocido o la presencia de archivos no reconocidos.

- Se recomienda encarecidamente contar con una solución de sistema de detección de intrusiones (HIDS) basado en host de terceros (como OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/), otros)

## Contención

### Exposición pública de RDS
Cuando lanza una instancia de base de datos dentro de una VPC, la instancia de base de datos tiene una dirección IP privada para el tráfico dentro de la VPC. Esta dirección IP privada no es accesible públicamente. Puede utilizar la opción Acceso público para indicar si la instancia de base de datos también tiene una dirección IP pública además de la dirección IP privada.

En la siguiente ilustración se muestra la opción Acceso público de la sección Configuración de conectividad adicional. Para configurar la opción, abra la sección Configuración de conectividad adicional de la sección Conectividad.

! [Imagen] (/images/VPC-example.png)

## Erradicación

### Recursos no autorizados o no reconocidos de RDS
Eliminar cualquier instantánea o base de datos públicas no reconocidas o no autorizadas

1. Inicie sesión en AWS Management Console y abra la consola de Amazon RDS en https://console.aws.amazon.com/rds/
1. En el panel de navegación, elija Instantáneas.
1. Aparece la lista Instantáneas manuales.
1. Elija la instantánea de base de datos que desea eliminar.
1. En Acciones, elija Eliminar instantánea.
1. Selecciona Eliminar en la página de confirmación.

## Recuperación
Los mismos procedimientos que los enumerados para la erradicación

## Acciones preventivas

### Comprobaciones de seguridad de RDS
- Comprobaciones de seguridad de RDS: `. /merodeador -g grupo13`

### Política de control de seguridad: cifrado RDS
[Aplicar el cifrado RDS obligatorio] (https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) tiene varias reglas automatizadas para protegerse contra el acceso público, incluido [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)

### Postura general de seguridad
Ejecute una [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook sobre cómo mitigar los recursos que se hacen públicos incorrectamente
- Como Respondedor de Incidentes, necesito poder detectar recursos públicos (AMI, volúmenes de EBS, repositorios de ECR, etc.)
- Como Respondedor de Incidentes, necesito saber qué roles son capaces de realizar cambios críticos en AWS
- Como Respondedor de Incidentes, necesito asegurarme de que todas las instantáneas (RDS, EBS, ECR) requieran cifrado

## Elementos atrasados actuales