# Manual de seguridad para credenciales de cuentas de AWS comprometidas

## Introducción

Como parte de nuestro compromiso continuo con los clientes, AWS ofrece este
manual de respuesta a incidentes de seguridad que describe los pasos necesarios para
detectar y responder a las credenciales comprometidas en su AWS
cuenta (s). El propósito de este documento es proporcionar prescripciones
guía sobre las medidas que se deben tomar cuando se sospecha que se ha producido un incidente de seguridad
tuvo lugar.

![Image](/images/nist_life_cycle.png)

*Aspectos de la respuesta a los incidentes de AWS*

## Preparación

Para ejecutar la respuesta a los incidentes de forma rápida y eficaz
actividades, es crucial preparar a las personas, los procesos y
tecnología dentro de su organización. Revise el [*incidente de seguridad de AWS
Respuesta
Guía*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html),
e implementar las medidas necesarias para garantizar la preparación de todos
tres dominios.

## Cómo solicitar ayuda al soporte de AWS

Es importante que notifique a AWS tan pronto como sospeche que se ha comprometido
credenciales de su cuenta u organización de AWS. Estos son los
pasos para contratar a AWS Support:

### Abrir un caso de AWS Support

- Inicie sesión en su cuenta de AWS:

- Esta es la primera cuenta de AWS que se vio afectada por la seguridad
evento, para validar la propiedad de la cuenta de AWS.

- Nota: Si no puede acceder a la cuenta, utilice [esto
formulario] (https://support.aws.amazon.com/#/contacts/aws-account-support/)
para enviar una solicitud de asistencia.

- Seleccione «*Servicios*», «*Centro de soporte*», «*Crear caso*».

- Seleccione el tipo de problema «*Cuenta y facturación» *.

- Seleccione el servicio y la categoría afectados:

- Ejemplo:

- Servicio: Cuenta

- Categoría: Seguridad

- Elija una gravedad:

- Clientes de Enterprise Support o On-Ramp: *Riesgo empresarial crítico
Pregunta*.

- Clientes de asistencia empresarial: *Pregunta urgente sobre riesgos comerciales*.

- Describa su pregunta o problema:

- Proporcione una descripción detallada del problema de seguridad
experiencia, los recursos afectados y el impacto empresarial.

- Hora en que se reconoció el incidente de seguridad por primera vez.

- Resumen de la cronología del evento hasta este momento.

- Artefactos que ha recopilado (capturas de pantalla, archivos de registro).

- ARN de los usuarios o funciones de IAM que sospecha que están comprometidos.

- Descripción de los recursos afectados y el impacto empresarial.

- Nivel de participación en su organización (por ejemplo: «Esta seguridad
el evento cuenta con la visibilidad del CEO y el consejo de administración de
Directores»).

- Opcional: añadir contactos adicionales al caso.

- Seleccione «*Póngase en contacto con nosotros».

- Idioma de contacto preferido (puede estar sujeto a disponibilidad)

- Método de contacto preferido: web, teléfono o chat (recomendado)

- Opcional: contactos adicionales para el caso

<!-- -->

- *Haga clic en «Enviar» *

- **Escalaciones**: notifique a su equipo de cuentas de AWS tan pronto como
posible, para que puedan emplear los recursos necesarios y aumentar a medida que
necesario.

**Nota: ** Es muy importante que compruebe su «seguridad alternativa»
El 'contacto' se define para cada cuenta de AWS. Para obtener más información, consulte
a [esto
artículo] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).

## Detección

Hay varias formas de detectar las credenciales comprometidas en su
Entorno de AWS. A continuación se muestran algunas formas de detectar posibles indicadores de
Compromiso (IoC). Para obtener una lista detallada de los IoC, consulte [apéndice]
UN.] (#appendix -a-reviewing-logs) **Nota**: Documente los detalles de cada uno
sospecha de IoC para analizarlo más a fondo.

1. Reseña Amazon [GuardDuty]
hallazgos] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_findings.html).
Los siguientes hallazgos están relacionados con una actividad sospechosa o anómala
por entidades de IAM. Revise el [hallazgo]
detalles] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html),
y si la actividad es inesperada, esto podría indicar que se ha comprometido
credencial.

1. CredentialAccess:IAMUser/AnomalousBehavior

2. DefenseEvasion:IAMUser/AnomalousBehavior

3. Discovery:IAMUser/AnomalousBehavior

4. Exfiltration:IAMUser/AnomalousBehavior

5. Impact:IAMUser/AnomalousBehavior

6. InitialAccess:IAMUser/AnomalousBehavior

7. PenTest:IAMUser/KaliLinux

8. PenTest:IAMUser/ParrotLinux

9. PenTest:IAMUser/PentooLinux

10. Persistence:IAMUser/AnomalousBehavior

11. Policy:IAMUser/RootCredentialUsage

12. PrivilegeEscalation:IAMUser/AnomalousBehavior

13. Recon:IAMUser/MaliciousIPCaller

14. Recon:IAMUser/MaliciousIPCaller.Custom

15. Recon:IAMUser/TorIPCaller

16. Stealth:IAMUser/CloudTrailLoggingDisabled

17. Stealth:IAMUser/PasswordPolicyChange

18. UnauthorizedAccess:IAMUser/ConsoleLoginSuccess.B

19. UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration

