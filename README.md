# Práctica 3.9: Despliegue web con bases de datos en AWS. Nivel básico.
# Realizado por Victor Fernandez, Pablo Merida y Andres Rodriguez
## EC2
Lanzaremos una instancia EC2.
Le pondremos la siguiente configuración:

La imagen del SO sera un Ubuntu Server 18.04.

![img](img/ec2creada1.png)

 Contará con una arquitectura de t2.micro y crearemos un par de claves de inicio de sesión.

![img](img/ec2creada2.png)

Asignaremos una IP elastica.

![img](img/ec2ipelastica1.png)

Asociamos la IP elastica.

![img](img/ec2ipelastica2.png)

Creamos el grupo de seguridad con las siguientes reglas de entradas.

![img](img/ec2grupodeseguridad.png)


## RDS
Lanzaremos una instancia RDS.
Le pondremos la siguiente configuración:
Tendrá un motor MySql y el tamaño será el que nos ofrece la capa gratuita, que en este caso es una t3.micro que tiene 2 CPUs y 1Gb de RAM.

También tendrá practica3-9 como nombre y el usuario maestro será admin. La contraseña que le pondremos sera Root1234$ que es la indicada en la práctica.

![img](img/rdscreada.png)

También cambiaremos en configuración adicional el acceso público y lo pondremos accesible públicamente.

![img](img/rdsAccesPublic.png)

El ounto de enlace es practica3-9.c1wspstddav2.us-east-1.rds.amazonaws.com y lo usaremos para conectarnos a la EC2.

![img](img/rdspuertodeenlace.png)

## S3
Lanzamos una instancia S3.
Le haremos las siguientes modificaciones al crearla:

Le ponemos la región de AWS asiganada por defecto, la propiedad de objetos la habilitamos, y por último desmarcamos la casilla para bloquear el acceso público (desbloqueamos el acceso público).

![img](img/s3creada1.png)
![img](img/s3creada2.png)

Cargamos los objetos en la pestaña de Objetos de la instancia de S3.

![img](img/s3archivos.png)

## Conexión
Una vez hechas las 3 máquinas necesarias haremos lo siguiente:

-> Conectar la máquina EC2 con la máquina S3

1º Nos conectamos por comando ssh a la instancia EC2

![img](img/conexionConexionEC2.png)

1.2º Modificar los roles de la instancia para poder conectar con S3

![img](img/ec2ModificarAMI.png)

2º Lanzaremos los siguientes comandos:

2.1º Estos comandos los usaremos para actualizar la máquina e instalar apache.
* `sudo apt update`
* `sudo apt install apache2`
* `sudo systemctl status apache2`

2.2º Ahora con estos comandos importamos los archivos que hay en la máquina S3 a la máquina EC2.
* `cd /var/www/html/`
* `sudo apt install awscli`

2.3º Descargamos los archivos de la máquina S3 con el siguiente comando.
* `sudo aws s3 sync s3://practica333 /var/www/html`

2.4º Ahora borraremos el index que viene predefinido en la ruta /var/www/html/
* `sudo rm /var/www/html/index.html`

