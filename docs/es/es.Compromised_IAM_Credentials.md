# Guía de respuesta a incidentes: credenciales de IAM comprometidas
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por los acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para responder cuando observa una actividad no autorizada dentro de su cuenta de AWS o cree que una parte no autorizada ha accedido a su cuenta.

## Indicadores potenciales de compromiso
- Usuarios de IAM nuevos o no reconocidos
- Recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
- Aumentos de facturación inusuales
- Notificación del investigador de seguridad
- Notificación de que mis recursos o cuenta de AWS podrían verse comprometidos

## Conclusiones potenciales de AWS GuardDuty
- Acceso a credenciales: comportamiento anómalo o de usuario de IAM
- Evasión de defensa: soy usuario/comportamiento anómalo
- Descubrimiento: comportamiento anómalo de usuario de IAM
- Exfiltración: comportamiento usuario/anómalo de IAM
- Impacto: comportamiento usuario/anómalo de IAM
- Acceso inicial: usuario de IAM/comportamiento anómalo
- Pentest: iamuser/kalilinux
- Pentest: usuario de IAM/Parrot Linux
- Pentest: usuario de iamuser/Pentoolinux
- Persistencia: comportamiento anómalo de usuario de IAM
- Política: Uso de credenciales de usuario/raíz de IAM
- Escalación de privilegios: comportamiento anómalo de usuario de IAM
- Recon: usuario de IA/Llamador IP malicioso
- Recon: usuario de IAM/invocador malicioso. Custom
- Recon: Llamador de usuario/toripi
- Sigilo: inicio de sesión de usuario de IAM/Cloud Trail deshabilitado
- Stealth: Cambio de política de usuario/contraseña de IAM
- Acceso no autorizado: IAM usuario/consola Login Success.
- Acceso no autorizado: filtración de credenciales de usuario/instancia de IAM
- Acceso no autorizado: usuario de IAM/IPS malintencionada
- Acceso no autorizado: usuario de IAM/IIP Caller.Custom
- Acceso no autorizado: usuario de IAM/TOIP Calller

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
1. [**PREPARACIÓN**] Realizar un inventario de activos
2. [**PREPARACIÓN**] Implementar un plan de formación para identificar y responder a las credenciales de IAM expuestas
3. [**PREPARACIÓN**] Implementar una estrategia de comunicación para la respuesta a incidentes
4. [**DETECCIÓN**] Identificar el acceso a la cuenta raíz (autorizado y no)
5. [**DETECCIÓN**] Identificar usuarios de IAM nuevos o no reconocidos
6. [**DETECCIÓN**] Identificar recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
7. [**DETECCIÓN**] Identificar y encontrar secretos expuestos
8. [**DETECCIÓN**] Identificar aumentos de facturación inusuales
9. [**DETECCIÓN**] Responder a la notificación de AWS o de un tercero de que mis recursos o cuenta de AWS podrían verse comprometidos
10. [**DETECCIÓN**] Identificar cualquier credencial de usuario de IAM potencialmente no autorizada
11. [**PREPARACIÓN**] Identificar procedimientos de escalado
12. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros de CloudTrail
13. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros de flujo de VPC
14. [**DETECCIÓN Y ANALÍSIS**] Revisar los registros basados en endpoint /host
15. [**CONTENCIÓN**] Realizar acciones de contención apropiadas
16. [**ERRADICATION**] Revise los hallazgos de Revisar el historial de eventos de CloudTrail para ver la actividad de la clave de
17. [**ERRADICACION**] Revisar la sección Evitar cargos inesperados
18. [**RECOVERY**] Realizar las acciones de recuperación adecuadas
19. [**PREPARACIÓN**] Realizar un escaneo IAM de Prowler
20. [**PREPARACIÓN**] Habilitar MFA
21. [**PREPARACION**] Verifica la información de tu cuenta
22. [**PREPARATION**] Utilizar proyectos de AWS Git para buscar pruebas de uso no autorizado
23. [**PREPARACION**] Evite utilizar el usuario raíz para las operaciones cotidianas
24. [**PREPARACIÓN**] Evalúe su postura de seguridad general

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [Publicación especial de NIST 800-61r2 Guía de manejo de incidentes de seguridad informática] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Exposición de credenciales
* **Categoría**: Exposición de credenciales de IAM
* **Recurso**: IAM
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros
* **Fuentes de registro**: AWS CloudTrail, AWS Config, registros de flujo de VPC, Amazon GuardDuty
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

