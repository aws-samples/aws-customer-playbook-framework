# Acelere la respuesta a los incidentes: cree un asistente de estrategia de seguridad inteligente con Amazon Q

## Descripción general

Esta guía proporciona instrucciones paso a paso para implementar una aplicación Amazon Q Business con seguridad reforzada diseñada para la respuesta a incidentes de seguridad y la administración del manual de estrategias. La solución crea una interfaz de búsqueda y chat basada en inteligencia artificial que permite a los equipos de seguridad encontrar rápidamente los manuales de seguridad e interactuar con ellos mediante consultas en lenguaje natural.

## Mejoras de seguridad

Esta versión segura incluye las siguientes mejoras de seguridad con respecto a la implementación estándar:

### 🔒 **Características de seguridad mejoradas**
- **Cifrado KMS**: todos los buckets de S3 utilizan el cifrado AWS KMS en lugar del AES256 básico
- **Registro de acceso**: registro completo de acceso a S3 con un depósito de registros dedicado
- **IAM con privilegios mínimos: las políticas de IAM se centran en recursos de aplicaciones específicos (sin comodín)
- **Administración segura de usuarios**: se han eliminado los grupos de IAM excesivamente permisivos y se utiliza Identity Center exclusivamente
- **Supervisión mejorada**: registros de acceso detallados con una política de retención automática de 90 días
- **Protección de datos**: el control de versiones S3 está habilitado en todos los depósitos de datos

### 🛡️ **Cumplimiento de seguridad**
- No hay permisos comodín en las políticas de IAM
- Todos los recursos están limitados a identificadores de aplicaciones específicos
- Acceso público bloqueado en todos los cubos S3
- Datos cifrados en reposo y en tránsito
- Registro de auditoría exhaustivo

## Arquitectura

La solución implementa:
- **Aplicación empresarial Amazon Q** con integración con el centro de identidad de IAM
- **Dos cubos S3 cifrados** para AWS y manuales de seguridad personalizados
- **Depósito de registros de acceso dedicado** con administración del ciclo de vida
- **Índice empresarial Q** con atributos de documentos que permiten realizar búsquedas
- **Experiencia web** para la interacción en lenguaje natural
- **Funciones de IAM** con permisos de mínimo privilegio

## Requisitos previos

- Instancia existente del AWS IAM Identity Center
- Usuarios creados en Identity Center
- AWS CLI configurada con los permisos adecuados
- Permisos de implementación de CloudFormation

## Paso 1: Implementar la plantilla Secure CloudFormation

1. **Descarga la plantilla segura**: `Q-SecurityPlayBooks-Secure.yaml`

2. **Obtenga el ARN** de su instancia de Identity Center**:
```bash
as sso-admin list-instances
```

3. **Implemente la plantilla**:
```bash
como Cloudformation desplegó\
--archivo de plantilla Q-SecurityPlaybooks-secure.yaml\
--nombre de pila q-security-playbooks-secure\
--anulación de parámetros\
Nombre de la aplicación = Security-Playbooks\
AWSPlaybooksBucketName=AWS-Security-Playbooks\
CXPlaybooksBucketName=Custom-Security-Playbooks\
IdentityCenterInstancearn=arn:aws:sso: :instance/ssoins-xxxxxxxxxx\
--capacidades CAPABILITY_IAM\
--región Estados Unidos-Este-1
```

4. **Espere a que finalice la implementación** (normalmente de 5 a 10 minutos)

## Paso 2: Cargue los manuales de seguridad de AWS

1. **Clone el marco del manual de estrategias para clientes de AWS**:
``bash
vrecursive https://github.com/aws-samples/aws-customer-playbook-framework.git
cd aws-customer-playbook-framework
```

2. **Suba a su paquete de libros de jugadas de AWS**:
``bash
aws s3 sync. s3://aws-security-playbooks-YOUR_ACCOUNT_ID/ --exclude «.git/*»
```

3. **Verifique la carga en la consola de AWS** o mediante CLI:
``bash
ls s3://aws-security-playbooks-YOUR_ACCOUNT_ID/ --recursive
```

## Paso 3: Cargue manuales de seguridad personalizados

1. **Prepara tus playbooks personalizados** en un directorio local

2. **Sube libros de jugadas personalizados al depósito S3 dedicado**:
``bash
aws s3 cp your-custom-playbooks/ s3://custom-security-playbooks-YOUR_ACCOUNT_ID/ --recursive
```

3. **Confirme la carga** revisando el contenido del bucket en la consola de AWS

