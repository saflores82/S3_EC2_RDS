# S3_EC2_RDS
Frontend en S3, Backend con EC2 y conexión a Base de datos RDS

Práctica de S3, EC2 y RDS en AWS
Esta guía te ayudará a desplegar una aplicación web en AWS utilizando varias tecnologías de la nube con Amazon Web Services.

*Paso 1: Configurar una instancia EC2 en AWS

Inicia sesión en la consola de AWS y navega a EC2.
Haz clic en "Launch Instance" para lanzar una nueva instancia.
Selecciona una AMI de Amazon Linux.
Selecciona el tipo de instancia t2.micro.
Asegúrate de configurar adecuadamente los ajustes de seguridad para permitir el tráfico HTTP y SSH.
Crea una nueva clave de par de claves o utiliza una existente para acceder a tu instancia mediante SSH.
Lanza la instancia.

*Paso 2: Instalar y configurar el servidor web Apache

Una vez conectado a la instancia EC2, ejecuta los siguientes comandos para instalar y configurar el servidor web Apache:

bash
Copy code
sudo yum update -y
sudo yum install httpd -y
sudo service httpd start
sudo chkconfig httpd on

Navega al directorio del servidor web:

bash
cd /var/www/html
Ejecuta los siguientes comandos para instalar PHP y Composer:

bash
sudo yum install php php-cli php-json php-mbstring –y
sudo php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php
sudo php -r "unlink('composer-setup.php');"
sudo php composer.phar require aws/aws-sdk-php
sudo service httpd restart
Paso 3: Crear un script PHP para procesar el formulario
Crea un archivo PHP llamado submit.php en el mismo directorio con el siguiente contenido. Asegúrate de reemplazar el ARN del tópico SNS por el tuyo.

submit.php
<?php 
// Código PHP
?>
Crea un archivo PHP de prueba en la ruta /var/www/html:

info.php
<?php phpinfo(); ?>

Accede al archivo PHP de prueba a través de tu navegador para verificar la configuración.

*Paso 4: Configuración adicional
Asegúrate de que el archivo de configuración de Apache (/etc/httpd/conf/httpd.conf) incluya la directiva AddType para PHP.

Añade esta línea al archivo:

bash
AddType application/x-httpd-php .php

Añade el rol "AWSSNSFullAccess" a la instancia EC2 en el servicio IAM.
Crea un tópico SNS y suscríbete a él con el protocolo "email".

*Paso 5: Frontend
Crea un bucket S3 y habilita el acceso público.
Habilita el hosting estático en la pestaña de propiedades con el documento index.html.

Añade la siguiente política de permisos en la pestaña de permisos:
json
{ 
    "Version": "2012-10-17", 
    "Statement": [ 
        { 
            "Effect": "Allow", 
            "Principal": "*", 
            "Action": [ 
                "s3:GetObject" 
            ], 
            "Resource": "arn:aws:s3:::YOUR_BUCKET_NAME/*" 
        } 
    ] 
}

Ejecuta los siguientes comandos en EC2 en la carpeta /tmp:

bash
wget https://github.com/technext/jobentry/archive/refs/tags/v1.0.0.zip
unzip v1.0.0.zip
Edita el archivo contact.html y añade el script JavaScript proporcionado.

Sustituye IP_de_la_ec2 por la IP pública de tu EC2:

bash
aws s3 cp jobentry-1.0.0/ s3://nombre-bucket/ --recursive

Accede a la URL del S3 y ve a la sección de contacto. Si envías el formulario, deberías recibir un correo electrónico con la información.

*Paso 6: Instalar MySQL en Amazon Linux 2023
MySQL no viene por defecto con Amazon Linux 2023. Sigue estos pasos para instalarlo:

Descarga el archivo RPM:
bash
sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-1.noarch.rpm 

Instala el archivo RPM:

bash
sudo dnf install mysql80-community-release-el9-1.noarch.rpm -y

Importa la clave pública de MySQL:

bash
sudo rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2023
Instala el cliente de MySQL:

bash
sudo dnf install mysql-community-client -y
Si necesitas el servidor:

bash
sudo dnf install mysql-community-server -y

*Paso 7: Utilizar la Base de datos y comprobar su correcto funcionamiento
¡Listo! Ahora puedes utilizar MySQL en tu instancia EC2 y verificar su correcto funcionamiento.

Asegúrate de seguir cada paso detalladamente y personalizar los detalles según tu configuración específica. ¡Disfruta tu práctica de Docker en AWS con MySQL! Si necesitas ayuda adicional, no dudes en preguntar.
