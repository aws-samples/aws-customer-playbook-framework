# Guía de respuesta a incidentes: análisis de VPC Flow Logs
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha aprobada:

## Resumen ejecutivo
En este manual se describen ejemplos de procesos y consultas para analizar los registros de flujo de AWS VPC.

## Preparación
Este manual hace referencia e integra, en la medida de lo posible, con [Prowler] (https://github.com/toniblyx/prowler), que es una herramienta de línea de comandos que le ayuda con la evaluación de la seguridad, la auditoría, el refuerzo y la respuesta a incidentes de AWS.

Sigue las directrices del CIS Amazon Web Services Foundations Benchmark (49 comprobaciones) y tiene más de 100 comprobaciones adicionales, incluidas las relacionadas con el RGPD, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 y otros.

Esta herramienta proporciona una instantánea rápida del estado de seguridad actual dentro de un entorno de cliente. Además, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) proporciona un análisis automatizado de conformidad y puede [integrarse con Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Herramienta: Athena
* **Categoría**: Análisis de registros
* **Recurso**: Athena
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros
* **Fuentes de registro**: VPC Flow Logs
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

### Pasos de respuesta
1. [**PREPARACION**] Crear un inventario de activos expuesto públicamente
2. [**PREPARACIÓN**] Registro de configuración
3. [**PREPARACION**] Implementar un programa de formación para identificar y evaluar las capacidades de análisis de registros
4. [**DETECCIÓN Y ANALÍSIS**] Visualización de VPC Flow Logs en la consola
5. [**DETECCIÓN Y ANALÍSIS**] Consulta de registros de flujo mediante Amazon Athena
6. [**DETECCIÓN Y ANALÍSIS**] Crear la tabla VPC Flow Logs como DDL
7. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: Crear la tabla NEW Logs como DDL
8. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: compruebe si está cargada y cuántas líneas hay en un archivo
9. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: compruebe si se devuelven datos visibles y utilizables:
10. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: Compruebe si hay las principales IP
11. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: Consultas estándar para realizar investigaciones
12. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: Buscar relación de datos para IP de origen/destino
13. [**DETECCIÓN Y ANALÍSIS**] Ejemplos de consultas de Athena: Búsqueda de IOCs conocidos
14. [**DETECCIÓN Y ANALÍSIS**] Ejemplo de consultas de Athena: tabla de amenazas
15. [**DETECCIÓN Y ANALÍSIS**] Ejemplo de consultas de Athena: vista de búsqueda para el tipo de amenaza objetivo

## Preparación
### Inventario de activos expuesto públicamente
Puede utilizar el [Panel de VPC] (https://console.aws.amazon.com/vpc/home) y el [Panel de Network Manager] (https://console.aws.amazon.com/networkmanager/home) que le permiten visualizar y supervisar los recursos de red. El panel de Network Manager incluye información sobre los recursos de la red global, su ubicación geográfica, la topología de red, las métricas y los eventos de Amazon CloudWatch, y le permite realizar análisis de rutas.

Debe mantener un inventario y supervisar los cambios realizados en los siguientes recursos:
- Nubes privadas virtuales (VPC)
- Listas de control de acceso a redes de VPC (NACL)
- Grupos de seguridad de VPC
- Tablas de enrutamiento de VPC
- Interfaces de red elásticas (ENI) de VPC
- Pasarelas de Internet de VPC
- Conexiones de emparejamiento VPC
- Pasarelas NAT VPC
- Puntos finales de VPC
- Pasarelas de clientes VPN
- Direcciones IP elásticas (EIP) de VPC

Prowler también puede ayudarlo a encontrar muchos de los recursos expuestos a Internet:». /merodeador -g grupo 17"

### Registro de configuración
**Registros de flujo: **
- Los registros de flujo capturan información sobre el tráfico IP desde y hacia las interfaces de red de la VPC.
- Puede crear un registro de flujo para una VPC, subred o interfaz de red individual.
- Los datos del registro de flujo se publican en CloudWatch Logs o Amazon S3 y pueden ayudarle a diagnosticar reglas de ACL de red y grupos de seguridad excesivamente restrictivas o excesivamente permisivas.

**Para crear un registro de flujo para una interfaz de red mediante la consola**
1. Abra la consola de Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. En el panel de navegación, elija Interfaces de red.
1. Seleccione las casillas de verificación de una o más interfaces de red y elija Acciones, Create flow log.
1. En Filtro, especifique el tipo de tráfico que desea registrar.
- Elija Todo para registrar el tráfico aceptado y rechazado, Rechazar para registrar solo el tráfico rechazado o Aceptar para registrar solo el tráfico aceptado.
1. En Intervalo de agregación máximo, elija el período máximo de tiempo durante el que se captura un flujo y se agrega en un registro de registro de flujo.
1. En Destino, selecciona Send to an Amazon S3 bucket.
1. Para S3 bucket ARN, especifique el Amazon Resource Name (ARN) de un bucket de Amazon S3 existente.
- Puede incluir una subcarpeta en el ARN del bucket. El depósito no puede utilizar AWSLogs como nombre de subcarpeta, ya que se trata de un término reservado.
- Por ejemplo, para especificar una subcarpeta denominada my-logs en un bucket denominado my-bucket, utilice el siguiente ARN: arn:aws:s3። :my-bucket/my-logs/
- Si eres el propietario del depósito, creamos automáticamente una política de recursos y la adjuntamos al bucket. Para obtener más información, consulte Permisos de bucket de Amazon S3 para los registros de flujo.
1. En Formato de registro de registro, seleccione el formato del registro de flujo.
* Para utilizar el formato predeterminado, elija el AWS default format.
* Para utilizar un formato personalizado, elija Formato personalizado y, a continuación, seleccione campos en Formato de registro.
1. Elija Create flow log.

**Para crear un registro de flujo para una VPC o una subred mediante la consola**
1. Abra la consola de Amazon [VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Sus VPC o elija Subnets.
1. Seleccione la casilla de verificación de una o más VPC o subredes y, a continuación, seleccione Acciones, Create flow log.
1. En Filtro, especifique el tipo de tráfico que desea registrar.
- Elija Todo para registrar el tráfico aceptado y rechazado, Rechazar para registrar solo el tráfico rechazado o Aceptar para registrar solo el tráfico aceptado.
1. En Intervalo de agregación máximo, elija el período máximo de tiempo durante el que se captura un flujo y se agrega en un registro de registro de flujo.
1. En Destino, selecciona Send to an Amazon S3 bucket.
1. Para S3 bucket ARN, especifique el Amazon Resource Name (ARN) de un bucket de Amazon S3 existente.
- Puede incluir una subcarpeta en el ARN del bucket. El depósito no puede utilizar AWSLogs como nombre de subcarpeta, ya que se trata de un término reservado.
- Por ejemplo, para especificar una subcarpeta denominada my-logs en un bucket denominado my-bucket, utilice el siguiente ARN: «arn:aws:s3። :my-bucket/my-logs/
- Si eres el propietario del depósito, creamos automáticamente una política de recursos y la adjuntamos al bucket. Para obtener más información, consulte Permisos de bucket de Amazon S3 para los registros de flujo.
1. En Formato de registro de registro, seleccione el formato del registro de flujo.
* Para utilizar el formato predeterminado, elija el AWS default format.
* Para utilizar un formato personalizado, elija Formato personalizado y, a continuación, seleccione campos en Formato de registro.
1. Elija Create flow log.

**Supervisión de puertas de enlace NAT: ** Puede supervisar la puerta de enlace NAT mediante CloudWatch, que recopila información de su gateway NAT y crea métricas legibles y casi en tiempo real.
1. Las métricas de puerta de enlace NAT se envían a CloudWatch a intervalos de 1 minuto.
1. Puede ver las métricas de sus puertas de enlace NAT de la siguiente manera.
* Abra la [consola de Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* En el panel de navegación, elija Métricas.
* En Todas las métricas, elija el espacio de nombres de métrica de puerta de enlace NAT.
* Para ver las métricas, seleccione la dimensión métrica.
1. Para crear una alarma para el tráfico saliente a través de la puerta de enlace NAT:
* Abra la [consola de Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* En el panel de navegación, elija Alarmas, Crear alarma.
* Elija puerta de enlace NAT.
* Seleccione la puerta de enlace NAT y la métrica «BytesOutToDestination» y elija Siguiente.
* Configure la alarma de la siguiente manera y elija Crear alarma cuando haya terminado:
* En Umbral de alarma, introduzca un nombre y una descripción para la alarma. En Whenever, elija >= e introduzca 5000000. Introduzca 1 durante los periodos consecutivos.
* En Acciones, seleccione una lista de notificaciones existente o elija Nueva lista para crear una nueva.
* En Vista previa de alarmas, seleccione un período de 15 minutos y especifique una estadística de Suma.
1. Para crear una alarma para supervisar los errores de asignación de puertos
* Abra la [consola de Amazon CloudWatch] (https://console.aws.amazon.com/cloudwatch/)
* En el panel de navegación, elija Alarmas, Crear alarma.
* Elija NAT Gateway.
* Seleccione la puerta de enlace NAT y la métrica «ErrorPortAllocation» y elija Siguiente.
* Configure la alarma de la siguiente manera y elija Crear alarma cuando haya terminado:
* En Umbral de alarma, introduzca un nombre y una descripción para la alarma. En Whenever, elija > e introduzca 0. Introduzca 3 durante los periodos consecutivos.
* En Acciones, seleccione una lista de notificaciones existente o elija Nueva lista para crear una nueva.
* En Vista previa de alarmas, seleccione un período de 5 minutos y especifique una estadística de Máximo.

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

## Detección y análisis
### Visualización de VPC Flow Logs en la consola
**Para ver información sobre los registros de flujo de las interfaces de red**
1. Abra la consola de Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. En el panel de navegación, elija Interfaces de red.
1. Seleccione una interfaz de red y elija Logs de flujo. La información sobre los registros de flujo se muestra en la pestaña. La columna Tipo de destino indica el destino en el que se publican los registros de flujo.

**Para ver información sobre los registros de flujo de sus VPC o subred**
1. Abra la [consola de Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Sus VPC o Subnets.
1. Seleccione su VPC o subred y elija Logs de flujo.
- La información sobre los registros de flujo se muestra en la pestaña.
- La columna Tipo de destino indica el destino en el que se publican los registros de flujo.

**Para ver los registros de flujo publicados en Amazon S3**
1. Abra la [consola de Amazon S3] (https://console.aws.amazon.com/s3/)
1. En Nombre del bucket, seleccione el bucket en el que se publican los registros de flujo.
1. En Nombre, active la casilla de verificación situada junto al archivo de registro. En el panel de descripción general de objetos, elija Descargar.

### Consultar registros de flujo mediante Amazon Athena
Amazon Athena es un servicio de consultas interactivas que le permite analizar datos en Amazon S3, como los registros de flujo, mediante SQL estándar. Puede utilizar Athena con VPC Flow Logs para obtener rápidamente información útil sobre el tráfico que fluye a través de la VPC. Por ejemplo, puede identificar qué recursos de las nubes privadas virtuales (VPC) son los principales hablantes o identificar las direcciones IP con las conexiones TCP más rechazadas.

Puede optimizar y automatizar la integración de los registros de flujo de VPC con Athena generando una plantilla de CloudFormation que crea los recursos de AWS necesarios y las consultas predefinidas que puede ejecutar para obtener información sobre el tráfico que fluye a través de la VPC.

1. La plantilla de CloudFormation crea los siguientes recursos:
- Una base de datos de Athena. El nombre de la base de datos es vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>.
- Un grupo de trabajo de Athena. El nombre del <flow-log-subscription-id><partition-load-frequency><start-date><flow-log-subscription-id><partition-load-frequency><start-date><end-date><end-date>workgroup es grupo de trabajo
- Tabla de Athena particionada que corresponde a sus registros de registro de flujo. El nombre de la tabla es <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
- Un conjunto de consultas con nombre de Athena. Para obtener más información, consulte Consultas predefinidas.
- Función Lambda que carga nuevas particiones en la tabla según la programación especificada (diaria, semanal o mensual).
- Un rol de IAM que concede permiso para ejecutar las funciones Lambda.
1. Requisitos
- Debe seleccionar una región que admita AWS Lambda y Amazon Athena.
- Los depósitos de Amazon S3 deben estar en la región seleccionada.
1. Precios
- Incurrirá en cargos estándar de Amazon Athena por ejecutar consultas.
- Incurrirá en cargos estándar de AWS Lambda por la función Lambda que carga nuevas particiones en un programa recurrente (cuando especifica una frecuencia de carga de partición pero no especifica una fecha de inicio y finalización).
1. Siga uno de estos procedimientos:
- Abra la consola de Amazon VPC. En el panel de navegación, elija Sus VPC y, a continuación, seleccione la VPC.
- Abra la consola de Amazon VPC. En el panel de navegación, elija Subnets y, a continuación, seleccione la subred.
- Abra la consola Amazon EC2. En el panel de navegación, elija Interfaces de red y, a continuación, seleccione la interfaz de red.
1. En la pestaña Registros de flujo, seleccione un registro de flujo que se publica en Amazon S3 y, a continuación, elija Acciones, Generar integración de Athena.
1. Especifique la frecuencia de carga de la partición.
- Si elige Ninguno, debe especificar la fecha de inicio y finalización de la partición, utilizando fechas anteriores.
- Si elige Diaria, Semanal o Mensual, las fechas de inicio y finalización de la partición son opcionales.
- Si no especifica fechas de inicio y finalización, la plantilla CloudFormation crea una función Lambda que carga nuevas particiones en una programación periódica.
1. Seleccione o cree un bucket de S3 para la plantilla generada y un bucket de S3 para los resultados de la consulta.
1. Elija Generar integración de Athena.
1. (Opcional) En el mensaje de éxito, elija el enlace para navegar hasta el depósito que especificó para la plantilla de CloudFormation y personalice la plantilla.
1. En el mensaje de éxito, elija Crear pila de CloudFormation para abrir el asistente Crear pila en la consola de AWS CloudFormation.
- La URL de la plantilla de CloudFormation generada se especifica en la sección Plantilla.
- Complete el asistente para crear los recursos especificados en la plantilla.

**Ejecución de una consulta predefinida**

La plantilla de CloudFormation generada proporciona un conjunto de consultas predefinidas que puede ejecutar para obtener rápidamente información significativa sobre el tráfico de su red de AWS. Después de crear la pila y verificar que todos los recursos se han creado correctamente, puede ejecutar una de las consultas predefinidas.

1. Para ejecutar una consulta predefinida mediante la consola
- Abre la consola Athena. En el panel Grupos de trabajo, seleccione el grupo de trabajo creado por la plantilla de CloudFormation.
- Seleccione una de las consultas predefinidas, modifique los parámetros según sea necesario y, a continuación, ejecute la consulta.
- Abra la consola de Amazon S3. Desplácese hasta el depósito que especificó para los resultados de la consulta y vea los resultados de la consulta.

Las siguientes son las consultas con nombre de Athena proporcionadas por la plantilla de CloudFormation generada:
* VPCFlowLogsAcceptedTraffic: las conexiones TCP permitidas en función de los grupos de seguridad y las ACL de red.
* VPCFlowLogsAdminPortTraffic: tráfico registrado en los puertos de aplicaciones web administrativas.
* VpcFlowLogsIPv4Traffic: el total de bytes de tráfico IPv4 registrados.
* VpcFlowLogsIPv6Traffic: el total de bytes de tráfico IPv6 registrados.
* VPCFlowLogsRejectedTCPTraffic: las conexiones TCP rechazadas en función de sus grupos de seguridad o ACL de red.
* VPCFlowLogsRejectedTraffic: tráfico rechazado en función de sus grupos de seguridad o ACL de red.
* VPCFlowLogSSSHRDPTraffic: tráfico SSH y RDP.
* VPCFlowLogStopTalkers: 50 direcciones IP con mayor tráfico registrado.
* VPCFlowLogStopTalkersPacketLevel: 50 direcciones IP a nivel de paquete con mayor tráfico registrado.
* VPCFlowLogStopTalkingInstances: ID de las 50 instancias con mayor tráfico registrado.
* VPCFlowLogStopTalkingSubnets: ID de las 50 subredes con mayor tráfico registrado.
* VPCFlowLogStopTCPTraffic: todo el tráfico TCP registrado para una dirección IP de origen.
* VpcFlowLogsTotalBytesTransferred: 50 pares de direcciones IP de origen y destino con la mayor cantidad de bytes registrados.
* VPCFlowLogsTotalBytesTransferredPacketLevel: los 50 pares de direcciones IP de origen y destino a nivel de paquete con la mayor cantidad de bytes registrados.
* VpcFlowLogsTrafficFrmSrcAddr: tráfico registrado para una dirección IP de origen específica.
* VpcFlowLogsTrafficToDstAddr: tráfico registrado para una dirección IP de destino específica

## Análisis de inmersión profunda:

Referencia: https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### Crear la tabla VPC Flow Logs como DDL
El siguiente código se utiliza para crear la tabla como DDL para los VPC Flow Logs que se examinarán. (Tome nota de los artículos TODO en línea):
```
/*
Crea una tabla de flujo de VPC para los registros de flujo de VPC entregados directamente a un bucket de S3
Incluye configuración de particiones para implementación multicuenta, multiregión y fecha en formato AAAA/MM/DD
Incluye todos los campos disponibles, incluidos los campos v3 y v4 no predeterminados.
NOTA: Los campos de flujo de VPC deben configurarse en el orden indicado cuando se configuran; si se configuraron en otro orden, deberá ajustar el orden que aparece en el DDL que aparece a continuación
*/

/*
TAREAS: actualice opcionalmente el nombre de la tabla «vpcflow» al nombre que desea utilizar para la tabla de registro de flujo de VPC
*/
CREAR TABLA EXTERNA SI NO EXISTE vpcflow (
/*
TAREAS: compruebe que los registros de flujo de VPC se hayan configurado para generarse en este orden; si no lo estaban, tendrá que reorganizar el pedido siguiente para que coincida con el orden en que se generaron
SUGERENCIA: descargue un ejemplo y compruebe la primera línea de cada archivo de registro para el orden de campo,
No te preocupes si los nombres de los campos no coinciden exactamente con los nombres de la línea superior del registro, Athena los importará según su pedido
NOTA: Estos campos v2 están en el formato predeterminado y, por lo general, no es necesario ajustarse, preste más atención al orden de los campos v3/v4/v5 a continuación, ya que no hay ordenamiento predeterminado de esos campos. Los campos y las descripciones se pueden encontrar en https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html
*/
versión int,
cadena de cuenta,
cadena de identificación de interfaz,
cadena de dirección de origen,
cadena de dirección de destino,
sourceport int,
destinationport int,
protocol int,
numpackets int,
numéricos bigint,
starttime int,
endtime int,
cadena de acción,
cadena logstatus,
/*
NOTA: inicio de campos v3 y v4 no predeterminados
no se preocupe si los registros de origen no incluyen todos estos campos, solo se mostrarán en blanco
TAREAS: Si los registros de flujo de la VPC incluyen estos campos, asegúrese de comprobar que el orden en el que se enumeran a continuación es el mismo orden en el que están configurados en el formato de registro de flujo
*/
cadena vpcid,
cadena de tipo,
tcpflags smallint,
cadena subnetid,
cadena de tipo de sububicación,
cadena de identificación de sublocalización,
cadena de región,
cadena pktsrcaddr,
cadena pktdstaddr,
cadena de instanceid,
cuerda azid,
/*
NOTA: inicio de campos v5 no predeterminados
no se preocupe si los registros de origen no incluyen todos estos campos, solo se mostrarán en blanco
TAREAS: Si los registros de flujo de la VPC incluyen estos campos, asegúrese de comprobar que el orden en el que se enumeran a continuación es el mismo orden en el que están configurados en el formato de registro de flujo
*/
cadena pkt-src-aws-service,
cadena pkt-dst-aws-service,
cadena de dirección de flujo,
trafic-path int
)
PARTICIONADO POR
(
date_partition STRING,
region_partition STRING,
account_partition STRING
)
FORMATO DE FILA DELIMITADO
CAMPOS TERMINADOS POR ''
/*
TAREAS: sustituir bucket_name y optional_prefix en el valor LOCATION,
si no hay prefijo, elimine el/adicional
ejemplo: s3: //my_central_log_bucket/awslogs/ o s3: //my_central_log_bucket/prod/awslogs/
*/
LOCATION 's3://<bucket_name>/<optional_prefix>/AWSlogs/ '
PROPIEDADES TBL/
(
«skip.header.line.count"="1",
«projection.enabled» = «true»,
«projection.date_partition.type» = «fecha»,
/* TODO: reemplace<YYYY>/<MM>/por <DD>la primera fecha de sus registros, por ejemplo: 2020/11/30 */
«projection.date_partition.range» = "<YYYY>/<MM>/<DD>, AHORA»,
«projection.date_partition.format» = «AAAA/mm/DD»,
«projection.date_partition.interval» = «1",
«projection.date_partition.interval.unit» = «DÍAS»,
«projection.region_partition.type» = «enum»,
«projection.region_partition.values» = «us-east-2, us-east-1, us-west-1, us-west-2, af-south-1, ap-east-1, ap-sur--1, ap-northeast-3, ap-northeast-2, ap-northeast-2, ap-northeast-1, cn-northeast-1, cn-northeast-1, cn-northeast-1, cn-northeast-1, cn-northeast-1, cn-northeast-1, cn-northeast-1 -norte-west-1, eu-central-1, eu-west-1, eu-west-2, eu-south-1, eu-west-3, eu-norte-1, me-sur--1, sa-este-1",
«projection.account_partition.type» = «enum»,
/*
TAREAS: sustituya los valores de projection.account_partition.values por la lista de números de cuenta de AWS que desea incluir en esta tabla
ejemplo: «0123456789, 0123456788,0123456777"
nota: no utilice espacios, separe los valores solo con una coma (si se incluyen espacios, se producirá un error de sintaxis)
si solo hay una cuenta, inclúyala sola sin coma, por ejemplo: «0123456789"
*/
«projection.account_partition.values» = "<account_num_1>,<account_num_2>, ... «,
/*
TAREAS: Igual que LOCATION, reemplace bucket_name y optional_prefix en el valor storage.location.template,
si no hay prefijo, elimine el/adicional
Asegúrese de que solo se sustituyan los valores de los corchetes <>, los valores de los {} corchetes son variables y deben dejarse tal como se definen a continuación.
ejemplo: s3: //my_central_log_bucket/awslogs/... o s3: //my_central_log_bucket/prod/awslogs/...
*/
«storage.location.template» = «s3://<bucket_name>/<optional_prefix>/awslogs/ $ {account_partition} /vpcflowlogs/$ {region_partition} /$ {date_partition}»
);
```
### Consultas de ejemplo:
#### Crea la tabla NEW Logs como DDL
Cree una nueva tabla de Athena ejecutando la siguiente consulta a continuación. Tenga en cuenta:

* Reemplácelo <bucket_name>por el cubo de destino.
* Compruebe la UBICACIÓN para asegurarse de que incluye cualquier prefijo de depósito relevante y su barra diagonal final («/»)
```
CREAR TABLA EXTERNA SI NO EXISTE vpc_flow_logs (
is_rejected_packet STRING,
avg_in_packet_size INT,
vpc_id STRING,
local_port INT,
source_dc STRING,
ip STRING,
end_time STRING,
remote_port INT,
start_time STRING,
protocolo INT,
en bytes BIGINT,
source_tag STRING,
account_id STRING,
instance_id STRING,
interface_public_ips STRING,
out_bytes BIGINT,
is_skipped_rejected_packets STRING,
source_az STRING,
source_region STRING,
avg_out_packet_size INT,
instance_type STRING,
interface_local_ips STRING
)
FORMATO DE FILA SERDE 'org.openx.data.jsonSerde.jsonSerde'
CON SERDEPROPERTIES ('ignore.malformed.json' = 'true')
UBICACIÓN 's3://<bucket_name>/';
```
#### Comprueba si está cargado y cuántas líneas hay en el archivo:
```
SELECCIONE count (*) DE vpc_flow_logs;
```
#### Comprueba si se devuelven datos visibles y utilizables:
```
SELECCIONE * DE vpc_flow_logs limit 1000;
```
#### Buscar las principales IP
```
seleccione ip, count (*) cnt
de vpc_flow_logs
agrupar por ip
ordenar por cnt desc
límite 100;

Toma algunas IP principales y ordena por tiempo (cambia la IP a continuación)

seleccione * de vpc_flow_logs
donde ip = '123.123.123.123'
ordenar por start_time;
```

### «Notas de interfaz de S3 Gateway /VPC»
Para los «endpoints de VPC», las conexiones se mostrarán en los registros de flujo como tráfico a través de sus direcciones privadas

S3 Gateway se muestra en los registros de flujo como la dirección privada que se conecta a los rangos públicos en la lista de prefijos

### Consultas estándar para realizar investigaciones

#### Buscar relación de datos para IP de origen/destino
La siguiente consulta agrega registros de flujo de VPC por IP, recuento de registros (número total de registros de la misma dirección de origen o destino) y bytes totales (bytes totales de comunicación entre la dirección de origen o destino). La consulta también proporciona la relación de datos, que es la cantidad de bytes enviados por comunicación (bytes totales divididos por recuento de registros).
```
SELECCIONAR dirección de origen,
dirección de destino,
count (*) AS record_count,
SUM (números) AS total_bytes,
SUM (números) /count (*) AS data_ratio,
date_format (from_unixtime (min (hora de inicio)),
'%y/%m/%d%h: %i: %s') AS primero visto, date_format (de_unixtime (máximo (hora final)), '%y/%m/%d%h: %i: %s') AS last_seen
DESDE "<database>«.» <table>»
WHERE (dirección de origen = '10.10.10.10'
OR destinationaddress = '10.10.10.10')
GROUP BY dirección de origen, dirección de destino
ORDENAR POR total_bytes DESC
```
#### Búsqueda de IOC conocidos
En la siguiente consulta se supone que los IOC son direcciones IP externas y utilizan una tabla de búsqueda para administrarlas.

#### Tabla de amenazas
```
CREAR TABLA EXTERNA SI NO EXISTE incident_iocs (
cadena threat_ip,
string threat_comment
)

FORMATO DE FILA SERDE
'org.apache.hadoop.hive.serde2.opencsvserde'

CON PROPIEDADES SERDE (
'SeparatorChar' = ', ',
'QuoteChar' = '\ "',
'Escapechar' = '\\'
)
ALMACENADO COMO ARCHIVO DE TEXTO
UBICACIÓN 's3: //bucket-analysis/iocs/iocs/iocfileaquí/'
```
#### Vista de búsqueda para el tipo de amenaza objetivo
```
CREAR VISTA incident_vpc_flow_directional AS
SELECCIONE vpc.account,
vpc.interfaz id,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) COMO fuente,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) COMO destino,
Dirección AS «ENTRANTE»,
vpc.sourceaddress AS connecting_ip,
vpc.sourceport AS connecting_port,
vpc.protocol,
vpc.numpackets,
vpc.numbytes,
vpc.action,
vpc.logstatus,
CAST (from_unixtime (vpc.starttime) AS TIMESTAMP) AS StartTime,
CAST (from_unixtime (vpc.endtime) AS TIMESTAMP) AS Final Time,
date_diff ('second', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) como SessionTime

DESDE "<database>«.» <table>"AS vpc

DONDE
vpc.destinationaddress IN (SELECCIONE threat_ip FROM incident_iocs)

UNION ALL

SELECCIONE vpc.account,
vpc.interfaz id,
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) COMO fuente,
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) COMO destino,
Dirección AS «SALIENTE»,
vpc.dirección de destino AS connecting_ip,
vpc.destinationport AS connecting_port,
vpc.protocol,
vpc.numpackets,
vpc.numbytes,
vpc.action,
vpc.logstatus,
CAST (from_unixtime (vpc.starttime) AS TIMESTAMP) AS StartTime,
CAST (from_unixtime (vpc.endtime) AS TIMESTAMP) AS Final Time,
date_diff ('second', from_unixtime (vpc.starttime), from_unixtime (vpc.endtime)) como SessionTime

DESDE "<database>«.» <table>"AS vpc
DONDE
vpc.sourceaddress IN (SELECCIONE threat_ip DE incident_iocs)

ORDENAR POR Hora de inicio, Hora de finalización, SessionTime, interfaceid, connecting_ip, direction
```

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito poder supervisar todos los entornos de VPC críticos
- Como Respondedor de Incidentes, necesito un libro de jugadas para consultar VPC FlowLogs a escala

## Elementos atrasados actuales