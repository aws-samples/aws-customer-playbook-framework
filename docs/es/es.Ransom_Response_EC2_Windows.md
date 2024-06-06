# Guía de respuesta a incidentes: respuesta de rescate para EC2 Microsoft Windows
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por los acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso de respuesta a los ataques de rescate contra instancias EC2.

Para obtener más información, consulte la [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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

! [Imagen] (/images/aws_caf.png)
* * *

### Pasos de respuesta
1. [**PREPARATION**] Utilizar AWS Config para ver el cumplimiento de la configuración
2. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
3. [**CONTAINMENT**] Aislar inmediatamente los recursos afectados
4. [**DETECCIÓN Y ANALÍSIS**] Procedimientos de uso que se encuentran en el Microsoft 365 Defender Threat Intelligence Team
5. [**DETECCIÓN Y ANALÍSIS**] Utilizar métricas de CloudWatch para determinar si es posible que se hayan extraído datos
6. [**DETECCIÓN Y ANALÍSIS**] Utilizar VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
7. [**CONTENCIÓN Y ERRADICACIÓN**] Elimine cualquier sistema comprometido de la red.
8. [**CONTENCIÓN Y ERRADICACIÓN**] Aplicar las NACL basadas en IOC de red para evitar más tráfico
9. [**CONTENCIÓN Y ERRADICACIÓN**] Otros elementos de interés
10. [**RECOVERY**] Ejecute los procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Rescate y destrucción de datos
* **Categoría**: Ataque de rescate
* **Recurso**: EC2
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: CloudTrail, CloudWatch, AWS Config
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Preparación
* Evaluar la postura de seguridad de la cuenta para identificar y corregir las brechas de seguridad
* AWS desarrolló una nueva herramienta de Self-Service Security Assessment de código abierto (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de su AWS cuenta
* Para proteger su organización, Microsoft recomienda que utilice la información del [Plan de proyecto de mitigación del ransomware operado por humanos] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) presentación de PowerPoint, que incluye [asegurar acceso privilegiado] (https://aka.ms/spa)
* Mantener un inventario completo de activos de todos los recursos, incluidos los controladores de dominio, las instancias EC2 de Microsoft Windows, los servidores y bases de datos de Microsoft Windows y cualquier integración con proveedores de identidad externos
* Realizar análisis de vulnerabilidades recurrentes de sus hosts mediante utilidades tales como [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
* Activar y actualizar Windows Defender Antivirus: se prefieren las soluciones comerciales de detección y respuesta de Endpoint de pago por suscripción (EDR)
* Realizar copias de seguridad de instancias EC2
* Considere utilizar [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) o [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
* [Realizar una copia de seguridad de los archivos con el historial de archivos] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* Verifique sus copias de seguridad y asegúrese de que la infección no se haya propagado a ellos
* Realice copias de seguridad de los archivos importantes regularmente. Usa la regla 3-2-1. Mantenga tres copias de seguridad de sus datos, en dos tipos de almacenamiento diferentes y al menos una copia de seguridad fuera del sitio
* Aplique las últimas actualizaciones a sus sistemas operativos y aplicaciones
* Educar a sus empleados para que puedan identificar los ataques de ingeniería social y phishing con lanza
* Implementar [acceso controlado a carpetas] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). Puede evitar que el ransomware encripte los archivos y retenga los archivos para obtener rescate
* [Bloquear macros en documentos de Office] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
* Bloquear tipos de archivos ransomware conocidos
* En Windows 10, active [Acceso controlado a carpetas] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) para proteger sus carpetas locales importantes de programas no autorizados como ransomware u otro malware
* Siga [Prácticas recomendadas para proteger los servicios de Active Directory] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
* Siga [Las 10 principales configuraciones de directivas de grupo más importantes para prevenir las infracciones de seguridad] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
* El Microsoft 365 Defender Threat Intelligence Team ha proporcionado un [informe completo] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) que identifica acciones para proteger los recursos de Microsoft antes de un evento de ransomware
* Refuerce los activos orientados a Internet y asegúrese de que tienen las últimas actualizaciones de seguridad. Utilice la administración de amenazas y vulnerabilidades para auditar estos activos regularmente en busca de vulnerabilidades, configuraciones erróneas y actividades sospechosas.
* Puerta de enlace segura de Escritorio remoto mediante soluciones como Azure Multi-Factor Authentication (MFA). Si no tiene una puerta de enlace MFA, habilite la autenticación a nivel de red (NLA)
* Practicar el principio del mínimo privilegio y mantener la higiene de las credenciales. Evite el uso de cuentas de servicio de todo el dominio a nivel de administrador. Aplicar contraseñas de administrador local sólidas aleatorizadas y justo a tiempo
* Supervisar los intentos de fuerza bruta. Comprobar intentos de autenticación fallidos excesivos (ID de evento de seguridad de Windows 4625
* Supervisar para borrar los registros de sucesos, especialmente el registro de sucesos de seguridad y los registros operativos de PowerShell. Microsoft Defender ATP genera la alerta «El Event log was cleared» y Windows genera un ID de evento 1102 cuando esto ocurre
* Activar las funciones de protección contra manipulaciones para evitar que los atacantes detengan los servicios de seguridad
* Determine dónde inician sesión las cuentas con alto privilegio y exponen las credenciales. Supervisar e investigar los eventos de inicio de sesión (ID de evento 4624) en busca de atributos de tipo de inicio de sesión. Las cuentas de administrador de dominio y otras cuentas con alto privilegio no deben estar presentes en las estaciones de trabajo
* Activa la protección proporcionada en la nube y el envío automático de muestras en Windows Defender Antivirus. Estas capacidades utilizan inteligencia artificial y aprendizaje automático para identificar y detener rápidamente amenazas nuevas y desconocidas.
* Activa las reglas de reducción de superficies de ataque, incluidas las reglas que bloquean el robo de credenciales, la actividad de ransomware y el uso sospechoso de PsExec y WMI. Para abordar la actividad maliciosa iniciada a través de documentos de Office armados, utilice reglas que bloqueen la actividad macro avanzada, el contenido ejecutable, la creación de procesos y la inyección de procesos iniciadas por las aplicaciones de Office. Para evaluar el impacto de estas reglas, desplegarlas en modo auditoría
* Activa AMSI para Office VBA si tienes Office 365
* Utilice el Windows Defender Firewall y el cortafuegos de red para evitar la comunicación RPC y SMB entre los endpoints siempre que sea posible. Esto limita el movimiento lateral, así como otras actividades de ataque.
* Utilice [Systems Manager y Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) para comprobar si las instancias EC2 que ejecutan Microsoft Windows contienen vulnerabilidades y exposiciones comunes (CVE)

### Utilice AWS Config para ver el cumplimiento de la configuración:
1. Inicie sesión en AWS Management Console y abra la consola de AWS Config en https://console.aws.amazon.com/config/
1. En el menú de AWS Management Console, compruebe que el selector de regiones esté configurado en una región compatible con las reglas de AWS Config. Para obtener la lista de regiones compatibles, consulte Regiones y puntos finales de AWS Config en la referencia general de Amazon Web Services
1. En el panel de navegación, elija Recursos. En la página Inventario de recursos, puede filtrar por categoría de recursos, tipo de recurso y estado de conformidad. Elija Incluir recursos eliminados si procede. En la tabla se muestra el identificador de recursos del tipo de recurso y el estado de conformidad de recursos de ese recurso. El identificador de recurso podría ser un ID de recurso o un nombre de recurso
1. Elija un recurso de la columna de identificador de recursos
1. Pulse el botón Línea temporal de recursos. Puede filtrar por eventos de configuración, eventos de cumplimiento o eventos de CloudTrail
1. Concéntrese específicamente en los siguientes eventos:
* ebs-in-backup-plan
* ebs-optimized-instance
* ebs-snapshot-public-restaurable-cheque
* ec2-ebs-encryption-by-default
* ec2-imdsv2-check
* ec2-instance-detailed-monitoring-enabled
* ec2-instance-managed-by-systems-manager
* ec2-instance-multiple-eni-check
* ec2-instance-no-public-ip
* ec2-instance-profile-attached
* ec2-managedinstance-applications-blacklisted
* ec2-managedinstance-applications-required
* ec2-managedinstance-association-compliance-status-check
* ec2-managedinstance-inventory-blacklisted
* ec2-managedinstance-patch-compliance-status-check
* ec2-managedinstance-platform-check
* ec2-security-group-attached-to-eni
* ec2-stopped-instance
* ec2-volume-inuse-check

## Procedimientos de escalada
- `Necesito una decisión empresarial sobre cuándo debe llevarse a cabo la investigación forense de EC2`
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Detección y análisis
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

### El Microsoft 365 Defender Threat Intelligence Team
El equipo ha proporcionado un informe completo (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) que identifica las cosas que debe buscar en relación con múltiples variantes de ransomware

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

## Contención
### Aislar inmediatamente los recursos afectados
**NOTA**: Asegúrese de que dispone de un proceso para solicitar una escalada y aprobación para aislar los recursos y garantizar que se lleve a cabo primero un análisis de impacto empresarial sobre cómo el aislamiento afectará a las operaciones actuales y a los flujos de ingresos.
1. Determine si la instancia forma parte de un grupo de Auto Scaling o está asociada a un equilibrador de carga
* Grupo de escalado automático: separar la instancia del grupo
* Elastic Load Balancer: anula el registro de la instancia del ELB y elimina la instancia de los grupos de destino
1. Cree un grupo de seguridad *nuevo* que bloquee todo el tráfico de entrada y salida; asegúrese de eliminar la regla predeterminada `permitir todo `para el tráfico de salida
1. Adjuntar el grupo de seguridad *nuevo* a las instancias afectadas

### Aplicar NACL basados en IOC de red para evitar más tráfico
1. Abra la [consola de Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Network ACLs
1. Elija Crear Network ACL
1. En el cuadro de diálogo Crear Network ACL, opcionalmente, asigne el nombre de la ACL de red y seleccione el ID de la VPC en la lista de VPC. A continuación, elija Sí, Crear
1. En el panel de detalles, elija la pestaña Reglas entrantes o Reglas salientes, según el tipo de regla que necesite agregar y, a continuación, elija Editar
1. En Regla n.º, introduzca un número de regla (por ejemplo, 100). El número de regla no debe estar ya en uso en la ACL de red. Procesamos las reglas en orden, empezando por el número más bajo
* Le recomendamos que deje huecos entre los números de regla (como 100, 200, 300), en lugar de utilizar números secuenciales (101, 102, 103). Esto facilita la adición de una nueva regla sin tener que volver a numerar las reglas existentes.
1. Seleccione una regla de la lista Tipo. Por ejemplo, para agregar una regla para HTTP, elija HTTP. Para agregar una regla allow all el tráfico TCP, elija Todo TCP. Para algunas de estas opciones (por ejemplo, HTTP), rellenamos el puerto por ti. Para utilizar un protocolo que no aparece en la lista, elija Regla de protocolo personalizada
1. (Opcional) Si va a crear una regla de protocolo personalizada, seleccione el número y el nombre del protocolo en la lista Protocolo. Para obtener más información, consulte Lista de números de protocolo de IANA
1. (Opcional) Si el protocolo seleccionado requiere un número de puerto, introduzca el número de puerto o el rango de puertos separados por un guión (por ejemplo, 49152-65535).
1. En el campo Origen o Destino (en función de si se trata de una regla entrante o saliente), introduzca el rango CIDR al que se aplica la regla
1. En la lista Permitir/Denegar, seleccione PERMITIR para permitir que el tráfico especificado o DENEGAR denegue el tráfico especificado
1. (Opcional) Para añadir otra regla, elija Agregar otra regla y repita los pasos anteriores según sea necesario
1. Cuando hayas terminado, elige Guardar
1. En el panel de navegación, elija Network ACLs y, a continuación, seleccione la ACL de red
1. En el panel de detalles, en la pestaña Asociaciones de subred, elija Editar. Active la casilla de verificación Asociar de la subred que se asociará a la ACL de red y, a continuación, seleccione Guardar

## Erradicación
### Elimine los sistemas comprometidos de la red.
* Siga los requisitos reglamentarios o la política interna de la empresa para determinar si se requiere análisis forense de la instancia EC2
* Si se requiere análisis forense de las instancias O es necesario recuperar los datos, siga el [Manual: EC2 Forensics] (. /docs/EC2_forensics.md)

### Otros artículos de interés
* [Eliminar los metadatos del controlador de dominio comprometidos del dominio] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
* Inspeccionar las copias de seguridad para detectar posibles infecciones

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
* [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM. NOTA: La eliminación de usuarios de IAM puede afectar las cargas de trabajo de producción y debe realizarse con cuidado * Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar sus cargas de trabajo en AWS
* Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restablezca desde el último evento programado anterior al evento de ransomware
* Crear nuevas instancias EC2 a partir de una AMI de confianza
* Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar las cargas de trabajo en AWS
* Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restablezca desde el último evento programado anterior al evento de ransomware

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook para llevar a cabo el análisis forense de EC2
- Como Respondedor de Incidentes, necesito una decisión empresarial sobre cuándo deben llevarse a cabo los análisis forenses de EC2
- Como Respondedor de Incidentes, necesito tener habilitado el registro en todas las regiones habilitadas independientemente de la intención de uso
- Como Respondedor de Incidentes, necesito poder detectar la minería de criptomonedas en mis instancias EC2 existentes

## Elementos atrasados actuales