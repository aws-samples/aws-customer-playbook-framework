# Guía de respuesta a incidentes: EC2 forense
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

[[_TOC_]]

## Puntos de contacto

Autor: `Nombre del autor`
Aprobador: `Nombre del aprobador`
Última fecha aprobada:

## Resumen ejecutivo
Este manual describe el proceso para realizar análisis forenses en instancias EC2 que se han identificado como maliciosas o tienen indicadores de compromiso que justifican una investigación adicional.

También es una práctica recomendada llevar a cabo la investigación en la misma región de AWS en la que se produjo el evento, en lugar de replicar los datos en otra región de AWS. Recomendamos esta práctica principalmente debido al tiempo adicional necesario para transferir los datos entre regiones. Para cada región de AWS en la que decida operar, asegúrese de que tanto el proceso de respuesta a incidentes como los respondedores cumplan las leyes de privacidad de datos pertinentes. Si necesita mover datos entre regiones de AWS, tenga en cuenta las implicaciones legales de mover datos entre jurisdicciones. En general, es una buena práctica mantener los datos dentro de la misma jurisdicción nacional.

Para obtener más información, consulte la [AWS Security Incident Response Guide] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

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
1. [**PREPARACION**] Identificar las mejores prácticas antes de ejecutar procedimientos forenses
2. [**PREPARACIÓN**] Crear una VPC forense
3. [**PREPARACIÓN**] Agregar VPC Endpoint para SSM
4. [**PREPARACION**] Crear una lista de control de acceso a la red para la VPC forense
5. [**PREPARACIÓN**] Crear una AMI dorada de EC2 forense
6. [**PREPARACION**] Implementar un programa de formación para identificar y evaluar las capacidades forenses en la nube
7. [**PREPARACIÓN**] Identificar, documentar y probar procedimientos de escalado
8. [**DETECCIÓN Y ANALÍSIS**] Crear una Amazon EBS Snapshot
9. [**DETECCIÓN Y ANALISIS**] Compartir una Amazon EBS Snapshot
10. [**DETECCIÓN Y ANALÍSIS**] Compartir una instantánea sin cifrar mediante la consola
11. [**DETECCIÓN Y ANALÍSIS**] Utilizar una instantánea sin cifrar que se haya compartido de forma privada contigo
12. [**DETECCIÓN Y ANALÍSIS**] Compartir una instantánea cifrada mediante la consola
13. [**DETECCIÓN Y ANALÍSIS**] Utilizar una instantánea cifrada que se ha compartido contigo
14. [**DETECCIÓN Y ANALISIS**] Crear un EC2 forense a partir de Forensics Golden AMI
15. [**DETECCIÓN Y ANALISIS**] Montar volumen sospechoso de EBS en EC2 forense
16. [**DETECCIÓN Y ANALÍSIS**] Ejecutar procesos forenses
17. [**CONTAINMENT**] Aislar inmediatamente los recursos afectados
18. [**RECOVERY**] Realizar procesos de recuperación según corresponda

