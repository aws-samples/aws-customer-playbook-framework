# Guía de respuesta a incidentes: Denegación de servicio/Denegación de servicio distribuida (DOS/DDoS)
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por los acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para responder a los ataques de denegación de servicio y denegación de servicio distribuida (DOS/DDoS) contra recursos alojados de AWS.

## Indicadores potenciales de compromiso
- Una aplicación orientada a Internet está bajo carga pesada, ya sea por popularidad o por intenciones maliciosas
- Se está utilizando una única instancia EC2 para alojar una aplicación y estamos eliminando o configurando el tráfico (se notifica al cliente mediante Abuso de AWS)
- Resultados presentados en el panel de AWS Shield

## Conclusiones potenciales de AWS GuardDuty
- Backdoor:EC2/DenialOfService.Dns
- Backdoor:EC2/DenialOfService.Tcp
- Backdoor:EC2/DenialOfService.Udp
- Backdoor:EC2/DenialOfService.UdpOnTcpPorts
- Backdoor:EC2/DenialOfService.UnusualProtocol
- Backdoor:EC2/Spambot
- Behavior:EC2/NetworkPortUnusual
- Behavior:EC2/TrafficVolumeUnusual

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
1. [**PREPARACION**] Crear un inventario de activos expuesto públicamente
2. [**PREPARACION**] Implementar capacitación para abordar los ataques DoS/DDoS
3. [**PREPARACIÓN**] Desarrollar una estrategia de comunicación para escalar e informar de los eventos
4. [**PREPARACIÓN**] Realizar revisiones de la documentación para garantizar que los procedimientos se mantengan y estén actualizados
5. [**DETECCIÓN Y ANALÍSIS**] Implementación de AWS Shield
6. [**DETECCIÓN Y ANALÍSIS**] Utilizar el Global Threat Environment Dashboard con AWS Shield
7. [**DETECCIÓN Y ANALÍSIS**] Considere utilizar AWS Shield Advanced
8. [**PREPARACION**] Implementar procedimientos de escalamiento y notificación
9. [**DETECCIÓN Y ANALISIS**] Realizar detección y mitigación
10. [**DETECCIÓN Y ANALISIS**] Identificar a los principales contribuyentes
11. [**CONTENCIÓN**] Realizar contención
12. [**ERRADICACIÓN**] Realizar erradicación
13. [**ACTIVIDADES POSTERIORES A INCIDENTE**] Solicitar un crédito en AWS Shield Advanced
14. [**PREPARACIÓN**] Acciones preventivas adicionales: técnicas de mitigación
15. [**PREPARACIÓN**] Acciones preventivas adicionales: reducción de la superficie de ataque
16. [**PREPARACIÓN**] Acciones preventivas adicionales: técnicas operativas
17. [**PREPARACIÓN**] Acciones preventivas adicionales: postura general de seguridad

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes

* **Tácticas, técnicas y procedimientos**: Denegación de servicio/ Ataques de denegación de servicio distribuidos
* **Categoría**: DOS/DDoS
* **Recurso**: Red
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch, AWS Shield
* **Fuentes de registro**: AWS CloudTrail, AWS Config, VPC Flow Logs, Amazon GuardDuty
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
Busque recursos expuestos a Internet: `. /merodeador -g grupo17`

