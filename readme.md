# AWS EC2 Backups Manager

Este pryecto contiene los scripts que permiten generar EBS Snapshots haciendo uso de AWS Lambda & CloudWatch

En este documento se explicará cómo automatizar las generación de snapshots de EBS para la infraestructura AWS utilizando Lambda y CloudWatch. La solución es capaz de crear snapshots para los volúmenes etiquetados y elimina cualquier snapshot anterior a N días. Esto funcionará en todas las regiones de AWS.

Lambda ofrece la capacidad de ejecutar código "serverless" lo que significa que AWS nos brindará la plataforma en tiempo de ejecución. Lambda es compatible con Node.js, Java, C # y Python. Las funciones de este proyecto se desarrollaron en Python

CloudWatch permite activar la ejecución de las funciones de Lambda basadas en una expresión cron.

## Getting Started

1. El primer paso será crear un rol IAM que se capaz de:

* Recuperar información de los volumenes y snapshots desde EC2.
* Poder hacer llamadas al CreateSnapshot API.
* Poder hacer llamadas al DeleteSnapshot API.
* Escribir logs en CloudWatch.

En la consola de AWS, se debe ir a IAM > Roles > Create New Role y en los tipos de roles seleccionar AWS Lambda.  

Luego de crear el rol debemos volver a el para que en la pestaña Permissions buscar el link para cear una politica customizada. Allí debemos cpiar el json adjunto llamado "policy.json"

2. Crear la función "Create Snapshots" en Lambda

En la consola de Lambda, ir a Functions > Create a Lambda Function. En la configuración se debe elegir como runtime Python 2.7

En el código se hace uso de la libreria Boto que pertenece al AWS SDK para Python. 

En el Lambda Function handle and role, dbemos elegir el rol que se creó en el primer paso. 

En la configuración avanzada aumentamos el timeout de 3 segundos (default) a 1 minuto. De esa forma ese será el timeout de la función en cada volumen.

3. Programar un trigger como regla de CloudWatch

Para eso debemos navegar al tab "Triggers" y hacer click en "Add Trigger", en la siguiente ventana se selecciona "CloudWatch Event - Schedule" en las opciones del dropdown. Esto permitirá crear un schedule basado en una regla.

Seguir el estandar Cron. 

4. Hacer lo mismo para la función DeleteSnapshot

5. Etiquetar un volumen para programar su backup

La función hara una busqueda de todos los volumenes que contengan el Tag "Backup" distinto de vacio. Esos serán los volumenes que se guardarán.

Nota importante: La instancia no debe contener nombres con tildes. Ej. "Ambiente de producción"

## Built With

Se hace uso del AWS SDK for Python (Boto). Más información [aquí](https://aws.amazon.com/es/developers/getting-started/python/)

## Authors

* **Luis Izaguirre** - *@lsizaguirre* - [GitHub](https://github.com/lsizaguirre)