***Los pasos de respuesta siguen el ciclo de vida de la respuesta a incidentes de [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

! [Imagen] (/images/nist_life_cycle.png) ***

### Clasificación y manejo de incidentes
* **Tácticas, técnicas y procedimientos**: Herramientas: Análisis forense
* **Categoría**: Análisis forense
* **Recurso**: EC2, VPC
* **Indicadores**: Inteligencia sobre amenazas cibernéticas, aviso de terceros, métricas de Cloudwatch, AWS Shield
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
### [Prácticas recomendadas] (https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* Activar AWS CloudTrail en todas las regiones
No limite el registro de CloudTrail en las regiones de AWS que utiliza activamente. Activar CloudTrail en todas las regiones de AWS le permitirá identificar comportamientos inusuales con mayor facilidad, como los servicios de AWS que se aprovisionan desde una región de AWS que su organización no utiliza.

* Proteger información de registro
Compruebe que los registros de auditoría estén activados y activos para los componentes del sistema, y haga copias de seguridad periódicas de ellos. Considere almacenarlos en una ubicación que requiera credenciales diferentes, de modo que los atacantes no pueden eliminar los archivos de registro que puedan proporcionar pruebas de su actividad maliciosa.

* Nunca ignore la comunicación de abuso de AWS
AWS enviará automáticamente un correo electrónico a su dirección de correo electrónico registrada cuando se presente un caso de abuso. Responda inmediatamente y considere la posibilidad de configurar una dirección de respuesta de correo electrónico dedicada. Cuanto más rápido responda a un incidente de seguridad, mejor podrá limitar su impacto en su negocio. Es bueno tener una [lista de distribución para el contacto alternativo para la seguridad] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts) en la cuenta que consta de varias partes interesadas para que los mensajes no se «atasquen» en una sola bandeja de entrada del empleado (por ejemplo, en vacaciones, terminación, enferma, etc.).

### Crear una VPC forense
**NOTA**: Se recomienda tener una configuración de VPC forense dentro de cada región en la que sea necesario realizar análisis forense contra los recursos de producción.

