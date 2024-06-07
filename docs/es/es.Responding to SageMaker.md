# Manual de respuesta a incidentes: respuesta a los eventos de seguridad de SageMaker
Este documento se proporciona únicamente con fines informativos. Representa las ofertas y prácticas de productos actuales de Amazon Web Services (AWS) en la fecha de publicación de este documento, que están sujetas a cambios sin previo aviso. Los clientes son responsables de realizar su propia evaluación independiente de la información de este documento y de cualquier uso de los productos o servicios de AWS, cada uno de los cuales se proporciona «tal cual» sin garantía de ningún tipo, ya sea expresa o implícita. Este documento no crea ninguna garantía, declaración, compromiso contractual, condición o garantía por parte de AWS, sus filiales, proveedores o licenciantes. Las responsabilidades y obligaciones de AWS con sus clientes están reguladas por los acuerdos de AWS, y este documento no forma parte de ningún acuerdo entre AWS y sus clientes ni lo modifica.

© 2024 Amazon Web Services, Inc. o sus filiales. Todos los derechos reservados. Esta obra está bajo una licencia internacional Creative Commons Attribution 4.0.

Este contenido de AWS se proporciona sujeto a los términos del Acuerdo de cliente de AWS disponible en http://aws.amazon.com/agreement u otro acuerdo escrito entre el cliente y Amazon Web Services, Inc. o Amazon Web Services EMEA SARL o ambos.


## Puntos de contacto

Autor: `Nombre del autor`\
Aprobador: `Nombre del aprobador`\
Última fecha de aprobación:

## Resumen ejecutivo
Como parte de nuestro compromiso continuo con los clientes, AWS proporciona este manual de respuesta a incidentes de seguridad en el que se describen los pasos necesarios para investigar los eventos de seguridad en los que Amazon SageMaker es la fuente o el objetivo del uso no autorizado de sus cuentas de AWS. El objetivo de este documento es proporcionar una guía prescriptiva sobre las medidas que se deben tomar en caso de sospechar que se ha producido un incidente de seguridad.

! [Imagen] (sagemaker_images/nist_life_cycle.png)

*Aspectos de la respuesta a incidentes de AWS*


### Amazon SageMaker

Amazon SageMaker es un servicio de aprendizaje automático (ML) totalmente gestionado. Con SageMaker, los científicos de datos y los desarrolladores pueden crear, entrenar e implementar modelos de aprendizaje automático de forma rápida y segura en un entorno hospedado listo para la producción. Proporciona una experiencia de interfaz de usuario para ejecutar flujos de trabajo de aprendizaje automático que hace que las herramientas de aprendizaje automático de SageMaker estén disponibles en varios entornos de desarrollo integrados (IDE).

Con SageMaker, puede almacenar y compartir sus datos sin tener que crear y administrar sus propios servidores. Gracias a la compatibilidad integrada con marcos y algoritmos propios, SageMaker ofrece opciones de formación distribuidas y flexibles que se ajustan a flujos de trabajo específicos. Para obtener más información, consulte la guía para desarrolladores de Amazon SageMaker aquí: https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## Preparación
Prepare su entorno de forma proactiva mediante la implementación de controles preventivos ([Políticas de control de servicios] (https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)) y de detección (consulte la sección Detección para ver las reglas de configuración).


**Preventivo (SCP) **

VPC Logs
* Por ejemplo, el siguiente SCP impedirá que los usuarios inicien cuadernos, cursos o trabajos de procesamiento a menos que se especifique una VPC.

```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Despliegue de VPC»,
«Effect»: «Denegar»,
«Acción»: [
«SageMaker: crea un trabajo de ajuste de hiperparámetros»,
«SageMaker: crea un modelo»,
«SageMaker: Crear instancia de Notebook»,
«SageMaker: Crear trabajo de procesamiento»,
«SageMaker: crea un trabajo de formación»
],
«Recurso»: [
«*»
],
«Condición»: {
«Nulo»: {
«SageMaker: vpcSecurityGroupIds»: «verdadero»,
«SageMaker: VPCsubnets»: «verdadero»
}
}
}
]
}
```


* Imponga el cifrado de los trabajos