20. UnauthorizedAccess:IAMUser/MaliciousIPCaller

21. UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom

22. UnauthorizedAccess:IAMUser/TorIPCaller

2. Revise la facturación de AWS para ver si hay picos inusuales que pueden ser un signo de cuenta
y poner en peligro las credenciales siguiendo estos pasos:

1. Inicie sesión en la consola de administración de AWS y abra la [Facturación]
consola] (https://console.aws.amazon.com/billing/).

2. Seleccione «*Facturas» * para ver los detalles de sus cargos actuales.

3. Elija «*Pagos» * para ver su historial de transacciones de pago.

4. Elija «*Informes de costes y uso de AWS» * para ver los informes desglosados
reduzca sus costes.

3. Descargue el informe de credenciales de IAM yendo a la consola de IAM y
elija el «*Informe de credenciales» * de la izquierda en «*Acceso
informes» * y, a continuación, revise lo siguiente:

1. Identifique la creación inusual de usuarios de IAM consultando la fecha de creación
y las columnas utilizadas o cambiadas por última vez de la contraseña.

2. Compruebe si algún usuario de IAM tiene dos o más claves de acceso.

3. Compruebe si algún usuario de IAM tiene
*Se adjunta AWS ExposedCredentialPolicy\ _DO\ _NOT\ _REMOVE*. Si es así,
gire sus teclas de acceso.

4. Revise las funciones de IAM en la cuenta de AWS para identificar cualquier cosa desconocida
roles que se han creado o a los que se ha accedido

1. En la consola de AWS, seleccione «*Service*s», «*IAM*» y
«*Roles*»

2. Revise cualquier «*nombres de roles*» que no conozca.

3. Haga clic en el nombre del rol y revise los detalles de la lista:

1. Fecha de creación del rol

2. ARN

3. Última actividad

4. Haga clic en «*Permisos*» para revisar las políticas de IAM adjuntas
al papel.

5. Haga clic en «*Relaciones de confianza*» para ver las entidades que pueden asumir
el papel.

6. Haga clic en «*Asesor de acceso*» para ver qué servicios han sido
accedido por el rol y la «*Fecha del último acceso*».

5. Detecte cualquier recurso no reconocido o no autorizado en su AWS
cuenta como:

1. Instancias EC2 ejecutando el siguiente comando en la AWS CLI
terminal «*aws ec2 describe-instances*» y valide el
resultados. Busque la hora de creación de cada instancia mediante el
*—consulta 'Reservas\ [\] .Instancias\ [\]. {ip: dirección IP pública,
tm: LaunchTime} '—filters 'Name=tag:name, valores=
myInstanceName' | jq 'ordenar\ _por (.tm) | invertir |.\ [0\] '* filtro.

2. Lambda funciona mediante la ejecución del siguiente comando en la AWS CLI
terminal «*aws lambda list-functions*» y valide la última
modificó la hora con el filtro «| *grep «lastModified*»».

