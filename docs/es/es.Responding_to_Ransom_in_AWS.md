# Respuesta a los ataques de rescate dentro de AWS

## Avisos

Este documento se proporciona únicamente con fines informativos. Representa
las ofertas y prácticas de productos actuales de Amazon Web Services
(AWS) a partir de la fecha de emisión de este documento, que están sujetos a
cambiar sin previo aviso. Los clientes son responsables de crear los suyos propios
evaluación independiente de la información contenida en este documento y cualquier uso
de productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin
garantía de cualquier tipo, ya sea expresa o implícita. Este documento no
crear garantías, declaraciones, compromisos contractuales,
condiciones o garantías de AWS, sus filiales, proveedores o
licenciantes. Responsabilidades y responsabilidades de AWS ante sus clientes
están controlados por acuerdos de AWS y este documento no forma parte, ni
lo modifica, cualquier acuerdo entre AWS y sus clientes. \
\
© 2021, Amazon Web Services, Inc. o sus filiales. Todos los derechos
reservado.

## Ransomware

Desde el primer ataque de ransomware registrado en 1989 llamado PC Cyborg,
ransomware se ha convertido en una estrategia de monetización prominente utilizada por delincuentes
organizaciones y actores de amenazas en Internet. Otros ejemplos de
ransomware incluye:

- CryptoLocker\ [2014\]
- Petya\ [2016\]
- WannaCry\ [2017\]
- NotPetya\ [2017\]
- Ryuk\ [2019\])

Los actores de amenazas aprovechan los problemas y las debilidades en todos los clientes
infraestructura, explotación de endpoints vulnerables, servicios inseguros y
empleados de ingeniería social.

Los ataques de ransomware están costando a gobiernos, organizaciones sin fines de lucro y empresas
miles de millones de dólares e interrupción de las operaciones. NotPetya forzada
Maersk, gigante de envío, reinstalará 4.000 servidores y 45.000 PC para
\ $300 millones debido a una «interrupción grave del negocio». El ataque de ransomware contra
la ciudad de Baltimore costó más de 18 millones de dólares, y los gobiernos locales de
Riviera Beach y Lake City, Florida pagarán a los hackers\ $1M combinados para
recuperar sus sistemas y datos. Oficina Federal de Investigaciones de los Estados Unidos
(FBI) prevé que la amenaza del ransomware sea «más dirigida,
sofisticado y costoso» en un futuro próximo. Estas advertencias alcanzan
más allá de las fronteras estadounidenses, Europol también calificó al ransomware como el «más»
forma de ciberataque generalizado y perjudicial desde el punto de vista financiero».

## ¿Qué son los ataques de rescate?

Los ataques de rescate son una estrategia de monetización, no una tecnología específica.
Los ataques a menudo aprovechan software malicioso específico o ransomware, que
amenaza con publicar información de la víctima o bloquear el acceso hasta el rescate
se paga.

En teoría, si el rescate se paga dentro del tiempo asignado, los sistemas y
los datos se descifran y se ponen a disposición una vez más y las operaciones normales
continuar. Sin embargo, si no se cumple el rescate, las organizaciones corren el riesgo
destrucción permanente o filtraciones de datos dirigidas al público controladas por el
atacante. Los atacantes también pueden extorsionar más al cliente para que se libere de
datos y sistemas sensibles al cliente a terceros o al público.

## Al ransomware no le importa

Muchos ataques de rescate son de naturaleza oportunista, lo que significa que el ransomware
infecta indiscriminadamente cualquier red accesible a través de seres humanos y/o
vectores de máquina. Equipos de seguridad para instituciones educativas, estatales y
los gobiernos locales y las organizaciones sanitarias están intensificando las medidas
para mantener sus datos a salvo de un aumento de los ataques de ransomware. Amenaza
los actores entienden cómo identificar puntos débiles en las verticales de la industria. Muchos
organizaciones educativas y gubernamentales son más propensas al ransomware debido a
a una combinación de presupuestos reducidos, lagunas en los recursos de seguridad y
sistemas de TI heredados con vulnerabilidades conocidas sin parches. Del mismo modo,
los ataques de rescate pueden dirigirse a industrias con intolerancia al tiempo de inactividad, como
hospitales, con la esperanza de aumentar la probabilidad de pago.

## ¿Por qué son efectivos los ataques de rescate?

- El conocimiento de seguridad entre los empleados es bajo
- Las organizaciones no están haciendo copias de seguridad de los datos o no prueban las copias de seguridad existentes
- Los ataques requieren poca habilidad y dan lugar a pagos significativos
- Las organizaciones tardan en corregir vulnerabilidades y exposiciones comunes críticas (CVE)
- El personal técnico sobrecargado no puede abordar ni anticipar todas las brechas de seguridad
- Se utilizan varios vectores o canales en un solo ataque
- Es posible que el robo de los datos (filtración de datos, copia no autorizada) no detenga un proceso empresarial y los clientes no reaccionen hasta que algo lo haga, como cifrar o eliminar la información

## ¿Pagar o no pagar?

Existen debates activos entre los profesionales de la seguridad cibernética sobre el
decisión de pagar rescates. Muchos expertos, incluido el FBI, aconsejan
organizaciones para no pagar el rescate, argumentando que pagar no
garantizar que se pondrán a disposición sistemas bloqueados o datos cifrados
y solo seguirá motivando comportamientos nefastos. Aunque
el acceso al sistema y a los datos no están garantizados después de pagar el rescate, algunos
las organizaciones asumen un riesgo calculado para pagar con la esperanza de reanudar la normalidad
operaciones. Al hacerlo, esperan reducir los costos auxiliares potenciales
de ataques, incluida la pérdida de productividad, la disminución de los ingresos a lo largo del tiempo,
exposición de datos confidenciales y daños a la reputación. El ransomware
la amenaza es grave, pero la preparación inteligente y la vigilancia continua son
contrarrestan eficazmente. El blindaje completo de la seguridad de datos incluye
controles humanos y técnicos, pero existen características de AWS
Nube que ayuda a mitigar los ataques de ransomware. AWS se compromete a
proporcionarle herramientas, prácticas recomendadas y servicios para ayudar con la alta
disponibilidad, seguridad y resiliencia para hacer frente a los malos actores en el
internet.

## La protección de los sistemas y los datos en AWS es una [responsabilidad compartida] (https://aws.amazon.com/compliance/shared-responsibility-model)

Cuando implementa aplicaciones e infraestructura en la nube de AWS, AWS
ayuda compartiendo las responsabilidades de seguridad con usted. Ingenieros de AWS
la infraestructura cloud subyacente utilizando principios de diseño seguros y
los clientes deben implementar su propia arquitectura de seguridad para cargas de trabajo
implementado en AWS. AWS es responsable de proteger la infraestructura
que ejecuta todos los servicios ofrecidos en la nube de AWS. Esta infraestructura
está compuesto por el hardware, el software, las redes y las instalaciones que
ejecutar servicios en la nube de AWS. Los servicios en la nube de AWS que selecciona un cliente
determinar la cantidad de trabajo de configuración que el cliente debe realizar como
parte de sus responsabilidades de seguridad. Los clientes siempre están
responsable de gestionar y proteger sus datos, clasificar sus
activos y uso de AWS Identity Access Management (IAM) para aplicar el
permisos apropiados.

## AWS Support

Si usted o sus clientes creen que una cuenta está bajo un rescate activo
ataque, el cliente afectado debería crear un caso desde AWS Support
Centrarse desde la cuenta afectada. Si el usuario root no puede ser
al que se accede o se ve comprometido, el cliente debe abrir un nuevo AWS
y abra un ticket de soporte desde allí. Este proceso permite a AWS
realizar la verificación de identidad con el cliente y comenzar a interactuar
recursos internos para ayudar mejor a corregir el evento.