Puede utilizar Security Hub y Firewall Manager para [configurar la supervisión centralizada de eventos DDoS y corregir automáticamente los recursos no conformes] (https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

### Capacitación
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

### Revisión de documentación
1. Familiarícese con la [Documentación de AWS Shield] (https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html)
1. Si está suscrito, siga la [Guía de introducción a AWS Shield Advanced] (https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html)

## Detección
### Métricas de Cloudwatch para detectar eventos DoS/DDoS
En la tabla Métricas recomendadas de Amazon CloudWatch se enumeran las descripciones de las métricas de Amazon CloudWatch que se utilizan habitualmente para detectar ataques DDoS y reaccionar ante ellos.

* AWS Shield Advanced: DDoSDetected
* Indica un evento DDoS para un Amazon Resource Name (ARN) específico.
* AWS Shield Advanced: DDoSAttackBitsPerSecond
* El número de bytes observados durante un evento DDoS para un ARN específico. Esta métrica solo está disponible para eventos DDoS de capa 3/4.
* AWS Shield Advanced: DDoSAttackPacketsPerSecond
* El número de paquetes observados durante un evento DDoS para un ARN específico. Esta métrica solo está disponible para eventos DDoS de capa 3/4.
* AWS Shield Advanced: DDoSAttackRequestsPerSecond
* El número de solicitudes observadas durante un evento DDoS para un ARN específico. Esta métrica solo está disponible para eventos DDoS de capa 7 y solo se informa para los eventos de capa 7 más significativos.
* AWS WAF: AllowedRequests permitidas
* El número de solicitudes web permitidas.
* AWS WAF: BlockedRequests
* El número de solicitudes web bloqueadas.
* AWS WAF: CountedRequests
* El número de solicitudes web contadas.
* AWS WAF: PassedRequests
* El número de solicitudes aprobadas. Esto solo se utiliza para las solicitudes que se someten a una evaluación de grupos de reglas sin coincidir ninguna de las reglas del grupo de reglas.
* CloudFront: Requests
* El número de solicitudes HTTP/S.
* CloudFront: TotalErrorRate
* El porcentaje de todas las solicitudes para las que el código de estado HTTP es 4xx o 5xx.
* Amazon Route 53: HealthCheckStatus
* El estado del extremo de la comprobación de estado.
* ALB: ActiveConnectionCount
* El número total de conexiones TCP simultáneas que están activas desde los clientes al balanceador de carga y desde el equilibrador de carga hasta los destinos.
* ALB: ConsumedLCUs
* El número de unidades de capacidad del equilibrador de carga (LCU) que utiliza el balanceador de carga.
* ALB: HTTPCode_ELB_4XX_Count HTTPCode_ELB_5XX_Count
* El número de códigos de error de cliente HTTP 4xx o 5xx generados por el equilibrador de carga.
* ALB: NewConnectionCount
* El número total de nuevas conexiones TCP establecidas desde los clientes al balanceador de carga y desde el balanceador de carga hasta los destinos.
* ALB: ProcessedBytes
* El número total de bytes procesados por el equilibrador de carga.
* ALB: RejectedConnectionCount
* El número de conexiones rechazadas porque el equilibrador de carga ha alcanzado su número máximo de conexiones.
* ALB: RequestCount
* El número de solicitudes que se han procesado.
* ALB: TargetConnectionErrorCount
* El número de conexiones que no se han establecido correctamente entre el equilibrador de carga y el objetivo.
* ALB: TargetResponseTime
* El tiempo transcurrido, en segundos, después de que la solicitud abandona el equilibrador de carga hasta que se recibe una respuesta del objetivo.
* ALB: UnHealthyHostCount
* El número de objetivos que se consideran poco saludables.
* NLB: ActiveFlowCount
* El número total de flujos TCP simultáneos (o conexiones) de clientes a destinos.
* NLB: ConsumedLCUs
* El número de unidades de capacidad del equilibrador de carga (LCU) que utiliza el balanceador de carga.
* NLB: NewFlowCount
* El número total de flujos TCP nuevos (o conexiones) establecidos desde los clientes a los destinos en el período de tiempo.
* NLB: ProcessedBytes
* El número total de bytes procesados por el equilibrador de carga, incluidos los encabezados TCP/IP.
* Global Accelerator NewFlowCount
* El número total de flujos TCP y UDP nuevos (o conexiones) establecidos desde los clientes a los endpoints en el período de tiempo.
* Global Accelerator: ProcessedBytesIn procesados
* El número total de bytes entrantes procesados por el acelerador, incluidos los encabezados TCP/IP.
* Auto Scaling: GroupMaxSize
* Tamaño máximo del grupo Auto Scaling.
* Amazon EC2: CPUUtilization
* El porcentaje de unidades informáticas EC2 asignadas que se están utilizando actualmente.
* Amazon EC2: NetworkIn
* El número de bytes recibidos por la instancia en todas las interfaces de red.
### AWS Shield
La [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/shieldv2) o la API proporcionan un resumen y detalles sobre los ataques detectados.
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Descripción general o Eventos

### Global Threat Environment Dashboard
El Global Threat Environment Dashboard proporciona información resumida sobre todos los ataques DDoS detectados por AWS.
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Panel de amenazas globales.
1. Elige un periodo de tiempo.

### AWS Shield Advanced
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Descripción general o Eventos

Tiene acceso a varias métricas de CloudWatch que pueden indicar que su aplicación está siendo segmentada.
1. Puede configurar la métrica DDOSdetected para indicar si se ha detectado un ataque.
1. Si quieres recibir una alerta según el volumen de ataque, también puedes usar las métricas DDoSAttackBitsPerSecond, DDoSAttackPacketsPerSecond o DDoSAttackRequestsPerSecond.

Hay disponible una lista completa de las métricas disponibles en la tabla de [Visibilidad DDoS] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)