```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Denegar volúmenes no cifrados»,
«Efecto»: «Denegar»,
«Acción»: [
«SageMaker: crea un trabajo de ajuste de hiperparámetros»,
«Sage Maker: crea un trabajo de formación»,
«SageMaker: cree la configuración de Endpoint»,
«SageMaker: crear un trabajo de transformación»
],
«Recurso»: [
«*»
],
«Condición»: {
«Nulo»: {
«SageMaker: clave de volumen»: [
«cierto»
]
}
}
}
]
}
```
* Imponga el cifrado del tráfico entre contenedores
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Denegar el tráfico no cifrado»,
«Efecto»: «Denegar»,
«Acción»: [
«Sage Maker: crea un trabajo de formación»,
«SageMaker: cree un trabajo de ajuste de hiperparámetros»
],
«Recurso»: [
«*»
],
«Condición»: {
«Bool»: {
«SageMaker: cifrado de tráfico entre contenedores»: «falso»
}
}
}
]
}
```

* Imponer el aislamiento de la red
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Denegar no está aislado»,
«Efecto»: «Denegar»,
«Acción»: [
«Sage Maker: crea un trabajo de formación»,
«SageMaker: cree un trabajo de ajuste de hiperparámetros»,
«SageMaker: crea un modelo»
],
«Recurso»: «*»,
«Condición»: {
«Bool»: {
«SageMaker: aislamiento de red»: «falso»
}
}
}
]
}
```
* Restringir la URL prefirmada del cuaderno a las IP
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Restringir la URL a IP»,
«Efecto»: «Denegar»,
«Acción»: «SageMaker: URL de creación de una instancia de bloc de notas prediseñada»,
«Recurso»: «*»,
«Condición»: {
«Para todos los valores: sin dirección IP»: {
«AWS: IP de origen»: [
«[INGRESAR_DIRECCIÓN_IP_PÚBLICA]»
]
}
}
}
]
}
```
* Desactivar el acceso a Internet
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Denegar Internet directo»,
«Efecto»: «Denegar»,
«Acción»: «SageMaker: crear una instancia de Notebook»,
«Recurso»: «*»,
«Condición»: {
«StringEquals»: {
«SageMaker: acceso directo a Internet»: [
«Habilitado»
]
}
}
}
]
}
```

* Desactivar el acceso Root en las libretas de SageMaker
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Sage Maker niega el acceso a Root»,
«Efecto»: «Denegar»,
«Acción»: [
«SageMaker: Crear instancia de Notebook»,
«SageMaker: instancia de Update Notebook»
],
«Recurso»: «*»,
«Condición»: {
«StringEquals»: {
«SageMaker: RootAccess»: [
«Habilitado»
]
}
}
}
]
}
```
* Restrinja los tipos de instancias que pueden iniciar los usuarios
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Tipos de instancia límite de SageMaker»,
«Effect»: «Denegar»,
«Acción»: «SageMaker: crear una instancia de Notebook»,
«Recurso»: «*»,
«Condición»: {
«Para cualquier valor: StringNotLike»: {
«SageMaker: tipos de instancia»: [
«[EXAMPLE_INSTANCE_TYPES]»,
«ml.c5.xlarge»,
«ml.m5.xlarge»,
«ml.t3.medium»
]
}
}
}
]
}
```

* Del mismo modo, para Studio, consulta el siguiente ejemplo de política. Tenga en cuenta que los administradores deben permitir la instancia del sistema para las aplicaciones predeterminadas de Jupyter Server.
```
{
«Versión»: «2012-10-17",
«Declaración»: [
{
«Sid»: «Tipos de instancia permitidos por SageMaker»,
«Efecto»: «Denegar»,
«Acción»: [
«SageMaker: crear una aplicación»
],
«Recurso»: «*»,
«Condición»: {
«Para cualquier valor: StringNotLike»: {
«SageMaker: tipos de instancia»: [
«ml.c5.large»,
«ml.m5.large»,
«ml.t3.medium»,
«sistema»
]
}
}
}
]
}
```


## Detección

### Comprobaciones de SageMaker

### AWS Config

AWS Config tiene varias [reglas administradas para evaluar SageMaker] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* sagemaker-endpoint-configuration-kms-key-configured
* sagemaker-endpoint-config-prod-instance-count
* sagemaker-notebook-instance-inside-vpc
* sagemaker-notebook-instance-kms-key-configurado
VPC
* Sagemaker-Notebook sin acceso directo a Internet

### Eventos de CloudTrail

En caso de acceso no autorizado a su entorno, consulte a continuación los posibles escenarios relacionados con las llamadas a la API de SageMaker relevantes como referencia rápida (tenga en cuenta que esta no es una lista completa de las llamadas a la API de SageMaker):