## AWS Security Reference Architecture (AWS SRA)

Arquitectura de referencia de seguridad [Amazon Web Services (AWS) (AWS)
SRA)] (https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
es un conjunto integral de directrices para implementar todo el complemento de AWS
servicios de seguridad en un entorno de varias cuentas. Se puede utilizar para ayudar
diseñar, implementar y administrar los servicios de seguridad de AWS para que se alineen
con las prácticas recomendadas de AWS.

## Agencia de Seguridad Cibernética e Infraestructura de los Estados Unidos

La [Guía de ransomware (Sept.
2020)] (https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
proporciona prácticas recomendadas y referencias para ayudar a gestionar el riesgo que plantea
ransomware y soporte coordinado y eficiente de una organización
respuesta a un incidente de ransomware.

## Agencia de la Unión Europea para la Ciberseguridad (ENISA)

[El panorama de amenazas de ENISA 2020 -
Ransomware] (https://www.enisa.europa.eu/publications/ransomware)
describe los hallazgos sobre el ransomware, proporciona una descripción y un análisis
del dominio y enumera los incidentes recientes relevantes. Una serie de propuestas
se proporcionan acciones de mitigación.

## Centro Australiano de Seguridad Cibernética (ACSC)

El ACSC proporciona varias guías, incluida la respuesta a [Ransomware en
Australia] (https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [RANSOMWARE
ATAQUES DE RESPUESTA DE EMERGENCIA
GUÍA] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf),
y [PREVENCIÓN Y PROTECCIÓN DE ATAQUES DE RANSOMWARE
GUÍA] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf).

## Marco de ciberseguridad del NIST

La guía, Marco de ciberseguridad del NIST (CSF): alineación con el CSF del NIST
en la nube de AWS, está diseñado para ayudar al sector público y comercial
entidades de cualquier tamaño y en cualquier parte del mundo se alinean con el MCA mediante
uso de servicios y recursos de AWS.

## Mitigación del ransomware: las 5 principales medidas de protección y preparación de la recuperación

AWS ha proporcionado un blog de seguridad pública que cubre [Mitigación del ransomware:
Las 5 mejores protecciones y preparación para la recuperación
acciones.] (https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
Estos incluyen:

- Configura la capacidad de recuperar tus aplicaciones y datos
- Cifrar los datos
- Aplicar parches críticos
- Siga un estándar de seguridad
- Asegúrese de que está monitoreando y automatizando las respuestas

También debe utilizar una autenticación segura para el acceso remoto expuesto.
servicios, en particular el protocolo de escritorio remoto y el shell seguro. [Multifactor
autenticación combinada con una sola
inicio de sesión] (https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
es un mecanismo técnico que permite la protección de las conexiones remotas
y solicitudes de recursos.

## Desarrollo, seguridad y operaciones (DevSecOps)

*Antecedentes *: El propósito de DevSecOps es ayudar a los clientes de forma segura
acelerar los bucles de comentarios con sus clientes (es decir, usuarios finales). Por
acelerando estos bucles de comentarios, los clientes pueden enviar más software
cambios en la producción con mayor calidad, seguridad y estabilidad. Por
incorporando seguridad en el proceso de desarrollo, los clientes pueden evitar
implementar vulnerabilidades a los usuarios finales en entornos de producción.
Según el NIST, el costo de reparar un defecto de seguridad una vez que está
hasta la producción puede ser hasta 60 veces más caro que durante
el ciclo de desarrollo
\ [[Fuente] (https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/)\].

Según los datos del equipo de respuesta a incidentes de clientes de AWS, la mayoría
los ataques de rescate de clientes se clasifican en una de las tres categorías: 1/ Un mal
actor escanea los repositorios de código fuente (por ejemplo, GitHub) en busca de claves de acceso de AWS.
Con estas claves de acceso a largo plazo, obtienen acceso a los recursos de AWS en
cuentas de clientes. Los riesgos aumentan según el nivel de acceso
permitidos por las claves de acceso. 2/ Escaneando depósitos públicos de S3 y
objetos, un actor defectuoso puede bloquear el acceso y cifrar los buckets de S3 evitando
los propietarios no accedan a los datos. 3/ Un mal actor escanea y aprovecha la web
vulnerabilidades de las aplicaciones (p. ej., inyección de código y sitios cruzados
secuencias de comandos) en puertos públicos para obtener acceso a los datos de los clientes.

Los clientes creen erróneamente que AWS proporciona muchas de estas salvaguardias
automáticamente como parte de tener una cuenta de AWS. Mientras que AWS proporciona
miles de funciones de seguridad, incluidas las capacidades privadas de forma predeterminada
(por ejemplo, grupos de seguridad de EC2 y buckets S3), como parte del [compartido
responsabilidad
modelo] (https://aws.amazon.com/compliance/shared-responsibility-model/),
los clientes deben detectar y corregir las vulnerabilidades de seguridad en su
código y/o en sus entornos de AWS. Existen servicios y herramientas de AWS
para detectar y corregir estos recursos no conformes, pero
los clientes deben tener conocimientos para aplicar las prácticas de forma automática
en todos sus recursos de AWS.

Una canalización de implementación rompe las acciones de compilación, prueba, implementación y lanzamiento
en una serie de etapas. Cada etapa proporciona una confianza cada vez mayor,
por lo general a costa de un tiempo extra. Las primeras etapas pueden encontrar la mayoría de los problemas
lo que proporciona una retroalimentación más rápida, mientras que las fases posteriores proporcionan más lenta y más
sondeo exhaustivo. Los constructores automatizan muchas de las acciones de estos
canalizaciones de implementación; las canalizaciones de implementación son una arquitectura clave
construct of [Continuous
Entrega] (https://aws.amazon.com/devops/continuous-delivery/). El
el trabajo de la canalización de despliegue es detectar cualquier cambio que provoque
problemas de producción. Estos pueden incluir rendimiento, seguridad o
problemas de usabilidad
\ [[Fuente] (https://martinfowler.com/bliki/DeploymentPipeline.html)\].
A efectos de este documento, nos referimos a la práctica de construir
pruebas de seguridad y registros en canalizaciones de implementación para evitar
y mitigar los ataques de rescate como seguridad continua.

Los principios del uso de la seguridad continua para prevenir y
mitigar los ataques de rescate son: 1/ Evitar que se cometan secretos en el origen
repositorios de código para que los actores defectuosos no tengan claves para acceder a AWS
recursos. 2/ Evite errores comunes del desarrollador que cometen la web
aplicaciones vulnerables (estáticas y en tiempo de ejecución). 3/ Asegúrese de que haya
copias de seguridad de recursos de AWS comprometidos para que los actores defectuosos no tengan
mucho que ganar al bloquear el acceso a los recursos 4. Cifrar todo
datos relevantes para que si un mal actor obtiene acceso a los datos, tenga
mucho menos para ganar a través de la extorsión. 5/ Detecte cuando usuarios no autorizados
acceder a los recursos de AWS para que se puedan llevar a cabo mitigaciones 6. Asegurar IAM
las claves de acceso son de corta duración utilizando roles de IAM, por lo que el acceso es limitado
duración de los recursos comprometidos 7. Asegúrese de que los permisos de IAM usen menos
privilegio para evitar que ocurran cosas realmente malas si es un mal actor
obtiene acceso a las claves. 8/ Prevenir o detectar errores de configuración que
hacer vulnerables los recursos de AWS.

*Evitar y detectar al confirmar secretos en los repositorios de código fuente***: **
Desde sus entornos de desarrollador (por ejemplo, sus IDE), los clientes deberían
ejecutar secretos detección herramientas de pruebas de seguridad de aplicaciones estáticas (SAST)
antes de confirmar código en un repositorio de código fuente. Además, la primera
etapa de una canalización de implementación automatizada ejecuta una detección de secretos SAST
herramienta para detectar cuándo se han confirmado secretos en el repositorio de código fuente.
Al descubrir secretos, configure la canalización para que falle, notifíquelo
desarrolladores y detenga las futuras confirmaciones hasta que se aplique una corrección. Además,
ejecutar automatización para limpiar el historial de git de los secretos y deshabilitar AWS
claves de acceso. Las herramientas de ejemplo para esta solución son AWS CodePipeline, AWS
CodeBuild, git-secrets y código personalizado para corregir secretos.

*Prevenir y detectar errores comunes del desarrollador que producen aplicaciones
vulnerable***: ** Desde sus entornos de desarrollador (por ejemplo, sus IDE),
los clientes deben ejecutar SAST y herramientas de revisión de código que detecten la web
vulnerabilidades tales como inyección de código, secuencias de comandos entre sitios y otros
prácticas de codificación inseguras. Además, la primera etapa de un sistema automatizado
pipeline de implementación ejecuta estas herramientas para detectar cuándo hay vulnerabilidades web
están comprometidos con el repositorio de código fuente. Al descubrir esta web
vulnerabilidades, configurar la canalización para que falle, notifique a los desarrolladores y
detener futuras confirmaciones hasta aplicar una solución. Además, los clientes pueden
configurar la detección de tiempo de ejecución y la corrección de estas vulnerabilidades web
como parte de una canalización de implementación de servicios compartidos que aprovisiona y
configura estos servicios como código. Las herramientas de ejemplo para esta solución son:
Amazon CodeGuru, AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
Sonarqube/CheckMarx y AWS WAF y WAF Security Automations.

*Copias de seguridad de recursos de AWS comprometidos: * En tiempo de ejecución, detecte sin etiquetar
y/o recursos de AWS relevantes que no tienen copias de seguridad. Además,
los clientes pueden configurar el aprovisionamiento de los recursos de AWS que proporcionan
detección de tiempo de ejecución como parte de una canalización de implementación de servicios compartidos que
provisione y configure estos servicios como código. Herramientas de ejemplo para esto
son AWS Backup, AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Normas] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(p. ej., required-tags, redshift-backup-enabled,
aurora-resources-protected-by-backup-plan,
backup-plan-min-frequency-and-min-retention-check,
backup-recovery-point-manual-deletion-disabled,
db-instance-backup-enabled, dynamodb-in-backup-plan,
ebs-in-backup-plan), Amazon SNS, and -.

*Cifrado de datos en tránsito y en reposo***: ** De su desarrollador
entornos (por ejemplo, sus IDE), los clientes deben ejecutar herramientas SAST que
detectar vulnerabilidades web como la inyección de código y entre sitios
secuencias de comandos. Además, la primera etapa de una implementación automatizada
pipeline ejecuta una herramienta de linting iAC para detectar cuándo definen los desarrolladores
recursos de AWS sin cifrar como código y confirmar el código de configuración a
el repositorio de código fuente. Al descubrir estos recursos no cifrados,
configurar la canalización para que falle, notifique a los desarrolladores y detenga el futuro
se compromete hasta que se aplique una corrección. Además, los clientes pueden configurar el
aprovisionamiento de recursos de AWS que proporcionan detección en tiempo de ejecución y
corrección de estos recursos de AWS no cifrados como parte de un recurso compartido
canalización de implementación de servicios que aprovisiona y configura estos
servicios como código. Las herramientas de ejemplo para esta solución son AWS
CloudFormation, AWS CodePipeline, AWS CloudFormation Guard, AWS
CodeBuild, Amazon EventBridge, [AWS Config
Normas] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(por ejemplo, ec2-ebs-encryption-by-default, dynamodb-table-encrypted-kms,
rds-snapshot-encrypted, encriptado cloudwatch-log-group-encrypted,
cloud-trail-encryption-enabled,
s3-bucket-server-side-encryption-enabled, api-gw-ssl-enabled,
cloudfront-custom-ssl-certificate), Amazon SNS, AWS KMS y AWS Lambda.

*Detectar cuándo usuarios no autorizados acceden a los recursos de AWS: * En tiempo de ejecución,
detectar la exfiltración de datos\ "picos\» (p. ej., mediante métricas de CloudWatch
específicamente
[NetworkOut] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)).
Por ejemplo, es posible que un atacante haya realizado la destrucción de datos y
dejó una nota de rescate y, en estos casos, no hay oportunidad de obtener datos
recuperación trabajando con el actor malicioso. Lo que es más, clientes
puede configurar el aprovisionamiento de recursos de AWS que proporcionan tiempo de ejecución
detección como parte de una canalización de implementación de servicios compartidos que
provisione y configure estos servicios como código. Herramientas de ejemplo para esto
son AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, Amazon
EventBridge, Amazon GuardDuty, Amazon SNS y Amazon CloudWatch
métricas.

*Asegúrese de que las claves de acceso de IAM***: ** En tiempo de ejecución, ejecute herramientas que
detectar varios estados de claves de acceso y políticas de IAM, incluida la rotación
claves de acceso de larga duración. Los clientes pueden configurar el aprovisionamiento de AWS
recursos que proporcionan detección de tiempo de ejecución como parte de un servicio compartido
canalización de implementación que aprovisiona y configura estos servicios como
código. Las herramientas de ejemplo para esta solución son AWS CloudFormation, AWS
CodePipeline, AWS CodeBuild, Amazon EventBridge, [AWS Config
Normas] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
(por ejemplo, iam-policy-no-statements-with-admin-access,
iam-policy-no-statements-with-full-access, iam-root-access-key-check,
iam-user-no-policies-check, iam-user-unused-credentials-check,
access-keys-rotated) y AWS Lambda.

*Asegúrese de que los permisos de IAM utilicen los mínimos privilegios***: ** De los desarrolladores»
entornos (por ejemplo, sus IDE), los clientes deben ejecutar herramientas SAST que
detectar definiciones de permisos excesivamente permisivas antes de comprometerse
código a un repositorio de código fuente. Además, la primera etapa de un sistema automatizado
pipeline de implementación ejecuta la herramienta SAST para detectar excesivamente permisiva
definiciones de políticas después de que el código se haya confirmado en un código fuente
repositorio. Al descubrir definiciones de políticas excesivamente permisivas, configure
la canalización falle, notifique a los desarrolladores y detenga futuras confirmaciones hasta
aplicando una solución. Además, detecte políticas excesivamente permisivas en toda la
Cuenta o cuentas de AWS (es decir, fuera de los repositorios de código fuente). A su vez,
los clientes pueden configurar el aprovisionamiento de los recursos de AWS que proporcionan
detección de tiempo de ejecución como parte de una canalización de implementación de servicios compartidos que
provisione y configure estos servicios como código. Herramientas de ejemplo para esto
son AWS CloudFormation, AWS CodePipeline, AWS CodeBuild,
PMapper/AWSPX, AWS IAM Access Analyzer, Amazon EventBridge, AWS Config
Reglas y AWS Lambda.

*Evitar la configuración errónea que hace que los recursos de AWS sean vulnerables *: El
la configuración errónea de determinados recursos de AWS o no habilitar puede
clientes vulnerables a un ataque de rescate. Los recursos más notables son
los relacionados con redes, informática, almacenamiento y bases de datos. Qué es
más, los clientes pueden configurar el aprovisionamiento de recursos de AWS que
proporcionar detección de tiempo de ejecución como parte de una implementación de servicios compartidos
canalizar esas provisiones y configurar estos servicios como código. Ejemplo
herramientas para esta solución son AWS CloudFormation, AWS CodePipeline, AWS
CodeBuild, Amazon EventBridge, AWS Config Rules (p. ej.,
s3-account-level-public-access-blocks s3-cuenta-, s3-bucket-default-lock-enabled,
s3-bucket-level-public-access-prohibited, s3-bucket-logging-enabled,
s3-bucket-policy-not-more-permissive, s3-bucket-public-read-prohibited,
s3-bucket-public-write-prohibited, s3-bucket-replication-enabled,
cloud-trail-log-file-validation-enabled,
ec2-security-group-attached-to-eni, vpc-default-security-group-closed,
y vpc-flow-logs-enabled, habilitados guardduty-enabled-centralized), Amazon
Accesibilidad de red del inspector, Amazon VPC Reachability Analyzer, Amazon
S3 y AWS Lambda.

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Microsoft Windows

### Identificar
- Evaluar la postura de seguridad de la cuenta para identificar y corregir las brechas de seguridad
- AWS desarrolló una nueva herramienta de código abierto [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de sus AWS cuenta.
- Mantenga un inventario completo de activos de todos los recursos, incluidos los controladores de dominio, las instancias EC2 de Microsoft Windows, los servidores y bases de datos de Microsoft Windows y cualquier integración con proveedores de identidad externos.
- Realice análisis de vulnerabilidades recurrentes de sus hosts mediante utilidades tales como [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
- Para proteger su organización, Microsoft recomienda que utilice la información del [**Plan de proyecto de mitigación de ransomware operado por humanos**] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) presentación de PowerPoint, que incluye [seguridad acceso privilegiado] (https://aka.ms/spa).
- Utilice [métricas de CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), específicamente NetworkPacketsOut, para buscar «picos» de exfiltración de datos. Es posible que un atacante haya realizado la destrucción de datos y haya dejado una nota de rescate, y en estos casos, no hay oportunidad de recuperación de datos trabajando con el actor malintencionado.

### Proteger
- Aplique las últimas actualizaciones a sus sistemas operativos y aplicaciones
- Haga una copia de seguridad de los archivos con Historial de archivos si el fabricante de su PC aún no lo ha encendido. [Más información sobre el historial de archivos] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
- Realice copias de seguridad de los archivos importantes regularmente. Usa la regla 3-2-1. Mantenga tres copias de seguridad de sus datos, en dos tipos de almacenamiento diferentes y al menos una copia de seguridad fuera del sitio
- Bloquear tipos de archivos ransomware conocidos
- [Bloquear macros en documentos de Office] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
- Educar a sus empleados para que puedan identificar los ataques de ingeniería social y phishing con lanza
- [Siga las prácticas recomendadas para proteger los servicios de Active Directory] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
- [Siga las 10 configuraciones de directivas de grupo más importantes para prevenir las infracciones de seguridad] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
- [Implementar acceso controlado a carpetas] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders). Puede evitar que el ransomware encripte los archivos y retenga los archivos para obtener rescate
- Realizar copias de seguridad de instancias EC2
- Considere utilizar [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) o [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
- El Microsoft 365 Defender Threat Intelligence Team ha proporcionado [informe completo] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) que identifica acciones para proteger los recursos de Microsoft antes de un evento de ransomware
- Refuerce los activos orientados a Internet y asegúrese de que tienen las últimas actualizaciones de seguridad. Utilice la administración de amenazas y vulnerabilidades para auditar estos activos regularmente en busca de vulnerabilidades, configuraciones erróneas y actividades sospechosas.
- Supervisar los intentos de fuerza bruta. Comprobar intentos de autenticación fallidos excesivos (ID de evento de seguridad de Windows 4625
- Supervisar para borrar los registros de sucesos, especialmente el registro de sucesos de seguridad y los registros operativos de PowerShell. Microsoft Defender ATP genera la alerta «El Event log was cleared» y Windows genera un ID de evento 1102 cuando esto ocurre
- Practicar el principio del mínimo privilegio y mantener la higiene de las credenciales. Evite el uso de cuentas de servicio de todo el dominio a nivel de administrador. Aplicar contraseñas de administrador local sólidas aleatorizadas y justo a tiempo. Usar herramientas como LAPS
- Puerta de enlace segura de Escritorio remoto mediante soluciones como Azure Multi-Factor Authentication (MFA). Si no tiene una puerta de enlace MFA, habilite la autenticación a nivel de red (NLA)
- Activa AMSI para Office VBA si tienes Office 365
- Activa las reglas de reducción de superficies de ataque, incluidas las reglas que bloquean el robo de credenciales, la actividad de ransomware y el uso sospechoso de PsExec y WMI. Para abordar la actividad maliciosa iniciada a través de documentos de Office armados, utilice reglas que bloqueen la actividad macro avanzada, el contenido ejecutable, la creación de procesos y la inyección de procesos iniciadas por las aplicaciones de Office. Para evaluar el impacto de estas reglas, desplegarlas en modo auditoría
- Activa la protección proporcionada en la nube y el envío automático de muestras en Windows Defender Antivirus. Estas capacidades utilizan inteligencia artificial y aprendizaje automático para identificar y detener rápidamente amenazas nuevas y desconocidas.
- Activa las funciones de protección contra manipulaciones para evitar que los atacantes detengan los servicios de seguridad
- Utilice el Windows Defender Firewall y su sistema de seguridad de red para evitar la comunicación RPC y SMB entre los endpoints siempre que sea posible. Esto limita el movimiento lateral así como otras actividades de ataque.
- Activar y actualizar Windows Defender Antivirus: se prefieren las soluciones comerciales de detección y respuesta de Endpoint (EDR) de pago por suscripción
- Activa [Acceso controlado a carpetas] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) en Windows 10 para proteger tus carpetas locales importantes de programas no autorizados como ransomware u otro malware
- Utilice [Systems Manager y Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) para comprobar si las instancias EC2 que ejecutan Microsoft Windows contienen vulnerabilidades y exposiciones comunes (CVE)
- Verifique sus copias de seguridad y asegúrese de que la infección no se haya propagado a ellos

### Detectar
- El Microsoft 365 Defender Threat Intelligence Team ha proporcionado un [informe completo] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) que identifica las cosas que debe buscar frente a múltiples variantes de ransomware
- Utilice VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
- Puede [investigar el flujo de VPC con Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) o [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Responder
- Aplicar NACL basados en IOC de red para evitar más tráfico
- Aislar los sistemas infectados
- Inspeccionar las copias de seguridad para detectar posibles infecciones
- Elimine los sistemas comprometidos de la red.
- Seguir los requisitos reglamentarios o la política interna de la empresa para determinar si se requiere análisis forense de la instancia EC2
- Si se requiere análisis forense de las instancias O deben recuperarse los datos, [poner en cuarentena la instancia EC2] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)
- [Eliminar los metadatos del controlador de dominio comprometidos del dominio] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
- Utilice los registros de flujo para determinar la IP de exfiltración, buscar cualquier otra instancia que se comunique con las IP de que los datos se han extraído a/acceso desde CloudTrail

### Recuperar
- Se recomienda no pagar el rescate
- Pagar el rescate es una apuesta en cuanto a si el delincuente cumplirá la transacción después de recibir el pago
- Si no existen copias de seguridad de datos, debe realizar un análisis de costes y beneficios y sopesar el valor del compromiso de datos/reputación frente al pago al atacante
- Usted permite directamente al atacante continuar sus operaciones contra su empresa u otras personas si decide pagar el rescate
- Cree nuevas instancias EC2 a partir de una AMI de confianza
- Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar las cargas de trabajo en AWS
- Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restaurarlas desde el último evento programado anterior al evento de ransomware
- Visita < https://www.nomoreransom.org/ > para identificar si hay un descifrado disponible para la variante de malware, los datos infectados

## Amazon Elastic Compute Cloud (*Amazon EC2*) - Unix/Linux

### Identificar
- Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
- AWS desarrolló una nueva herramienta de código abierto [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener rápidamente información valiosa sobre la postura de seguridad de su cuenta de AWS.
- Mantenga un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo
- Realice análisis de vulnerabilidades recurrentes de sus hosts mediante utilidades tales como [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/)
- Utilice [métricas de CloudWatch] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html), específicamente NetworkOut, para buscar «picos» de exfiltración de datos. Es posible que un atacante haya realizado la destrucción de datos y haya dejado una nota de rescate, y en estos casos, no hay oportunidad de recuperación de datos trabajando con el actor malintencionado.

### Proteger
- Aplique las últimas actualizaciones a sus sistemas operativos y aplicaciones
- Realice copias de seguridad de los archivos importantes regularmente. Usa la regla 3-2-1. Mantenga tres copias de seguridad de sus datos, en dos tipos de almacenamiento diferentes y al menos una copia de seguridad fuera del sitio
- Bloquear tipos de archivos ransomware conocidos
- Considere utilizar AWS Config para crear reglas para supervisar los cambios en los servicios de AWS. AWS Config tiene varias reglas administradas prediseñadas para supervisar EBS y EC2, entre ellas:
- [ebs-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
- [ebs-optimized-instance ebs-] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
- [ebs-snapshot-public-restorable-check] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
- [ec2-ebs-encryption-by-default] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
- [ec2-imdsv2-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
- [ec2-instance-detailed-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
- [ec2-instance-managed-by-systems-manager] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
- [ec2-instance-multiple-eni-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
- [ec2-instance-no-public-ip] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
- [ec2-instance-profile-attached] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
- [ec2-managedinstance-applications-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
- [ec2-managedinstance-applications-required] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
- [ec2-managedinstance-association-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
- [ec2-managedinstance-inventory-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
- [ec2-managedinstance-patch-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
- [ec2-managedinstance-platform-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
- [ec2-security-group-attached-to-eni] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
- [ec2-stopped-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
- [ec2-volume-inuse-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
- Educar a sus empleados para que puedan identificar los ataques de ingeniería social y phishing con lanza
- Tener habilitados el antivirus y la detección de malware en todos los sistemas; se prefieren las soluciones comerciales de detección y respuesta de Endpoint (EDR) de pago por suscripción.
- Puede encontrar una guía de ejemplo en [Cómo instalar y usar Linux Malware Detect (LMD) con ClamAV como motor antivirus] (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/)
- Realizar copias de seguridad de instancias EC2
- Considere utilizar [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) o [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/)
- El Microsoft 365 Defender Threat Intelligence Team ha proporcionado un [informe completo] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) que identifica acciones para proteger los recursos de Microsoft antes de un evento de ransomware. Muchas de estas recomendaciones se pueden adaptar para Unix y Linux, entre ellas:
- Determine dónde inician sesión las cuentas con alto privilegio y exponen las credenciales.
- Refuerce los activos orientados a Internet y asegúrese de que tienen las últimas actualizaciones de seguridad. Utilice la administración de amenazas y vulnerabilidades para auditar estos activos regularmente en busca de vulnerabilidades, configuraciones erróneas y actividades sospechosas.
- Supervisar los intentos de fuerza bruta. Comprobar los intentos de autenticación fallidos excesivos (/var/log/auth.log o /var/log/secure) /var/log/auth.log
- Supervisar para borrar los registros de eventos
- Practicar el principio del mínimo privilegio y mantener la higiene de las credenciales. Evite el uso de cuentas de servicio de todo el dominio a nivel de administrador. Aplicar contraseñas de administrador local sólidas aleatorizadas y justo a tiempo.
- Activa las funciones de protección contra manipulaciones para evitar que los atacantes detengan los servicios de seguridad
- Utilice un agente de seguridad de endpoints como [Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html)
- Utilice un monitor de integridad de archivos como [Tripwire] (https://github.com/Tripwire/tripwire-open-source) para detectar cambios en los archivos críticos
- Utilice [Systems Manager y Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) para comprobar si las instancias EC2 contienen vulnerabilidades y exposiciones comunes (CVE)
- Verifique sus copias de seguridad y asegúrese de que la infección no se haya propagado a ellos

### Detectar
- La tabla 1 de [No Strings on Me: Linux and Ransomware] (https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870), de Richard Horne, identifica varios indicadores que hay que supervisar, entre ellos:
- Un nuevo proceso que nunca se había visto en el endpoint anterior que tiene solicitudes de conectividad de red externa
- Intento de comunicación con nombres DNS donde se encuentra entropía o directamente con IP desnudas
- Cambio en los repositorios de distribución Yum o Apt
- Creación de archivos.sh dentro de directorios domésticos no basados en usuarios
- Enumeración de árboles de directorios de almacenamiento de archivos, bases de datos o web compartidos
- Escalamiento de privilegios o intentos de obtener acceso sudo
- Comunicaciones de red externas e internas
- Nombres de archivo generados con entropía o que se encuentran en rápida sucesión
- Los archivos cambian de nombre varias veces en el mismo árbol de directorios
- Archivos creados con extensiones de archivo extrañas
- Se presta atención a los valores atípicos y los eventos que quedan fuera de las operaciones cotidianas típicas
- Elevado uso de memoria durante periodos de tiempo cortos o prolongados
- Gran esfuerzo de proceso en el que el proceso principal finaliza antes de los procesos secundarios
- Modificación de las opciones de arranque
- Varias copias del mismo archivo en varios directorios
- Múltiples modificaciones dentro de un único directorio
- Posibles llamadas para bibliotecas de cifrado
- Posibilidad de creación y ejecución de procesos dentro del directorio/tmp
- Enumeración de directorios rápida y sucesiva
- Eliminación de archivos de directorios mediante comodines o sin confirmación
- El uso de chmod con comodines (o chmod 777)
- El uso de cmds wget o curl
- Uso de chmod cmd para cambiar los archivos ejecutables a derechos excesivamente permisivos
- Uso de las cadenas cmd para intentar codificar las comunicaciones antes o durante la infección
- Utilice VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
- Puede [investigar el flujo de VPC con Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) o [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### Responder
- Inspeccionar las copias de seguridad para detectar posibles infecciones
- Aislar los sistemas infectados
- Elimine los sistemas comprometidos de la red.
- Siga los requisitos reglamentarios o la política interna de la empresa para determinar si se requiere análisis forense de la instancia EC2
- Si se requiere análisis forense de las instancias O deben recuperarse los datos, [poner en cuarentena la instancia EC2] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)

### Recuperar
- Se recomienda no pagar el rescate
- Pagar el rescate es una apuesta en cuanto a si el delincuente cumplirá la transacción después de recibir el pago
- Si no existen copias de seguridad de datos, debe realizar un análisis de costes y beneficios y sopesar el valor del compromiso de datos/reputación frente al pago al atacante
- Usted permite directamente al atacante continuar sus operaciones contra su empresa u otras personas si decide pagar el rescate
- Cree nuevas instancias EC2 a partir de una AMI de confianza
- Seguir los requisitos reglamentarios o la política interna de la empresa para determinar si se requiere análisis forense de la instancia EC2
- Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar las cargas de trabajo en AWS
- Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restaurarlas desde el último evento programado anterior al evento de ransomware
- Visita < https://www.nomoreransom.org/ > para identificar si hay un descifrado disponible para la variante de malware, los datos infectados

## Amazon Simple Storage Service (S3)

### Identificar
- Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
- AWS desarrolló una nueva herramienta de código abierto [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de sus AWS cuenta.
- Considere la posibilidad de implementar [AWS GuardDuty] (https://aws.amazon.com/guardduty/) para supervisar continuamente la actividad maliciosa y el comportamiento no autorizado a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon S3
- Considere utilizar [AWS Config] (https://aws.amazon.com/config/), que es un servicio que le permite evaluar, auditar y evaluar las configuraciones de sus recursos de AWS
- Darkbit proporciona un ejemplo de [AWS S3 Data Loss Prevention] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) que se puede utilizar para identificar la copia no autorizada de objetos. También se podrían añadir otras operaciones específicas al patrón en función de los casos de uso de su empresa
- Mantenga un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo

### Proteger
- Considere [habilitar la replicación] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html): misma región o entre regiones. La replicación entre regiones protege los datos contra defectos de las aplicaciones y errores del operador al mantener una segunda copia en otra región
- Considere utilizar AWS Config para crear reglas para supervisar los cambios en los servicios de AWS. AWS Config tiene varias reglas administradas prediseñadas para supervisar EBS y EC2, entre ellas:
- [s3-account-level-public-access-blocks] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html)
- [s3-cuenta-nivel-public-acceso-bloqueos-periódico-] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
- [s3-bucket-blacklisted-actions-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
- [s3-bucket-default-lock-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
- [s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
- [s3-bucket-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
- [s3-bucket-policy-grantee-check policy-concesionario-cheque] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
- [s3-bucket-policy-not-more-permissive] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
- [s3-bucket-public-read-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
- [s3-bucket-public-write-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
- [s3-bucket-replication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
- [s3-bucket-server-side-encryption-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
- [s3-bucket-ssl-requests-only] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
- [s3-bucket-versioning-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
- [s3-default-encryption-kms] (https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
- Considere utilizar las claves [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) para cifrar todos los objetos e impedir que un atacante aplique su clave de cifrado
- Considere utilizar [AWS S3 Block Public Access Feature] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) para mitigar la exposición involuntaria de objetos
- Considere utilizar [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) para copias de seguridad de objetos y optimización de costes
- Considere utilizar [S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) para poder almacenar objetos utilizando un modelo *escritura una lectura* muchos* (WORM). Object Lock puede ayudar a evitar que los objetos se eliminen o sobrescriban durante un tiempo fijo o indefinidamente.
- En el modo de cumplimiento de bloqueo de objeto**, ** ningún usuario puede sobrescribir ni eliminar una versión de objeto protegido, incluido el usuario raíz de su cuenta de AWS. Cuando un objeto está bloqueado en modo de conformidad, su modo de retención no se puede cambiar y su período de retención no se puede acortar. El modo de cumplimiento ***garantiza*** que una versión de objeto no se puede sobrescribir ni eliminar durante el período de retención.
- Habilite el [registro de eventos de CloudTrail para depósitos y objetos de S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) que contiene información confidencial o crítica. De forma predeterminada, las pistas de CloudTrail no registran eventos de datos, pero puede configurar pistas para registrar los eventos de datos de los buckets de S3 que especifique o para registrar eventos de datos para todos los buckets de Amazon S3 de su cuenta de AWS.
- Habilite el registro de nivel de servidor de CloudTrail para buckets S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) y objetos que contienen información confidencial o crítica. El registro de acceso al servidor proporciona registros detallados de las solicitudes que se realizan a un bucket
- Habilitar [S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) para permitir la restauración de objetos modificados
- Para establecer el número de versiones conservadas, [establece una política de ciclo de vida] (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) para aplicarlas a versiones no actuales
- Habilitar [S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) para evitar que un atacante deshabilite el control de versiones y sobrescriba todos los objetos de un bucket
- Aplicar la autenticación multifactor (MFA)
- Cumplir los requisitos de complejidad de las contraseñas y establecer periodos de
- Implementar [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html), incluida la caducidad de las cuentas y las rotaciones de credenciales obligatorias
- Ejecute un [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) para enumerar todos los usuarios de su cuenta y el estado de sus diversas credenciales, incluidas contraseñas, claves de acceso y dispositivos MFA
- Tomar medidas para [evitar que cualquier nueva credencial se exponga públicamente] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
- Utilice [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) para identificar los recursos de su organización y cuentas, como los depósitos de Amazon S3 o las funciones de IAM que se comparten con una entidad externa. Esto le permite identificar el acceso no deseado a sus recursos y datos, lo que supone un riesgo para la seguridad
- Utilizar roles de IAM para administrar los permisos
- Implemente privilegios mínimos y no permita permisos s3:\ *
- Utilizar y auditar rutinariamente las políticas de bucket
- Haz público solo lo que necesita ser y asegúrate de que todos los objetos estén protegidos por ser privados
- Aunque no podemos recomendar soluciones específicas de terceros, también existen productos de nuestros socios, incluidos Veritas y CommVault, y otros proveedores de software de backup que tienen la capacidad nativa de hacer copias de seguridad de objetos en S3 en sus ubicaciones de almacenamiento de backup administrado dentro o fuera de S3

### Detectar

- Compruebe su registro de CloudTrail en busca de actividades no autorizadas, como la creación de usuarios, políticas, roles o credenciales de seguridad temporales de IAM no autorizados
- Compruebe su registro de CloudTrail para revisar su cuenta de AWS en busca de cualquier uso no autorizado de AWS, como instancias EC2 no autorizadas, funciones Lambda o pujas puntuales de EC2. También puede comprobar el uso iniciando sesión en la consola de administración de AWS y revisando cada página de servicio. La página\ "Facturas\» de la consola de facturación también se puede comprobar para detectar un uso inesperado
- Tenga en cuenta que el uso no autorizado puede producirse en cualquier región y que la consola puede mostrarle solo una región a la vez. Para cambiar entre regiones, puedes utilizar el menú desplegable en la esquina superior derecha de la pantalla de la consola
- Nota de rescate proporcionada como objeto dentro del depósito o por correo electrónico al cliente
- Los objetos S3 se eliminan o se eliminan cubos S3 completos
- Nota: Con los eventos de destrucción de datos, se puede proporcionar una nota de rescate o no. Asegúrese también de comprobar las métricas de CloudWatch y los eventos de CloudTrail S3 para verificar si se produjo o no la exfiltración de datos para delimitar entre un ataque de rescate o destrucción de datos
- Los objetos S3 se cifran mediante una clave de una cuenta que no es propiedad del cliente

### Responder
- Eliminar o rotar [Claves de usuario de IAM] (https://console.aws.amazon.com/iam/home#users) y [Claves de usuario Root] (https://console.aws.amazon.com/iam/home#security_credential); puede que desee rotar todas las claves de su cuenta si no puede identificar una clave o claves específicas que se hayan expuesto
- Eliminar [Usuarios de IAM] no autorizados (https://console.aws.amazon.com/iam/home#users.)
- Eliminar [políticas] no autorizadas (https://console.aws.amazon.com/iam/home#/policies)
- Eliminar [roles] no autorizados (https://console.aws.amazon.com/iam/home#/roles)
- Si su aplicación utiliza claves de acceso expuestas, debe sustituirla. Para reemplazar la clave, cree primero una segunda clave (en ese momento, ambas claves estarán activas) y, a continuación, modifique la aplicación para utilizar la nueva clave. A continuación, deshabilite (pero no elimine) las claves expuestas haciendo clic en la opción «Hacer inactivo» de la consola. Si hay algún problema con la aplicación, puede reactivar las claves expuestas. Cuando su aplicación esté completamente funcional con la nueva clave, elimine las claves expuestas
- [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM. *NOTA: * La eliminación de usuarios de IAM puede afectar las cargas de trabajo de producción y debe realizarse con cuidado

### Recuperar
- Se recomienda no pagar el rescate
- Pagar el rescate es una apuesta en cuanto a si el delincuente cumplirá la transacción después de recibir el pago
- Si no existen copias de seguridad de datos, debe realizar un análisis de costes y beneficios y sopesar el valor del compromiso de datos/reputación frente al pago al atacante
- Usted permite directamente al atacante continuar sus operaciones contra su empresa u otras personas si decide pagar el rescate
- Implementar procedimientos en Proteger antes de intentar restaurar datos u objetos
- Eliminar [eliminar marcadores] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) para objetos versionados
- Vuelve a crear depósitos eliminados
- Restaurar objetos desde el uso de copias de seguridad de objetos de [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) o bucket de región replicada
- *Nota: * Actualmente no existe ninguna capacidad de\ "undelete\» para S3 y AWS no tiene la capacidad de recuperar los datos que se han eliminado. En la era actual de cumplimiento y regulaciones del almacenamiento de datos como [RGPD] (https://gdpr-info.eu/) y [CCPA] (https://oag.ca.gov/privacy/ccpa), Amazon S3 no puede seguir almacenando datos de clientes eliminados explícitamente de la cuenta del cliente. Una vez eliminado un objeto, AWS ya no puede recuperarlo, independientemente de la rapidez con que se informe de la eliminación no deseada a AWS.

## Servicio de bases de datos relacionales (RDS)

### Identificar
- Evalúe su postura de seguridad para identificar y corregir las brechas de seguridad
- AWS desarrolló una nueva herramienta de código abierto [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) que proporciona a los clientes una evaluación puntual para obtener información valiosa sobre la postura de seguridad de sus AWS cuenta.
- Considere utilizar [AWS Config] (https://aws.amazon.com/config/), que es un servicio que le permite evaluar, auditar y evaluar las configuraciones de sus recursos de AWS
- Considere la posibilidad de implementar [AWS GuardDuty] (https://aws.amazon.com/guardduty/) para supervisar continuamente la actividad maliciosa y el comportamiento no autorizado a fin de proteger sus cuentas, cargas de trabajo y datos de AWS almacenados en Amazon RDS
- Mantenga un inventario completo de activos de todos los recursos, incluidos servidores, dispositivos de red, recursos compartidos de red/archivos y máquinas de desarrollo

### Proteger
- Hay referencias y pasos adicionales disponibles en [Seguridad en Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)
- Considere utilizar AWS Config para crear reglas para supervisar los cambios en los servicios de AWS. AWS Config tiene varias reglas administradas prediseñadas para supervisar RDS, entre ellas:
- [rds-automatic-minor-version-upgrade-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
- [rds-cluster-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
- [rds-cluster-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
- [rds-cluster-multi-az-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
- [rds-enhanced-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
- [rds-instance-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
- [rds-instance-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
- [rds-instance-public-access-check] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
- [rds-in-backup-plan] (https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html)
- [rds-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
- [rds-multi-az-support] (https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
- [rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
- [rds-snapshot-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
- [rds-storage-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
- Disponer de una solución de sistema de detección de intrusiones (HIDS) basado en host de terceros (como OSSEC, Tripwire, Wazuh, [Amazon Inspector] (https://aws.amazon.com/inspector/))
- Ejecute su instancia de base de datos en una nube privada virtual (VPC) basada en el servicio Amazon VPC para obtener el mayor control posible de acceso a la red. Para obtener más información sobre cómo crear una instancia de base de datos en una VPC, consulte [Amazon Virtual Private Cloud VPC y Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html).
- Utilice las políticas de AWS Identity and Access Management (IAM) para asignar permisos que determinan quién puede administrar los recursos de Amazon RDS. Por ejemplo, puede utilizar IAM para determinar quién puede crear, describir, modificar y eliminar instancias de base de datos, etiquetar recursos o modificar grupos de seguridad.
- Utilice el cifrado de Amazon RDS para proteger sus instancias de base de datos e instantáneas en reposo. El cifrado de Amazon RDS utiliza el algoritmo de cifrado AES-256 estándar del sector para cifrar los datos en el servidor que aloja la instancia de base de datos. Para obtener más información, consulte [Cifrado de recursos de Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html)
- Utilice conexiones Secure Socket Layer (SSL) o Transport Layer Security (TLS) con instancias de base de datos que ejecutan los motores de bases de datos MySQL, MariaDB, PostgreSQL, Oracle o Microsoft SQL Server. Para obtener más información sobre el uso de SSL/TLS con una instancia de base de datos, consulte [Uso de SSL/TLS para cifrar una conexión a una instancia de base de datos] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
- Utilice grupos de seguridad para controlar qué direcciones IP o instancias de Amazon EC2 se pueden conectar a las bases de datos de una instancia de base de datos. Cuando crea por primera vez una instancia de base de datos, su sistema de seguridad impide el acceso a la base de datos excepto mediante reglas especificadas por un grupo de seguridad asociado.
- Utilizar cifrado de red y cifrado de datos transparente con instancias de base de datos Oracle; para obtener más información, consulte [Encriptación de red nativa de Oracle] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) y [Oracle Transparent Data Encryption] ( https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
- Utilice las funciones de seguridad de su motor de base de datos para controlar quién puede iniciar sesión en las bases de datos de una instancia de base de datos. Estas características funcionan como si la base de datos estuviera en la red local.

### Detectar
- El aprendizaje automático de AWS GuardDuty puede detectar comportamientos sospechosos, incluida la generación de instantáneas de Amazon Relational Database Service (Amazon RDS) como parte de los intentos de exfiltración de datos. Se proporciona un ejemplo en el blog de seguridad de AWS [Cómo puede utilizar Amazon GuardDuty para detectar actividades sospechosas dentro de su cuenta de AWS] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/)
- Revise los registros de aplicaciones y sistemas operativos EC2 para detectar inicios de sesión inapropiados, instalación de software desconocido o la presencia de archivos no reconocidos
- Utilice VPCFlowLogs para identificar el acceso inadecuado a la base de datos desde direcciones IP externas
- Puede [investigar el flujo de VPC con Amazon Detective] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) o [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
- El [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) permite a los clientes realizar investigaciones de seguridad en los registros de servicios de AWS proporcionando un entorno de análisis de Amazon Athena que es rápido de implementar, listo para usar y fácil de mantener

### Responder
- Eliminar o rotar [Claves de usuario de IAM] (https://console.aws.amazon.com/iam/home#users) y [Claves de usuario Root] (https://console.aws.amazon.com/iam/home#security_credential); puede que desee rotar todas las claves de su cuenta si no puede identificar una clave o claves específicas que se hayan expuesto
- Eliminar [Usuarios de IAM] no autorizados (https://console.aws.amazon.com/iam/home#users.)
- Eliminar [políticas] no autorizadas (https://console.aws.amazon.com/iam/home#/policies)
- Eliminar [roles] no autorizados (https://console.aws.amazon.com/iam/home#/roles)
- Eliminar instantáneas o bases de datos públicas no reconocidas o no autorizadas
- Identificar instancias EC2 que tuvieran acceso permisivo a las bases de datos e investigarlas también
- Si su aplicación utiliza claves de acceso expuestas, debe sustituirla. Para reemplazar la clave, cree primero una segunda clave (en ese momento, ambas claves estarán activas) y, a continuación, modifique la aplicación para utilizar la nueva clave. A continuación, deshabilite (pero no elimine) las claves expuestas haciendo clic en la opción «Hacer inactivo» de la consola. Si hay algún problema con la aplicación, puede reactivar las claves expuestas. Cuando su aplicación esté completamente funcional con la nueva clave, elimine las claves expuestas
- [Revocar credenciales temporales] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time). Las credenciales temporales también se pueden revocar eliminando el usuario de IAM. *NOTA: * La eliminación de usuarios de IAM puede afectar las cargas de trabajo de producción y debe realizarse con cuidado

### Recuperar
- Se recomienda no pagar el rescate
- Pagar el rescate es una apuesta en cuanto a si el delincuente cumplirá la transacción después de recibir el pago
- Si no existen copias de seguridad de datos, debe realizar un análisis de costes y beneficios y sopesar el valor del compromiso de datos/reputación frente al pago al atacante
- Usted permite directamente al atacante continuar sus operaciones contra su empresa u otras personas si decide pagar el rescate
- Elimine la base de datos comprometida y cree nuevas bases de datos RDS
- Siga los requisitos reglamentarios o la política interna de la empresa para determinar si se requiere análisis forense de la base de datos de RDS
- Visita < https://www.nomoreransom.org/ > para identificar si hay un descifrado disponible para la variante de malware, los datos infectados
- Utilice CloudEndure Disaster Recovery para seleccionar el último punto de recuperación antes del ataque de ransomware o la corrupción de datos para restaurar las cargas de trabajo en AWS
- Si se utiliza una estrategia de copia de seguridad de datos alternativa, valide que las copias de seguridad no se hayan infectado y restaurarlas desde el último evento programado anterior al evento de ransomware

### Tras un incidente

### Denunciar ataques de rescate a las autoridades
El FBI alienta a las organizaciones a denunciar los incidentes de rescate a las fuerzas del orden. El Centro de Denuncias contra Delitos de Internet (IC3) acepta denuncias por delitos en Internet en línea de la víctima real o de un tercero al demandante y trabajará con ellas para determinar la mejor forma de actuar en el futuro. Prepárate para compartir:
- Cualquier información relevante que considere necesaria para respaldar su queja
- Encabezados de correo electrónico
- Información de las transacciones financieras (información de la cuenta, fecha e importe de la transacción, detalles del destinatario)
- Nombre, dirección, teléfono, correo electrónico, sitio web y dirección IP del sujeto
- Detalles específicos sobre cómo te victimizaron
- Nombre, dirección, teléfono y correo electrónico de la víctima

### Realizar análisis de la causa raíz
En la mayoría de los casos, las herramientas forenses existentes funcionarán en el entorno de AWS. Los equipos forenses se benefician de la implementación automatizada de herramientas en todas las regiones de AWS y de la capacidad de recopilar grandes volúmenes de datos rápidamente con baja fricción utilizando los mismos servicios robustos y escalables en los que se basan sus aplicaciones críticas para el negocio.

[Amazon Detective] (https://aws.amazon.com/detective/%20) facilita el análisis, la investigación y la identificación rápida de la causa raíz de posibles problemas de seguridad o actividades sospechosas. Amazon Detective recopila automáticamente los datos del registro de servicios de AWS de sus recursos de AWS y utiliza aprendizaje automático, análisis estadístico y teoría de gráficos para crear un conjunto de datos vinculado que le permite llevar a cabo investigaciones de seguridad más rápidas y eficientes.

### Actualiza el programa de seguridad con las lecciones aprendidas
Documentar y montar en bicicleta las lecciones aprendidas durante las simulaciones y los incidentes en directo en procesos y procedimientos «nuevos normales» permite a las organizaciones comprender mejor cómo se violaron, por ejemplo, dónde eran vulnerables, dónde pudo haber fallado la automatización o dónde faltaba visibilidad, y oportunidad de fortalecer su postura general de seguridad.

## Capacite a tus empleados
Capacite a sus empleados mediante un programa de concienciación de seguridad eficaz. Esto debe ser entretenido y atractivo. Centrarse en informar de phishing, procedimientos de notificación y cómo denunciar cosas «malas» y luego reconocer ese comportamiento puede ser muy efectivo.

## Conclusión
Los ataques de rescate están evolucionando, pero también lo pueden hacer tu concienciación y preparación sobre seguridad. Las agencias gubernamentales, las organizaciones sin fines de lucro y las empresas de todo el mundo confían en AWS para impulsar su infraestructura y mantener seguros sus sistemas y datos. Mediante los servicios y las prácticas recomendadas de AWS compartidos en este libro de jugadas, puede tomar medidas proactivas para reducir la probabilidad y el impacto del rescate en sus entornos de AWS.

## Apéndice A: Servicios y herramientas para prevenir o detectar ataques de rescate
[Amazon CloudWatch Synthetics] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
[Amazon CloudWatch] (https://aws.amazon.com/cloudwatch)
[Amazon CodeGuru] (https://aws.amazon.com/codeguru/)
[Amazon EventBridge] (https://aws.amazon.com/eventbridge/)
[Amazon GuardDuty] (https://aws.amazon.com/guardduty/)
[Accesibilidad de red de Amazon Inspector] (https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)
[Amazon Inspector] (https://aws.amazon.com/inspector/)
[Amazon Macie] (https://aws.amazon.com/macie/)
[Analizador de accesibilidad de Amazon VPC] (https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
[AWS CDK] (https://aws.amazon.com/cdk/)
[AWS Cloud9] (https://aws.amazon.com/cloud9/)
[AWS CloudFormation Guard] (https://github.com/aws-cloudformation/cloudformation-guard)
[AWS CloudFormation] (https://aws.amazon.com/cloudformation/)
[AWS CodeArtiFACT] (https://aws.amazon.com/codeartifact/)
[AWS CodeBuild] (https://aws.amazon.com/codebuild/)
[AWS CodeCommit] (https://aws.amazon.com/codecommit/)
[AWS CodeDeploy] (https://aws.amazon.com/codedeploy/)
[AWS CodePipeline] (https://aws.amazon.com/codepipeline/)
[Reglas de AWS Config] (https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)
[Torre de control de AWS] (https://aws.amazon.com/controltower/)
[AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
[AWS IAM] (https://aws.amazon.com/iam/)
[AWS KMS] (https://aws.amazon.com/kms/)
[AWS Lambda] (https://aws.amazon.com/lambda/)
[AWS Organizations] (https://aws.amazon.com/organizations/)
[AWS Secrets Manager] (https://aws.amazon.com/secrets-manager/)
[AWS Security Hub] (https://aws.amazon.com/security-hub/)
[AWS Step Functions] (https://aws.amazon.com/step-functions/)
[AWS Systems Manager] (https://aws.amazon.com/systems-manager/)
[AWS WAF] (https://aws.amazon.com/waf/)
[AWSPX] (https://github.com/FSecureLABS/awspx)
[CheckMarx] (https://checkmarx.com/)
[Creador de imágenes EC2] (https://aws.amazon.com/image-builder/)
[Escaneo nativo de imágenes de contenedores ECR] (https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)
[git-secrets] (https://github.com/awslabs/git-secrets)
[pMapper] (https://github.com/nccgroup/PMapper)
[Prowler] (https://github.com/toniblyx/prowler)
[SonarQube] (https://www.sonarqube.org/)