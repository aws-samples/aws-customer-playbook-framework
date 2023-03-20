#Guía de respuesta de Incidentes: Credenciales de IAM comprometidas
Este documento se proporciona únicamente con fines informativos. Representa las ofertas de productos y prácticas actuales de Amazon Web Services (AWS) en la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información de este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, declaración, compromiso contractual, condición o garantía por parte de AWS, sus filiales o proveedores. Las responsabilidades y obligaciones de AWS con sus clientes están reguladas por los acuerdos de AWS, y este documento no forma parte de ningún acuerdo entre AWS y sus clientes ni lo modifica.
© 2021 Amazon Web Services, Inc. o sus filiales. Todos los derechos reservados. Esta obra está licenciada bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona de conformidad con las condiciones del contrato de cliente de AWS, disponible en https://aws.amazon.com/es/agreement/ u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc.

## Puntos de contacto

Autor: `Nombre del autor` \
Aprobador: `Nombre del aprobador` \
Ultima fecha de aprobación:   

## Resumen ejecutivo
Este manual describe el proceso de respuesta cuando observa actividad no autorizada en su cuenta de AWS o cree que una persona no autorizada ha accedido a su cuenta.

## Posibles indicadores de compromiso
- Usuarios de IAM nuevos o no reconocidos
- Recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
- Aumentos inusuales de facturación 
- Notificación del investigador de seguridad
- Notificación de que mis recursos o mi cuenta de AWS podrían estar en peligro

## Posibles hallazgos de AWS GuardDuty
- CredentialAccess:IAMUser/AnomalousBehavior
- DefenseEvasion:IAMUser/AnomalousBehavior
- Discovery:IAMUser/AnomalousBehavior
- Exfiltration:IAMUser/AnomalousBehavior
- Impact:IAMUser/AnomalousBehavior
- InitialAccess:IAMUser/AnomalousBehavior
- PenTest:IAMUser/KaliLinux
- PenTest:IAMUser/ParrotLinux
- PenTest:IAMUser/PentooLinux
- Persistence:IAMUser/AnomalousBehavior
- Policy:IAMUser/RootCredentialUsage
- PrivilegeEscalation:IAMUser/AnomalousBehavior
- Recon:IAMUser/MaliciousIPCaller
- Recon:IAMUser/MaliciousIPCaller.Custom
- Recon:IAMUser/TorIPCaller
- Stealth:IAMUser/CloudTrailLoggingDisabled
- Stealth:IAMUser/PasswordPolicyChange
- UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B
- UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration
- UnauthorizedAccess:IAMUser/MaliciousIPCaller
- UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom
- UnauthorizedAccess:IAMUser/TorIPCaller

### Objetivos
A lo largo de la ejecución del manual, concéntrese en los _***resultados deseados***_ y tome notas para mejorar las capacidades de respuesta a los incidentes.

#### Determine: 
* **Vulnerabilidades explotadas**
* **Exploits y herramientas observados**
* **Intención del actor**
* **Atribución del actor**
* **Daños infligidos al ambiente y a los negocios**

#### Recuperar:

* **Volver a la configuración original reforzada

#### Mejore la perspectiva de los componentes de seguridad de CAF:
[Perspectiva de seguridad del marco de adopción de la nube de AWS] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **Directive**
* **Detective**
* **Responsive**
* **Preventative**

* * *

### Pasos de respuestas