## Procedimientos de escalada

### AWS Shield
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

### AWS Shield Advanced
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Ha especificado los recursos de AWS que desea proteger con Shield Advanced? `
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Recursos protegidos
- `¿ Ha autorizado al equipo de respuesta DDoS (DRT) a acceder a sus reglas y registros de WAF? Esto les ayuda a mitigar los ataques cuando solicita ayuda. `
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Descripción general
1. Configurar el soporte de AWS DRT `Editar acceso DRT`
- `¿ Necesita asistencia del equipo de respuesta DDoS de AWS? `
1. Puede abrir un caso en AWS Shield en el centro de AWS Support.
* Para obtener soporte del equipo de respuesta DDoS (DRT), póngase en contacto con AWS Support Center y seleccione las siguientes opciones:
1. Tipo de funda: Soporte técnico
1. Servicio: Denegación de servicio distribuida (DDoS)
1. Categoría: Entrante a AWS
* Con la participación proactiva de AWS Shield Advanced, DRT se pone en contacto con usted directamente si la comprobación de estado de Amazon Route 53 asociada a su recurso protegido no funciona durante un evento detectado por Shield Advanced.

## Análisis
1. Inicie sesión en AWS Management Console y abra la [AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/)
1. En la barra de navegación de AWS Shield, elija Eventos

### Detección y mitigación
La pestaña Detección y mitigación muestra gráficos que muestran el tráfico que AWS Shield evaluó antes de informar de este evento y el efecto de cualquier mitigación que se haya colocado. También proporciona métricas para eventos de denegación de servicio distribuido (DDoS) de capa de infraestructura para los recursos de su propia Amazon Virtual Private Cloud VPC y aceleradores de AWS Global Accelerator. Las métricas de detección y mitigación no se incluyen para los recursos de Amazon CloudFront ni Amazon Route 53.

### Colaboradores principales
La pestaña Principales colaboradores proporciona información sobre el origen del tráfico durante un evento detectado. Puede ver los contribuyentes de mayor volumen, ordenados por aspectos como protocolo, puerto de origen y indicadores TCP. Puede descargar la información y ajustar las unidades utilizadas y el número de colaboradores que se van a mostrar.

## Contención

### [Prácticas recomendadas de AWS para la resiliencia de DDoS] (https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
En este documento técnico, AWS le proporciona orientación sobre DDoS prescriptiva para mejorar la resiliencia de las aplicaciones que se ejecutan en AWS. Esto incluye una arquitectura de referencia resistente a DDOS que se puede utilizar como guía para ayudar a proteger la disponibilidad de las aplicaciones. En este documento técnico también se describen diferentes tipos de ataque, como los ataques de capa de infraestructura y los ataques de capa de aplicaciones. AWS explica qué prácticas recomendadas son más eficaces para administrar cada tipo de ataque. Además, se describen los servicios y las características que encajan en una estrategia de mitigación de DDoS y se explica cómo se puede utilizar cada uno para ayudar a proteger sus aplicaciones.
Los procedimientos de contención suponen el uso de cortafuegos de aplicaciones web de AWS. Si se utiliza un WAF o un firewall de terceros, personalice estos procedimientos aquí para que coincidan con su caso de uso.

### AWS WAF
1. Cree condiciones en AWS WAF que coincidan con el comportamiento inusual.
1. Añada estas condiciones a una o más reglas AWS WAF.
1. Agregue estas reglas a una ACL web y configure la ACL web para contar las solicitudes que coinciden con las reglas.
* **NOTA** Siempre debes probar primero tus reglas inicialmente usando Count en lugar de Bloquear. Una vez que te sientas cómodo de que la nueva regla esté identificando las solicitudes correctas, puedes modificarla para bloquearlas.
1. Supervise esos recuentos para determinar si se debe bloquear el origen de las solicitudes. Si el volumen de solicitudes sigue siendo inusualmente elevado, cambie la ACL web para bloquear esas solicitudes.

## Erradicación
No aplicable

## Recuperación
### Solicitud de crédito en AWS Shield Advanced
Si está suscrito a AWS Shield Advanced y experimenta un ataque DDoS que aumenta la utilización de un recurso protegido de Shield Advanced, puede solicitar un crédito por los cargos relacionados con el aumento de la utilización en la medida en que Shield Advanced no lo mitigue.

Puede encontrar información adicional en los documentos de AWS en [Solicitud de crédito en AWS Shield Advanced] (https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html)

## Acciones preventivas
### Técnicas de mitigación
En AWS, las capacidades de mitigación de DDoS se proporcionan automáticamente
1. AWS Shield Standard se defiende contra los ataques DDoS de redes y capas de transporte más comunes y frecuentes dirigidos a su sitio web o aplicaciones. Esto se ofrece en todos los servicios de AWS y en todas las regiones de AWS sin coste adicional.
1. Puede mejorar su preparación para responder y mitigar los ataques DDoS mediante la suscripción a AWS Shield Advanced. Este servicio opcional de mitigación de DDoS le ayuda a proteger una aplicación alojada en cualquier región de AWS. El servicio está disponible en todo el mundo para Amazon CloudFront y Amazon Route 53. También está disponible en determinadas regiones de AWS para Classic Load Balancer (CLB), Application Load Balancer (ALB) y direcciones IP elásticas (EIP). El uso de AWS Shield Advanced con EIP le permite proteger Network Load Balancer (NLB) o instancias de Amazon EC2.
1. Las consideraciones clave para ayudar a mitigar los ataques DDoS volumétricos incluyen garantizar que haya suficiente capacidad de tránsito y diversidad disponibles y la protección de los recursos de AWS, como las instancias Amazon EC2, contra el tráfico de ataques
1. Los ataques DDoS grandes pueden agobiar la capacidad de una única instancia de Amazon EC2, por lo que agregar equilibrio de carga puede ayudar a su resiliencia.
1. Amazon CloudFront le permite almacenar en caché contenido estático y servirlo desde ubicaciones perimetrales de AWS, lo que puede ayudar a reducir la carga en su origen.
1. Mediante AWS WAF, puede configurar listas de control de acceso web (ACL web) en sus distribuciones de CloudFront o Application Load Balancers para filtrar y bloquear solicitudes basándose en firmas de solicitudes.
### Reducción de la superficie de ataque
1. Puede especificar grupos de seguridad al lanzar una instancia o asociarla a un grupo de seguridad más adelante. Todo el tráfico de Internet a un grupo de seguridad se niega implícitamente a menos que cree una regla de permiso para permitir el tráfico.
* Si está suscrito a AWS Shield Advanced, puede registrar IPs elásticas (EIP) como recursos protegidos.
1. Si utiliza Amazon CloudFront con un origen que se encuentra dentro de la VPC, debe utilizar una función de AWS Lambda para actualizar automáticamente las reglas del grupo de seguridad y permitir solo el tráfico de Amazon CloudFront.
1. Normalmente, cuando debe exponer una API al público, existe el riesgo de que la interfaz de la API esté dirigida por un ataque DDoS. Para ayudar a reducir el riesgo, puede utilizar Amazon API Gateway como entrada a las aplicaciones que se ejecutan en Amazon EC2, AWS Lambda u otros lugares.
* Cuando utiliza Amazon CloudFront y AWS WAF con Amazon API Gateway, configure las siguientes opciones:
1. Configure el comportamiento de la caché de sus distribuciones para reenviar todos los encabezados al extremo regional de API Gateway. Al hacerlo, CloudFront tratará el contenido como dinámico y omitirá el almacenamiento en caché del contenido.
1. Proteja su API Gateway contra el acceso directo configurando la distribución para que incluya el encabezado personalizado de origen x-api-key, configurando el keyvalue de API en API Gateway.
1. Proteja su backend del exceso de tráfico mediante la configuración de límites estándar o de velocidad de ráfaga para cada método de sus RESTAPIs.

### Técnicas operativas
Cuando una métrica operativa clave se desvía sustancialmente del valor esperado, es posible que un atacante esté intentando orientar la disponibilidad de su aplicación. Si estás familiarizado con el comportamiento normal de tu aplicación, puedes actuar más rápidamente cuando detectas una anomalía.
1. Si ha diseñado la aplicación siguiendo la arquitectura de referencia resistente a DDOS, los ataques de capa de infraestructura comunes se bloquearán antes de llegar a la aplicación.
1. Si utiliza AWS WAF, puede utilizar CloudWatch para supervisar y hacer alarmas de los aumentos de las solicitudes que ha configurado en WAF para que se permitan, contabilizen o bloqueen. Esto le permite recibir una notificación si el nivel de tráfico supera lo que puede manejar su aplicación.

Para obtener una descripción de las métricas de Amazon CloudWatch que se utilizan habitualmente para detectar ataques DDoS y reaccionar ante ellos, consulte la tabla de [Visibilidad DDoS] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html)
### Postura general de seguridad
Ejecute una [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito una ruta de escalación documentada tanto interna como externamente a AWS.

## Elementos atrasados actuales