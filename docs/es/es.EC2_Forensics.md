**Manual de estrategias de respuesta a incidentes: EC2 Forensics**

Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas actuales de productos de Amazon Web Services (AWS) a la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación de forma independiente sobre la información presentada en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «as is» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, representación, compromiso contractual, condición o garantía de parte de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y las obligaciones de AWS con sus clientes están controladas por los acuerdos de AWS, y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2024 Amazon Web Services, Inc. o sus filiales. Todos los derechos
reservados. Este trabajo está licenciado bajo una licencia internacional
Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo
de cliente de AWS disponible en <http://aws.amazon.com/agreement> u otro
acuerdo por escrito entre el cliente y Amazon Web Services, Inc. o
Amazon Web Services EMEA SARL o ambos. 

**Puntos de contacto**

Autor: Nombre del autor\
Aprobador: Nombre del aprobador\
Ultima fecha de aprobación:

**Resumen ejecutivo**

Este manual describe el proceso para realizar análisis forenses en
instancias de EC2 que se han identificado como maliciosas o que tienen
indicadores de riesgo que justifican una investigación adicional.

También es una práctica recomendada realizar la investigación en la
misma región de AWS en la que se produjo el evento, en lugar de replicar
los datos en otra región de AWS. Recomendamos esta práctica
principalmente por el tiempo adicional necesario para transferir los
datos entre regiones. Para cada región de AWS en la que decida operar,
asegúrese de que tanto el proceso de respuesta ante incidentes como los
respondedores cumplan con las leyes de privacidad de datos pertinentes.
Si necesita mover datos entre regiones de AWS, tenga en cuenta las
implicaciones legales de mover datos entre jurisdicciones.

Para obtener más información, consulte la Guía de respuesta a incidentes
de seguridad de AWS [AWS Security Incident Response
Guide](https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html)

**Objetivos**

Durante la ejecución del manual de estrategias, céntrese en los
***resultados deseados*** y tome notas para mejorar las capacidades de
respuesta ante incidentes.

**Determine:**

-   **Vulnerabilidades explotadas**

-   **Exploits y herramientas observadas**

-   **Intención del actor**

-   **Atribución del actor**

-   **Daños infligidos al medio ambiente y a la empresa**

**Recuperar:**

-   **Volver a la configuración original y asegurada**

**Mejorar los components de Perspectiva de seguridad de CAF:**

