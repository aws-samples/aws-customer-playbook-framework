# Manual de seguridad para responder a los eventos de seguridad de Amazon Bedrock
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) en la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información de este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, declaración, compromiso contractual, condición o garantía por parte de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y obligaciones de AWS con sus clientes están reguladas por los acuerdos de AWS, y este documento no forma parte de ningún acuerdo entre AWS y sus clientes ni lo modifica.

© 2024 Amazon Web Services, Inc. o sus filiales. Todos los derechos reservados. Esta obra está bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponible en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha de aprobación:

## Resumen ejecutivo

Como parte de nuestro compromiso continuo con los clientes, AWS ofrece este
manual de respuesta a incidentes de seguridad que describe los pasos necesarios para solicitar asistencia a AWS Support en caso de eventos de seguridad.

! [Imagen] (/images/nist_life_cycle.png)

*Aspectos de la respuesta a incidentes de AWS*
## Preparación

Para ejecutar de forma rápida y eficaz las actividades de respuesta a incidentes, es fundamental preparar a las personas, los procesos y la tecnología de su organización. Consulte la [*Guía de respuesta a incidentes de seguridad de AWS*] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/preparation.html) e implemente los pasos necesarios para garantizar la preparación de los tres dominios.

## Cómo solicitar asistencia al soporte de AWS

Es importante que notifique a AWS tan pronto como sospeche que hay credenciales comprometidas en su cuenta u organización de AWS. Estos son los pasos para contratar a AWS Support:

### Abrir un caso de AWS Support

- Inicie sesión en su cuenta de AWS:
- Esta es la primera cuenta de AWS que se vio afectada por el evento de seguridad, para validar la propiedad de la cuenta de AWS.
- Nota: Si no puede acceder a la cuenta, utilice [este formulario] (https://support.aws.amazon.com/#/contacts/aws-account-support/) para enviar una solicitud de soporte.
- Selecciona «*Servicios*», «*Centro de asistencia*», «*Crear caso*».
- Selecciona el tipo de problema «*Cuenta y facturación» *.
- Seleccione el servicio y la categoría afectados:
- Ejemplo:
- Servicio: cuenta
- Categoría: Seguridad
- Elige una gravedad:
- Clientes de Enterprise Support o On-Ramp: *Pregunta crítica sobre los riesgos empresariales.
- Clientes de asistencia empresarial: *Pregunta urgente sobre riesgos comerciales*.
- Clientes de soporte básico y para desarrolladores: *Pregunta general*.
- Para obtener más información, puede [comparar los planes de AWS Support] (https://aws.amazon.com/premiumsupport/plans/).
- Describa su pregunta o problema:
- Proporcione una descripción detallada del problema de seguridad al que se enfrenta, los recursos afectados y el impacto empresarial.
- Hora en que se reconoció por primera vez el incidente de seguridad.
- Resumen de la cronología del evento hasta el momento.
- Los artefactos que has recopilado (capturas de pantalla, archivos de registro).
- ARN de los usuarios o roles de IAM que sospeche que están comprometidos.
- Descripción de los recursos afectados y del impacto empresarial.
- Nivel de participación en su organización (por ejemplo: «Este evento de seguridad cuenta con la visibilidad del director ejecutivo y del consejo de administración»).
- Opcional: añada contactos adicionales al caso.
- Selecciona «*Contáctenos*».
- Idioma de contacto preferido (puede estar sujeto a disponibilidad)
- Método de contacto preferido: web, teléfono o chat (recomendado)
- Opcional: contactos de caja adicionales
- *Haga clic en «Enviar» *

- **Escalaciones**: notifique a su equipo de cuentas de AWS lo antes posible para que puedan contratar los recursos necesarios y escalar según sea necesario.

**Nota: ** Es muy importante comprobar que su «contacto de seguridad alternativo» esté definido para cada cuenta de AWS. Para obtener más información, consulte [esto
artículo] (https://aws.amazon.com/blogs/security/update-the-alternate-security-contact-across-your-aws-accounts-for-timely-security-notifications/).