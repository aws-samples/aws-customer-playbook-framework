# Guía de respuesta a incidentes: Respuesta de rescate para S3
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

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
3. [**DETECCIÓN Y ANALÍSIS**] Realizar la detección y analizar CloudTrail en busca de API no reconocida
4. [**RECOVERY**] Ejecute los procedimientos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Rescate y destrucción de datos
* **Categoría**: Ataque de rescate
* **Recurso**: S3
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch
* **Fuentes de registro**: registros de servidor de S3, registros de S3 Access Logs, CloudTrail, CloudWatch, AWS Config
* **Equipos**: Centro de operaciones de seguridad (SOC), investigadores forenses, ingeniería en la nube

## Proceso de manejo de incidentes
### El proceso de respuesta a incidentes tiene las siguientes etapas:
* Preparación
* Detección y análisis
* Contención y erradicación
* Recuperación
* Actividad posterior al incidente

## Resumen ejecutivo
Este manual describe el proceso de respuesta a los ataques de rescate contra AWS Simple Storage Service (S3).

Para obtener más información, consulte la [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

## Preparación
* Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
* AWS desarrolló una nueva herramienta de código abierto [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de sus AWS cuenta.
* Mantener un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo
* Considere utilizar [AWS Config] (https://aws.amazon.com/config/), que es un servicio que le permite evaluar, auditar y evaluar las configuraciones de sus recursos de AWS
* Considere la posibilidad de implementar [AWS GuardDuty] (https://aws.amazon.com/guardduty/) para supervisar continuamente la actividad maliciosa y el comportamiento no autorizado a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon S3
* Darkbit proporciona un ejemplo de [AWS S3 Data Loss Prevention] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) que se puede utilizar para identificar la copia no autorizada de objetos. También se podrían añadir otras operaciones específicas al patrón en función de los casos de uso de su empresa
* Utilizar roles de IAM para administrar los permisos
* Implementar menos privilegios y no permitir s3: * permisos
* Implementar [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), incluida la caducidad de las cuentas y las rotaciones de credenciales obligatorias
* Aplicar la autenticación multifactor (MFA)
* Cumplir los requisitos de complejidad de las contraseñas y establecer periodos de
* Ejecute un [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) para enumerar todos los usuarios de su cuenta y el estado de sus diversas credenciales, incluidas contraseñas, claves de acceso y dispositivos MFA
* Utilice [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) para identificar los recursos de su organización y cuentas, como los depósitos de Amazon S3 o las funciones de IAM que se comparten con una entidad externa. Esto le permite identificar el acceso no deseado a sus recursos y datos, lo que supone un riesgo para la seguridad
* [Habilitar el control de S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) para permitir la restauración de objetos modificados
* Para establecer el número de versiones conservadas, establezca una política de ciclo de vida (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) para que se aplique a versiones no actuales
* [Habilitar la S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) para evitar que un atacante deshabilite el control de versiones y sobrescriba todos los objetos de un bucket
* Considere utilizar [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) para poder almacenar objetos utilizando un modelo de escritura, una lectura múltiple (WORM). Object Lock puede ayudar a evitar que los objetos se eliminen o sobrescriban durante un tiempo fijo o indefinidamente.
* En el modo de cumplimiento de bloqueo de objeto*, * ningún usuario puede sobrescribir ni eliminar una versión de objeto protegido, incluido el usuario raíz de su cuenta de AWS. Cuando un objeto está bloqueado en modo de cumplimiento, su modo de retención no se puede cambiar y su período de retención no se puede acortar. El modo de cumplimiento *garantiza* que una versión de objeto no se pueda sobrescribir ni eliminar durante el período de retención.
* Considere utilizar [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) para copias de seguridad de objetos y optimización de costos
* Utilizar y auditar rutinariamente las políticas de bucket
* Solo haga público lo que necesita ser y asegúrese de que todos los objetos estén protegidos por ser privados
* Considere utilizar claves de [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) para cifrar todos los objetos e impedir que un atacante aplique su clave de cifrado
* Considere utilizar la [función de acceso público de bloque de AWS S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) para mitigar la exposición involuntaria de objetos
* Habilitar [el registro de eventos de CloudTrail para depósitos y objetos de S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) que contiene información confidencial o crítica. De forma predeterminada, las pistas de CloudTrail no registran eventos de datos, pero puede configurar pistas para registrar los eventos de datos de los buckets de S3 que especifique o para registrar eventos de datos para todos los buckets de Amazon S3 de su cuenta de AWS.
* Habilite el [registro a nivel de servidor CloudTrail para depósitos S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) y objetos que contengan información confidencial o crítica. El registro de acceso al servidor proporciona registros detallados de las solicitudes que se realizan a un bucket
* Para cargas de datos de misión crítica, considere [habilitar la replicación] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html): la misma región o entre regiones. La replicación entre regiones protege los datos contra defectos de las aplicaciones y errores del operador al mantener una segunda copia en otra región
* Tomar medidas para [evitar que cualquier nueva credencial se exponga públicamente] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
* Aunque no podemos recomendar soluciones específicas de terceros, también existen productos de nuestros socios, incluidos Veritas y CommVault, y otros proveedores de software de backup que tienen la capacidad nativa de hacer copias de seguridad de objetos en S3 en sus ubicaciones de almacenamiento de backup administrado dentro o fuera de S3

### Utilice AWS Config para ver el cumplimiento de la configuración:
1. Inicie sesión en AWS Management Console y abra la consola de AWS Config en https://console.aws.amazon.com/config/
1. En el menú de AWS Management Console, compruebe que el selector de regiones esté configurado en una región compatible con las reglas de AWS Config. Para obtener la lista de regiones compatibles, consulte Regiones y puntos finales de AWS Config en Amazon Web Services General Reference
1. En el panel de navegación, elija Recursos. En la página Inventario de recursos, puede filtrar por categoría de recursos, tipo de recurso y estado de conformidad. Elija Incluir recursos eliminados si procede. En la tabla se muestra el identificador de recursos del tipo de recurso y el estado de conformidad de recursos de ese recurso. El identificador de recurso podría ser un ID de recurso o un nombre de recurso
1. Elija un recurso de la columna de identificador de recursos
1. Pulse el botón Línea temporal de recursos. Puede filtrar por eventos de configuración, eventos de cumplimiento o eventos de CloudTrail
1. Concéntrese específicamente en los siguientes eventos:
* s3-account-level-public-access-blocks
* s3-account-level-public-access-blocks-periodic
* s3-bucket-blacklisted-actions-prohibited
* s3-bucket-default-lock-enabled
* s3-bucket-level-public-access-prohibited
* s3-bucket-logging-enabled
* s3-bucket-policy-grantee-check policy-concesionario-cheque
* s3-bucket-policy-not-more-permissive
* s3-bucket-public-read-prohibited
* s3-bucket-public-write-prohibited
* s3-bucket-replication-enabled
* s3-bucket-server-side-encryption-enabled
* s3-bucket-ssl-requests-only
* s3-bucket-versioning-enabled
* s3-default-encryption-kms

## Procedimientos de escalada
- `Necesito una decisión empresarial sobre cuándo debe llevarse a cabo la investigación forense de S3»
- «¿Quién está monitoreando los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Detección y análisis
* Los objetos S3 se eliminan o se eliminan cubos S3 completos
* Nota: Con los eventos de destrucción de datos, se puede proporcionar una nota de rescate o no. Asegúrese también de comprobar las métricas de CloudWatch y los eventos de CloudTrail S3 para verificar si se produjo o no la exfiltración de datos para delimitar entre un ataque de rescate o destrucción de datos
* Los objetos S3 se cifran mediante una clave de una cuenta que no es propiedad del cliente
* Nota de rescate proporcionada como objeto dentro del depósito o por correo electrónico al cliente
* Compruebe su registro de CloudTrail en busca de actividades no autorizadas, como la creación de usuarios, políticas, roles o credenciales de seguridad temporales de IAM no autorizados
* Revise CloudTrail en busca de llamadas API no reconocidas. En concreto, busca los siguientes eventos:
* DeleteBucket
* DeleteBucketCors
* DeleteBucketEncryption
* DeleteBucketLifecycle Bucket
* DeleteBucketPolicy
* DeleteBucketReplication
* DeleteBucketTagging
* DeleteBucketPublicAccessBlock
* Si los registros de acceso al servidor de S3 están habilitados, busque `REST.COPY.OBJECT_GET` alto y secuencial desde la misma IP remota y solicitante.
* Compruebe su registro de CloudTrail para revisar su cuenta de AWS en busca de cualquier uso no autorizado de AWS, como instancias EC2 no autorizadas, funciones Lambda o pujas puntuales de EC2. También puede comprobar el uso iniciando sesión en la consola de administración de AWS y revisando cada página de servicio. La página «Facturas» de la consola de facturación también se puede comprobar para detectar un uso inesperado
* Tenga en cuenta que el uso no autorizado puede producirse en cualquier región y que la consola puede mostrarle solo una región a la vez. Para cambiar entre regiones, puedes utilizar el menú desplegable en la esquina superior derecha de la pantalla de la consola

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
* Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restaura desde el último evento programado anterior al evento de ransomware
* Implementar procedimientos en Proteger antes de intentar restaurar datos u objetos
* [Eliminar marcadores de eliminación] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) para objetos versionados
* Volver a crear depósitos eliminados
* [Restaurar objetos desde el uso de S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) copias de seguridad de objetos o bucket de región replicada
* Nota: Actualmente no existe ninguna capacidad de «recuperar» para S3 y AWS no tiene la capacidad de recuperar los datos que se han eliminado. En la era actual del cumplimiento y las regulaciones del almacenamiento de datos, como el RGPD (https://gdpr-info.eu/) y la CCPA (https://oag.ca.gov/privacy/ccpa), Amazon S3 no puede seguir almacenando datos de clientes que se han eliminado explícitamente de la cuenta del cliente. Una vez eliminado un objeto, AWS ya no puede recuperarlo, independientemente de la rapidez con que se informe de la eliminación no deseada a AWS.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook para llevar a cabo el análisis forense de S3
- Como Respondedor de Incidentes, necesito una decisión empresarial sobre cuándo deben llevarse a cabo los análisis forenses de S3
- Como Respondedor de Incidentes, necesito tener habilitado el registro en todas las regiones habilitadas independientemente de la intención de uso

## Elementos atrasados actuales