[AWS Cloud Adoption Framework: Perspectiva de
seguridad](https://d1.awsstatic.com/whitepapers/es_ES/aws-cloud-adoption-framework_XL.pdf)

-   **Directiva**

-   **Detectiva**

-   **Responsiva**

-   **Preventativa**

**Pasos de respuesta**

1.  \[**PREPARACIÓN**\] Identificar las mejores practices antes de
    ejecutar los procedimients forenses

2.  \[**PREPARACIÓN**\] Creación de una VPC forense

3.  \[**\[PREPARACIÓN\]** Agregar un punto de enlace de VPC para SSM

4.  \[**\[PREPARACIÓN\]** Crear una lista de control de acceso a la red
    para la VPC forense

5.  \[**\[PREPARACIÓN\]** Crear una EC2 Golden AMI forense

6.  \[**\[PREPARACIÓN\]** Implementar un programa de capacitación para
    identificar y evaluar las capacidades de análisis forense de la nube

7.  \[**\[PREPARACIÓN\]** Identificar, documentar y probar los
    procedimientos de escalamiento

8.  \[**DETECCIÓN Y ANÁLISIS**\] Crear una Amazon EBS Snapshot

9.  \[**DETECCIÓN Y ANÁLISIS**\] Compartir una Amazon EBS Snapshot

10. \[**DETECCIÓN Y ANÁLISIS**\] Compartir una snapshot sin cifrar
    utilizando la consola

11. \[**DETECCIÓN Y ANÁLISIS**\] Usar una snapshot sin cifrar que se
    haya compartido con usted de forma privada

12. \[**DETECCIÓN Y ANÁLISIS**\] Compartir una instantánea sin cifrar
    mediante la consola

13. \[**DETECCIÓN Y ANÁLISIS**\] Usar una snapshot sin cifrar que se
    haya compartido con usted

14. \[**DETECCIÓN Y ANÁLISIS**\] Crear un EC2 forense a partir de
    Forensics Golden AMI

15. \[**DETECCIÓN Y ANÁLISIS**\] Montar el volumen de EBS sospechoso en
    Forensics EC2

16. \[**DETECCIÓN Y ANÁLISIS**\] Ejecutar los procesos forenses

17. \[**CONTENCIÓN**\] Aislar los recursos afectados inmediatamente

18. \[**RECUPERACIÓN**\] Realizar los procesos de recuperación según
    corresponda

> *\*Los pasos de respuesta siguen el ciclo de vida de respuesta ante
> incidentes de la publicación especial del* [NIST Special Publication 800-61r2 Computer Security Incident Handling Guide](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)

**Clasificación y manejo de incidents**

-   **Tácticas, técnicas y procedimientos: Herramientas:** Forense

-   **Categoría:** Forense

-   **Recurso**: EC2, VPC

-   **Indicadores**: Inteligencia sobre ciberamenazas, aviso de
    terceros, métricas de Cloudwatch, AWS Shield

-   **Fuentes de registro:** Endpoint

-   **Equipos**: Centro de operaciones de seguridad (SOC),
    investigadores forenses, ingeniería en la nube

**Proceso de manejo de incidentes**

**El proceso de respuesta a incidentes consta de las siguientes
etapas:**

\[PREPARACIÓN\]

-   Detección y análisis

-   Contención y erradicación

-   Recuperación

-   Actividad posterior al incidente

**\[PREPARACIÓN\]**

[**Mejores Practicas**](https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)

-   Active AWS CloudTrail en todas las regions. No limite el registro de
    CloudTrail a las regiones de AWS que utilice activamente. Activar
    CloudTrail en todas las regiones de AWS le permitirá identificar
    comportamientos inusuales con mayor facilidad, como el
    aprovisionamiento de servicios de AWS desde una región de AWS que su
    organización no utiliza.

-   Proteger la información del registro. Verifique que las audit trails
    estén habilitadas y activas para los componentes del sistema, y
    realice copias de seguridad de las mismas Considere guardarlos en
    una ubicación que requiera credenciales diferentes, de modo que los
    atacantes no puedan eliminar los archivos de registro que puedan
    proporcionar pruebas de su actividad maliciosa.

-   No ignorar nunca las comunicaciones de AWS que se enviarán
    automáticamente desde un correo electrónico a su dirección de correo
    electrónico registrada cuando se presente un caso de abuso. Responda
    de inmediato y considere la posibilidad de configurar una dirección
    de respuesta por correo electrónico dedicada. Cuanto más rápido
    responda a un incidente de seguridad, mejor podrá limitar su impacto
    en su negocio. Es bueno tener una lista de distribución para el
    contacto alternativo por motivos de seguridad en la cuenta que esté
    formada por varias partes interesadas para que los mensajes no
    queden «atascados» en la bandeja de entrada de un solo empleado (por
    ejemplo, de vacaciones, despido, enfermedad, etc.).

**Creación de una VPC forense**

**NOTA: Se recomienda tener una configuración de VPC forense en cada
región en la que sera necesario realizar análisis forenses contra los
recursos de producción.**

1.  Abra la [Consola VPC de
    Amazon](https://console.aws.amazon.com/vpc/)

2.  En el panel de navegación, seleccione Your VPC, Create VPC.

3.  Especifique los siguientes detalles de VPC según sea necesario.

    -   Tag de nombre: Si lo desea, proporcione un nombre para la VPC.
        Al hacerlo, se crea una etiqueta con una clave de Name y el
        valor que especifique.

    -   Bloque CIDR IPv4: especifique un bloque CIDR IPv4 para la VPC.
        El bloque CIDR más pequeño que puede especificar es /28 y el más
        grande es /16. Le recomendamos que especifique un bloque de CIDR
        de los rangos de direcciones IP privadas (no enrutables
        públicamente) como se especifica en RFC 1918; por ejemplo,
        10.0.0.0/16 o 192.168.0.0/16.

        -   Nota: Puede especificar un rango de direcciones IPv4
            enrutables públicamente. Sin embargo, actualmente no
            admitimos el acceso directo a Internet desde bloques CIDR
            enrutables públicamente en una VPC.

        -   Las instancias de Windows no pueden iniciarse correctamente
            si se lanzan en una VPC con rangos de 224.0.0.0 a
            255.255.255.255 (rangos de direcciones IP de clase D y clase
            E).

    -   Bloque CIDR IPv6: si lo desea, asocie un bloque CIDR IPv6 a la
        VPC. Elija una de las siguientes opciones y, a continuación,
        seleccione Seleccionar CIDR:

        -   Bloque CIDR IPv6 proporcionado por Amazon: solicita un
            bloque CIDR IPv6 del grupo de direcciones IPv6 de Amazon.
            Para Network Border Group, seleccione el grupo desde el que
            AWS anuncia las direcciones IP.

        -   CIDR IPv6 de mi propiedad: (BYOIP) asigna un bloque de CIDR
            IPv6 de su grupo de direcciones IPv6. Para Pool, elija el
            conjunto de direcciones IPv6 desde el que desea asignar el
            bloque de CIDR IPv6..

        -   Tenancy: Seleccione una opción de tenancy,

            -   Seleccione Dedicada para asegurarse de que las
                instancias lanzadas en esta VPC sean instancias de
                tenencia dedicadas, independientemente del atributo de
                tenencia especificado en el lanzamiento.

            -   Seleccione Predeterminado para garantizar que las
                instancias lanzadas en esta VPC usen el atributo de
                tenencia especificado en el lanzamiento.

    -   (Opcional) Añada o quite un tag.

    -   Elija crear.

[**Agregar punto de enlace de VPC para
SSM**](https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1.  Compruebe que el agente de SSM esté instalado en la instancia.

2.  Cree un perfil de instancia de AWS Identity and Access Management
    (IAM) para Systems Manager. Puede crear una nueva función o agregar
    los permisos necesarios a una función existente.

3.  Asocie la función de IAM a la instancia privada de EC2.

4.  Abra la consola de Amazon EC2 y, a continuación, seleccione la
    instancia. En la pestaña Descripción, anote el ID de VPC y el ID de
    subred.

5.  Cree un punto de enlace de VPC para Systems Manager.

    -   Para Nombre del servicio, seleccione com.amazonaws. \[region\]
        .ssm (por ejemplo, com.amazonaws.us-east-1.ssm). Para obtener
        una lista completa de los códigos de región, consulte Regiones
        disponibles.

    -   Para una VPC, elija el ID de VPC de la instancia.

    -   Para las subredes, elija un ID de subred en la VPC. Para obtener
        una alta disponibilidad, elija al menos dos subredes de
        diferentes zonas de disponibilidad dentro de la región.

    -   Nota: Si tiene más de una subred en la misma zona de
        disponibilidad, no necesita crear puntos de enlace de VPC para
        las subredes adicionales. Cualquier otra subred dentro de la
        misma zona de disponibilidad puede acceder a la interfaz y
        utilizarla.

    -   En Habilitar nombre DNS, seleccione Habilitar para este punto
        final. Para obtener más información, consulte DNS privado para
        puntos finales de interfaz.

    -   Para Grupo de seguridad, seleccione un grupo de seguridad
        existente o cree uno nuevo. El grupo de seguridad debe permitir
        el tráfico HTTPS entrante (puerto 443) de los recursos de la VPC
        que se comunican con el servicio.

    -   Si creó un grupo de seguridad nuevo, abra la consola de VPC,
        elija Security Groups y, a continuación, seleccione el nuevo
        grupo de seguridad. En la pestaña Reglas de entrada, elige
        Editar reglas de entrada. Añada una regla con los siguientes
        detalles y, a continuación, seleccione Guardar reglas:

    -   Para Tipo, elija HTTPS.

    -   Para Source, seleccione su CIDR de VPC. Para una configuración
        avanzada, puede permitir que las instancias EC2 usen CIDR de
        subredes específicas.

    -   Anote el ID del grupo de seguridad. Utilizará este ID con los
        demás puntos finales.

6.  Cree políticas para los puntos de enlace de la interfaz de VPC para
    AWS Systems Manager.

    -   Repita el paso 5 con el siguiente cambio: Para Nombre del
        servicio, seleccione com.amazonaws. \[region\] .ec2 mensajes.

    -   Repita el paso 5 con el siguiente cambio: Para Nombre del
        servicio, seleccione com.amazonaws. \[region\] .sms mensajes.
        Debe hacerlo si quiere usar el Administrador de sesiones.

**Crear una lista de control de acceso a la red para la VPC forense**

Este proceso aislará la VPC para que el tráfico no pueda entrar ni salir
de las instancias que contiene:

1.  Abra la [consola VPC de
    Amazon](https://console.aws.amazon.com/vpc/).

2.  En el panel de navegación, seleccione Seguridad, ACL de red.

3.  Seleccione la casilla de verificación junto a Forensics VPC.

4.  En la parte superior derecha, selecciona el menú desplegable
    Acciones y Editar reglas de entrada.

5.  Cambiar la regla 100 All Traffic (todo el tráfico) a SSH

6.  Selecciona Guardar cambios.

7.  Repita estos pasos para las reglas de salida.

**Crear un EC2 Golden AMI forense**

1.  Abra la [consola EC2 de
    Amazon.](https://console.aws.amazon.com/ec2/)

2.  Seleccionar Launch Instance (iniciar instancia)

3.  En la barra de busqueda escribir Ubuntu y presionar enter.

4.  Seleccionar Ubuntu Server 18.04 LTS

5.  Elija un tipo de instancia

> Nota: Para fines forenses, se recomienda elegir una instancia de tipo
> M5

6.  Seleccionar Next: Configure Instance Details (siguiente: Configure
    detalles de instancia)

    -   Network: Elija la VPC network forense

    -   Auto-asignar IP Publica: deshabilitar

    -   IAM Role: Seleccionar un rol apropiado para fines forenses

    -   Habilitar la protección de terminación: asegúrese de que esta
        casilla esté marcada

7.  Seleccionar Proximo: agregar almacenamiento

8.  Seleccionar Agregar nuevo volumen y cree un volumen de tamaño
    suficiente para gestionar los datos forenses que se capturan. Un
    buen valor inicial es de 100 GB, pero base este valor en las
    necesidades y experiencias de su organización

9.  Seleccionar Proximo: Agregar Tags/Etiquetas.

10. Seleccionar Revisar e iniciar

11. Seleccionar Iniciar

12. Inicie sesión en las instancias de EC2 a través de EC2 Session
    Manager

13. Instala tus herramientas forenses en la instancia. Los siguientes
    son un par de ejemplos para herramientas Open Source forenses:

    -   Instalación SANS SIFT Toolkit

        -   sudo su - ubuntu
        -   sudo curl -Lo /usr/local/bin/sift
            [https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux](https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux)
        -   sudo chmod +x /usr/local/bin/sift
        -   sudo /usr/local/bin/sift install \--mode=server \--user=ubuntu
        -   sudo /usr/local/bin/sift upgrade
    -   Instalación Google Rapid Response
        -   sudo apt install mysql-server
        -   sudo mysql_secure_installation
        -   create database grr; grant all privileges on grr.\* to
            grr@localhost identified by \'password\'; flush privileges;
        -   wget [https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb](https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb)
        -   sudo apt install ./grr-server_3.2.4-6_amd64.deb
        -   sudo systemctl restart grr-server
        -   sudo systemctl status grr-server
            1.  Para mas información referirse a [how to Install and Setup GRR clients on Ubuntu
                18.04](https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/)

```{=html}
<!-- -->
```
1.  Abra la [consola VPC de Amazon](https://console.aws.amazon.com/vpc/).

2.  Vaya a Instancias

```{=html}
<!-- -->
```
1.  Haga clic con el botón derecho en la instancia que desee utilizar
    como base para la AMI y seleccione Crear imagen en el menú
    contextual.

2.  En el cuadro de diálogo Crear imagen, escriba un nombre y una
    descripción únicos y, a continuación, seleccione Crear imagen.

    -   De forma predeterminada, Amazon EC2 cierra la instancia, toma
        instantáneas de los volúmenes adjuntos, crea y registra la AMI
        y, a continuación, reinicia la instancia.

    -   Elija No reboot si no quiere que se cierre la instancia.

    -   Si elige No reboot, no podemos garantizar la integridad del
        sistema de archivos de la imagen creada.

La creación de la AMI puede tardar unos minutos. Una vez creada,
aparecerá en la vista de AMI de AWS Explorer. Para mostrar esta vista,
haga doble clic en el nodo Amazon EC2 \| AMIs en AWS Explorer. Para ver
tus AMI, en la lista desplegable Visualización, selecciona Mi propiedad.
Es posible que tenga que elegir Actualizar para ver la AMI. Cuando la
AMI aparece por primera vez, puede estar en un estado pendiente, pero
después de unos momentos, pasa a un estado disponible.

**Entrenamiento**

• ¿Cuál es la expectativa del conocimiento para el personal con
responsabilidades forenses? (conocimientos/habilidades/habilidades)

• ¿Qué formación y procedimientos forenses recibe mi personal?

• ¿Qué requisitos reglamentarios para la ciencia forense se aplican a mi
empresa?

• ¿Qué formación existe para que los analistas de la empresa se
familiaricen con la API de AWS (entorno de línea de comandos), S3, RDS y
otros servicios de AWS?

\*\*Es necesario agregar aquí más contexto y elementos, ya que la
ciencia forense es una habilidad, no solo un libro de jugadas

**Procedimientos de escalamiento**

• Necesito una decisión empresarial sobre cuándo se deben realizar los
análisis forenses de EC2

• ¿Quién supervisa los registros/alertas, los recibe y actúa en función
de cada uno?

• ¿A quién se le notifica cuando se descubre una alerta?

• ¿Cuándo se involucran las relaciones públicas y legales en el proceso?

• ¿Cuándo solicitaría ayuda a AWS Support?

**DETECCIÓN AND ANÁLISIS**

**Posibles fuentes de alertas**

-   Billing

-   CloudWatch (EC2 Metric / Event Rule)

-   Config

-   GuardDuty

-   Inspector

-   Macie

-   Security Hub

-   Trusted Advisor

-   External Notification

**Protección de datos**

**SIEMPRE** debes:

-   Hacer evidencia **SOLO LECTURA.**

-   La evidencia **NUNCA** debe modificarse.

-   Cree copias de las pruebas y trabaje SOLO a partir de esas.

    -   **NUNCA trabaje desde la evidencia fuente (source) para el
        análisis. Acceda directamente.**

-   Montar todas las pruebas **SOLO LECTURA**

-   Realizar un seguimiento de **QUIÉN** accedió **A QUÉ** y **CUÁNDO**

-   Tome notas de investigación de:

    -   Qué analizaste (qué datos)

    -   Cuándo lo analizaste (fecha/hora)

    -   Lo que has realizado (comandos/procedimiento/proceso
        específicos)

    -   Lo que encontraste (resultados)

**Obtención de Metadatos**

Capturar información relacionada con la instancia que pueda ser útil
para la investigación

-   Tipo o ID de instancia

-   Dirección (es) IP

-   Grupo (s) de seguridad

-   VPC/ID de subred

-   Región

-   AMI ID

-   Hora de lanzamiento

**Obtención de memoria**

Se recomienda utilizar un utility de terceros para adquirir memoria de
las instancias en ejecución.\
Es esencial capturar la memoria ANTES DEL aislamiento o apagado del
sistema para capturar todos los datos volátiles (y valiosos)
disponibles.

[**Crear una instantánea de Amazon EBS**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)

1.  Abra la [consola EC2 de Amazon.](https://console.aws.amazon.com/ec2/)

2.  Elija Instantáneas en Elastic Block Store en el panel de navegación.

3.  Elija crear Snapshot.

4.  Para Seleccionar tipo de recurso, elija Volumen.

5.  Para Volumen, seleccione el volumen.

6.  (Opcional) Introduzca una descripción para la instantánea.

7.  (Opcional) Elija Agregar tag para añadir tags a la instantánea. Para
    cada tag, proporciona una clave de tag y un valor de tag.

8.  Elija crear Snapshot.

[**Compartir un Snapshot de Amazon EBS**](https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)

**Comparte un snapshot sin cifrado utilizando la consola**

1.  Abra la [consola EC2 de Amazon.](https://console.aws.amazon.com/ec2/)

2.  Elija Snapshots en el panel de navegación.

3.  Seleccione la snapshot y, a continuación, elija Acciones, Modificar permisos.

4.  Haga que la snapshot sea pública o compártala con cuentas de AWS específicas de la siguiente manera:

    -   Para hacer pública la snapshot, elija Público.
    -   Esta opción no es válida para snapshots cifradas o snapshots con
        un código de producto de AWS Marketplace.
    -   Para compartir la snapshot con una o más cuentas de AWS, elija
        Privado, introduzca el ID de la cuenta de AWS (sin guiones) en
        Número de cuenta de AWS y elija Agregar permiso. Repita el
        procedimiento para cualquier cuenta de AWS adicional.

5.  Selecciona Guardar.

> **Para usar una snapshot sin cifrar que se haya compartido con usted de forma privada**

1.  Abra la [consola EC2 de Amazon.](https://console.aws.amazon.com/ec2/)

2.  Elija Snapshots en el panel de navegación.

3.  Elija el filtro de snapshots privadas.

4.  Busque la snapshot por ID o descripción. Puede usar esta snapshot
    como cualquier otra; por ejemplo, puede crear un volumen a partir de
    la snapshot o copiar la snapshot en una región diferente.

**Compartir una snapshot cifrada mediante la consola**

1.  Abra la [Consola KMS de AWS.](https://console.aws.amazon.com/kms)

2.  Para cambiar la región de AWS, utilice el selector de regiones en la
    esquina superior derecha de la página.

3.  Elija Claves administradas por el cliente en el panel de navegación.

4.  En la columna Alias, elija el alias (enlace de texto) de la clave
    administrada por el cliente que utilizó para cifrar la snapshot. Los
    detalles clave se abren en una página nueva.

5.  En la sección Política clave, verá la vista de políticas o la vista
    predeterminada. La vista de políticas muestra el documento de
    políticas clave. La vista predeterminada muestra secciones para
    administradores de claves, eliminación de claves, uso de claves y
    otras cuentas de AWS. La vista predeterminada se muestra si creó la
    política en la consola y no la ha personalizado. Si la vista
    predeterminada no está disponible, tendrá que editar manualmente la
    política en la vista de políticas. Para obtener más información,
    consulte Visualización de una política de claves (consola) en la
    Guía para desarrolladores de AWS Key Management Service. Utilice la
    vista de políticas o la vista predeterminada, según la vista a la
    que pueda acceder, para agregar uno o más ID de cuenta de AWS a la
    política, de la siguiente manera:

    -   (Vista predeterminada) Desplázate hacia abajo hasta Otras
        cuentas de AWS. Elija Agregar otras cuentas de AWS e introduzca
        el ID de cuenta de AWS según se le solicite. Para añadir otra
        cuenta, elija Agregar otra cuenta de AWS e introduzca el ID de
        la cuenta de AWS. Cuando haya agregado todas las cuentas de AWS,
        elija Guardar cambios.

    -   (Vista Política) Elija Editar. Agregue uno o más ID de cuenta de
        AWS a las siguientes instrucciones: «Permitir el uso de la
        clave» y «Permitir adjuntar recursos persistentes». Selecciona
        Guardar cambios. En el siguiente ejemplo, se agrega el ID de
        cuenta de AWS 444455556666 a la política.
```
    {
      "Sid": "Allow use of the key",
      "Effect": "Allow",
      "Principal": {"AWS": [
        "arn:aws:iam::111122223333:user/CMKUser",
        "arn:aws:iam::444455556666:root"
      ]},
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:ReEncrypt*",
        "kms:GenerateDataKey*",
        "kms:DescribeKey"
      ],
      "Resource": "*"
    },
    {
      "Sid": "Allow attachment of persistent resources",
      "Effect": "Allow",
      "Principal": {"AWS": [
        "arn:aws:iam::111122223333:user/CMKUser",
        "arn:aws:iam::444455556666:root"
      ]},
      "Action": [
        "kms:CreateGrant",
        "kms:ListGrants",
        "kms:RevokeGrant"
      ],
      "Resource": "*",
      "Condition": {"Bool": {"kms:GrantIsForAWSResource": true}}
    }    
```
6.  Abra la [consola EC2 de
    Amazon.](https://console.aws.amazon.com/ec2/)

7.  Elija snapshots en el panel de navegación.

8.  Seleccione la snapshot y, a continuación, elija Acciones, Modificar
    permisos.

9.  Para cada cuenta de AWS, introduzca el ID de cuenta de AWS en Número
    de cuenta de AWS y seleccione Agregar permiso. Cuando haya agregado
    todas las cuentas de AWS, elija Guardar.

**Para usar una instantánea cifrada que se ha compartido con usted**

1.  Abra la [consola EC2 de
    Amazon.](https://console.aws.amazon.com/ec2/)

2.  Elija snapshots en el panel de navegación.

3.  Elija el filtro snapshots privadas. Si lo desea, añada el filtro
    Cifrado.

4.  Busque la snapshot por ID o descripción.

5.  Seleccione la snapshot y elija Acciones, Copiar.

6.  (Opcional) Seleccione una región de destino.

7.  La copia de la snapshot se cifra con la clave que se muestra en
    Master Key. De forma predeterminada, la clave seleccionada es la CMK
    predeterminada de tu cuenta. Para seleccionar una CMK administrada
    por el cliente, haga clic dentro del cuadro de entrada para ver una
    lista de claves disponibles.

8.  Selecciona Copiar.

**Crear una EC2 Forense desde una Golden AMI Forense**

Cree una instancia forense nueva utilizando la **Golden AMI Forense**
anteriormente creada.

**Montar el volumen de EBS sospechoso en EC2 Forense**

1.  Abra la [consola EC2 de
    Amazon.](https://console.aws.amazon.com/ec2/)

2.  En el panel de navegación, seleccione Elastic Block Store, Volumes.

3.  Seleccione un volumen disponible y elija Acciones, Adjuntar volumen.

4.  Por ejemplo, empieza a escribir el nombre o el ID de la instancia.

5.  Seleccione la instancia de la lista de opciones (solo se muestran
    las instancias que están en la misma zona de disponibilidad que el
    volumen).

6.  Para Dispositivo, puede conservar el nombre del dispositivo sugerido
    o escribir un nombre de dispositivo compatible diferente.

    -   Para obtener más información, consulte [Nombres de dispositivos
        en instancias de
        Linux.](https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/device_naming.html)

7.  Selecciona Adjuntar.

8.  Conéctese a la instancia y monte el volumen.

    -   Para mas información, consulte [Hacer que un volumen de Amazon
        EBS esté disponible para su uso en
        Linux.](https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/ebs-using-volumes.html)

**Ejecutar Procesos Forenses**

-   ¿Qué resultados forenses busca la empresa?

-   ¿Qué se incluirá en el informe final?

-   ¿A quién esta dirigido el informe?

-   ¿Existen requisitos legales o reglamentarios que identifiquen la
    información requerida de una investigación forense?

-   ¿Hay personal en la empresa que esté debidamente capacitado,
    licenciado o certificado para realizar análisis forenses?

-   ¿Hay algún requisito de retención para los registros, los datos o
    las pruebas? Potencialmente, buscar en Glacier Vaults

-   Un ejemplo de [Digital Forensic Analysis of Amazon Linux EC2 Instances](https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) está disponible en SANS institute.

**Contención**

**Aislar los recursos afectados inmediatamente**

**NOTA:** Asegúrese de contar con un proceso para solicitar escalamiento
y aprobación para aislar los recursos y garantizar que primero se lleve
a cabo un análisis del impacto en el negocio sobre cómo afectará el
aislamiento a las operaciones actuales y a los flujos de ingresos.

**Decomisionar la instancia**

Habilitar las protecciones de terminación

-   [Terminar una instancia.](https://docs.aws.amazon.com/es_es/AWSEC2/latest/UserGuide/terminating-instances.html)

    1.  Establecer el comportamiento de cierre de la instancia en Detener (vs. terminar)

    2.  Deshabilitar «DeleteOnTermination» para todos los volúmenes de instancias

-   [Desconexión de instancias EC2 del grupo de Auto Scaling.](https://docs.aws.amazon.com/es_es/autoscaling/ec2/userguide/detach-instance-asg.html)

    1.  Abra la [consola de Amazon EC2 Auto Scaling](https://console.aws.amazon.com/ec2autoscaling/).

    2.  Selecciona la casilla de verificación junto a tu grupo de Auto Scaling.

    3.  Se abre un panel dividido en la parte inferior de la página de
        grupos de Auto Scaling, en el que se muestra información sobre
        el grupo que está seleccionado.

    4.  En la pestaña Administración de instancias, en Instancias,
        seleccione una instancia y elija Acciones, Detach.

    5.  En el cuadro de diálogo Separar instancia, active la casilla de
        verificación para lanzar una instancia de reemplazo o déjelo sin
        marcar para reducir la capacidad deseada. elija Separar (detach)
        instancia.

-   [Desuscribir de Load Balancer(s)](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)

    1.  Abra la [consola de Amazon EC2](https://console.aws.amazon.com/ec2/).

    2.  En el panel de navegación, en LOAD BALANCING, seleccione Load Balancers.

    3.  Selecciona tu balanceador de cargas.

    4.  En el panel inferior, seleccione la pestaña Instancias.

    5.  Elija Editar instancias.

    6.  Selecciona la instancia que deseas registrar en tu balanceador
        de cargas.

    7.  Selecciona Guardar.

    ```{=html}
    <!-- -->
    ```
    1.  Cree un nuevo grupo de seguridad que bloquee todo el tráfico de
        entrada y salida; asegúrese de eliminar la regla predeterminada
        de permitir todo para el tráfico de salida

    2.  djuntar el *nuevo* grupo de seguridad a las instancias afectadas

**Erradicación**

No hay pasos para este proceso aplicables a este runbook.

**Recuperación**

Una vez finalizadas las actividades forenses y tras la emisión del
informe final, debe cancelar la instancia de EC2 forense y eliminar la
instantánea de EBS **A MENOS QUE** exista un requisito legal o
reglamentario para mantener los datos.

**Lecciones aprendidas**

Este es un lugar para agregar elementos específicos de su empresa que no
necesariamente necesitan «reparación», pero que es importante conocer al
ejecutar este manual de estrategias junto con los requisitos operativos
y comerciales.

**Elementos del Backlog abordados**

-   Como Respondedor de Incidentes, necesito un runbook para realizar el
    análisis forense de **EC2.**

-   Como Respondedor a Incidentes, necesito una decisión empresarial
    sobre cuándo se deben realizar los análisis forenses de EC2.

-   Como Respondedor a Incidentes, necesito tener el registro habilitado
    en todas las regiones que estén habilitadas, independientemente de
    la intención de uso.

-   Como Respondedor a Incidentes, debo asegurarme de que solo se usen
    las AMI aprobadas.

-   Como Respondedor a Incidentes, necesito poder detectar crypto mining
    en mis instancias EC2 existentes

-   Como Respondedor a Incidentes, necesito un método preparado para
    eliminar números masivos de instancias.