Esta herramienta proporciona una instantánea rápida del estado de seguridad actual dentro de un entorno de cliente. Como alternativa, [AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) proporciona un escaneo automatizado de conformidad y puede [integrarse con Prowler] ( https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### Inventario de activos
Identificar a todos los usuarios existentes y tener una lista actualizada del propósito de cada cuenta

1. Vaya a [AWS Console] (https://console.aws.amazon.com/)
1. Vaya a `Services` y seleccione `IAM`
1. En el menú de la izquierda, selecciona `Informe de credencias`
1. Seleccione `Descargar informe`
1. Preste especial atención a cuándo se crearon las cuentas, se usó por última vez la contraseña y las columnas cambiadas por última vez.
* **NOTA** si un usuario tiene varias claves de acceso, valide el propósito de cada

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

### Acceso a la cuenta raíz
Se recomienda eliminar todas las claves de acceso asociadas a la cuenta raíz: `. /merodeador -c check_112`

### Usuarios de IAM nuevos o no reconocidos
Revisa el informe de credenciales de IAM desde tu [Inventario de activos] (. /compromised_IAM_credentials.md/ #asset -inventory)
Compruebe si los usuarios de IAM tienen dos claves de acceso activas: `. /merodeador -c check_extra712`
Asegúrese de que no se crean las políticas de IAM que permiten los privilegios administrativos completos de\ "*: *\»: `. /merodeador -c check_122`
Compruebe si IAM Access Analyzer está habilitado y sus conclusiones: `. /merodeador -c check_extra769`

### Recursos no reconocidos o no autorizados (por ejemplo, EC2, Lambda)
aws ec2 describe-instancias
funciones de lista aws lambda

### Buscando secretos
Se ha encontrado un secreto potencial en los datos de usuario de la instancia EC2: `. /merodeador -c check_extra741`
Posible secreto encontrado en las variables de función Lambda: `. /merodeador -c check_extra759`
Se encontró un secreto potencial en las variables de definición de tareas de ECS: `. /merodeador -c check_extra768`
Posible secreto encontrado en Configuración de escalado automático: `. /merodeador -c check_extra775`

### Aumentos de facturación inusuales
Para ver su factura de AWS, abra el panel [Facturas] (https://console.aws.amazon.com/billing/home #) de la consola Administración de facturación y costos y, a continuación, elija el mes que desea ver en el menú desplegable.

Puede ver el historial de los pagos de AWS en el panel [Pedidos y facturas] (https://console.aws.amazon.com/billing/home?/paymenthistory) de la consola Administración de facturación y costos.

También puede utilizar [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) para monitoreo y alertas dinámicas.

Consulte su factura para ver lo siguiente:
* Servicios de AWS que no utiliza normalmente
* Recursos de las regiones de AWS que no utiliza normalmente
* Un cambio significativo en el tamaño de su factura

Puede utilizar esta información para ayudarle a eliminar o cancelar cualquier recurso que no desee conservar.

### Notificación de que mis recursos o cuenta de AWS podrían verse comprometidos
Si ha recibido una notificación de AWS sobre su cuenta, inicie sesión en AWS Support Center y, a continuación, responda a la notificación.

Si no puede iniciar sesión en su cuenta, utilice el formulario de contacto para solicitar ayuda de AWS Support.

Si tiene alguna pregunta o inquietud, cree un nuevo caso de AWS Support en AWS Support Center.
* **NOTA**: No incluya información confidencial en su correspondencia, como claves de acceso, contraseñas o información de tarjetas de crédito de AWS.
Utilizar proyectos de AWS Git para buscar pruebas de uso no autorizado

### Identifica cualquier credencial de usuario de IAM potencialmente no autorizada
1. Abra la consola de IAM.
1. Seleccione Usuarios en el panel de navegación.
1. Elija cada usuario de IAM de la lista y, a continuación, compruebe en Directivas de permisos una política denominada AWSExposedCredentialPolicy_DO_NOT_Remove. 1. Si el usuario tiene esta política adjunta, debe rotar las claves de acceso del usuario.

## Procedimientos de escalada
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Análisis
Es muy recomendable exportar registros a una solución de administración de eventos de incidentes de seguridad (SIEM) (como Splunk, pila ELK, etc.) para facilitar la visualización y el análisis de una variedad de registros para un análisis de cronograma de ataque más completo.

### CloudTrail
Busca una actividad de inicio de sesión inusual
1. Vaya a su [CloudTrail Dashboard] (https://console.aws.amazon.com/cloudtrail)
1. En el margen izquierdo, selecciona `Historial de eventos`
1. En el menú desplegable, cambie de `Solo lectura` a `Nombre del evento`
1. En el campo de búsqueda, introduzca `ConsoleLogin` o `AssumeRole` o `GetFederationToken` o `getCredentialReport` o `GenerateCredentialReport` y revise los eventos disponibles para detectar cualquier actividad sospechosa
* **NOTA** UserIdentify aparecerá como `"type»: «Root"` para root o `"type»: «IAMUser"` para usuarios

Busque el ID de clave de acceso de IAM y el nombre de usuario utilizados para lanzar una instancia EC2 sospechosa
1. Abra la consola de CloudTrail y, a continuación, elija Historial de eventos.
1. Seleccione el menú desplegable Filtro y, a continuación, elija Nombre del recurso.
1. En el campo Introducir nombre de recurso, pegue el ID de instancia EC2 y, a continuación, elija Intro en su dispositivo.
1. Expanda el nombre del evento de RunInstances.
1. Copie la clave de acceso de AWS y anote el nombre de usuario.

Revisar la actividad del historial de eventos de CloudTrail mediante la clave de acceso comprometida
1. Abra la consola de CloudTrail y, a continuación, elija Historial de eventos en el panel de navegación.
1. Seleccione el menú desplegable Filtro y, a continuación, elija Filtro de claves de acceso de AWS.
1. En el campo Introducir clave de acceso de AWS, introduzca el ID de clave de acceso de IAM comprometido.
1. Expanda el nombre del evento de la llamada a la API RunInstances.
* **NOTA**: Solo puedes ver el historial de eventos de los últimos 90 días a menos que hayas configurado previamente un rastro que se guarda en un bucket de S3

### Registros de flujo de VPC
Los registros de flujo de VPC es una característica que le permite capturar información sobre el tráfico IP que entra y sale de las interfaces de red de la VPC. Esto puede resultar útil para las direcciones IP detectadas en CloudTrail para determinar los tipos de conexiones externas a cualquier recurso público.

Para obtener más información y pasos, incluida la consulta con Athena, consulte la [Documentación de AWS para registros de flujo de VPC] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html). Se recomienda que el análisis de Atenea se incluya en un libro de jugadas separado y se vincule a otros elementos relevantes.

### Endpoint/Basado en host
* **Identificar la última hora de creación de un nombre de instancia**: `aws ec2 describe-instances —query 'Reservaciones [] .Instancias []. {ip: publiciPAddress, tm: launchTime} '—filtros 'name=Tag:name, Values= myInstanceName' | jq 'sort_by (.tm) | reverse |. [0] '`

* **Identificar la hora de la última modificación para las funciones lambda**: `aws lambda list-functions | grep «LastModified"`

## Contención
1. Deshabilitar el usuario de IAM, crear una clave de acceso de IAM de copia de seguridad y, a continuación, deshabilitar la clave de acceso comprometida
1. Abra la [consola de IAM] (https://console.aws.amazon.com/iam/) y, a continuación, pegue el ID de clave de acceso de IAM en la barra Buscar IAM.
1. Elija el nombre de usuario y, a continuación, elija la pestaña Credenciales de seguridad.
1. En Contraseña de consola, selecciona Administrar.
* **NOTA**: Si la contraseña de AWS Management Console se establece en Deshabilitado, puede omitir este paso.
1. En Acceso a la consola, elija Deshabilitar y, a continuación, seleccione Aplicar.
1. Importante: Los usuarios cuyas cuentas están deshabilitadas no pueden acceder a AWS Management Console. Sin embargo, si el usuario tiene claves de acceso activas, puede seguir accediendo a los servicios de AWS mediante llamadas a la API.
1. Para la clave de acceso de IAM comprometida, elija Hacer inactivo.
1. Deshabilitar la clave de IAM de la aplicación, crear una clave de acceso de IAM de respaldo y, a continuación, deshabilitar la clave de acceso comprometida
1. Primero, crea una segunda clave. A continuación, modifique la aplicación para utilizar la nueva clave.
1. Desactiva (pero no elimine) la primera clave.
1. Si hay algún problema con la aplicación, vuelva a activar la clave temporalmente. Cuando la aplicación esté completamente funcional y la primera clave esté deshabilitada, elimine la primera clave.
1. Rotar y eliminar todas las claves de acceso de AWS inactivas o comprometidas
* **! Asegúrese de mantener un registro de todas las claves de acceso eliminadas para poder seguir buscándolas en CloudTrail. **

## Erradicación
### Revise los hallazgos de [Revisar el historial de eventos de CloudTrail en busca de actividad mediante la clave de acceso comprometida] (. /#cloudtrail)
Elimine los recursos creados por la (s) clave (s) comprometida (s). Asegúrese de comprobar todas las regiones de AWS, incluso las regiones en las que nunca ha lanzado recursos de AWS.
* **Importante**: Si necesitas conservar algún recurso para la investigación, considera respaldarlos. Por ejemplo, si tiene una necesidad normativa, de conformidad o legal de conservar una instancia EC2, realice una instantánea de EBS antes de finalizar la instancia.

### Revise el [Evitar cargos inesperados] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
Compruebe y elimine los servicios que se reconozcan en su cuenta. Preste especial atención a los siguientes recursos:
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

Eliminar cualquier usuario de IAM que no haya creado
1. Inicie sesión en AWS Management Console y abra la [consola de IAM] (https://console.aws.amazon.com/iam/)
1. En el panel de navegación, seleccione Usuarios y, a continuación, active la casilla de verificación situada junto al nombre de usuario que desea eliminar, no el nombre o la fila en sí.
1. En la parte superior de la página, selecciona Eliminar usuario.
1. En el cuadro de diálogo de confirmación, espere a que se cargue la última información a la que se accede antes de revisar los datos. El cuadro de diálogo muestra cuándo cada uno de los usuarios seleccionados accedió por última vez a un servicio de AWS. Si intenta eliminar un usuario que ha estado activo en los últimos 30 días, debe seleccionar una casilla de verificación adicional para confirmar que desea eliminar al usuario activo. Si quieres continuar, elige Sí, Eliminar.

La documentación sobre la eliminación de otros servicios se encuentra en la página [terminar todos mis recursos] (https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/).

## Recuperación
AWS tiene una guía publicada para [¿qué hago si noto actividad no autorizada en mi cuenta de AWS?] (https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## Acciones preventivas
### Exploración de IAM Prowler
`. /prowler -g check 122, check 111, cheque 110, cheque 19, cheque 18, cheque 17, cheque 16, cheque 15, cheque 11, cheque 116, cheque 12, check 114, cheque 115, cheque 14, cheque 13, check 112, cheque 119, 71 extra, 7100 extra, 7123 extra, 7125 extra, 769 extra, 774` extra

### Activar MFA
Para aumentar la seguridad, es recomendable configurar MFA para ayudar a proteger sus recursos de AWS. Puede habilitar [MFA para usuarios de IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) o [usuario raíz de la cuenta de AWS] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html). La activación de MFA para el usuario raíz afecta solo a las credenciales de usuario raíz. Los usuarios de IAM de la cuenta son identidades distintas con sus propias credenciales y cada identidad tiene su propia configuración de MFA.

### Verifica la información de tu cuenta
AWS necesita información precisa de la cuenta para ponerse en contacto con usted y ayudar a resolver cualquier problema de cuenta. Comprueba que la información de tu cuenta es correcta.
* El nombre de la cuenta y la dirección de correo electrónico.
* Su información de contacto, especialmente su número de teléfono.
* Los contactos alternativos de tu cuenta.

### Utilizar proyectos de AWS Git para buscar pruebas de uso no autorizado
AWS ofrece proyectos de Git que puede instalar para ayudarle a proteger su cuenta:
* [Git Secrets] (https://github.com/awslabs/git-secrets) puede escanear fusiones, confirmaciones y mensajes de confirmación en busca de información secreta (es decir, claves de acceso). Si Git Secrets detecta expresiones regulares prohibidas, puede rechazar que esas confirmaciones se publiquen en repositorios públicos.
* Utilice [AWS Step Functions y AWS Lambda para generar Amazon CloudWatch Events] (https://aws.amazon.com/step-functions) desde AWS Health o AWS Trusted Advisor. Si hay pruebas de que sus claves de acceso están expuestas, los proyectos pueden ayudarlo a detectar, registrar y mitigar automáticamente el evento.

### Evite utilizar el usuario raíz para las operaciones cotidianas
La clave de acceso para el usuario raíz de su cuenta de AWS proporciona acceso completo a todos los recursos de AWS, incluida la información de facturación. No puede reducir los permisos asociados a la clave de acceso de usuario raíz de su cuenta de AWS. Es recomendable no utilizar el acceso de usuario root a menos que sea absolutamente necesario.

Si aún no tiene una clave de acceso para el usuario raíz de su cuenta de AWS, no cree una a menos que sea absolutamente necesario. En su lugar, cree un usuario de IAM para usted mismo con permisos administrativos. Puede iniciar sesión en AWS Management Console con la dirección de correo electrónico y la contraseña de su cuenta de AWS para crear un usuario de IAM.

### Postura general de seguridad
Ejecute una [Evaluación de seguridad de autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) contra el entorno para identificar otros riesgos y potencialmente otras exposiciones públicas no identificadas a lo largo de este libro de jugadas.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan necesariamente «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un libro de jugadas sobre cómo manejar tokens de sesión y claves de acceso después de un incidente
- Como Respondedor de Incidentes, necesito un método para escanear y eliminar claves de los repositorios de código público
- Como Respondedor de Incidentes, necesito una toma de decisiones clara sobre cuándo romper un entorno frente a permitir una exposición continua
## Elementos atrasados actuales