## Paso 4: Configurar el acceso de los usuarios (solo en el centro de identidades)

⚠️ **Importante**: esta versión segura elimina los grupos de usuarios de IAM. Toda la administración de usuarios debe realizarse a través de Identity Center.

1. **Navegue a la consola del IAM Identity Center**
2. **Asegúrese de que los usuarios** estén creados** en Identity Center (no en IAM)
3. **Ve a la consola de Amazon Q Business**
4. **Seleccione su aplicación**
5. **Haga clic en «Gestión de acceso"**
6. **Agregue usuarios y grupos** únicamente desde Identity Center
7. **Asigne los permisos adecuados** en función de las funciones de los usuarios

## Paso 5: Sincroniza Playbooks con Amazon Q

1. **Navega a la consola de Amazon Q Business**
2. **Seleccione su aplicación**
3. **Haga clic en la pestaña «Fuentes de datos» **
4. **Para cada fuente de datos** (manuales de estrategias y manuales personalizados de AWS):
- Localice la fuente de datos en la lista
- Haz clic en «Sincronizar ahora»
- Supervisa el progreso de la sincronización (puede tardar varios minutos)
5. **Compruebe que la sincronización se ha realizado satisfactoriamente** para ambas fuentes de datos

## Paso 6: configurar las actualizaciones automatizadas del manual de estrategias de AWS (seguridad mejorada)

Cree una función Lambda con seguridad mejorada para actualizaciones automáticas:

1. **Navegue a la consola de AWS Lambda**
2. **Haga clic en «Crear función"**
3. **Elija «Autor desde cero"** con el nombre «UpdateAWSPlayBooks-Secure»
4. **Seleccione Python 3.11** (o la última versión disponible)

5. **Utilice este código de seguridad mejorado**:
```python
importar boto3
subproceso de importación
importar os
vrecursive
al escribir import Dict, Any
importar json

vls ls ls ls ls
logger = logging.getLogger ()
Logger.setLevel (Logging.info)

def lambda_handler (evento: Dict [str, cualquiera], contexto: cualquiera) -> Dict [str, cualquiera]:
«»
Proteja la función AWS Lambda para sincronizar AWS Customer Playbook Framework con S3.
Mejorado con una mejor gestión de errores y un mejor registro de seguridad.
«»
prueba:
# Limpia cualquier archivo existente
si os.path.exists ('/tmp/playbooks'):
subprocess.run (['rm', '-rf', '/tmp/playbooks'], check=True)

# Clona lo último de GitHub con verificación de seguridad
logger.info («Repositorio de clonación con verificación de seguridad...»)
resultado = subprocess.run (
['git', 'clone', '--depth', '1', '--single-branch',
'https://github.com/aws-samples/aws-customer-playbook-framework.git',
'/tmp/playbooks'],
CAPTURE_OUTPUT=TRUE,
texto=verdadero,
check=True,
tiempo de espera = 300 # Tiempo de espera de 5 minutos
)

# Inicialice el cliente S3 con seguridad mejorada
s3 = boto3.client ('s3')
bucket_name = os.environ ['BUCKET_NAME']

# Compruebe que el depósito existe y que tenemos acceso
prueba:
s3.head_bucket (bucket = nombre del bucket)
excepto la excepción e:
raise Exception (f «No se puede acceder al depósito {bucket_name}: {str (e)}»)

# Cargar archivos a S3 con metadatos
logger.info (f"Cargar archivos al bucket de S3: {bucket_name}»)
files_uploaded = 0

para archivos root, _, en os.walk ('/tmp/playbooks'):
para el archivo en archivos:
si no es file.startswith ('.git') y no file.startswith (' . '):
local_path = os.path.join (raíz, archivo)
s3_key = os.path.relpath (local_path, '/tmp/playbooks')

# Añadir metadatos para el seguimiento
s3.upload_file (
Nombre de archivo = LOCAL_PATH,
BUCKET=BUCKET_NAME,
clave=S3_KEY,
Args adicionales = {
'Metadatos': {
'fuente': 'sincronización automática',
'sync-timestamp': str (context.aws_request_id)
},
'ServerSideEncryption': 'aws:kms'
}
)
archivos_subidos += 1

# El registro se completó correctamente
logger.info (f «Se cargaron correctamente los archivos {files_uploaded} a S3")

# Sincroniza la fuente de datos de Q Business
qbusiness = boto3.client ('qbusiness')
app_id = os.environ.get ('QBUSINESS_APP_ID')
data_source_id = os.environ.get ('QBUSINESS_DATA_SOURCE_ID')
index_id = os.environ.get ('QBUSINESS_INDEX_ID')

si todos ([app_id, data_source_id, index_id]):
prueba:
qbusiness.start_data_source_sync_job (
ID de aplicación = APP_ID,
dataSourceID=Data_Source_ID,
indexId=index_ID
)
logger.info («Se activó la sincronización de la fuente de datos de Q Business»)
excepto la excepción e:
logger.warning (f «No se pudo activar Q Business sync: {str (e)}»)

return {
'Código de estado': 200,
'cuerpo': json.dumps ({
'message': f'Se cargaron correctamente los archivos {files_uploaded} a S3',
'bucket': bucket_name,
'archivos_subidos': archivos_subidos
})
}

excepto Subprocess.CalledProcessError como e:
error_message = f"Falló el clon de Git: {e.stderr}»
logger.error (mensaje_error)
Generar una excepción (error_message)

excepto Exception como e:
error_message = f"Se ha producido un error: {str (e)}»
logger.error (mensaje_error)
Generar una excepción (error_message)

finalmente:
# Limpieza segura
si os.path.exists ('/tmp/playbooks'):
subprocess.run (['rm', '-rf', '/tmp/playbooks'], check=True)
```