**Exfiltración de datos/modelos: **
- Copiar o descargar datos confidenciales de los almacenes de datos o artefactos de modelos de SageMaker.
- Extraer parámetros del modelo o datos de entrenamiento, lo que podría exponer la propiedad intelectual o la información personal.

<ins>Ejemplos de llamadas a la API:</ins>
- `DescribeModelPackage` para recuperar información sobre los paquetes modelo.
- `DescribeTrainingJob` para acceder a los detalles de los trabajos de formación y a sus datos de salida.
- `getModelPackageModelMetrics` para recuperar las métricas del modelo y los datos potencialmente confidenciales.



**Envenenamiento de datos: **
* Modificar o envenenar modelos entrenados mediante la inyección de datos maliciosos o ejemplos contradictorios.
* El despliegue de modelos comprometidos en los puntos finales de SageMaker provoca predicciones incorrectas o resultados maliciosos.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateModelPackage` o `UpdateModelPackage` para implementar un paquete modelo comprometido.
- `createTransformJob` para ejecutar un trabajo de transformación con un modelo envenenado.
- `createEndpointConfig` y `createEndpoint` para implementar un modelo malicioso en un punto final.
- `CreateTrainingJob` o `UpdateTrainingJob` para inyectar datos maliciosos en los trabajos de formación.



**Uso indebido de recursos: **
* Lanzar cuadernos o instancias de SageMaker no autorizados para minar criptomonedas u otras actividades maliciosas.
* Uso de los recursos de SageMaker como puntos de entrada o puntos de pivote para el movimiento lateral dentro del entorno de AWS.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateNotebookInstance` o `UpdateNotebookInstance` para lanzar instancias de bloc de notas no autorizadas.
- `CreateTrainingJob` o `CreateHyperParameterTuningJob` para iniciar demasiados trabajos de formación.
- `CreateEndpoint` o `UpdateEndpoint` para crear o modificar puntos finales con fines no autorizados.

**Denegación de servicio (DoS) :**
* Agotar los recursos de SageMaker (por ejemplo, instancias de cómputo o almacenamiento) al lanzar un exceso de tareas de formación o puntos finales.
* Abrumar las API o los servicios de SageMaker con un gran volumen de solicitudes, lo que provoca interrupciones en el servicio.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateTrainingJob` o `CreateHyperParameterTuningJob` para lanzar numerosos trabajos de formación y agotar los recursos.
- `CreateEndpoint` o `UpdateEndpoint` para crear varios puntos finales y consumir recursos informáticos excesivos.

**Cambios de configuración: **
* Modificar las funciones, políticas o permisos de SageMaker para aumentar los privilegios o conceder acceso no autorizado.
* Modificar las configuraciones de VPC, los grupos de seguridad o los ajustes de red de SageMaker para eludir los controles de seguridad.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateRole` o `UpdateRole` para modificar los roles y permisos de SageMaker.
- `CreateNotebookInstanceLifecycleConfig` o `UpdateNotebookInstanceLifecycleConfig` para modificar las configuraciones de las instancias del bloc de notas.
- `createEndpointConfig` o `updateEndpointConfig` para cambiar las configuraciones de los terminales o los ajustes de seguridad.

**Manipulación de registros: **
* Modificar o eliminar los registros o pistas de auditoría de SageMaker para cubrir las pistas y dificultar la investigación de los incidentes.
* Inyectar entradas de registro falsas para engañar a los analistas de seguridad u ocultar actividades maliciosas.

<ins>Ejemplos de llamadas a la API:</ins>
- `PutModelPackageModelMetrics` para inyectar métricas de modelo falsas en los registros.
- `StopTrainingJob` o `StopTransformJob` para modificar o eliminar potencialmente los datos del registro.

