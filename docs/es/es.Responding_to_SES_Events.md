# Guía de respuesta a incidentes: respuesta a eventos simples de servicio de correo electrónico

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

### Objetivos
Durante la ejecución del libro de jugadas, concéntrese en los resultados _***deseados***_, tomando notas para mejorar las capacidades de respuesta a incidentes.

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

### Pasos de respuesta
1. [**PREPARATION**] Utilizar las detecciones de AWS GuardDuty para IAM
2. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
3. [**DETECCIÓN Y ANALÍSIS**] Realizar la detección y analizar CloudTrail en busca de eventos API no reconocidos
4. [**DETECCIÓN Y ANALÍSIS**] Realizar la detección y analizar CloudWatch en busca de eventos no reconocidos
3. [**CONTENCIÓN Y ERRADICACIÓN**] Eliminar o rotar claves de usuario de IAM
4. [**CONTENCIÓN Y ERRADICACIÓN**] Eliminar o rotar recursos no reconocidos
5. [**RECOVERY**] Ejecute los procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [Publicación especial de NIST 800-61r2 Guía de manejo de incidentes de seguridad informática] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**:
* **Categoría**:
* **Recurso**: SES
* **Indicadores**:
* **Fuentes de registro**:
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Resumen ejecutivo
En este manual se describe el proceso de respuesta a los ataques contra AWS Simple Email Service (SES). En combinación con esta guía, consulte las [Preguntas frecuentes sobre el proceso de revisión de envío de Amazon SES] (https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html) para obtener respuestas a las acciones de cumplimiento y respuestas al uso adverso de SES.

Para obtener más información, consulte la [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Preparación - General
* Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
* AWS desarrolló una nueva herramienta de código abierto [Evaluación de seguridad de autoservicio] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de sus AWS cuenta.
* Mantener un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo
* Considere la posibilidad de implementar [AWS GuardDuty] (https://aws.amazon.com/guardduty/) para supervisar continuamente la actividad maliciosa y el comportamiento no autorizado a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon SES
* Implementar [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), incluida la caducidad de las cuentas y las rotaciones de credenciales obligatorias
* **Aplicar la autenticación multifactor (MFA) **
* Cumplir los requisitos de complejidad de las contraseñas y establecer periodos de
* Ejecute un [Informe de credenciales de IAM] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) para enumerar todos los usuarios de su cuenta y el estado de sus diversas credenciales, incluidas contraseñas, claves de acceso y dispositivos MFA
* Utilice [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) para identificar los recursos de su organización y cuentas, como los roles de IAM que se comparten con una entidad externa. Esto le permite identificar el acceso no deseado a sus recursos y datos, lo que supone un riesgo para la seguridad

## Preparación - Específico de SES
* Utilice las políticas de autorización de envío para controlar quién puede enviar correos electrónicos y desde dónde
* https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* Utilizar conjuntos de configuración para controlar las direcciones IP e identidades permitidas para enviar mensajes
* https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* Considere la posibilidad de utilizar direcciones IP dedicadas para Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* Gestiona tus propias listas de correo y suscripciones, así como para supresión de correo electrónico en Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* Configurar la publicación de eventos de Amazon SES para notificaciones en tiempo real
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* Utilizar la administración de acceso e identidad con menos privilegios en Amazon SES
* https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* Revisar las mejores prácticas de SES, centrándose en la seguridad y el acceso
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html (clásico)
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html (clásico)
* Configurar SPF, DKIM y DMARC para su propio dominio para ayudar a evitar el phishing y la suplantación de identidad
* https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### Detecciones potenciales de AWS GuardDuty
Los siguientes hallazgos son específicos de las entidades de IAM y las claves de acceso y siempre tienen un tipo de recurso de AccessKey. La gravedad y los detalles de los hallazgos difieren según el tipo de hallazgo. Para obtener más información sobre cada tipo de búsqueda, consulte la página web de [Tipos de búsqueda GuardDuty IAM] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html).

