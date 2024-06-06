# Guía de respuesta a incidentes: Respuesta de rescate para RDS
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por los acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso de respuesta a los ataques de rescate contra Amazon Relational Database Service (RDS)

Para obtener más información, consulte la [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

### Objetivos
Durante la ejecución del libro de jugadas, concéntrese en los _***resultados deseados***_, tomando notas para mejorar las capacidades de respuesta a incidentes.

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
1. [**PREPARATION**] Utilizar AWS Config para ver el cumplimiento de la configuración
2. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
3. [**CONTAINMENT**] Aislar inmediatamente los recursos afectados
5. [**DETECCIÓN Y ANALÍSIS**] Utilizar métricas de CloudWatch para determinar si es posible que se hayan extraído datos
6. [**DETECCIÓN Y ANALÍSIS**] Utilizar VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
7. [**RECOVERY**] Ejecute los procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Rescate y destrucción de datos
* **Categoría**: Ataque de rescate
* **Recurso**: RDS
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: RDS Database Log Files, S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Preparación
* Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
* AWS desarrolló una nueva herramienta de Self-Service Security Assessment de código abierto (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de su AWS cuenta.
* Mantener un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo
* Considere utilizar [AWS Config] (https://aws.amazon.com/config/), que es un servicio que le permite evaluar, auditar y evaluar las configuraciones de sus recursos de AWS
* Considere la posibilidad de implementar [AWS GuardDuty] (https://aws.amazon.com/guardduty/) para supervisar continuamente la actividad maliciosa y el comportamiento no autorizado a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon RDS
* [Ejecute su instancia de base de datos en una nube privada virtual (VPC)] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) basada en el servicio Amazon VPC para obtener el mayor control posible de acceso a la red
* Utilice las políticas de AWS Identity and Access Management (IAM) para asignar permisos que determinan quién puede administrar los recursos de Amazon RDS. Por ejemplo, puede utilizar IAM para determinar quién puede crear, describir, modificar y eliminar instancias de base de datos, etiquetar recursos o modificar grupos de seguridad.
* Utilice grupos de seguridad para controlar qué direcciones IP o instancias de Amazon EC2 se pueden conectar a las bases de datos de una instancia de base de datos. Cuando crea por primera vez una instancia de base de datos, su cortafuegos impide el acceso a la base de datos excepto mediante reglas especificadas por un grupo de seguridad asociado.
* [Utilizar conexiones Secure Socket Layer (SSL) o Transport Layer Security (TLS)] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html) con instancias de base de datos que ejecutan los motores de bases de datos MySQL, MariaDB, PostgreSQL, Oracle o Microsoft SQL Server
* [Utilice el cifrado de Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) para proteger sus instancias de base de datos e instantáneas en reposo. El cifrado de Amazon RDS utiliza el algoritmo de cifrado AES-256 estándar del sector para cifrar los datos en el servidor que aloja la instancia de base de datos
* [Usar cifrado de red] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) y [cifrado de datos transparente] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) con instancias de base de datos Oracle
* Utilice las funciones de seguridad de su motor de base de datos para controlar quién puede iniciar sesión en las bases de datos de una instancia de base de datos. Estas características funcionan como si la base de datos estuviera en la red local.
* Se recomienda encarecidamente contar con una solución de sistema de detección de intrusiones (HIDS) basado en host de terceros (como OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
* Hay referencias y pasos adicionales disponibles en [Seguridad en Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)

### Utilice AWS Config para ver el cumplimiento de la configuración:
1. Inicie sesión en AWS Management Console y abra la consola de AWS Config en https://console.aws.amazon.com/config/
1. En el menú de AWS Management Console, compruebe que el selector de regiones esté configurado en una región compatible con las reglas de AWS Config. Para obtener la lista de regiones compatibles, consulte Regiones y puntos finales de AWS Config en la referencia general de Amazon Web Services
1. En el panel de navegación, elija Recursos. En la página Inventario de recursos, puede filtrar por categoría de recursos, tipo de recurso y estado de conformidad. Elija Incluir recursos eliminados si procede. En la tabla se muestra el identificador de recursos del tipo de recurso y el estado de conformidad de recursos de ese recurso. El identificador de recurso podría ser un ID de recurso o un nombre de recurso
1. Elija un recurso de la columna de identificador de recursos
1. Pulse el botón Línea temporal de recursos. Puede filtrar por eventos de configuración, eventos de cumplimiento o eventos de CloudTrail
1. Concéntrese específicamente en los siguientes eventos:
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
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted

## Procedimientos de escalada
- `Necesito una decisión empresarial sobre cuándo debe llevarse a cabo la investigación forense de EC2`
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Detección y análisis
* [AWS GuardDuty machine learning] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) puede detectar comportamientos sospechosos, incluida la generación de instantáneas de Amazon Relational Database Service (Amazon RDS) como parte de los intentos de exfiltración de datos
* Revise los registros de aplicaciones y sistemas operativos EC2 para detectar inicios de sesión inapropiados, instalación de software desconocido o la presencia de archivos no reconocidos