6. Revise cualquier notificación de seguridad de AWS sobre su cuenta de
iniciar sesión en [AWS Support
Center] (https://support.console.aws.amazon.com/support/home#/) entonces
leer y responder a los mensajes.

7. Revise los resultados siguiendo estos pasos:

1. Abra la [consola de IAM] (https://console.aws.amazon.com/iam/).

2. Elija «*Analizador de acceso» * en la columna de la izquierda, en Acceso
informes.

3. En «*Hallazgos activos» * revise los hallazgos para identificar los recursos
en su organización, como los cubos de S3, las funciones de IAM o Lambda
funciones que se comparten con entidades externas.

## Análisis

Una vez que haya identificado cualquier recurso o actividad sospechosa que pueda
indique el compromiso, realice un análisis más detallado en su SIEM o registre
herramientas de análisis. AWS tiene varias herramientas de servicios de seguridad que le ayudan en
analizar los eventos de seguridad. Algunas de estas herramientas incluyen:

1. **Amazon Guardduty**: Amazon GuardDuty es una detección de amenazas
servicio que monitorea continuamente la presencia de actividades maliciosas y
comportamiento no autorizado para proteger sus cuentas de AWS, Amazon Elastic
Cargas de trabajo de Compute Cloud (EC2), aplicaciones de contenedores, Amazon Aurora
bases de datos y datos almacenados en Amazon Simple Storage Service (S3).

2. **Centro de seguridad de AWS**: El Centro de seguridad de AWS es una postura de seguridad en la nube
servicio de gestión (CSPM) que funciona de forma automática y continua
las mejores prácticas de seguridad comparan sus recursos de AWS para ayudarlo
identifica los errores de configuración y agrega sus alertas de seguridad
(es decir, hallazgos) en un formato estandarizado para que pueda más fácilmente
enriquecerlos, investigarlos y remediarlos.

3. **Amazon Detective**: Amazon Detective facilita el análisis,
investigar e identificar rápidamente la causa raíz de la posible
problemas de seguridad o actividades sospechosas.

También puede buscar en el historial de eventos de AWS CloudTrail siguiendo
estos pasos para analizar y recopilar pruebas:

1. Analice los registros de AWS CloudTrail para ver lo siguiente:

1. Cualquier actividad inusual asociada a los inicios de sesión:

1. Ir al [Sendero de nubes]
Panel de control] (https://console.aws.amazon.com/cloudtrail).

2. En el lado izquierdo, elija Historial de eventos.

3. En el menú desplegable, cambie Solo lectura a Nombre del evento.

4. Revise los eventos disponibles para detectar cualquier actividad sospechosa de
buscando los siguientes términos a través del cuadro de búsqueda:
«*ConsoleLogin*», «*AssumeRole*», «*GetFederationToken*»,
«*GetCredentialReport*», «*Generar informe credencial*». También
es importante tener en cuenta que debería aparecer «*UserIdentity*»
como «*type*»: «*Root*» para el usuario root o «*type*»:
«*IAmUser*» para cualquier usuario de IAM local de la cuenta.

<!-- -->

1. Busque cualquier identificador de clave de acceso y nombre de usuario de IAM utilizados para lanzar un
instancia sospechosa de Amazon EC2:

1. Abra la consola AWS CloudTrail y elija «*Evento»
historial» * desde el panel de navegación.

2. Seleccione el menú desplegable «*Buscar atributos» * y, a continuación
elija «*Nombre del recurso» *.

3. En el campo Introduzca el nombre del recurso, pegue el ID de la instancia EC2,
y, a continuación, pulse Entrar en su dispositivo.

4. Amplíe el nombre del evento para *RunInstances*.

5. Copie la clave de acceso de AWS y anote el nombre de usuario.

2. Consulte el historial de eventos de AWS CloudTrail para ver la actividad del
clave de acceso comprometida:

1. Abra la consola de CloudTrail y elija «*Evento»
> historial» * desde el panel de navegación.

2. Seleccione el menú desplegable «*Buscar atributos» * y, a continuación
> elija «*clave de acceso de AWS» *.

3. En el campo «*Introduzca una clave de acceso de AWS» *, introduzca el
> ID de clave de acceso de IAM comprometido.

4. Amplíe el nombre del evento para la llamada a la API *RunInstances*.

3. Si accede a AWS a través de un proveedor de identidad externo, sea
asegúrese de auditar los registros de acceso y seguir las instrucciones del proveedor
sobre responder a un suceso y proteger su entorno.

<!-- -->

1. Analice las credenciales comprometidas de IAM Access Advisor a las que se accedió por última vez
información para ver cuál fue el último servicio al que accedió siguiendo
estos pasos:

1. Vaya a la [consola de IAM] (https://console.aws.amazon.com/iam).

2. Vaya a usuarios o funciones y haga clic en el nombre de la persona comprometida
director, elija la pestaña «*asesor de acceso» * y mire qué
recursos a los que accedió por última vez.

## Contención

Tras analizar y recopilar más información sobre el comprometido
credenciales y cualquier otro recurso afectado, es hora de controlar y
suprimir el incidente de seguridad.

1. Desactive los usuarios de IAM, cree una clave de acceso a IAM de respaldo y, a continuación
desactive la clave de acceso comprometida siguiendo estos pasos:

1. Abra la [consola de IAM] (https://console.aws.amazon.com/iam/) y
pegue el ID de la clave de acceso de IAM en la barra de búsqueda de IAM.

2. Elija el nombre de usuario y, a continuación, elija «*Seguridad
pestaña credenciales*».

3. En el campo Contraseña de la consola, elija «*Administrar*».

4. En Acceso a la consola, seleccione «*Desactivar*» y, a continuación, seleccione Aplicar.

5. Para la clave de acceso de IAM comprometida, elija «*Hacer inactivo*».

2. Gire las teclas de acceso siguiendo estos pasos:

1. Primero, cree una segunda clave yendo al [IAM
consola] (https://console.aws.amazon.com/iam/).

2. En el panel de navegación, seleccione «*Usuarios» *.

3. Elija el nombre del usuario previsto y, a continuación, elija
la pestaña «*Credenciales de seguridad» *.

4. En la sección «*Teclas de acceso» *, elija «*Crear clave de acceso» *. En
la página «*Acceda a las mejores prácticas clave* *y alternativas» *,
elija «*Otros» * y, a continuación, elija «*Siguiente» . *

5. A continuación, modifique su solicitud para usar la nueva clave.

6. Desactive (pero no elimine) la primera clave.

7. Si hay algún problema con su solicitud, vuelva a activar el
clave temporal. Cuando su solicitud esté en pleno funcionamiento y
la primera clave está deshabilitada, solo entonces es seguro eliminar la
primera clave. Asegúrese de llevar un registro de todos los accesos eliminados
claves para seguir buscándolas en los registros de AWS CloudTrail.

3. Revoca las funciones de IAM en las sesiones activas siguiendo estos pasos:

1. Abra la [consola de IAM] (https://console.aws.amazon.com/iam/) y
vaya al rol y haga clic en el rol de SOY que quiere provocar activo
sesiones para.

2. Haga clic en el nombre de la función de IAM y vaya a la pestaña «*revocar sesiones» *.

3. Haga clic en el botón «*revocar sesiones activas*» y confirme el paso.

4. Aísle los recursos afectados siguiendo estos pasos:

1. Para las instancias de Amazon EC2, vaya a la consola de instancias EC2 y compruebe
la casilla situada junto a la instancia EC2 que quiere aislar, haga clic en
*acciones*, haga clic en *seguridad* y, a continuación, en *cambiar seguridad
grupos*. Separe los grupos de seguridad actuales y adjunte un
grupo de seguridad aislado que bloquea la entrada y la salida
comunicación al EC2.

2. Para los cubos de Amazon S3, utilice [bucket
políticas] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)
para impedir que cualquier dirección IP sospechosa acceda a la S3
cubos.

5. Revocar las sesiones del Centro de Identidad:

Con Identity Center, hay dos sesiones de las que preocuparse
que son la sesión del portal de acceso y las sesiones de roles o solicitudes:

1. Sesión del portal de acceso:

1. Desactivar al usuario en el Centro de identidad:

1. Vaya a la consola de Identity Center y seleccione «Usuarios»

2. Elija el nombre de usuario del usuario que se va a deshabilitar

3. En el cuadro de información general del usuario, haga clic en «Desactivar»
> acceso de usuario»

2. Revocar cualquier sesión activa:

1. En la página del usuario del Centro de identidad, seleccione «Activo»
> pestaña de sesiones

2. Seleccione cualquier sesión de la lista y, a continuación, haga clic en «Eliminar sesión»

2. Revocar sesiones de roles:

1. Identifique los conjuntos de permisos que utiliza el usuario.

1. En la consola de Identity Center, haga clic en Conjuntos de permisos

2. Seleccione el nombre del conjunto de permisos.

3. Desplácese hacia abajo hasta «Política en línea» y haga clic en el botón Editar

4. Añada la siguiente política:

{

«Versión»: «17-10-2012»,

«Declaración»:\ [

{

«Efecto»: «Denegar»,

«Acción»: «\ *»,

«Recurso»: «\ *»,

«Condición»: {

«StringEquals»: {

«identityStore:userID»: «ejemplo»

},

«Fecha inferior a»: {

«AWS: Hora de emisión de fichas»: «2023-09-26T 15:00:00.000 Z»

}

}

}

\]

}

Para esta política, actualice «ejemplo» por el seudónimo del Centro de Identidad del usuario.
El seudónimo se encuentra en el cuadro «Información general» del usuario
Página de usuario. El valor de AWS:TokenIssueTime debe ser igual al tiempo en
en el que aplica esta política.

1. Sesiones de solicitud

Las sesiones de aplicación se crean cuando un usuario accede a un tercero
aplicación o servicio de AWS directamente desde el portal de acceso.

Para revocar las sesiones de solicitud, consulte la documentación del
aplicación a la que se accedió.

1. Centro de identidad comprometido

Si su proveedor de identidad está en peligro, debería bloquear el acceso a
Centro de identidad de un proveedor de identidad comprometido (es decir, externo)
Proveedor de identidad basado en SAML (o Active Directory) cambiando el
fuente de identidad al «directorio de fuentes de identidad».

Para cambiar su fuente de identidad:

6.1 Vaya a la consola de Identity Center

6.2 Seleccione «Configuración»

6.3 En la página de configuración, haga clic en la pestaña «Fuente de identidad»

6.4 Haga clic en el botón «Acciones» y, a continuación, seleccione «Cambiar identidad»
fuente'

6.5 Seleccione «Directorio del centro de identidad» en «Elegir origen de identidad»
página

6.6 Haga clic en el botón «Siguiente»

6.7 Lea la información incluida en el cuadro «Revisar y confirmar»,

6.8 Escriba «ACEPTAR» en el campo correspondiente y haga clic en «Cambiar»
botón «fuente de identidad»

Tenga en cuenta que si accede a AWS a través de un proveedor de identidad externo,
asegúrese de auditar los registros de acceso y seguir las instrucciones del proveedor sobre
responder a un suceso y proteger su entorno.

Además, si va a cambiar de Active Directory al Centro de Identidad
directorio, se eliminarán todos los usuarios y grupos. Los conjuntos de permisos serán
no se eliminará, pero se eliminarán las asignaciones de conjuntos de permisos. Si usted
están cambiando de un proveedor de identidad externo basado en SAML, todos los usuarios
y los grupos permanecerán poblados en el Centro de Identidad, al igual que los
asignaciones de conjuntos de permisos

## Erradicación

Cuando termine de contener el incidente de seguridad, es hora de trabajar
sobre eliminar y limpiar la causa del problema de seguridad.

1. Elimine todos los recursos creados por las claves comprometidas que estaban
detectado en la «Fase de análisis, paso 1». Compruebe todas las regiones de AWS, incluso
regiones en las que no lanza los recursos de AWS. Si lo necesita
mantenga un recurso para la investigación, considere la posibilidad de respaldarlo.

2. Compruebe si hay y
[eliminar] (https://repost.aws/knowledge-center/terminate-resources-account-closure)
cualquier servicio no reconocido que se ejecute en su cuenta. Pagar
atención especial a los siguientes recursos:

1. Instancias EC2 y AMI, incluidas las instancias de las paradas
estado.

2. Volúmenes e instantáneas de EBS.

3. Funciones y capas de AWS Lambda.

3. Eliminar los permisos innecesarios del registro de los cubos o el registro de S3
recursos de agregación identificados en la «Fase de detección, paso 6» que
podría usarse para evitar que lo detecten. Esto le permite identificar lo no intencionado
acceso a sus recursos y datos.

4. Elimine todos los datos expuestos que no sean necesarios para las operaciones.

5. Realizar escaneos de vulnerabilidades de seguridad en la cara pública
recursos. Puede utilizar herramientas como [Amazon
Inspector] (https://docs.aws.amazon.com/inspector/latest/user/scanning-resources.html)
para escanear el sistema operativo y una aplicación de software de <sup>terceros</sup> para
vulnerabilidades.

## Recuperación

Una vez que termine de erradicar las causas de los eventos de seguridad. Es la hora
para restaurar los recursos afectados a un estado conocido.

1. Restaure los datos necesarios de copias de seguridad limpias conocidas anteriores a la
evento:

1. [Restauración a partir de una instantánea de Amazon EBS o un
AMI] (https://docs.aws.amazon.com/prescriptive-guidance/latest/backup-recovery/restore.html).

2. [Restauración desde una base de datos
instantánea] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_RestoreFromSnapshot.html) Amazon
RDS.

3. [Restaurar la anterior
versiones] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RestoringPreviousVersions.html) Amazon
Versiones de objetos S3.

2. Si es necesario, reconstruya los sistemas desde cero, incluida la redistribución
de una fuente confiable que utiliza la automatización, en algún momento en una nueva cuenta de AWS.

3. Si se pierde el acceso a la autenticación multifactor «MFA» y no
tenga otro dispositivo de MFA registrado en su cuenta root, por favor
siga estos pasos:

1. Inicie sesión en [AWS Management]
Consola] (https://console.aws.amazon.com/) como propietario de la cuenta
seleccionando Usuario Raíz e introduciendo el correo electrónico de su cuenta de AWS
dirección. En la página siguiente, introduzca su contraseña.

2. En la página Se requiere una verificación adicional, seleccione un MFA
método para autenticarse y elegir Siguiente.

3. Según el tipo de MFA que utilice, debería ver un
página diferente, pero la opción «*Solucionar problemas de MFA» * funciona
lo mismo. En la página «*Se requiere verificación adicional» *
o página «*Autenticación multifactor» *, elija «*Solución de problemas
MFA» *.

4. Si es necesario, vuelva a escribir su contraseña y elija *Iniciar sesión*.

5. En la página «*Solucionar problemas del dispositivo de autenticación» *, en
el «*Iniciar sesión con factores alternativos de
sección autenticación» *, elija «*Iniciar sesión con una alternativa
factores» *.

6. En «*Iniciar sesión con un factor alternativo» de
página de autenticación» *, autentique su cuenta verificando
la dirección de correo electrónico, seleccione «*Enviar correo electrónico de verificación» *

7. Compruebe el correo electrónico asociado a su cuenta de AWS para ver un
mensaje de Amazon Web Services
(recover-mfa-no-reply@verify.signin.aws). Siga las instrucciones
en el correo electrónico.

> Si no ve el correo electrónico en su cuenta, compruebe la carpeta de correo no deseado o
> vuelva a su navegador y seleccione «*Reenviar el correo electrónico*».

1. Tras comprobar su dirección de correo electrónico, podrá seguir autenticándose
su cuenta. Para comprobar su número de teléfono de contacto principal,
elija Llámeme ahora.

2. Responda a la llamada de AWS y, cuando se le pida, introduzca los 6 dígitos
número del sitio web de AWS en el teclado de su teléfono.

> Si no recibe ninguna llamada de AWS, seleccione Iniciar sesión para iniciar sesión en
> consola de nuevo y empezar de nuevo. O consulte [Multifactor perdido o inutilizable
> Autenticación (MFA)
> dispositivo] (https://support.aws.amazon.com/#/contacts/aws-mfa-support) a
> póngase en contacto con el servicio de asistencia para obtener ayuda.

1. Tras comprobar su número de teléfono, podrá iniciar sesión en su cuenta
> seleccionando Iniciar sesión en la consola.

2. El siguiente paso varía según el tipo de MFA que utilice:

1. Para un dispositivo MFA virtual, elimine la cuenta del dispositivo.
> Luego vaya a [AWS Security]
> Credenciales] (https://console.aws.amazon.com/iam/home? página #security_credential)
> y elimine la antigua entidad de dispositivo virtual de MFA antes de crear
> uno nuevo.

2. Para obtener una clave de seguridad FIDO, vaya a [AWS Security
> Credenciales] (https://console.aws.amazon.com/iam/home? página #security_credential)
> y desactive la antigua clave de seguridad FIDO antes de activar una nueva
> uno.

3. Para obtener un token TOTP de hardware, póngase en contacto con el proveedor externo para
> ayuda para arreglar o sustituir el dispositivo. Puede seguir firmando
> en el uso de factores de autenticación alternativos hasta que
> reciba su nuevo dispositivo. Después de tener el nuevo MFA de hardware
> dispositivo, vaya a [AWS Security
> Credenciales] (https://console.aws.amazon.com/iam/home? página #security_credential)
> y elimine la antigua entidad del dispositivo de hardware de MFA antes que
> crear uno nuevo.

<!-- -->

1. Confirme que los directores de IAM tienen el acceso y los permisos adecuados
tras el incidente de seguridad.

2. Solucione las vulnerabilidades e instale los parches necesarios con [AWS
parche SSM
gerente] (https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager.html)
o cualquier otra herramienta de <sup>terceros</sup> que utilice.

3. Sustituya los archivos comprometidos o dañados por versiones limpias de
copia de seguridad.

## Actividad posterior al incidente

Cuando se recupere correctamente del incidente de seguridad,
debería realizar los ejercicios descritos en [*Incidente de seguridad de AWS
Guía de respuesta, posterior al incidente
actividad*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/establish-framework-for-learning.html).
Esto le ayudará a documentar las lecciones aprendidas del incidente, medir
y mejore la eficacia de sus capacidades de respuesta a los incidentes,
e implementar controles adicionales para evitar que se produzca un incidente similar
recurrente.

## Conclusión

En este manual, describimos los pasos iniciales que se deben seguir cuando un
un evento de seguridad de credenciales comprometido se produce en sus cuentas de AWS.
Esto incluye contratar el soporte de AWS, detectar un compromiso, analizar
eventos en su (s) cuenta (s), que incluyen el incidente de seguridad,
erradicar la amenaza, recuperar su (s) cuenta (s) por una «de funcionalidad comprobada»
estado operativo y actividades posteriores al incidente, incluidas las clases
aprendido. Como siguiente paso, consulte los siguientes recursos de AWS para
ayude a mejorar sus capacidades de respuesta a incidentes:

1. [Respuesta a un incidente de seguridad de AWS
Guía] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/aws-security-incident-response-guide.html)

2. [Marco del manual de estrategias para clientes de AWS para una IAM comprometida
Credenciales] (https://github.com/aws-samples/aws-customer-playbook-framework/blob/main/docs/Compromised_IAM_Credentials.md)

3. [Prácticas recomendadas de seguridad de AWS en
YO SOY] (https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

4. [AWS Well Architected Framework — Seguridad
Pilar] (https://docs.aws.amazon.com/wellarchitected/latest/security-pillar/welcome.html)

## Apéndice A: Revisión de los registros

**Estructura de registro de CloudTrail**

Al buscar en los registros de CloudTrail, es importante entender
los distintos campos que contiene un registro de eventos de Sendero de nubes. Para un
lista completa, consulte el [Sendero de <u>nubes oficial]
</u>documentación] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-record-contents.html).

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th>Campo</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Hora del evento</td>
<td>La fecha y la hora en que se completó la solicitud, en forma coordinada
hora universal (UTC).</td>
</tr>
<tr class="even">
<td>Nombre del evento</td>
<td>La acción solicitada, que es una de las acciones de la API para
ese servicio</td>
</tr>
<tr class="odd">
<td>Origen del evento</td>
<td>El servicio al que se hizo la solicitud. Este nombre suele ser un
forma abreviada del nombre del servicio sin espacios más .amazonaws.com.</td>
</tr>
<tr class="even">
<td>Identidad de usuario</td>
<td>Información sobre la identidad de IAM que hizo una solicitud. Para más
información, consulte <a
<u>href=» https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference-user-identity.html «> Sendero de nubes
Elemento de identidad de usuario</u></a></td>.
</tr>
<tr class="odd">
<td>recursos</td>
<td><p>Una lista de los recursos a los que se accedió en el caso. El campo puede contener
la siguiente información:</p>
<ul>
<li><p>ARN de recursos</p></li>
<li><p>ID de cuenta del propietario del recurso</p></li>
<li><p>Identificador del tipo de recurso en el formato:
AWS: :aws-service-name: :nombre</p></li> de tipo de datos
</ul></td>
</tr>
<tr class="even">
<td>Región de AWS</td>
<td>La región de AWS a la que se hizo la solicitud, como us-east-2</td>
</tr>
<tr class="odd">
<td>Dirección IP de origen</td>
<td>La dirección IP desde la que se hizo la solicitud. Para acciones que
provienen de la consola de servicio, la dirección indicada es la del
recurso de cliente subyacente, no el servidor web de la consola.</td>
</tr>
</tbody>
</table>

**Eventos**

Hay una serie de acciones que los actores de amenazas realizarán después
comprometer las credenciales de la cuenta. Si bien no es práctico enumerar todos
posible acción, estos son algunos patrones que puede buscar durante su
investigación.

**Nota: ** Las siguientes acciones de la API no indican necesariamente una seguridad
se ha producido un incidente. Evalúe toda la actividad registrada dentro del contexto
de su investigación.

**Cambios en la configuración del proveedor de identidad SAML/OIDC**

Los actores de amenazas pueden crear o modificar el proveedor de identidad SAML/OIDC
configuraciones para evitar la detección y mantener la persistencia en su
entorno de nube.

<table>
<colgroup>
<col style="width: 32%" />
<col style="width: 67%" />
</colgroup>
<thead>
<tr class="header">
<th><strong>Acción de API</strong></th>
<th><strong>Descripción</strong></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Yo soy: crear un proveedor de SAML</td>
<td>Crea un recurso de IAM que describe un proveedor de identidad (IdP)
que sea compatible con SAML 2.0</td>
</tr>
<tr class="even">
<td>Yo soy: eliminar un proveedor de SAML</td>
<td>Elimina un recurso de un proveedor de SAML en IAM.</td>
</tr>
<tr class="odd">
<td>Yo soy: Actualizar un proveedor de SAML</td>
<td>Actualiza el documento de metadatos de un proveedor de SAML existente</td>
</tr>
<tr class="even">
<td>Yo soy: crear un proveedor de OpenID Connect</td>
<td>Crea una entidad de IAM para describir un proveedor de identidad (IdP) que
es compatible con OpenID Connect (OIDC)</td>.
</tr>
<tr class="odd">
<td>Soy: añadir el ID de cliente al proveedor de OpenID Connect</td>
<td>Añade un nuevo identificador de cliente (también conocido como público) a la lista de clientes
Los identificadores ya están registrados para el IAM OpenID Connect (OIDC) especificado
recurso para proveedores</td>
</tr>
<tr class="even">
<td>Yo soy: eliminar el proveedor de OpenID Connect</td>
<td>Elimina un objeto de recurso del proveedor de identidad (IdP) de OpenID Connect en
</td>SOY
</tr>
<tr class="odd">
<td>Yo soy: eliminar el ID de cliente del proveedor de OpenID Connect</td>
<td>Elimina el ID de cliente especificado (también conocido como público) del
lista de identificadores de clientes registrados para el IAM OpenID Connect especificado
(OIDC) objeto de recurso del proveedor</td>
</tr>
<tr class="even">
<td>Yo soy: actualice la huella digital del proveedor de OpenID Connect</td>
<td>Sustituye a la lista existente de huellas digitales de certificados de servidor
asociado a un objeto de recurso del proveedor de OpenID Connect (OIDC) con un
nueva lista de huellas dactilares</td>
</tr>
</tbody>
</table>

**Cambios en la configuración de IAM**

Los actores de amenazas pueden crear o modificar los principios, las credenciales de IAM o
permisos para evitar la detección o mantener la persistencia en la nube
entorno.

<table>
<colgroup>
<col style="width: 28%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th>Acción de API</th>
<th>Descripción</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Yo soy: cambiar la contraseña</td>
<td>Cambia la contraseña del usuario de IAM que llama a esto
</td>operación
</tr>
<tr class="even">
<td>Yo soy: Crear usuario</td>
<td>Crea un nuevo usuario de IAM para su cuenta de AWS</td>
</tr>
<tr class="odd">
<td>Yo soy: crear un rol</td>
<td>Crea un nuevo rol para su cuenta de AWS</td>
</tr>
<tr class="even">
<td>Yo soy: crear un grupo</td>
<td>Crea un grupo nuevo</td>
</tr>
<tr class="odd">
<td>IAM: Adjuntar política de usuario</td>
<td>Adjunta la política gestionada especificada al usuario especificado</td>
</tr>
<tr class="even">
<td>Soy: adjuntar política de funciones</td>
<td>Adjunta la política gestionada especificada a la función de IAM especificada</td>
</tr>
<tr class="odd">
<td>Soy: adjuntar política de grupo</td>
<td>Adjunta la política gestionada especificada al IAM especificado
</td>grupo
</tr>
<tr class="even">
<td>Yo soy: Listar las claves de acceso</td>
<td>Devuelve información sobre los identificadores de las claves de acceso asociados al
usuario de IAM especificado</td>
</tr>
<tr class="odd">
<td>Soy: Crear versión de política</td>
<td>Crea una nueva versión de la política gestionada especificada</td>
</tr>
<tr class="even">
<td>iam: Actualizar perfil de inicio de sesión</td>
<td>Cambia la contraseña del usuario de IAM especificado</td>
</tr>
<tr class="odd">
<td>Soy: Crear clave de acceso</td>
<td>Crea una nueva clave de acceso secreta de AWS y la clave de acceso de AWS correspondiente
ID del usuario especificado</td>
</tr>
<tr class="even">
<td>Yo soy: actualizar la clave de acceso</td>
<td>Cambia el estado de la clave de acceso especificada de Activa a
Inactivo o viceversa.</td>
</tr>
<tr class="odd">
<td>Yo soy: desactivar el dispositivo MFA</td>
<td>Desactiva el dispositivo de MFA especificado y lo elimina de la asociación
con el nombre de usuario para el que estaba activado originalmente</td>
</tr>
</tbody>
</table>

**Cambios en las configuraciones de registro y supervisión**

Los actores de amenazas pueden deshabilitar la supervisión de los recursos o eliminar los registros para evitar
detectar o impedir la investigación de incidentes.

<table>
<colgroup>
<col style="width: 46%" />
<col style="width: 53%" />
</colgroup>
<thead>
<tr class="header">
<th>Cloud Trail: Eliminar rastro</th>
<th>Elimina un rastro: desactiva la entrega de eventos de CloudTrail a un
Cubeta de Amazon S3, Vigilancia en la nube o Amazon EventBridge</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>CloudTrail: dejar de iniciar sesión</td>
<td>Suspende la grabación de las llamadas a la API de AWS y la entrega de archivos de registro para
el sendero especificado</td>
</tr>
<tr class="even">
<td>Cloud Trail: actualizar Trail</td>
<td>Actualiza la configuración que especifica la entrega de los archivos de registro</td>
</tr>
<tr class="odd">
<td>CloudTrail: eliminar el almacén de datos de eventos</td>
<td>Elimina un <a
<u>href=» https://docs.aws.amazon.com/awscloudtrail/latest/userguide/query-event-data-store.html «> Sendero de nubes
Tienda Lake Event Date</u></a></td>
</tr>
<tr class="even">
<td>Servicio de guardia: eliminar el detector</td>
<td>Elimina un detector Amazon GuardDuty y desactiva GuardDuty en un
región en particular</td>
</tr>
<tr class="odd">
<td>Servicio de guardia: eliminar el destino de publicación</td>
<td>Elimina el destino de publicación, al que se exportan los resultados
</td>a
</tr>
<tr class="even">
<td>Analizador de acceso: Eliminar analizador</td>
<td>Elimina el analizador especificado y desactiva el analizador de acceso de IAM
servicio para esa región.</td>
</tr>
<tr class="odd">
<td>Configuración: eliminar regla de configuración</td>
<td>Elimina la regla de Configuración de AWS especificada y toda su evaluación
</td>resultados
</tr>
<tr class="even">
<td>Configuración: Eliminar grabadora de configuración</td>
<td>Elimina el grabador de configuración de AWS Config</td>
</tr>
<tr class="odd">
<td>Configuración: Eliminar canal de entrega</td>
<td>Elimina el canal de entrega de AWS Config</td>
</tr>
<tr class="even">
<td>Configuración: detener la grabadora de configuración</td>
<td>Deja de registrar las configuraciones y los cambios de configuración del
grupo de grabación especificado</td>
</tr>
</tbody>
</table>

**Cambios en la configuración del depósito de S3**

Los actores de amenazas pueden modificar o eliminar las configuraciones de los cubos de S3 para
extraer datos.

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>S3: Listar cubos</th>
<th>Devuelve una lista de todos los cubos que son propiedad del remitente autenticado de
la solicitud</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>S3: Crear cubo</td>
<td>Crea un nuevo cubo</td>
</tr>
<tr class="even">
<td>S3: Eliminar bloque de acceso público a Bucket</td>
<td>Elimina la configuración PublicAccessBlock de un Amazon S3
balde</td>
</tr>
<tr class="odd">
<td>S3: Política de Put Bucket</td>
<td>Añade o actualiza una política en un bucket</td>
</tr>
<tr class="even">
<td>S3: Política de eliminación de cubos</td>
<td>Elimina la política del paquete</td>
</tr>
<tr class="odd">
<td>S3: Put Object ACL</td>
<td>Establece los permisos de la lista de control de acceso (ACL) de un objeto en un
Cubeta Amazon S3</td>
</tr>
<tr class="even">
<td>S3: ACL para eliminar objetos</td>
<td>Eliminar la lista de control de acceso (ACL) de un objeto</td>
</tr>
<tr class="odd">
<td>S3: Poner cubos de cordones</td>
<td>Establece la configuración CORS de su balde</td>
</tr>
<tr class="even">
<td>S3: Eliminar los cordones del cubo</td>
<td>Elimina el conjunto de información de configuración CORS del depósito</td>
</tr>
<tr class="odd">
<td>S3: Cifrado Putbucket</td>
<td>Configurar el cifrado predeterminado y las claves de bucket de Amazon S3 para un
balde existente</td>
</tr>
<tr class="even">
<td>S3: Eliminar el cifrado de cubos</td>
<td>Restablece el cifrado predeterminado del depósito como del lado del servidor
cifrado con claves gestionadas por Amazon S3 (SSE-S3</td>)
</tr>
</tbody>
</table>

**Otras acciones**

Estas son otras acciones que debería destacar durante su
investigación

<table>
<colgroup>
<col style="width: 43%" />
<col style="width: 56%" />
</colgroup>
<thead>
<tr class="header">
<th>KMS: Desactivar clave</th>
<th>Establece el estado de una clave KMS en Desactivado. Este cambio temporal
impide el uso de la clave KMS para operaciones criptográficas</th>.
</tr>
</thead>
<tbody>
<tr class="odd">
<td>KMS: Planificar la eliminación de claves</td>
<td>Programa la eliminación de una clave KMS.</td>
</tr>
<tr class="even">
<td>KMS: Política de PutKey</td>
<td>Adjunta una política clave a la clave KMS especificada. Las políticas clave son las
forma principal de controlar el acceso a las claves KMS.</td>
</tr>
<tr class="odd">
<td>KMS: Crear clave</td>
<td>Crea una clave KMS única gestionada por el cliente en su cuenta de AWS y
Región.</td>
</tr>
<tr class="even">
<td>EC2: Describir las instancias</td>
<td>Describe las instancias EC2 de su cuenta.</td>
</tr>
<tr class="odd">
<td>EC2: Ejecutar instancias</td>
<td>Lanza instancias EC2 en su cuenta.</td>
</tr>
<tr class="even">
<td>EC2: Crear VPC</td>
<td>Crea una VPC en su cuenta.</td>
</tr>
<tr class="odd">
<td>EC2: Describa los VPC</td>
<td>Describe los VPC de su cuenta.</td>
</tr>
<tr class="even">
<td>EC2: Crear un grupo de seguridad</td>
<td>Crea un grupo de seguridad. Un grupo de seguridad actúa de forma virtual
firewall para su instancia para controlar el tráfico entrante y saliente</td>.
</tr>
<tr class="odd">
<td>EC2: Eliminar grupo de seguridad</td>
<td>Elimina un grupo de seguridad.</td>
</tr>
<tr class="even">
<td>RDS: creó una instancia de base de datos</td>
<td>Crea una instancia de base de datos RDS.</td>
</tr>
<tr class="odd">
<td>RDS: instancia base de datos eliminada</td>
<td>Elimina una instancia de base de datos de RDS.</td>
</tr>
</tbody>
</table>