1. [**PREPARACIÓN**] Realizar un inventario de assets
2. [**PREPARACIÓN**] Implemente un plan de capacitación para identificar y responder a la exposición de credenciales IAM 
3. [**PREPARACIÓN**] Implementar una estrategia de comunicación para responder a los incidentes
4. [**DETECCIÓN**] Identificar el acceso a la cuenta raíz/Root account (autorizado y no)
5. [**DETECCIÓN**] Identificar usuarios de IAM nuevos o no reconocidos
6. [**DETECCIÓN**] Identifique recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
7. [**DETECCIÓN**] Identifica y encuentra los secretos expuestos
8. [**DETECCIÓN**] Identifique aumentos inusuales de facturación
9. [**DETECCIÓN**] Responder a una notificación de AWS o de un tercero de que mis recursos o mi cuenta de AWS podrían estar en peligro
10. [**DETECCIÓN**] Identifique cualquier credencial de usuario de IAM potencialmente no autorizada
11. [**PREPARACIÓN**] Identifique los procedimientos de escalamiento
12. [**DETECCIÓN Y ANÁLISIS**] Revise los registros de CloudTrail
13. [**DETECCIÓN Y ANÁLISIS**] Revise los registros de flujo de VPC
14. [**DETECCIÓN Y ANÁLISIS**] Revise los registros basados en servidores y terminales
15. [**CONTENIDO**] Realice las acciones de contención apropiadas
16. [**ERRADICATION**] Revise los hallazgos del historial de eventos de Review CloudTrail para ver la actividad de la clave de acceso comprometida
17. [**ERRADICACIÓN**] Revisa la sección Cómo evitar cargos inesperados
18. [**RECUPERACIÓN**] Realice las acciones de recuperación adecuadas
19. [**PREPARACIÓN**] Realice un escaneo de IAM de Prowler
20. [**PREPARACIÓN**] Habilitar MFA
21. [**PREPARACIÓN**] Verifica la información de tu cuenta
22. [**PREPARACIÓN**] Utilice proyectos de AWS Git para buscar pruebas de uso no autorizado
23. [**PREPARACIÓN**] Evite utilizar el usuario root para las operaciones del día a día
24. [**PREPARACIÓN**] Evalúe su postura general de seguridad

***Los pasos de respuesta siguen el ciclo de vida de respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)  



### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Exposición de credenciales
* **Categoría**: Exposición de credenciales de IAM
* **Recurso**: IAM
* **Indicadores**: inteligencia sobre ciber amenazas, aviso de terceros
* **Fuentes de registro**: AWS CloudTrail, AWS Config, registros de flujo de VPC, Amazon GuardDuty
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a un incidente consta de las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Preparación
Este manual hace referencia y se integra, siempre que sea posible, con [Prowler] (https://github.com/toniblyx/prowler), que es una herramienta de línea de comandos que le ayuda a evaluar la seguridad, auditar, reforzar y responder a los incidentes de AWS.

Sigue las directrices del índice de referencia de Amazon Web Services Foundations de la CEI (49 checks) y cuenta con más de 100 checks adicionales, incluidas las relacionadas con el RGPD, la HIPAA, la PCI-DSS, la ISO-27001, la FFIEC, el SOC2 y otras. 

Esta herramienta proporciona una instantánea rápida del estado actual de la seguridad en el entorno del cliente. Como alternativa, [AWS Security Hub] (https://aws.amazon.com/es/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) ofrece un análisis automático de las normas y puede [integrarse con Prowler] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventario de activos
Identifique a todos los usuarios existentes y tenga una lista actualizada del propósito de cada cuenta

1. Vaya a [AWS Console] (https://console.aws.amazon.com/)
1. Vaya a Servicios y selecciona IAM
1. En el menú de la izquierda, selecciona Informe de credenciales
1. Seleccione Descargar informe
1. Presta especial atención a las columnas de cuándo se crearon las cuentas, cuándo se usó la contraseña por última vez y cuándo se cambió la contraseña por última vez. 
 * **NOTA** si un usuario tiene varias claves de acceso, valide el propósito de cada una

### Entrenamiento
- ¿Qué entrenamiento se le ofrece a los analistas de la empresa para familiarizarse con la API (entorno de línea de comandos) de AWS, S3, RDS y otros servicios de AWS? `
> >>
Las oportunidades aquí para la detección de amenazas y la respuesta a incidentes incluyen:\
[AWS RE:INFORCE] (https://reinforce.awsevents.com/faq/)\
[Evaluación de seguridad mediante autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
> >>

- ¿ Qué funciones pueden realizar cambios en los servicios de tu cuenta? 
- ¿ A qué usuarios se les han asignado esas funciones? ¿Se siguen los privilegios mínimos o existen usuarios super admin? 
- ¿ Se ha realizado una evaluación de seguridad en su entorno? ¿Tiene una base conocida para detectar cosas «nuevas» o «sospechosas»? `

### Tecnología de la comunicación
- ¿ Qué tecnología se utiliza en el equipo/empresa para comunicar los problemas? ¿Hay algo automatizado? 
>>>
Telefono \
E-mail \
AWS SES \
AWS SNS \
Slack \
Chime \
Otro? 
>>>

## Detección

### Acceso a la Root account/Cuenta raíz
Se recomienda eliminar todas las claves de acceso asociadas a la Root account/cuenta raíz: `. /prowler -c check_112`

### Usuarios de IAM nuevos o no reconocidos
Consulte el informe de credenciales de IAM de su [Inventario de activos] (. /comprosed_IAM_Credentials.md/ #asset -inventory)
Compruebe si los usuarios de IAM tienen dos claves de acceso activas: `. /prowler -c check_extra712`
Asegúrese de que no se creen políticas de IAM que permitan todos los privilegios administrativos de\ "*: *\»: `. /prowler -c comprobaciónk_122`
Compruebe si el analizador de acceso de IAM (IAM Access Analyzer) está activado y sus resultados son: `. /prowler -c check_extra769`

### Recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
aws ec2 describe-instances
aws lambda list-functions

### Buscando `secrets`
Se encontró un posible secret en los datos de usuario de la instancia EC2: `. /prowler -c check_extra741`
Se encontró un posible secret en las variables de la función Lambda: `. /prowler -c check_extra759`
Se encontró un posible secret en las variables de definición de tareas de ECS: `. /prowler -c check_extra768`
Se encontró un posible secret en la configuración de ajuste de escala automático: `. /prowler -c check_extra775`

### Aumentos inusuales de facturación
Para ver su factura de AWS, abra el panel [Facturas] (https://console.aws.amazon.com/billing/home #) de la consola de administración de facturación y costos y, a continuación, elija el mes que desea ver en el menú desplegable.

Puede ver el historial de sus pagos de AWS en el panel [Pedidos y facturas] (https://console.aws.amazon.com/billing/home?/paymenthistory) de la consola de administración de facturación y costes.

También puede utilizar [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) para la supervisión dinámica y las alertas.

Revisa tu factura para buscar lo siguiente:
* Servicios de AWS que no utiliza normalmente
* Recursos en regiones de AWS que no utiliza normalmente
* Un cambio significativo en el tamaño de su factura

Puede utilizar esta información para eliminar o cancelar cualquier recurso que no desee conservar.

### Notificación de que mis recursos o mi cuenta de AWS podrían estar en peligro
Si ha recibido una notificación de AWS sobre su cuenta, inicie sesión en el Centro de soporte de AWS y, a continuación, responda a la notificación.

Si no puede iniciar sesión en su cuenta, utilice el formulario de contacto para solicitar ayuda a AWS Support.

Si tiene alguna pregunta o duda, cree un nuevo caso de AWS Support en el Centro de soporte de AWS.
 * **NOTA**: No incluya información confidencial en su correspondencia, como claves de acceso a AWS, contraseñas o información de tarjetas de crédito.
Utilice AWS Git Projects para buscar pruebas de uso no autorizado


### Identifique cualquier credencial de usuario de IAM potencialmente no autorizada
1. Abra la consola de IAM.
1. Seleccione Usuarios en el panel de navegación.
1. Elija cada usuario de IAM de la lista y, a continuación, busque en Políticas de permisos una política denominada AWSExposedCredentialPolicy_do_NOT_REMOVE. 1. Si el usuario tiene esta política adjunta, debe rotar las claves de acceso del usuario.

## Procedimientos de escalamiento
- `¿ Quién monitorea los registros o alertas, los recibe y actúa en función de cada uno de ellos? `
- `¿ Quién recibe una notificación cuando se descubre una alerta? `
- `¿ Cuándo intervienen las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo solicitaría ayuda a AWS Support? `

## Análisis
Se recomienda encarecidamente exportar los registros/logs a una solución de gestión de eventos de incidentes de seguridad (SIEM) (como Splunk, ELK Stack, etc.) para facilitar la visualización y el análisis de una variedad de registros y así poder analizar la cronología de los ataques de forma más completa.

### CloudTrail
Busque una actividad de inicio de sesión inusual
1. Ve a tu [Panel de control/Dashboard de CloudTrail] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. En el campo de búsqueda, introduce `ConsoleLogin` o `AssumeRole` o `GetFederationToken` o `GetCredentialReport` o `GenerateCredentialReport` y revisa los eventos disponibles para detectar cualquier actividad sospechosa
 * **NOTA** UserIdentify aparecerá como `"type»: «Root"` para root o `"type»: «IAMUser"` para los usuarios 

Busque el ID de la clave de acceso de IAM y el nombre de usuario utilizados para lanzar una instancia de EC2 sospechosa
1. Abra la consola de CloudTrail y, a continuación, seleccione Historial de eventos.
1. Seleccione el menú desplegable Filtrar y, a continuación, elija Nombre del recurso.
1. En el campo Introduzca el nombre del recurso, pegue el ID de instancia de EC2 y, a continuación, seleccione enter en su dispositivo.
1. Amplíe el nombre del evento para RunInstances.
1. Copie la clave de acceso de AWS y anote el nombre de usuario.

Revise el historial de eventos de CloudTrail para ver la actividad de la clave de acceso comprometida
1. Abra la consola de CloudTrail y, a continuación, seleccione Historial de eventos en el panel de navegación.
1. Seleccione el menú desplegable Filtrar y, a continuación, elija el filtro de clave de acceso de AWS.
1. En el campo Introduzca la clave de acceso de AWS, introduzca el ID de clave de acceso de IAM comprometido.
1. Expande el nombre del evento de la llamada a la API RunInstances.
 * **NOTA**: Solo puedes ver el historial de eventos de los últimos 90 días, a menos que hayas configurado previamente una ruta que se guarde en un bucket de S3

### Registros de flujo de VPC
Los registros de flujo de VPC son una función que permite capturar información sobre el tráfico IP que va y viene de las interfaces de red de la VPC. Esto puede resultar útil para las direcciones IP descubiertas en CloudTrail para determinar los tipos de conexiones externas a cualquier recurso público. 


Para obtener más información y seguir los pasos, incluida la consulta a Athena, consulte la [Documentación de AWS para los registros de flujo de VPC] (https://docs.aws.amazon.com/es_es/vpc/latest/userguide/flow-logs-athena.html). Se recomienda incluir el análisis de Athena en un manual de estrategias independiente y vincularlo a otros elementos relevantes. 

### Endpoint/Basado en host
* **Identifique la última hora de creación de un nombre de instancia determinado**: `aws ec2 describe-instances --query 'Reservations [] .Instances []. {ip: publicIPAddress, tm: launchTime} '--filters 'name=tag:name, values= myInstanceName' | jq 'sort_by (.tm) | reverse |. [0] '`

* **Identify Last Modified time for lambda functions**: `aws lambda list-functions | grep "LastModified"`

## Contención
1. Desactive el usuario de IAM, cree una clave de acceso de IAM de respaldo y, a continuación, desactive la clave de acceso comprometida
 1. Abra la [consola de IAM] (https://console.aws.amazon.com/iam/) y, a continuación, pegue el ID de la clave de acceso de IAM en la barra de búsqueda de IAM.
 1. Elija el nombre de usuario y, a continuación, elija la pestaña Credenciales de seguridad.
 1. En Contraseña de consola, selecciona Administrar.
 * **NOTA**: Si la contraseña de la consola de administración de AWS está configurada como Deshabilitada, puede omitir este paso.
 1. En Acceso a la consola, selecciona Deshabilitar y, a continuación, selecciona Aplicar.
 1. Importante: Los usuarios cuyas cuentas están deshabilitadas no pueden acceder a la consola de administración de AWS. Sin embargo, si el usuario tiene claves de acceso activas, puede seguir accediendo a los servicios de AWS mediante llamadas a la API.
 1. Para la clave de acceso de IAM comprometida, elija Hacer inactiva.
1. Desactive la clave de IAM de la aplicación, cree una clave de acceso de IAM de respaldo y, a continuación, desactive la clave de acceso comprometida 
 1. Primero, crea una segunda clave. A continuación, modifique la aplicación para utilizar la nueva clave.
 1. Desactive (pero no elimine) la primera clave.
 1. Si hay algún problema con la aplicación, reactiva la clave temporalmente. Cuando la aplicación esté en pleno funcionamiento y la primera clave esté deshabilitada, elimine la primera clave.
1. Rote y elimine todas las claves de acceso de AWS inactivas/comprometidas
 * **!! ¡Asegúrese de mantener un registro de todas las claves de acceso eliminadas para poder seguir buscándolas en CloudTrail! **

## Erradicación
### Revise los hallazgos de [Revise el historial de eventos de CloudTrail para ver la actividad de la clave de acceso comprometida] (. /#cloudtrail) 
Elimine cualquier recurso creado por las claves comprometidas. Asegúrese de revisar todas las regiones de AWS, incluso las regiones en las que nunca ha lanzado recursos de AWS.
 * **Importante**: Si necesitas conservar algún recurso para investigar, considera hacer una copia de seguridad de él. Por ejemplo, si tiene una necesidad normativa, de cumplimiento o legal de conservar una instancia de EC2, tome una instantánea de EBS antes de finalizar la instancia.

### Consulte [Cómo evitar cargos inesperados] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html) 
Comprueba si hay servicios reconocidos en tu cuenta y elimínalos. Presta especial atención a los siguientes recursos:
* Instancias EC2 y AMI, incluidas las instancias en estado detenido
* Volúmenes e instantáneas de EBS
* Funciones y layers de AWS Lambda

Para eliminar funciones y layers de Lambda, haga lo siguiente:
1. Abre la consola Lambda.
1. En el panel de navegación, seleccione Funciones.
1. Seleccione las funciones que desee eliminar.
1. En Acciones, elija Eliminar.
1. En el panel de navegación, seleccione Layers.
1. Seleccione la layer que desee eliminar.
1. Selecciona Eliminar.

Elimine cualquier usuario de IAM que no haya creado
1. Inicie sesión en la consola de administración de AWS y abra la [consola de IAM] (https://console.aws.amazon.com/iam/)
1. En el panel de navegación, seleccione Usuarios y, a continuación, active la casilla de verificación situada junto al nombre de usuario que desee eliminar, no al nombre o la fila en sí.
1. En la parte superior de la página, selecciona Eliminar usuario.
1. En el cuadro de diálogo de confirmación, espere a que se cargue la última información a la que se accedió antes de revisar los datos. El cuadro de diálogo muestra cuándo cada uno de los usuarios seleccionados accedió por última vez a un servicio de AWS. Si intenta eliminar un usuario que ha estado activo en los últimos 30 días, debe seleccionar una casilla de verificación adicional para confirmar que desea eliminar el usuario activo. Si desea continuar, elija Sí, Eliminar.

Encontrará documentación sobre la eliminación de otros servicios en la página [cancelar todos mis recursos] (https://aws.amazon.com/es/premiumsupport/knowledge-center/terminate-resources-account-closure/).

## Recuperación
AWS ha publicado una guía sobre [¿qué hago si observo actividad no autorizada en mi cuenta de AWS?] (https://aws.amazon.com/es/premiumsupport/knowledge-center/potential-account-compromise/)
## Acciones preventivas
### Escaneo IAM de Prowler 
`./prowler -g check122,check111,check110,check19,check18,check17,check16,check15,check11,check116,check12,check114,check115,check14,check13,check112,check119,extra71,extra7100,extra7123,extra7125,extra769,extra774`

### Habilitar MFA
Para aumentar la seguridad, se recomienda configurar MFA para ayudar a proteger los recursos de AWS. Puede habilitar [MFA para usuarios de IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) o [Root account de la cuenta de AWS] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html). Habilitar la MFA para el Root account afecta únicamente a las credenciales de la Root account. Los usuarios de IAM de la cuenta son identidades distintas con sus propias credenciales y cada identidad tiene su propia configuración de MFA.

### Verifica la información de tu cuenta
AWS necesita información de cuenta precisa para ponerse en contacto con usted y ayudarlo a resolver cualquier problema con la cuenta. Comprueba que la información de tu cuenta es correcta.
* El nombre de la cuenta y la dirección de correo electrónico.
* Su información de contacto, especialmente su número de teléfono.
* Los contactos alternativos de tu cuenta.


### Utilice AWS Git Projects para buscar pruebas de uso no autorizado
AWS ofrece Git Projects que puede instalar para proteger su cuenta:
* [Git Secrets] (https://github.com/awslabs/git-secrets) puede analizar las fusiones, confirmaciones y mensajes de confirmación en busca de información secreta (es decir, claves de acceso). Si Git Secrets detecta expresiones regulares prohibidas, puede rechazar que esas confirmaciones se publiquen en repositorios públicos.
* Utilice [AWS Step Functions y AWS Lambda para generar Amazon CloudWatch Events] (https://aws.amazon.com/step-functions) desde AWS Health o desde AWS Trusted Advisor. Si hay pruebas de que sus claves de acceso están expuestas, los proyectos pueden ayudarlo a detectar, registrar y mitigar el evento automáticamente.


### Evite utilizar el usuario root para las operaciones diarias
La clave de acceso del usuario root de su cuenta de AWS le da acceso completo a todos sus recursos de AWS, incluida la información de facturación. No puede reducir los permisos asociados a la clave de acceso de Root account de su cuenta de AWS. Se recomienda no utilizar el acceso de usuario root a menos que sea absolutamente necesario.

Si aún no dispone de una clave de acceso para el usuario root de su cuenta de AWS, no la cree a menos que sea absolutamente necesario. En su lugar, cree un usuario de IAM para usted con permisos administrativos. Puede iniciar sesión en la consola de administración de AWS con la dirección de correo electrónico y la contraseña de su cuenta de AWS para crear un usuario de IAM.

### Postura general de seguridad
Ejecute una [evaluación de seguridad de autoservicio] (https://aws.amazon.com/es/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) en relación con el medio ambiente para identificar con más detalle otros riesgos y, potencialmente, otras exposiciones públicas que no se identifican en este manual de estrategias.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesiten «arreglarse», pero que es importante conocerlos a la hora de ejecutar este manual junto con los requisitos operativos y comerciales. `

## Temas pendientes 
- Como respondedor a incidentes, necesito un manual sobre cómo gestionar los tokens de sesión y las claves de acceso después de un incidente
- Como respondedor de incidentes, necesito un método para escanear y eliminar las claves de los repositorios de código público
- Como respondedor a incidentes, necesito una persona que tome decisiones claras sobre cuándo interrumpir un entorno en lugar de permitir una exposición continua
## Artículos pendientes actuales

