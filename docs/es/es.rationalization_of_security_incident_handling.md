# Racionalización de las alertas de incidentes de seguridad
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) a partir de la fecha de emisión de este documento, que están sujetos a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información contenida en este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea garantías, declaraciones, compromisos contractuales, condiciones o garantías de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y responsabilidades de AWS ante sus clientes están controladas por acuerdos de AWS y este documento no forma parte ni modifica ningún acuerdo entre AWS y sus clientes.

© 2021 Amazon Web Services, Inc. o sus filiales. Reservados todos los derechos. Este trabajo está licenciado bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponibles en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.

> «Si protege sus clips de papel y diamantes con igual vigor, pronto tendrá más clips de papel y menos diamantes», atribuido al ex secretario de Estado estadounidense, Dean Rusk

Las alertas se determinan mediante el modelado de amenazas de una carga de trabajo durante el desarrollo de controles de seguridad. El uso de las alertas se define mediante controles Responsive; sin embargo, su definición se basa en toda la perspectiva de seguridad: Directiva, Preventiva, Detective y Responsive.

## Definiciones

### Modelado de amenazas:

* **Carga de trabajo**: lo que intentas proteger
* **Amenaza**: lo que temes que pase
* **Impacto**: cómo se ve afectado el negocio cuando la amenaza cumple con la carga de trabajo
* **Vulnerabilidad**: qué puede facilitar la amenaza
* **Mitigación**: qué controles existen para compensar la vulnerabilidad
* **Probabilidad**: cuán probable es que la amenaza ocurra con las mitigaciones implementadas

**La priorización de las amenazas es un factor de impacto y probabilidad. **

### Controles de seguridad:

* **Directiva** Los controles establecen los modelos de gobernanza, riesgo y cumplimiento en los que operará el entorno.
* **Los controles preventivos** protegen sus cargas de trabajo y mitigan amenazas y vulnerabilidades.
* **Los controles detectivos** proporcionan visibilidad y transparencia completas sobre el funcionamiento de sus implementaciones en AWS.
* **Los controles receptivos** impulsan la corrección de posibles desviaciones de las líneas de base de seguridad.


! [Imagen] (/images/image-caf-sec.png)

## Para minimizar la fatiga de las alertas y un mejor manejo de los eventos de seguridad, el contexto de modelado de amenazas debe tener en cuenta:

* Relevancia de la carga de trabajo (*) para la empresa.
* Clasificación de datos (*) de cada componente de carga de trabajo.
* Metodología utilizada sistemáticamente como STRIDE (falsificación, manipulación, divulgación de información, repudio, denegación de servicio y elevación de privilegios)
* Preparación y madurez de la seguridad en la nube.
* Capacidad de respuesta ante incidentes y caza de amenazas.
* Capacidades de corrección automática.
* Madurez de automatización de manejo de incidentes.


(*) ***La relevancia de la carga de trabajo y la clasificación de datos*** se definen por la iniciativa de marco de riesgo de la corporación. Ejemplos:

### Relevancia de la carga de trabajo:

**Alto**: importantes pérdidas monetarias y daños en la percepción de la imagen, impacto empresarial a largo plazo, bajo éxito en la recuperación
**Medio**: pérdida monetaria sostenible y daños en la percepción de la imagen, impacto empresarial a corto plazo, alto éxito en la recuperación
**Bajo**: sin pérdidas monetarias medibles ni daños en la percepción de la imagen, sin impacto empresarial, no se aplica la recuperación

### Clasificación de datos:

**Secreto**: importantes pérdidas monetarias y daños en la percepción de la imagen, impacto empresarial a largo plazo, bajo éxito en la recuperación
**Confidencial**: pérdida monetaria sostenible y daños en la percepción de la imagen, impacto empresarial a corto plazo, alto éxito en la recuperación
**Sin clasificar**: sin pérdidas monetarias mensurables ni daños en la percepción de la imagen, sin impacto empresarial, no se aplica la recuperación


## El objetivo de la priorización de alertas es enviarlas a la cola adecuada:

* Cola de clasificación de alertas
* Cola de búsqueda de amenazas
* Cola de archivado


El proceso para definir dónde terminará la alerta depende de todos los factores enumerados anteriormente. Una decisión básica de cola es la siguiente:

### Cola de clasificación de alertas:

1. Mandato marco de riesgo específico, que incluye, entre otros, la regulación de la industria, el cumplimiento legal y los requisitos comerciales. En este escenario, la carga de trabajo tiene una relevancia **alta** o los datos se clasifican como **secreto**.
2. Requisito de modelo de amenazas específico para iniciar un proceso formal de respuesta a incidentes
3. Error de corrección automática para las alertas en las que la carga de trabajo tiene relevancia **media** o **alta** o la clasificación de datos es **secreta** o **confidencial**.

### **Cola de búsqueda de amenazas**:

1. La corrección automática correcta para la carga de trabajo con relevancia de **media** o **alta** o clasificación de datos es **secreta** o **confidencial**.
2. Error de corrección automática para la carga de trabajo con una relevancia **baja** o la clasificación de datos está **no clasificada**.

### Cola de archivado:

1. La corrección automática se ha realizado correctamente para la carga de trabajo con la relevancia **baja** o la clasificación de datos está **no clasificada**.