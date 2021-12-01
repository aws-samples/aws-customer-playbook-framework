# Guía de respuesta a incidentes: cambios de red no autorizados
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por los acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para responder a cambios no autorizados o sospechosos en los activos de red dentro de un entorno de AWS.

## Indicadores potenciales de compromiso
- Recursos de servicio nuevos o no reconocidos
- Recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
- Aumentos de facturación inusuales
- Notificación del investigador de seguridad
- Notificación de que mis recursos o cuenta de AWS podrían verse comprometidos

## Conclusiones potenciales de AWS GuardDuty
- Puerta trasera: EC2/C&C Activity.B
- Puerta trasera: EC2/C&C Activity.B! DNS
- Puerta trasera: EC2/spambot
- Comportamiento: EC2/puerto de red inusual
- Comportamiento: EC2/Volumen de tráfico inusual
- Criptomoneda: EC2/BitcoinTool.b
- Criptomoneda: EC2/BitcoinTool.B! DNS
- Impacto: EC2/solicitud de dominio abusado. Reputación
- Impacto: solicitud de dominio EC2/Bitcoin. Reputación
- Impacto: EC2/solicitud de dominio malicio.reputación
- Impacto: EC2/Solicitud de dominio sospechoso. Reputación
- Impacto: EC2/WinRM Bruteforce
- Recon: puerto protegido de ejecución de sonda EC2/puerto
- Recon: puerto no protegido de sonda EC2/puerto
- Trojan: EC2/tráfico de agujeros negros
- Troyano: EC2/tráfico de agujeros negros! DNS
- Trojan: solicitud de dominio EC2/DG.b
- Trojan: solicitud de dominio EC2/DG.C! DNS
- Trojan: exfiltración de datos EC2/DNS
- Trojan: EC2/Drive by Source Traffic! DNS
- Troyano: EC2/Droppoint
- Troyano: EC2/Droppoint! DNS
- Trojan: EC2/solicitud de dominio de phishing DNS
- Acceso no autorizado: EC2/IPICaller.Custom
- Acceso no autorizado: EC2/metadatos DNS Rebind
- Acceso no autorizado: EC2/RDP Brute Force
- Acceso no autorizado: EC2/SSH Brute Force
- Acceso no autorizado: EC2/TOR Client
- Acceso no autorizado: EC2/Tor Relay
- Descubrimiento: S3/Llamador IP malicioso
- Acceso a credenciales: comportamiento anómalo o de usuario de IAM
- Evasión de defensa: soy usuario/comportamiento anómalo
- Descubrimiento: Comportamiento anómalo o usuario/de IAM
- Exfiltración: Comportamiento anómalo o usuario/de IAM
- Impacto: Comportamiento anómalo o usuario/de IAM
- Acceso inicial: usuario de IAM/comportamiento anómalo
- Persistencia: comportamiento de usuario/anómalo de IAM
- Política: Uso de credenciales de usuario/raíz de IAM
- Escalación de privilegios: comportamiento anómalo de usuario de IAM
- Recon: usuario de IA/Llamador IP malicioso
- Recon: usuario de IAM/invocador malicioso. Personalizado
- Recon: Llamador de usuario/toripi
- Sigilo: inicio de sesión de usuario de IAM/Cloud Trail deshabilitado
- Stealth: Cambio de política de usuario/contraseña de IAM
- Acceso no autorizado: IAM usuario/consola Login Success.
- Acceso no autorizado: filtración de credenciales de usuario/instancia de IAM
- Acceso no autorizado: usuario de IAM/IPS malintencionada
- Acceso no autorizado: usuario de IAM/IIP Caller.Custom
- Acceso no autorizado: usuario de IAM/TOIP Calller

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
1. [**PREPARACION**] Inventario de activos expuesto públicamente
2. [**PREPARACIÓN**] Registro de configuración
3. [**PREPARACION**] Implementar un programa de formación para identificar y evaluar las capacidades forenses en la nube
4. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
5. [**DETECCIÓN Y ANALISIS**] Revisar CloudTrail
6. [**DETECCIÓN Y ANALISIS**] Revisión de CloudWatch
7. [**DETECCIÓN Y ANALÍSIS**] Utilice AWS Config para comprobar la configuración de un grupo de seguridad
8. [**DETECCIÓN Y ANALÍSIS**] Visualización de registros de flujo de VPC en la consola
9. [**DETECCIÓN Y ANALÍSIS**] Consulta de registros de flujo mediante Amazon Athena
10. [**CONTENCIÓN**] Identificar qué usuario o rol ejecutó los cambios de red no autorizados
11. [**ERRADICATION**] Revise los hallazgos de Revisar el historial de eventos de CloudTrail para ver la actividad de la clave de
12. [**ERRADICACION**] Revisar la sección Evitar cargos inesperados
13. [**RECOVERY**] Revertir cualquier cambio que se haya producido en los recursos de red
14. [**PREPARATION**] Acciones preventivas adicionales: AWS Config y AWS System Manager
15. [**PREPARACIÓN**] Acciones preventivas adicionales: AWS Security Hub
16. [**PREPARACIÓN**] Acciones preventivas adicionales: postura general de seguridad

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de la publicación especial 800-61r2 del NIST
[Guía de manejo de incidentes de seguridad informática de NIST] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
! [Imagen] (images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Herramientas: Análisis forense
* **Categoría**: Análisis forense
* **Recurso**: EC2, VPC
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: Endpoint
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Preparación

Este manual hace referencia e integra, en la medida de lo posible, con [Prowler] (https://github.com/toniblyx/prowler), que es una herramienta de línea de comandos que le ayuda con la evaluación de la seguridad, la auditoría, el refuerzo y la respuesta a incidentes de AWS.

Sigue las directrices del CIS Amazon Web Services Foundations Benchmark (49 comprobaciones) y tiene más de 100 comprobaciones adicionales, incluidas las relacionadas con el RGPD, HIPAA, PCI-DSS, ISO-27001, FFIEC, SOC2 y otros.

Esta herramienta proporciona una instantánea rápida del estado de seguridad actual dentro de un entorno de cliente. Además, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) proporciona un análisis automatizado de conformidad y puede [integrarse con Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventario de activos expuesto públicamente
Puede utilizar el [Panel de VPC] (https://console.aws.amazon.com/vpc/home) y el [Panel de Network Manager] (https://console.aws.amazon.com/networkmanager/home) que le permiten visualizar y supervisar los recursos de red. El panel de Network Manager incluye información sobre los recursos de la red global, su ubicación geográfica, la topología de red, las métricas y los eventos de Amazon CloudWatch, y le permite realizar análisis de rutas.

Debe mantener un inventario y supervisar los cambios en los siguientes recursos:
- Nubes privadas virtuales (VPC)
- Listas de control de acceso a redes de VPC (NACL)
- Grupos de seguridad de VPC
- Tablas de enrutamiento de VPC
- Interfaces de red elásticas (ENI) de VPC
- Pasarelas de Internet de VPC
- Conexiones de emparejamiento VPC
- Pasarelas NAT VPC
- Endpoints de VPC
- Pasarelas de clientes VPN
- Direcciones IP elásticas (EIP) de VPC

Prowler también puede ayudarlo a encontrar muchos de los recursos expuestos a Internet: `. /merodeador -g grupo17`

### Registro de configuración
**Registros de flujo: **
- Los registros de flujo capturan información sobre el tráfico IP desde y hacia las interfaces de red de la VPC.
- Puede crear un registro de flujo para una VPC, subred o interfaz de red individual.
- Los datos del registro de flujo se publican en CloudWatch Logs o Amazon S3 y pueden ayudarlo a diagnosticar reglas de ACL de red y grupos de seguridad excesivamente restrictivas o excesivamente permisivas.

**Para crear un registro de flujo para una interfaz de red mediante la consola**
1. Abra la consola de Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. En el panel de navegación, elija Interfaces de red.
1. Seleccione las casillas de verificación de una o más interfaces de red y elija Acciones, Crear registro de flujo.
1. En Filtro, especifique el tipo de tráfico que desea registrar.
- Elija Todo para registrar el tráfico aceptado y rechazado, Rechazar para registrar solo el tráfico rechazado o Aceptar para registrar solo el tráfico aceptado.
1. En Intervalo de agregación máximo, elija el período máximo de tiempo durante el que se captura un flujo y se agrega en un registro de registro de flujo.
1. En Destino, selecciona Enviar a un depósito de Amazon S3.
1. Para ARN de bucket de S3, especifique el nombre de recurso de Amazon (ARN) de un bucket de Amazon S3 existente.
- Puede incluir una subcarpeta en el ARN del bucket. El depósito no puede utilizar AWSLogs como nombre de subcarpeta, ya que se trata de un término reservado.
- Por ejemplo, para especificar una subcarpeta denominada my-logs en un bucket denominado my-bucket, utilice el siguiente ARN: `arn:aws:s3። :my-bucket/my-logs/`
- Si eres el propietario del depósito, creamos automáticamente una política de recursos y la adjuntamos al bucket. Para obtener más información, consulte Permisos de bucket de Amazon S3 para los registros de flujo.
1. En Formato de registro de registro, seleccione el formato del registro de flujo.
* Para utilizar el formato predeterminado, elija el formato predeterminado de AWS.
* Para utilizar un formato personalizado, elija Formato personalizado y, a continuación, seleccione campos en Formato de registro.
1. Elija Crear registro de flujo.

**Para crear un registro de flujo para una VPC o una subred mediante la consola**
1. Abra la consola de Amazon [VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Sus VPC o elija Subredes.
1. Seleccione la casilla de verificación de una o más VPC o subredes y, a continuación, seleccione Acciones, Crear registro de flujo.
1. En Filtro, especifique el tipo de tráfico que desea registrar.
- Elija Todo para registrar el tráfico aceptado y rechazado, Rechazar para registrar solo el tráfico rechazado o Aceptar para registrar solo el tráfico aceptado.
1. En Intervalo de agregación máximo, elija el período máximo de tiempo durante el que se captura un flujo y se agrega en un registro de registro de flujo.
1. En Destino, selecciona Enviar a un depósito de Amazon S3.
1. Para ARN de bucket de S3, especifique el nombre de recurso de Amazon (ARN) de un bucket de Amazon S3 existente.
- Puede incluir una subcarpeta en el ARN del bucket. El depósito no puede utilizar AWSLogs como nombre de subcarpeta, ya que se trata de un término reservado.
- Por ejemplo, para especificar una subcarpeta denominada my-logs en un bucket denominado my-bucket, utilice el siguiente ARN: `arn:aws:s3። :my-bucket/my-logs/`
- Si eres el propietario del depósito, creamos automáticamente una política de recursos y la adjuntamos al bucket. Para obtener más información, consulte Permisos de bucket de Amazon S3 para los registros de flujo.
1. En Formato de registro de registro, seleccione el formato del registro de flujo.
* Para utilizar el formato predeterminado, elija el formato predeterminado de AWS.
* Para utilizar un formato personalizado, elija Formato personalizado y, a continuación, seleccione campos en Formato de registro.
1. Elija Crear registro de flujo.

**Supervisión de puertas de enlace NAT: ** Puede supervisar la puerta de enlace NAT mediante CloudWatch, que recopila información de su gateway NAT y crea métricas legibles y casi en tiempo real.
1. Las métricas de puerta de enlace NAT se envían a CloudWatch a intervalos de 1 minuto.
1. Puede ver las métricas de sus puertas de enlace NAT de la siguiente manera.
* Abra la consola de CloudWatch en https://console.aws.amazon.com/cloudwatch/
* En el panel de navegación, elija Métricas.
* En Todas las métricas, elija el espacio de nombres de métrica de puerta de enlace NAT.
* Para ver las métricas, seleccione la dimensión métrica.
1. Para crear una alarma para el tráfico saliente a través de la puerta de enlace NAT:
* Abra la consola de CloudWatch en https://console.aws.amazon.com/cloudwatch/
* En el panel de navegación, elija Alarmas, Crear alarma.
* Elija puerta de enlace NAT.
* Seleccione la puerta de enlace NAT y la métrica BytesOutToDestination y elija Siguiente.
* Configure la alarma de la siguiente manera y elija Crear alarma cuando haya terminado:
* En Umbral de alarma, introduzca un nombre y una descripción para la alarma. En Whenever, elija >= e introduzca 5000000. Introduzca 1 durante los periodos consecutivos.
* En Acciones, seleccione una lista de notificaciones existente o elija Nueva lista para crear una nueva.
* En Vista previa de alarmas, seleccione un período de 15 minutos y especifique una estadística de Suma.
1. Para crear una alarma para supervisar los errores de asignación de puertos
* Abra la consola de CloudWatch en https://console.aws.amazon.com/cloudwatch/
* En el panel de navegación, elija Alarmas, Crear alarma.
* Elija NAT Gateway.
* Seleccione la puerta de enlace NAT y la métrica ErrorPortAllocation y elija Siguiente.
* Configure la alarma de la siguiente manera y elija Crear alarma cuando haya terminado:
* En Umbral de alarma, introduzca un nombre y una descripción para la alarma. En Whenever, elija > e introduzca 0. Introduzca 3 durante los periodos consecutivos.
* En Acciones, seleccione una lista de notificaciones existente o elija Nueva lista para crear una nueva.
* En Vista previa de alarmas, seleccione un período de 5 minutos y especifique una estadística de Máximo.

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

### CloudTrail
1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. Revise los registros de CloudTrail para ver los nombres de eventos `AuthorizeSecurityGroupIngress`, `RevokeSecurityGroupIngress`, `AuthorizeSecurityGroupGress` y `RevokeSecurityGroupPrens`

### CloudWatch
Crear una regla de CloudWatch Events que se activa en un evento mediante la consola de CloudWatch

1. Abra la consola de CloudWatch.
1. En el panel de navegación, elija Reglas en Eventos y, a continuación, elija Crear regla.
1. Seleccione el patrón de eventos.
1. En Nombre de servicio, elija EC2.
1. En Tipo de evento, elija Llamada a la API de AWS mediante CloudTrail.
1. Seleccione Operación específica y proporcione las siguientes llamadas a la API. Estas llamadas a la API se utilizan para agregar o quitar reglas de grupos de seguridad.
- Autorizar la entrada a grupos de seguridad
- Autorizar la prensa del grupo de seguridad
- Revocar la entrada del grupo de seguridad
- Revocar la prensa del grupo de seguridad
1. Esta configuración crea el siguiente patrón de eventos.
```json
{
«fuente»: [
«aws.ec2"
],
«tipo de detalle»: [
«Llamada a la API de AWS mediante CloudTrail»
],
«detalle»: {
«EventSource»: [
«ec2.amazonaws.com»
],
«Nombre del evento»: [
«Autorizar la entrada a grupos de seguridad»,
«Autorizar la prensa del grupo de seguridad»,
«Revocar la entrada del grupo de seguridad»,
«Revocar la prensa del grupo de seguridad»
]
}
}
```
1. Selecciona Añadir objetivo.
1. En la lista de destinos, elija el tema SNS.
1. En Tema, elija un tema existente para crear uno nuevo.
1. Elija Configurar detalles.
1. En la página de detalles Configurar regla, introduzca un nombre y una descripción opcional.
- En Estado, deje seleccionada la casilla Habilitado.
1. Elija Crear regla.

### Utilice AWS Config para comprobar la configuración de un grupo de seguridad
Cuando planee utilizar AWS Config y AWS Config Rules, determine primero si desea utilizar el servicio de forma reactiva o proactiva.
- Para utilizar AWS Config de forma reactiva, basta con habilitar el servicio y comenzará a crear un inventario de recursos de AWS y un historial de cambios.
- Para utilizar AWS Config para la notificación de cambios proactiva, realice las siguientes tareas:
1. Cree un tema de Amazon SNS para recibir notificaciones de cambios de configuración.
1. Desarrolle funciones de AWS Lambda o procesadores de mensajes SQS basados en EC2 para identificar los cambios específicos de la red y enviar las notificaciones adecuadas.
1. Considere al menos dos destinatarios diferentes para las notificaciones de cambios:
- Una lista de distribución de correo electrónico o paginación de seguridad de red para recibir notificaciones que requieren intervención humana inmediata (por ejemplo, modificar un grupo de seguridad para permitir todo el acceso global o adjuntar una puerta de enlace de Internet a una VPC solo interna).
- Sistema central de registro o administración de cambios que se puede utilizar para conciliar las solicitudes de cambio aprobadas con los cambios de configuración reales.
1. Suscríbase sus funciones o colas SQS de AWS Lambda al tema SNS de notificación de cambios.
1. Configure AWS Config para publicar notificaciones de cambio en el tema SNS de notificación de cambios.

A continuación se muestra un ejemplo del uso de AWS Config en combinación con una función Lambda:

1. Cree un [rol de ejecución de Lambda] (http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) y un [rol de AWS Config] (http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html).
1. Permita que AWS Config registre la configuración de los grupos de seguridad para que AWS Config pueda supervisar los grupos de seguridad en busca de cambios en sus configuraciones. En la siguiente captura de pantalla se muestra un ejemplo de la configuración.
Captura de pantalla que muestra un ejemplo de los ajustes de configuración
1. [Cree un grupo de seguridad] (http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) que permita puertos relacionados con los requisitos de su empresa (por ejemplo, 80/443 para HTTP, 25 para SMTP)
1. Cree la función Python Lambda utilizando el [código de la biblioteca de reglas de AWS Config en GitHub] (https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py).
- El código de función Lambda define una lista denominada REQUIRED_PERMISSIONS con elementos que representan un protocolo, un rango de puertos y un rango de IP que juntos definen un permiso de seguridad. En este caso, solo se autorizarán los puertos 80 y 443.
- Personalice esta función de Lmabda con los puertos y rangos IP necesarios para las necesidades de su empresa.
1. [Cree la regla de AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) utilizando la función Lambda que creó en el paso 4.
- En Tipo de desencadenador, elija Cambios de configuración.
- En Alcance de los cambios, elija EC2: SecurityGroup y, a continuación, escriba el ID del grupo de seguridad que creó en el paso 3.
1. Ejecute la regla de configuración. Esto pondrá en cola la regla para su ejecución y la regla debería ejecutarse hasta completarse en unos 10 minutos.
1. Compruebe el grupo de seguridad que ha creado en el paso 3.

## Procedimientos de escalada
- `¿ Quién monitorea los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Análisis
### Visualización de registros de flujo de VPC en la consola
**Para ver información sobre los registros de flujo de las interfaces de red**
1. Abra la consola de Amazon [EC2] (https://console.aws.amazon.com/ec2/)
1. En el panel de navegación, elija Interfaces de red.
1. Seleccione una interfaz de red y elija Logs de flujo. La información sobre los registros de flujo se muestra en la pestaña. La columna Tipo de destino indica el destino en el que se publican los registros de flujo.

**Para ver información sobre los registros de flujo de sus VPC o subred**
1. Abra la consola de Amazon VPC en https://console.aws.amazon.com/vpc/
1. En el panel de navegación, elija Sus VPC o subredes.
1. Seleccione su VPC o subred y elija Logs de flujo.
- La información sobre los registros de flujo se muestra en la pestaña.
- La columna Tipo de destino indica el destino en el que se publican los registros de flujo.

**Para ver los registros de flujo publicados en Amazon S3**
1. Abra la consola de Amazon S3 en https://console.aws.amazon.com/s3/
1. En Nombre del bucket, seleccione el bucket en el que se publican los registros de flujo.
1. En Nombre, active la casilla de verificación situada junto al archivo de registro. En el panel de descripción general de objetos, elija Descargar.

### Consultar registros de flujo mediante Amazon Athena
Amazon Athena es un servicio de consultas interactivas que le permite analizar datos en Amazon S3, como los registros de flujo, mediante SQL estándar. Puede utilizar Athena con registros de flujo de VPC para obtener rápidamente información útil sobre el tráfico que fluye a través de la VPC. Por ejemplo, puede identificar qué recursos de las nubes privadas virtuales (VPC) son los principales hablantes o identificar las direcciones IP con las conexiones TCP más rechazadas.

Puede optimizar y automatizar la integración de los registros de flujo de VPC con Athena generando una plantilla de CloudFormation que crea los recursos de AWS necesarios y las consultas predefinidas que puede ejecutar para obtener información sobre el tráfico que fluye a través de la VPC.

1. La plantilla CloudFormation crea los siguientes recursos:
- Una base de datos de Athena. El nombre de la base de datos es vpcflowlogsathenadatabase<flow-logs-subscription-id>.
- Un grupo de trabajo de Atenea. El nombre del grupo de trabajo es <flow-log-subscription-id><partition-load-frequency><start-date><end-date>grupo de trabajo.
- Tabla de Athena particionada que corresponde a sus registros de registro de flujo. El nombre de la tabla es <flow-log-subscription-id><partition-load-frequency><start-date><end-date>.
- Un conjunto de consultas con nombre de Atenea. Para obtener más información, consulte Consultas predefinidas.
- Función Lambda que carga nuevas particiones en la tabla según la programación especificada (diaria, semanal o mensual).
- Función de IAM que concede permiso para ejecutar las funciones Lambda.
1. Requisitos
- Debe seleccionar una región que admita AWS Lambda y Amazon Athena.
- Los depósitos de Amazon S3 deben estar en la región seleccionada.
1. Precios
- Incurrirá en cargos estándar de Amazon Athena por ejecutar consultas.
- Incurrirá en cargos estándar de AWS Lambda por la función Lambda que carga nuevas particiones en un programa recurrente (cuando especifica una frecuencia de carga de partición pero no especifica una fecha de inicio y finalización).
1. Siga uno de estos procedimientos:
- Abra la consola de Amazon VPC. En el panel de navegación, elija Sus VPC y, a continuación, seleccione la VPC.
- Abra la consola de Amazon VPC. En el panel de navegación, elija Subredes y, a continuación, seleccione la subred.
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
- Abra la consola de Amazon S3. Navegue hasta el depósito que especificó para los resultados de la consulta y vea los resultados de la consulta.

Las siguientes son las consultas con nombre de Athena proporcionadas por la plantilla de CloudFormation generada:
* VPCFlowLogsAcceptedTraffic: las conexiones TCP permitidas en función de los grupos de seguridad y las ACL de red.
* VPCFlowLogsAdminPortTraffic: tráfico registrado en los puertos de aplicaciones web administrativas.
* VPCFlowLogsiPv4Traffic: el total de bytes de tráfico IPv4 registrados.
* VPCFlowLogsiPv6Traffic: el total de bytes de tráfico IPv6 registrados.
* VPCFlowLogsRejectedTCPTraffic: las conexiones TCP rechazadas en función de sus grupos de seguridad o ACL de red.
* VPCFlowLogsRejectedTraffic: tráfico rechazado en función de sus grupos de seguridad o ACL de red.
* VPCFlowLogSSSHRDPTraffic: tráfico SSH y RDP.
* VPCFlowLogStopTalkers: 50 direcciones IP con mayor tráfico registrado.
* VPCFlowLogStopTalkersPacketLevel: 50 direcciones IP a nivel de paquete con mayor tráfico registrado.
* VPCFlowLogStopTalkingInstances: ID de las 50 instancias con mayor tráfico registrado.
* VPCFlowLogStopTalkingSubnets: ID de las 50 subredes con mayor tráfico registrado.
* VPCFlowLogStopTCPTraffic: todo el tráfico TCP registrado para una dirección IP de origen.
* VPCFlowLogsTotalBytesTransferred: 50 pares de direcciones IP de origen y destino con la mayor cantidad de bytes registrados.
* VPCFlowLogsTotalBytesTransferredPacketLevel: los 50 pares de direcciones IP de origen y destino a nivel de paquete con la mayor cantidad de bytes registrados.
* vpcFlowLogsTrafficFRMSRCADDR: tráfico registrado para una dirección IP de origen específica.
* vpcFlowLogsTraffictoDStaddr: tráfico registrado para una dirección IP de destino específica

## Contención
### Identificación de usuario
Identificar qué usuario o rol ejecutó los cambios de red no autorizados

A continuación se muestra un ejemplo para identificar el usuario o el perfil detrás de la creación de una instancia EC2:

1. Identificar uno de los ID de instancia de las instancias EC2 sospechosas en el paso 1
1. Abra la consola de AWS
1. Seleccione Servicios y luego CloudTrail
1. En el margen izquierdo, selecciona «Historial de eventos»
1. Borrar el filtro actual seleccionando la «X» a la derecha de «false»
1. En el extremo derecho (debajo de Personalizado) haz clic en el icono de engranaje
1. Desplázate hacia abajo y habilita «AWS Access Key»
1. En el medio, cambie el menú desplegable de filtros a «Nombre del recurso»
1. Introduzca el ID de instancia EC2 en el cuadro de búsqueda y pulse enter
1. Debería ver un evento RunInstances; haga clic en este
1. Registra los nombres de usuario que has descubierto.
- Puede haber varios eventos por recorrer para encontrar eventos originales, como los creados a partir de una plantilla de CloudFormation
1. Vuelve al Historial de eventos y cambia el menú desplegable a «Nombre de usuario» y publica la entrada del paso anterior en la búsqueda, luego pulsa Intro
1. Identificar cualquier otro evento del usuario sospechoso o comprometido
1. Ahora vamos a Servicios, IAM (parte superior izquierda de la pantalla)
1. En Recursos de IAM, seleccione Usuarios
1. Busque la cuenta que identificamos en CloudTrail y selecciónela
1. Seleccionar credenciales de seguridad
1. Desplázate hacia abajo hasta la clave de acceso y selecciona la «x» para borrar la clave
1. Ve al panel de desafíos y selecciona el botón Comprobar mi progreso

## Erradicación
### Revise los hallazgos de [Revisar el historial de eventos de CloudTrail en busca de actividad mediante la clave de acceso comprometida] (. /#cloudtrail)
Elimine los recursos creados por la (s) clave (s) comprometida (s). Asegúrese de comprobar todas las regiones de AWS, incluso las regiones en las que nunca ha lanzado recursos de AWS.
* **Importante**: Si necesitas conservar algún recurso para la investigación, considera respaldarlos. Por ejemplo, si tiene una necesidad normativa, de conformidad o legal de conservar una instancia EC2, tome una instantánea de EBS antes de finalizar la instancia.

### Revise el [Evitar cargos inesperados] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
Compruebe y elimine los servicios reconocidos en su cuenta. Preste especial atención a los siguientes recursos:
* Instancias y AMI de EC2, incluidas las instancias en estado detenido
* Volúmenes e instantáneas de EBS
* Funciones y capas de AWS Lambda

Para eliminar funciones y capas de Lambda, haga lo siguiente:
1. Abra la consola Lambda.
1. En el panel de navegación, elija Funciones.
1. Seleccione las funciones que desea eliminar.
1. En Acciones, elija Eliminar.
1. En el panel de navegación, elija Capas.
1. Seleccione la capa que desea eliminar.
1. Selecciona Eliminar.

## Recuperación
Revertir cualquier cambio que se haya producido en los recursos de red

Identifique los recursos expuestos a Internet y asegúrelos: `. /merodeador -g grupo17`

## Acciones preventivas
### AWS Config y AWS System Manager
[Cómo corregir automáticamente los puertos accesibles a Internet con AWS Config y AWS System Manager] (https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[Habilitar el nuevo estándar de seguridad de prácticas recomendadas de seguridad de AWS Foundational Security] (https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### Postura general de seguridad
Ejecute una [Evaluación de seguridad de autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito poder supervisar todos los entornos de VPC críticos
- Como Respondedor de Incidentes, necesito una forma de coordinarme con el equipo empresarial, el equipo de infraestructura y los equipos de seguridad
- Como Respondedor de Incidentes, necesito un libro de jugadas para consultar los FlowLogs de VPC a escala
- Como Respondedor de Incidentes, necesito garantizar la correcta utilización de Config en todas las cuentas

## Elementos atrasados actuales