**Despliegue de malware: **
* Implementar malware o puertas traseras en las libretas o instancias de SageMaker para el acceso persistente o el robo de datos.
* Utilizar los recursos de SageMaker para distribuir malware o lanzar ataques contra otros sistemas o redes.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateNotebookInstance` o `UpdateNotebookInstance` para lanzar instancias de bloc de notas con malware.
- `CreateModelPackage` o `UpdateModelPackage` para desplegar paquetes modelo que contengan código malicioso.

**Robo de credenciales: **
* Robar credenciales de AWS o claves de API de SageMaker almacenadas en blocs de notas o instancias.
* Usar credenciales robadas para obtener más acceso no autorizado a otros recursos o servicios de AWS.

<ins>Ejemplos de llamadas a la API:</ins>
- `DescribeNotebookInstance` o `DescribeTrainingJob` para acceder potencialmente a las credenciales almacenadas o a las claves de API.
- `GetModelPackageModelMetrics` o `DescribeModelPackage` para recuperar información o credenciales confidenciales.

**Criptojacking: **
* Secuestrar los recursos informáticos de SageMaker (por ejemplo, instancias o puntos finales) para realizar actividades de minería de criptomonedas no autorizadas.
* Consume recursos informáticos excesivos y podría provocar interrupciones en el servicio o un aumento de los costes.

<ins>Ejemplos de llamadas a la API:</ins>
- `CreateNotebookInstance` o `UpdateNotebookInstance` para lanzar instancias para la minería de criptomonedas.
- `CreateTrainingJob` o `CreateHyperParameterTuningJob` para iniciar tareas con uso intensivo de recursos informáticos con fines de minería.


**Nota: Es importante tener en cuenta que estas llamadas a la API también se pueden utilizar con fines legítimos, pero en el contexto del acceso no autorizado, podrían utilizarse indebidamente para realizar acciones riesgosas. La implementación de controles de acceso, supervisión y mecanismos de auditoría sólidos es crucial para detectar y prevenir este uso indebido de las API y los recursos de SageMaker. **

### Comprensión de las entradas de registro de SageMaker

Las siguientes capturas de pantalla proporcionan una ayuda visual al personal de respuesta a incidentes para ayudarlo a interpretar los eventos detectados durante una investigación. Cada imagen que aparece a continuación representa las acciones realizadas que coinciden con el nombre del evento que se registra

---

**Crear dominio**
<details>
<summary>Expandir captura de pantalla</summary>
-
Crea un dominio. Un dominio consta de un volumen asociado de Amazon Elastic File System, una lista de usuarios autorizados y diversas configuraciones de seguridad, aplicaciones, políticas y Amazon Virtual Private Cloud (VPC). Los usuarios de un dominio pueden compartir archivos de cuadernos y otros artefactos entre sí.

! [Crear dominio] (/sagemaker_images/sagemaker-01.png)
</details>

---

**Detalles del dominio de SageMaker**
<details>
<summary>Expandir captura de pantalla</summary>

El dominio Amazon SageMaker es compatible con los entornos de aprendizaje automático (ML) de SageMaker. Un dominio de SageMaker se compone de las siguientes entidades: dominio, perfil de usuario, espacio compartido y aplicación


! [Detalles del dominio] (/sagemaker_images/sagemaker-02.png)
! [Detalles del dominio 2] (/sagemaker_images/sagemaker-03.png)

</details>

---


**Evento CloudTrail para CreateDomain**

<details>
<summary>Expandir captura de pantalla</summary>

Ten en cuenta que el evento `CreateDomain` de Cloudtrail contiene toda la siguiente información: VPC, subredes, rol de ejecución, aplicaciones, etc.

! [Dominio de SageMaker con VPC, rol de ejecución, aplicaciones, etc.] (/sagemaker_images/sagemaker-04.png)


</details>

---

**Evento CloudTrail para CreateEndpoint**

<details>
<summary>Expandir capturas de pantalla</summary>

Tenga en cuenta que el rol de servicio `SageMaker-ExecutionRole` invoca el evento `CreateEndpoint` de SageMaker en CloudTrail.

! [Ejemplo de creación de punto final] (/sagemaker_images/sagemaker-05.png)

</details>

---



## Análisis

En caso de que se produzca un incidente, además de investigar los indicadores de compromiso, el actor de la amenaza, el plazo, etc., hay algunas preguntas adicionales que debe tener en cuenta una vez que se confirme que se trata de un incidente relacionado con los recursos de SageMaker:

1. ¿A qué recursos de SageMaker se accedió sin autorización? (por ejemplo, cuadernos, modelos, terminales, almacenes de datos)
2. ¿Cómo se obtuvo el acceso no autorizado? (por ejemplo, credenciales comprometidas, permisos mal configurados, vulnerabilidades explotadas)
3. ¿Qué acciones se realizaron en los recursos de SageMaker afectados durante el acceso no autorizado?
4. ¿Se exfiltró o manipuló algún modelo o dato?
5. Si se accedió a los modelos, ¿existe el riesgo de que se los envenene o se produzca un ataque adverso?
6. ¿Se creó o modificó algún recurso nuevo (por ejemplo, cuadernos o terminales) durante el acceso no autorizado?
7. ¿Se utilizó alguna API o SDK de SageMaker durante el acceso no autorizado y qué acciones se realizaron a través de ellos?
8. ¿Se modificó o eliminó algún registro o pista de auditoría de SageMaker para cubrir las pistas?
9. ¿Se modificó o utilizó indebidamente alguna función o política de IAM de SageMaker durante el incidente?
10. ¿Se modificó alguna configuración de VPC o configuración de red de SageMaker?
11. ¿Se utilizó algún cuaderno o instancia de SageMaker como punto de entrada o punto de giro para seguir accediendo sin autorización?
12. ¿Se utilizó algún recurso de SageMaker para lanzar ataques o actividades maliciosas contra otros recursos de AWS o sistemas externos?
13. ¿Cuál es el posible impacto del acceso no autorizado en la confidencialidad, integridad y disponibilidad de los recursos de SageMaker y los datos asociados?
14. ¿Cómo se pueden aislar de forma segura los recursos de SageMaker afectados, hacer copias de seguridad y, si es posible, recuperarlos o reconstruirlos?
15. ¿Qué configuraciones o prácticas recomendadas de seguridad específicas de SageMaker no se siguieron, lo que provocó el acceso no autorizado?

## Contención y erradicación

Si un usuario no autorizado ha creado algún recurso o si no se ha autorizado su creación, siga las siguientes instrucciones sobre cómo eliminar/modificar los recursos o permisos creados:

- [Cómo eliminar un dominio de SageMaker (consola)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
- [Cómo eliminar un dominio de SageMaker (CLI)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
- [Cómo eliminar un SageMaker Endpoint] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
- [Cómo eliminar una configuración de SageMaker Endpoint] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
- [Cómo eliminar un modelo de SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
- [Eliminar el acceso a la raíz de los cuadernos de SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html) (Vaya a la instancia de bloc de notas que desee, detenga la instancia/Una vez completada, haga clic en Editar/En Permisos, seleccione Desactivar/Actualizar la instancia del bloc de apunte/Ejecutar instancia)


## Elementos pendientes resueltos
- Como personal de respuesta a incidentes, necesito poder monitorear todos los eventos críticos de SageMaker
- Como agente de respuesta a incidentes, necesito un manual sobre cómo consultar los eventos de SageMaker Cloudtrail a gran escala

## Elementos pendientes actuales

## Apéndice: Mejores prácticas

**Muy recomendable**

- [Implementar en una VPC aislada] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Utilice el punto final de VPC para acceder a los recursos
- [Los cuadernos de Sagemaker deberían restringir el acceso a las conexiones desde la VPC] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
- Utilice grupos de seguridad y NACL para controlar el tráfico que entra y sale de su entorno
- [Cifrado del tráfico entre contenedores para tareas de formación con varias instancias informáticas] (https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
- [Habilitar el cifrado en reposo mediante KMS] (https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
- [Deshabilite el acceso root a los cuadernos si no es necesario] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
- [Prácticas recomendadas de configuración del ciclo de vida] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
- Cree una lista de paquetes permitidos que el equipo pueda utilizar para reducir el riesgo de que se ejecute código malicioso
- [Privilegios mínimos al utilizar las funciones de IAM y las políticas basadas en recursos (por ejemplo, políticas de bucket para acceder a los datos de bucket de S3), aproveche ML Governance] (https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
- [Utilice el centro de identidad de IAM] (https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
- [Almacene y rote las credenciales en Secrets Manager] (https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
- [Supervise la entrada y salida del modelo mediante SageMaker Model Monitor] (https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
- [Habilite el registro de eventos de datos de CloudTrail S3 para la auditoría de datos y artefactos de modelos de S3] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
- [Habilite los experimentos de SageMaker y aproveche el control de versiones para los artefactos del modelo] (https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
- [Habilite los registros de flujo de VPC para monitorear el tráfico de red en su VPC] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
- [Utilice CodeArtifact para descargar las bibliotecas y paquetes necesarios de Internet] (https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
- [CloudWatch también se puede utilizar para supervisar SageMaker] (https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

**Alentado**

- [Utilice Amazon Macie para proteger los datos confidenciales de S3] (https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
- [Utilice el catálogo de servicios para vender recursos de SageMaker previamente aprobados, como cuadernos, para restringir el tamaño de las instancias y exigir el uso de configuraciones seguras] (https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)