1. Abra la [consola de Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Sus VPC, Crear VPC.
1. Especifique los siguientes detalles de la VPC según sea necesario.
* Etiqueta de nombre: si lo desea, proporcione un nombre para su VPC. Al hacerlo, se crea una etiqueta con una clave de Nombre y el valor que especifique.
* Bloque IPv4 CIDR: especifique un bloque IPv4 CIDR para la VPC. El bloque CIDR más pequeño que puede especificar es /28 y el más grande es /16. Le recomendamos que especifique un bloque CIDR a partir de los rangos de direcciones IP privados (no enrutables públicamente) tal como se especifica en RFC 1918; por ejemplo, 10.0.0.0/16 o 192.168.0.0/16.
* Nota: Puede especificar un rango de direcciones IPv4 enrutables públicamente. Sin embargo, actualmente no admitimos el acceso directo a Internet desde bloques CIDR enrutables públicamente en una VPC.
* Las instancias de Windows no pueden arrancar correctamente si se inician en una VPC con rangos de 224.0.0.0 a 255.255.255.255 (rangos de direcciones IP de clase D y clase E).
* Bloque CIDR IPv6: asocie opcionalmente un bloque CIDR IPv6 a la VPC. Elija una de las siguientes opciones y, a continuación, seleccione Seleccionar CIDR:
* Bloque CIDR IPv6 proporcionado por Amazon: solicita un bloqueo CIDR IPv6 del grupo de direcciones IPv6 de Amazon. En Network Border Group, seleccione el grupo desde el que AWS anuncia las direcciones IP.
* CIDR IPv6 de mi propiedad: (BYOIP) Asigna un bloque CIDR IPv6 de su grupo de direcciones IPv6. En Grupo, elija el grupo de direcciones IPv6 desde el que desea asignar el bloque CIDR IPv6.
* Arrendamiento: Seleccione una opción de arrendamiento.
* Seleccione Dedicado para asegurarse de que las instancias lanzadas en esta VPC sean instancias de arrendamiento dedicadas, independientemente del atributo de arrendamiento especificado en el lanzamiento.
* Seleccione Predeterminado para asegurarse de que las instancias lanzadas en esta VPC utilicen el atributo de arrendamiento especificado en el lanzamiento.
* (Opcional) Añadir o quitar una etiqueta.
* Selecciona Crear.

### [Agregar VPC Endpoint para SSM] (https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. Compruebe que SSM Agent esté instalado en la instancia.
2. Cree un perfil de instancia de AWS Identity and Access Management (IAM) para Systems Manager. Puede crear un nuevo rol o agregar los permisos necesarios a un rol existente.
3. Adjunte el rol de IAM a la instancia EC2 privada.
4. Abra la consola de Amazon EC2 y, a continuación, seleccione la instancia. En la pestaña Descripción, anote el ID de VPC y el Subnet ID.
5. Cree un endpoint de VPC para Systems Manager.
* En Nombre del servicio, seleccione com.amazonaws. [region] .ssm (por ejemplo, com.amazonaws.us-east-1.ssm). Para obtener una lista completa de los códigos de región, consulte Regiones disponibles.
* Para la VPC, elige el ID de VPC de tu instancia.
* En Subnets, elija un Subnet ID en la VPC. Para obtener alta disponibilidad, elija al menos dos subredes de distintas zonas de disponibilidad de la región.
* Nota: Si tiene más de una subred en la misma zona de disponibilidad, no es necesario crear VPC endpoints para las subredes adicionales. Cualquier otra subred de la misma zona de disponibilidad puede acceder a la interfaz y utilizarla.
* En Habilitar nombre DNS, seleccione Habilitar para este endpoint. Para obtener más información, consulte DNS privado para endpoints de interfaz.
* En Grupo de seguridad, seleccione un grupo de seguridad existente o cree uno nuevo. El grupo de seguridad debe permitir el tráfico HTTPS entrante (puerto 443) de los recursos de la VPC que se comunican con el servicio.
* Si ha creado un nuevo grupo de seguridad, abra la consola de VPC, elija Grupos de seguridad y, a continuación, seleccione el nuevo grupo de seguridad. En la pestaña Reglas entrantes, elija Editar reglas entrantes. Agregue una regla con los siguientes detalles y, a continuación, elija Guardar reglas:
* En Tipo, elija HTTPS.
* En Source, elija el VPC CIDR. Para una configuración avanzada, puede permitir el CIDR de subredes específicas que utilizan las instancias EC2.
* Anote el ID del grupo de seguridad. Usarás este ID con los demás puntos finales.
6. Cree políticas para los extremos de la interfaz de VPC para AWS Systems Manager.
* Repita el paso 5 con el siguiente cambio: En Nombre del servicio, seleccione com.amazonaws. [región] .ec2 mensajes.
* Repita el paso 5 con el siguiente cambio: En Nombre del servicio, seleccione com.amazonaws. [región] .ssmmessages. Debe hacerlo si desea utilizar el Session Manager.

### Crear una lista de control de acceso a la red para la VPC forense
Este proceso aislará la VPC para que el tráfico no pueda entrar ni salir de las instancias contenidas en

1. Abra la [consola de Amazon VPC] (https://console.aws.amazon.com/vpc/)
1. En el panel de navegación, elija Seguridad, Network ACLs.
1. Seleccione la casilla de verificación situada junto a la VPC forense
1. En la parte superior derecha, selecciona el menú desplegable Acciones y Editar reglas entrantes.
1. Cambiar la regla 100 de `Todo el trafico` a `SSH`
1. Seleccione Guardar cambios
1. Repita estos pasos para Reglas salientes

### Crear una AMI dorada de EC2 forense
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Seleccione `Iniciar instancia`
1. En la barra de búsqueda escribe `Ubuntu` y presiona enter
1. Seleccione `Ubuntu Server 18.04 LTS`
1. Elija un tipo de instancia
* Nota: Para fines forenses, se recomienda elegir una instancia de tipo M5
1. Seleccione `Siguiente: Configurar detalles de instancia`
* Red: elija la red de VPC forense
* Asignación automática de IP pública: deshabilitar
* Función de IAM: Seleccione un rol apropiado para fines forenses
* Habilitar protección contra terminación: asegúrese de que esta casilla esté marcada
1. Seleccione `Siguiente: Agregar almacenamiento`
* Seleccione `Añadir nuevo volumen` y cree un volumen que tenga el tamaño suficiente para gestionar los datos forenses que se capturan. Un buen valor inicial es de 100 GB, pero basar este valor en sus necesidades y experiencias organizativas
1. Seleccione `Siguiente: Añadir etiquetas`
1. Seleccione `Revisar y lanzar`
1. Seleccione `Lanzamiento`
1. Inicie sesión en las instancias de EC2 mediante EC2 Session Manager
1. Instala tus herramientas forenses en la instancia. A continuación se presentan un par de ejemplos de herramientas forenses de código abierto:
1. Instalación del SANS SIFT Toolkit
* sudo su - ubuntu
* sudo curl -Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
* sudo chmod +x /usr/local/bin/sift
* sudo /usr/local/bin/sift install —mode=server —user=ubuntu
* sudo /usr/local/bin/sift upgrade
1. Instalación de Google Rapid Response
* sudo apt install mysql-server
* sudo mysql_secure_installation
* create database grr; grant all privileges on grr.* to grr @localhost identified by 'password'; flush privileges; @localhost
* wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
* sudo apt install. /grr-server_3.2.4-6_amd64.deb
* sudo systemctl restart grr-server
* sudo systemctl status grr-server
1. Para obtener más información, consulte [cómo instalar y configurar clientes GRR en Ubuntu 18.04] (https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/)
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Navegar a instancias
1. Haga clic con el botón derecho en la instancia que desea utilizar como base para su AMI y elija Crear imagen en el menú contextual.
1. En el cuadro de diálogo Crear imagen, escriba un nombre y una descripción únicos y, a continuación, elija Crear imagen.
* De forma predeterminada, Amazon EC2 cierra la instancia, toma instantáneas de cualquier volumen adjunto, crea y registra la AMI y, a continuación, reinicia la instancia.
* Elija Sin reiniciar si no desea que se apague la instancia.
* Si elige Sin reiniciar, no podemos garantizar la integridad del sistema de archivos de la imagen creada.

La AMI puede tardar unos minutos en crearse. Una vez creado, aparecerá en la vista AMI de AWS Explorer. Para mostrar esta vista, haga doble clic en el nodo Amazon EC2 | AMI de AWS Explorer. Para ver sus AMI, en la lista desplegable Visualización, elija Propiedad mía. Es posible que tenga que elegir Actualizar para ver su AMI. Cuando aparece la AMI por primera vez, puede estar en estado pendiente, pero después de unos instantes, pasa a un estado disponible.

### Capacitación
- «¿Cuál es la expectativa de conocimientos para el personal con responsabilidades forenses? (conocimientos/aptitudes y aptitudes)»
- `¿ Qué formación y procedimientos forenses recibe mi personal? `
- `¿ Qué requisitos reglamentarios para la investigación forense se aplican a mi empresa? `
- `¿ Qué formación está impartida para que los analistas de la empresa se familiaricen con la AWS API (entorno de línea de comandos), S3, RDS y otros servicios de AWS? `

**Necesita añadir aquí más contexto y elementos, ya que el forense es una habilidad, no solo un libro de jugadas

## Procedimientos de escalada
- `Necesito una decisión empresarial sobre cuándo debe llevarse a cabo la investigación forense de EC2`
- `¿ Quién monitorea los registros y alertas, los recibe y actúa en función de cada uno? `
- `A quién se le notifica cuando se descubre una alerta? `
- `¿ Cuándo se involucran las relaciones públicas y los asuntos legales en el proceso? `
- `¿ Cuándo se pondría en contacto con AWS Support para obtener ayuda? `

## Detección y análisis
### Posibles fuentes de alertas
* Facturación
* CloudWatch (métrica de EC2/regla de evento)
* Configuración
* Servicio de guardia
* Inspector
* Macie
* Centro de seguridad
* Asesor de confianza
* Notificación externa

### Protección de datos
Debería**SIEMPRE **:
* Presentar pruebas de SOLO LECTURA
* Las evidencias deben modificarse **NEVER**
* Crear copias de pruebas y trabajar SOLAMENTE a partir de ellos
* **NEVER** trabajar desde evidencia de origen (acceso directo) para el análisis
* Montar todas las pruebas DE SOLO LECTURA
* Realizar un seguimiento de la OMS accedió a QUÉ y CUÁNDO
* Tomar abundantes notas de investigación de:
* Lo que analizó (qué dato)
* Cuando lo analizaste (fecha/hora)
* Qué ha realizado (comandos/procedimiento/proceso específicos)
* Lo que has encontrado (resultados)

### Adquisición de metadatos
Capturar información relacionada con la instancia que puede ser útil para la investigación
* Tipo de instancia/ID
* Direcciones IP
* Grupo (s) de seguridad
* VPC/Subnet ID
* Región
* ID DE AMI
* Hora de lanzamiento

### Adquisición de memoria
Se recomienda utilizar una utilidad de terceros para adquirir memoria de instancias en ejecución.
Es esencial capturar la memoria **ANTES DE Aislamiento/apagado del sistema para capturar todos los datos volátiles (y valiosos) disponibles

### [Crear una Amazon EBS Snapshot] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Elija Instantáneas en Elastic Block Store en el panel de navegación.
1. Elija Crear instantánea.
1. En Seleccionar tipo de recurso, elija Volumen.
1. En Volumen, selecciona el volumen.
1. (Opcional) Introduzca una descripción para la instantánea.
1. (Opcional) Elija Agregar etiqueta para añadir etiquetas a la instantánea. Para cada etiqueta, proporciona una clave de etiqueta y un valor de etiqueta.
1. Elija Crear instantánea.

### [Compartir una Amazon EBS Snapshot] (https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### Compartir una instantánea sin cifrar mediante la consola
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Elija Instantáneas en el panel de navegación.
1. Seleccione la instantánea y, a continuación, elija Acciones, Modificar permisos.
1. Haga pública la instantánea o compártala con cuentas de AWS específicas de la siguiente manera:
* Para hacer pública la instantánea, elija Público.
* Esta opción no es válida para instantáneas cifradas o instantáneas con un código de producto de AWS Marketplace.
* Para compartir la instantánea con una o más cuentas de AWS, elija Privado, introduzca el ID de cuenta de AWS (sin guiones) en el número de cuenta de AWS y elija Agregar permiso. Repita el procedimiento para cualquier cuenta de AWS adicional.
1. Selecciona Guardar.
#### Para utilizar una instantánea sin cifrar que se ha compartido de forma privada contigo
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Elija Instantáneas en el panel de navegación.
1. Elija el filtro Instantáneas privadas.
1. Busque la instantánea por ID o descripción. Puede utilizar esta instantánea como lo haría con cualquier otra; por ejemplo, puede crear un volumen a partir de la instantánea o copiarla en otra región.

#### Compartir una instantánea cifrada mediante la consola
1. Abra la [consola de AWS KMS] (https://console.aws.amazon.com/kms)
1. Para cambiar la región de AWS, utilice el selector de región en la esquina superior derecha de la página.
1. Seleccione Claves administradas por el cliente en el panel de navegación.
1. En la columna Alias, elija el alias (enlace de texto) de la clave administrada por el cliente que utilizó para cifrar la instantánea. Los detalles clave se abren en una nueva página.
1. En la sección Política clave, verá la vista de política o la vista predeterminada. La vista de políticas muestra el documento de política clave. La vista predeterminada muestra secciones para administradores de claves, eliminación de claves, uso de claves y otras cuentas de AWS. La vista predeterminada se muestra si ha creado la política en la consola y no la ha personalizado. Si la vista predeterminada no está disponible, tendrá que editar manualmente la política en la vista de políticas. Para obtener más información, consulte Visualización de una política clave (consola) en la AWS Key Management Service Developer Guide.
Utilice la vista de política o la vista predeterminada, según la vista a la que pueda acceder, para agregar uno o varios ID de cuenta de AWS a la política, de la siguiente manera:
* (Vista predeterminada) Desplácese hacia abajo hasta Otras cuentas de AWS. Elija Agregar otras cuentas de AWS e introduzca el ID de cuenta de AWS según se le solicite. Para agregar otra cuenta, elija Agregar otra cuenta de AWS e introduzca el ID de cuenta de AWS. Cuando haya agregado todas las cuentas de AWS, elija Guardar cambios.
* (Vista de políticas) Elija Editar. Agregue uno o varios ID de cuenta de AWS a las siguientes declaraciones: «Permitir el uso de la clave» y «Permitir adjuntar recursos persistentes». Selecciona Guardar cambios. En el siguiente ejemplo, se agrega el ID de cuenta de AWS 444455556666 a la política.
```
{
«Sid»: «Permitir el uso de la llave»,
«Efecto»: «Permitir»,
«Principal»: {"AWS»: [
«arn:aws:iam: 111122223333: usuario/usuario de CMK»,
«arn:aws:iam: 444455556666: root»
]},
«Acción»: [
«KMS: cifrar»,
«KMS: descifrar»,
«KMS: volver a cifrar*»,
«KMS: clave de datos generada*»,
«KMS: descripción clave»
],
«Recurso»: «*»
},
{
«Sid»: «Permitir el adjunto de recursos persistentes»,
«Efecto»: «Permitir»,
«Principal»: {"AWS»: [
«arn:aws:iam: 111122223333: usuario/usuario de CMK»,
«arn:aws:iam: 444455556666: root»
]},
«Acción»: [
«KMS: Crear subvención»,
«KMS: lista de subvenciones»,
«KMS: revocar la subvención»
],
«Recurso»: «*»,
«Condición»: {"Bool»: {"kms:GrantisForAWSResource»: true}}
}
```
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Elija Instantáneas en el panel de navegación.
1. Seleccione la instantánea y, a continuación, elija Acciones, Modificar permisos.
1. Para cada cuenta de AWS, introduzca el ID de cuenta de AWS en el número de cuenta de AWS y elija Agregar permiso. Cuando haya agregado todas las cuentas de AWS, elija Guardar.

#### Para utilizar una instantánea cifrada que se ha compartido contigo
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. Elija Instantáneas en el panel de navegación.
1. Elija el filtro Instantáneas privadas. Si lo desea, agregue el filtro Cifrado.
1. Busque la instantánea por ID o descripción.
1. Seleccione la instantánea y elija Acciones, Copiar.
1. (Opcional) Seleccione una región de destino.
1. La copia de la instantánea se cifra mediante la clave que se muestra en la clave maestra. De forma predeterminada, la clave seleccionada es la CMK predeterminada de tu cuenta. Para seleccionar una CMK administrada por el cliente, haga clic dentro del cuadro de entrada para ver una lista de claves disponibles.
1. Selecciona Copiar.

### Crear un EC2 forense a partir de Forensics Golden AMI
Cree una nueva instancia forense utilizando la AMI de oro de Forensics creada anteriormente

### Montar volumen sospechoso de EBS en forense EC2
1. Abra la [consola de Amazon EC2] (https://console.aws.amazon.com/ec2/)
1. En el panel de navegación, elija Elastic Block Store, Volumes.
1. Seleccione un volumen disponible y elija Acciones, Adjuntar volumen.
1. Por ejemplo, empiece a escribir el nombre o el ID de la instancia.
1. Seleccione la instancia de la lista de opciones (solo se muestran las instancias que se encuentran en la misma zona de disponibilidad que el volumen).
1. En Dispositivo, puede conservar el nombre de dispositivo sugerido o escribir otro nombre de dispositivo compatible.
* Para obtener más información, consulte [Nombres de dispositivos en instancias Linux] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html).
1. Selecciona Adjuntar.
1. Conéctate a tu instancia y monta el volumen.
* Para obtener más información, consulte [Hacer que un volumen de Amazon EBS esté disponible para su uso en Linux] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html).

### Ejecutar procesos forenses
* `¿ Qué resultados forenses busca la empresa? `
* `¿ Qué se incluirá en el informe final? `
* `A quién va el informe? `
* `¿ Existen requisitos legales o reglamentarios que identifiquen la información requerida de una investigación forense? `
* `¿ Hay personal en la empresa que esté debidamente capacitado, licenciado/certificado para realizar análisis forense? `
* `¿ Existen requisitos de retención para registros, datos o pruebas? Potencialmente, busque Glacier Vaults para respaldar`
* Un ejemplo de [Análisis forense digital de instancias EC2 de Amazon Linux] (https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) está disponible en SANS institute.

## Contención
### Aislar inmediatamente los recursos afectados
**NOTA**: Asegúrese de que dispone de un proceso para solicitar una escalada y aprobación para aislar los recursos y garantizar que se lleve a cabo primero un análisis de impacto empresarial sobre cómo el aislamiento afectará a las operaciones actuales y a los flujos de ingresos.

### Retirada de instancias
Habilitar protecciones de terminación
* [Deshabilitar la finalización de la instancia (mediante Console/API/CLI)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
1. Establecer el comportamiento de apagado de instancias en Detener (en lugar de finalizar)
1. Deshabilitar «DeleteOnTermination» para todos los volúmenes de instancias
* [Eliminar (separar) del Auto-Scaling group] (https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
1. Abra la consola de Amazon EC2 Auto Scaling en https://console.aws.amazon.com/ec2autoscaling/
1. Active la casilla de verificación situada junto a su grupo Auto Scaling.
1. Se abre un panel dividido en la parte inferior de la página de grupos de Auto Scaling, que muestra información sobre el grupo seleccionado.
1. En la pestaña Administración de instancias, en Instancias, seleccione una instancia y elija Acciones, Desconectar.
1. En el cuadro de diálogo Separar instancia, active la casilla de verificación para lanzar una instancia de reemplazo o déjala sin marcar para reducir la capacidad deseada. Selecciona Separar instancia.
* [Anular el registro de los equilibradores de carga] (https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
1. Abra la consola de Amazon EC2 en https://console.aws.amazon.com/ec2/
1. En el panel de navegación, en BALANCEO DE CARGA, elija Balanceadores de carga.
1. Selecciona el balanceador de carga.
1. En el panel inferior, selecciona la pestaña Instancias.
1. Elija Editar instancias.
1. Seleccione la instancia que desea registrar con el equilibrador de carga.
1. Selecciona Guardar.
1. Cree un grupo de seguridad *nuevo* que bloquee todo el tráfico de entrada y salida; asegúrese de eliminar la regla predeterminada `permitir todo `para el tráfico de salida
* Adjuntar el grupo de seguridad *nuevo* a las instancias afectadas

## Erradicación
No hay pasos para este proceso aplicable a este runbook.

## Recuperación
Una vez concluidas las actividades forenses y tras la publicación del informe final, debe terminar la instancia de EC2 forense y eliminar la instantánea de EBS **A MENOS QUE exista un requisito legal o reglamentario para mantener los datos.

## Lecciones aprendidas
`Este es un lugar para añadir elementos específicos de su empresa que no necesariamente necesitan «arreglos», pero que es importante saber al ejecutar este libro de jugadas junto con los requisitos operativos y comerciales. `

## Elementos atrasados resueltos
- Como Respondedor de Incidentes, necesito un runbook para llevar a cabo el análisis forense de EC2
- Como Respondedor de Incidentes, necesito una decisión empresarial sobre cuándo deben llevarse a cabo los análisis forenses de EC2
- Como Respondedor de Incidentes, necesito tener habilitado el registro en todas las regiones habilitadas independientemente de la intención de uso
- Como Respondedor de Incidentes, necesito asegurarme de que solo se utilicen las AMI aprobadas
- Como Respondedor de Incidentes, necesito poder detectar la minería de criptomonedas en mis instancias EC2 existentes
- Como Respondedor de Incidentes, necesito un método preparado para eliminar el número masivo de instancias

## Elementos atrasados actuales