6. **Configurar las variables de entorno**:
- `BUCKET_NAME`: el nombre del bucket de S3 de AWS playbooks
- `QBUSINESS_APP_ID`: el ID de su aplicación Q Business (de los resultados de CloudFormation)
- `QBUSINESS_DATA_SOURCE_ID`: el ID de su fuente de datos
- `QBUSINESS_INDEX_ID`: Su ID de índice

7. **Establezca el tiempo de espera de ejecución** en 5 minutos
8. **Configure la función de IAM mejorada** con los permisos mínimos requeridos
9. **Despliega la función**

## Paso 7: Programe actualizaciones automatizadas con una seguridad mejorada

1. **Vaya a la consola de Amazon EventBridge**
2. **Haga clic en «Crear regla"**
3. **Regla de configuración**:
- Nombre: `SecurePlaybookUpdate`
- Descripción: `Actualizaciones seguras y automatizadas del libro de jugadas`
- Tipo de regla: `Horario`
4. **Establece un patrón de programación** (p. ej., todos los días a las 2 a.m. UTC)
5. **Añadir objetivo**:
- Servicio: AWS Lambda
- Función: su función Lambda segura
6. **Agregue un transformador de entrada** para mejorar el registro
7. **Revisar y crear**

## Paso 8: Supervise y pruebe la solución segura

### Pruebas
1. **Abra su aplicación Amazon Q** a través del portal Identity Center
2. **Pruebe consultas relacionadas con la seguridad**:
- «¿Cómo puedo mitigar un ataque de ransomware?»
- «Muéstreme los procedimientos de respuesta a incidentes en caso de filtraciones de datos»
- «¿Cuáles son los pasos para resolver los problemas de conformidad con AWS Config?»
3. **Verifique que las respuestas** incluyan la información relevante del manual

### Supervisión
1. **Compruebe los registros de acceso de S3** en su depósito de registros dedicado
2. **Supervise CloudTrail** para las llamadas a la API de Q Business
3. **Revise los registros de ejecución de Lambda** para ver las operaciones de sincronización
4. **Configurar las alarmas de CloudWatch** en caso de sincronizaciones fallidas o anomalías de acceso

## Supervisión y cumplimiento de la seguridad

### Registro de acceso
- **Registros de acceso a S3**: almacenados en un depósito dedicado con una retención de 90 días
- **Integración con CloudTrail**: todas las llamadas a la API se registran y supervisan
- **Registros de ejecución de Lambda**: registro detallado de operaciones de sincronización

### Prácticas recomendadas de seguridad
- **Revisiones de acceso habituales**: revise el acceso de los usuarios trimestralmente
- **Auditorías de contenido del manual de jugadas**: asegúrese de que la información confidencial esté debidamente clasificada
- **Supervisión de costes**: configure los presupuestos de AWS para controlar los costos
- **Análisis de seguridad**: escaneos periódicos del contenido cargado

## Estimación de costes (versión de seguridad mejorada)

La versión segura incluye componentes adicionales que pueden afectar a los costes:

| Componente | Configuración | Coste mensual (USD) |
|-----------|---------------|-------------------|
| Amazon Q Business Lite | 4500 usuarios × 3$ por usuario | 13.500$ |
| Amazon Q Business Pro | 500 usuarios × 20$ por usuario | 10 000$ |
| Índice empresarial | 50 unidades de índice × 0,264 dólares/hora | 9.504$ |
| AWS Lambda | 1 millón de solicitudes al mes | 46,91$ |
| Amazon API Gateway | 1 millón de solicitudes al mes | 5$ |
| **Registro de acceso a S3** | **Costos de almacenamiento adicionales** | **~50-100** |
| **Cifrado KMS** | **Uso de clave adicional** | **~$10-20** |
| **Supervisión mejorada** | **Registros y métricas de CloudWatch** | **~25-50 USD** |
| **Costo total estimado** | | **33.140 $ - 33.225 dólares** |

### Optimización de costos para una versión segura
- Supervise el almacenamiento de los registros de acceso y ajuste la retención según sea necesario
- Utilice S3 Intelligent Tiering para el almacenamiento de registros
- Optimice la frecuencia de ejecución de Lambda
- Limpieza periódica de los recursos no utilizados
- Configure etiquetas detalladas de asignación de costes

## Clean Up (versión segura)

Para evitar cargos continuos y garantizar una limpieza completa:

1. **Eliminar la pila de CloudFormation:
``bash
aws cloudformation delete-stack --stack-name q-security-playbooks-secure
```

2. **Verifique la limpieza del depósito de S3**:
- Compruebe que se hayan eliminado los tres depósitos (datos + registros)
- Vacíe los cubos manualmente si la eliminación no se realiza correctamente

3. **Limpiar Lambda y EventBridge**:
- Eliminar la función Lambda
- Eliminar las reglas de EventBridge

4. **Verifique la limpieza del centro de identidad**:
- Eliminar las asignaciones de aplicaciones
- Limpia los usuarios de prueba si los has creado

5. **Compruebe el uso de claves KMS**:
- Revise las políticas clave de KMS si usa claves personalizadas

## Consideraciones de seguridad

### Clasificación de datos
- Asegúrese de que los manuales de estrategias no contengan credenciales confidenciales
- Implemente etiquetas de clasificación de datos adecuadas
- Auditorías de contenido periódicas para comprobar su conformidad

### Control de acceso
- Utilice los grupos del Centro de Identidad para el acceso basado en roles
- Implemente los principios del mínimo privilegio
- Acceso periódico, revisiones y limpieza

### Supervisión y alertas
- Configure alertas para patrones de acceso inusuales
- Supervise los intentos de autenticación fallidos
- Rastrea los errores de sincronización de la fuente de datos

## Solución de problemas

### Problemas comunes relacionados con la seguridad
1. **Errores de acceso denegado**: compruebe las asignaciones de usuarios de Identity Center
2. **Fallos de sincronización**: compruebe que los permisos de los roles de IAM tengan el alcance correcto
3. **Registros faltantes**: verifica los permisos del depósito de registro de acceso
4. **Errores de KMS**: asegúrese de que las políticas clave de KMS sean adecuadas

### Recursos de soporte
- Soporte de AWS para problemas de Q Business
- Documentación de CloudFormation para la solución de problemas de plantillas
- Documentación del Identity Center para la gestión de usuarios

## Próximos pasos

1. **Implemente controles de seguridad adicionales** según sea necesario para su organización
2. **Configure el escaneo de seguridad automatizado** del contenido del manual
3. **Intégrelo con los sistemas SIEM** para mejorar la supervisión
4. **Desarrolle libros de jugadas personalizados** específicos para su entorno
5. **Capacite al equipo de seguridad** sobre el uso efectivo de Q Business
6. **Establezca procesos de gobernancia** para la gestión del manual de estrategias

## Avisos

Esta guía de implementación segura proporciona controles de seguridad mejorados para la implementación de AWS Q Business. Los clientes son responsables de:
- Garantizar el cumplimiento de las políticas de seguridad de la organización
- Revisiones y actualizaciones de seguridad periódicas
- Clasificación y manejo adecuados de los datos
- Procedimientos de monitoreo y respuesta a incidentes

Los productos y servicios de AWS se proporcionan «tal cual» sin garantías. Esta guía representa las mejores prácticas actuales y está sujeta a cambios. Consulte siempre la documentación de AWS y las prácticas recomendadas de seguridad más recientes para su caso de uso específico.

[q-securityplaybooks-secure.json] (https://github.com/user-attachments/files/21760628/Q-SecurityPlaybooks-Secure.json)