### [Utilizar métricas de CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
Busque «picos» de exfiltración de datos. Es posible que un atacante haya realizado la destrucción de datos y haya dejado una nota de rescate, y en estos casos no hay oportunidad de recuperación de datos trabajando con el actor malintencionado.
1. Abra la consola de CloudWatch en https://console.aws.amazon.com/cloudwatch/
1. En el panel de navegación, elija Métricas y luego Todas las métricas
1. En la pestaña Todas las métricas, seleccione la región en la que se implementa la instancia
1. En la pestaña Todas las métricas, introduzca el término de búsqueda `NetworkPacketsOut` y pulse Intro
1. Selecciona uno de los resultados de tu búsqueda para ver las métricas
1. Para graficar una o más métricas, active la casilla de verificación situada junto a cada métrica. Para seleccionar todas las métricas, active la casilla de verificación de la fila de encabezado de la tabla
1. (Opcional) Para cambiar el tipo de gráfico, elija Opciones de gráfico. A continuación, puede elegir entre un gráfico de líneas, un gráfico de áreas apiladas, un gráfico de barras, un gráfico circular o un número
1. (Opcional) Para agregar una banda de detección de anomalías que muestre los valores esperados para la métrica, elija el icono de detección de anomalías en Acciones junto a la métrica

### Utilice VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
1. Utilice el [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) para analizar los datos de registro
1. Obtenga un resumen con el número de bytes de cada quad src_ip, src_port, dst_ip, dst_port en todos los registros hacia o desde una IP específica
```
SELECCIONAR dirección de origen, dirección de destino, puerto de origen, puerto de destino, suma (números) como byte_count FROM vpcflow
WHERE (dirección de origen = '192.0.2.1' OR dirección de destino = '192.0.2.1')
AND date_partition >= '2020/07/01'
AND date_partition <= '2020/07/31'
AND account_partition = '111122223333'
AND region_partition en («us-east-1», «us-east-2», «us-west-2», «us-west-2»)
GRUPO POR dirección de origen, dirección de destino, puerto de origen, puerto de destino
ORDENAR POR byte_count DESC
```
1. Otras consultas de ejemplo se proporcionan en [vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql)


## Recuperación
* Se recomienda no pagar el rescate
* Pagar el rescate es una apuesta en cuanto a si el criminal cumplirá la transacción después de recibir el pago
* Si no existen copias de seguridad de datos, debe realizar un análisis de costes y beneficios y sopesar el valor del compromiso de datos/reputación frente al pago al atacante
* Usted permite directamente al atacante continuar sus operaciones contra su empresa u otras personas si decide pagar el rescate
* Visite https://www.nomoreransom.org/ para identificar si hay un descifrado disponible para la variante de malware que infecta sus datos
* [Eliminar o rotar claves de usuario de IAM] (https://console.aws.amazon.com/iam/home#users) y [Claves de usuario Root] (https://console.aws.amazon.com/iam/home#security_credential); puede que desee rotar todas las claves de su cuenta si no puede identificar una clave o claves específicas que se hayan expuesto
* [Eliminar usuarios de IAM no autorizados] (https://console.aws.amazon.com/iam/home#users.)
* [Eliminar políticas no autorizadas] (https://console.aws.amazon.com/iam/home#/policies)
* [Eliminar roles no autorizados] (https://console.aws.amazon.com/iam/home#/roles)
* [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM. NOTA: La eliminación de usuarios de IAM puede afectar las cargas de trabajo de producción y debe realizarse con cuidado
* Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar las cargas de trabajo en AWS
* Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restablezca desde el último evento programado anterior al evento de ransomware
* Eliminar cualquier instantánea o base de datos públicas no reconocidas o no autorizadas
* Eliminar la base de datos comprometida y crear nuevas bases de datos RDS
* Identificar instancias EC2 que tuvieran acceso permisivo a las bases de datos e investigarlas también

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook para llevar a cabo el análisis forense de EC2
- Como Respondedor de Incidentes, necesito una decisión empresarial sobre cuándo deben llevarse a cabo los análisis forenses de EC2
- Como Respondedor de Incidentes, necesito tener habilitado el registro en todas las regiones habilitadas independientemente de la intención de uso
- Como Respondedor de Incidentes, necesito poder detectar la minería de criptomonedas en mis instancias EC2 existentes

## Elementos atrasados actuales