* Acceso a credenciales: usuario de IAM/comportamiento anómalo
* Evasión de defensa: soy usuario/comportamiento anómalo
* Descubrimiento: Comportamiento anómalo o usuario/de IAM
* Exfiltración: Comportamiento anómalo o usuario/de IAM
* Impacto: comportamiento usuario/anómalo de IAM
* Acceso inicial: usuario de IAM/comportamiento anómalo
* Pentest: usuario de iamuser/Kalilinux
* Pentest: usuario de IA/PARROT Linux
* Pentest: usuario de ia/Pentoolinux
* Persistencia: comportamiento de usuario/anómalo de IAM
* Política: Uso de credenciales de usuario/raíz de IAM
* Escalación de privilegios: comportamiento anómalo de usuario de IAM
* Recon: usuario/IP malicioso que llama
* Recon: IAMUSER/IPIC malintencionado.Custom
* Recon: Llamador de usuario/toripi
* Sigilo: inicio de sesión de usuario de IAM/Cloud Trail deshabilitado
* Sigilo: cambio de política de usuario/contraseña de IAM
* Acceso no autorizado: inicio de sesión de usuario y consola de IAM.B
* Acceso no autorizado: filtración de credenciales de usuario/instancia de IAM. Fuera de AWS
* Acceso no autorizado: usuario de IA/IP maliciosa que llama
* Acceso no autorizado: usuario de IAM/IIP Caller.Custom
* Acceso no autorizado: usuario de IAM/TOIP Calller

## Procedimientos de escalada
- `Necesito una decisión empresarial sobre cuándo debe llevarse a cabo la investigación forense de EC2`
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Detección y análisis
### CloudTrail
Amazon SES se integra con AWS CloudTrail, un servicio que proporciona un registro de las acciones realizadas por un usuario, rol o servicio de AWS en Amazon SES. CloudTrail captura las llamadas a la API de Amazon SES como eventos. Las llamadas capturadas incluyen llamadas desde la consola de Amazon SES y llamadas de código a las operaciones de la API de Amazon SES.

A continuación se presentan eventos `EventName` de CloudTrail específicos para buscar cambios en su cuenta relacionados con SES:

* Eliminar identidad
* Eliminar política de identidad
* Eliminar filtro de recepción
* Eliminar regla de recepción
* Eliminar conjunto de normas de recepción
* Eliminar dirección de correo electrónico verificada
* Cuota de obtención de envío
* Política de identidad de PUT
* Receiptrule de actualización
* Verificar dominio en DKim
* Verificar identidad de dominio
* Verificar la dirección de correo electrónico
* Verificar la identidad del correo electrónico

[Visualización de eventos con el historial de eventos de CloudTrail] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

### Otros controles de detectives
* Registrar y supervisar eventos de SES (por ejemplo, envíos, rebotes, quejas, etc.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Supervisar métricas de reputación de remitente
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Configurar alarmas o panel de control de Cloudwatch apropiados para métricas de SES
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Contención y erradicación
* [Eliminar o rotar claves de usuario de IAM] (https://console.aws.amazon.com/iam/home#users) y [Claves de usuario raíz] (https://console.aws.amazon.com/iam/home#security_credential); puede que desee rotar todas las claves de su cuenta si no puede identificar una clave o claves específicas que se hayan expuesto
* [Eliminar usuarios de IAM no autorizados] (https://console.aws.amazon.com/iam/home#users.)
* [Eliminar políticas no autorizadas] (https://console.aws.amazon.com/iam/home#/policies)
* [Eliminar roles no autorizados] (https://console.aws.amazon.com/iam/home#/roles)
* [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM.
* NOTA: La eliminación de usuarios de IAM puede afectar las cargas de trabajo de producción y debe realizarse con cuidado

## Recuperación
* Crear nuevos usuarios de IAM con políticas de acceso de mínimos privilegios
* Implementar los pasos y recursos que se encuentran en la sección `Preparación - Especificaciones de SES `
* Registrar y supervisar eventos de SES (por ejemplo, envíos, rebotes, quejas, etc.)
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* Supervisar métricas de reputación de remitente
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* Configurar alarmas o panel de control de Cloudwatch apropiados para métricas de SES
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesitan necesariamente «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos

## Elementos atrasados actuales