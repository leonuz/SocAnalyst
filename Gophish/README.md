---
title: "Gophish"
author:
    name: "Leonardo Uzcátegui"
    url: https://leonuz.github.io
date: 14/11/2021
---
![gophish](img/gophish.jpg)
# Campaña de Phishing utilizando Gophish
# ¿Que es Gophish?
Gophish es un kit de herramientas de phishing de código abierto, diseñado para que los pentesters y las empresas realicen campañas de phishing. Tiene la capacidad de soportar una fácil y rápida configuración para ejecutar campañas de phishing. El objetivo detrás de su desarrollo es dar una formación de concienciación de seguridad mediante la simulación de ataques de phishing reales.  
La base de Gophish es la JSON API, con esta, el crear campañas de phishing automatizadas es muy fácil para los administradores y desarrolladores. Gophish utiliza la API para recopilar información para una campaña específica y crear informes personalizados. Existen otros atributos que están disponibles, tales como páginas de destino y plantillas, que ayudan en la creación de un programa de simulación totalmente automatizado para el phishing. Esta característica se utiliza para configurar campañas que pueden ejecutarse automáticamente durante todo el año. 

## Características  

- API REST completa  
- Interfaz de usuario fácil de usar  
- Plataforma cruzada  
- Cliente API Python  
- API JSON (documentación de la API disponible)  
- Plantillas y páginas de destino  
- Fácil de configurar y ejecutar campañas de phishing  

# Escenario  

El objetivo de una campaña de phishing es educar continuamente al usuario contra la amenaza que supone el phishing real. Hay que recordar que en la mayoría de los casos las fugas de información se producen por malas prácticas del propio usuario, o que el 80% de los casos de ransomware se producen a través del correo electrónico.  

# Objetivos del laboratorio  

El objetivo del laboratorio sera aprender a instalar, configurar y utilizar de manera exitosa Gophish, para realizar una campaña de suplantación y el posterior robo de credenciales de un portal web previamente acordado.  

- Mediante el lanzamiento de una campaña de phishing controlada, se puede evaluar la concienciación, y la formación sobre este tipo de amenazas, que son las causantes de un gran número de incidentes de seguridad en las empresas (fugas de información, robo de credenciales y ransomware).  
- Incluirá el envío de correos electrónicos con enlaces falsos (inofensivos y controlados por el equipo de comunidadDOJO), con control sobre quién y cuándo se accede a dichos enlaces.  
- Redactar un informe con los resultados de las pruebas y proponer un plan de mejora para mitigar o reducir los riesgos que este tipo de amenazas pueden suponer para los sistemas de información de la organización.  

# Instalación de Gophish  

Gophish es muy sencillo de instalar en tres pasos. Descargue el archivo, descomprima los archivos del software y ejecute el binario de ejecución. 

Gophish contiene el archivo de ejecución pre-construido, que funciona en la mayoría de los sistemas operativos. 

### Paso 1 - Actualizar el sistema
Es recomendable antes de instalar un nuevo programa que el sistema este completamente actualizado.  
```
# sudo apt update && apt upgrade
``` 
### Paso 2 - Descargar Gophish  
Podemos descargar Gophish de diferentes maneras, utilizando el comando WGET (dirección del enlace del archivo) y GIT CLONE (dirección del enlace del archivo) o simplemente descargando el paquete desde la página.  
```
# wget https://github.com/gophish/gophish/releases/download/v0.11.0/gophish-v0.11.0-linux-64bit.zip
```
```
leonuz@leonuzDojo:~/gophish$ wget https://github.com/gophish/gophish/releases/download/v0.11.0/gophish-v0.11.0-linux-64bit.zip
--2021-11-14 17:50:00--  https://github.com/gophish/gophish/releases/download/v0.11.0/gophish-v0.11.0-linux-64bit.zip
Resolving github.com (github.com)... 140.82.113.4
Connecting to github.com (github.com)|140.82.113.4|:443... connected.
HTTP request sent, awaiting response... 302 Found
Location: https://objects.githubusercontent.com/github-production-release-asset-2e65be/14508450/1b269d80-e95c-11ea-96b2-76f4d06f5fea?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20211114%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20211114T215002Z&X-Amz-Expires=300&X-Amz-Signature=786dc7e3fedb5e28732b79f5476b3dd309dade7e17f61a854c9ef57f1d4d0a77&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=14508450&response-content-disposition=attachment%3B%20filename%3Dgophish-v0.11.0-linux-64bit.zip&response-content-type=application%2Foctet-stream [following]
--2021-11-14 17:50:02--  https://objects.githubusercontent.com/github-production-release-asset-2e65be/14508450/1b269d80-e95c-11ea-96b2-76f4d06f5fea?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20211114%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20211114T215002Z&X-Amz-Expires=300&X-Amz-Signature=786dc7e3fedb5e28732b79f5476b3dd309dade7e17f61a854c9ef57f1d4d0a77&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=14508450&response-content-disposition=attachment%3B%20filename%3Dgophish-v0.11.0-linux-64bit.zip&response-content-type=application%2Foctet-stream
Resolving objects.githubusercontent.com (objects.githubusercontent.com)... 185.199.108.133, 185.199.109.133, 185.199.110.133, ...
Connecting to objects.githubusercontent.com (objects.githubusercontent.com)|185.199.108.133|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 32841663 (31M) [application/octet-stream]
Saving to: ‘gophish-v0.11.0-linux-64bit.zip’

gophish-v0.11.0-linux-64bit.zip     100%[=================================================================>]  31,32M  75,9KB/s    in 8m 26s  

2021-11-14 17:58:32 (63,3 KB/s) - ‘gophish-v0.11.0-linux-64bit.zip’ saved [32841663/32841663]

```
### Paso 3 - Descomprir el programa  
Si no tenemos la aplicación `unzip` instalada, la podemos instalar utilizando `sudo apt install unzip` 
```
# unzip gophish-v0.11.0-linux-64bit.zip  
```
```
leonuz@leonuzDojo:~/gophish$ unzip gophish-v0.11.0-linux-64bit.zip 
Archive:  gophish-v0.11.0-linux-64bit.zip
  inflating: gophish                 
   creating: static/js/dist/
   creating: static/js/dist/app/
  inflating: static/js/dist/app/landing_pages.min.js  
  inflating: static/js/dist/app/campaign_results.min.js  
  inflating: static/js/dist/app/users.min.js  
  inflating: static/js/dist/app/autocomplete.min.js  
  inflating: static/js/dist/app/groups.min.js  
  inflating: static/js/dist/app/passwords.min.js  
  inflating: static/js/dist/app/dashboard.min.js  
  inflating: static/js/dist/app/gophish.min.js  
  inflating: static/js/dist/app/settings.min.js  
  inflating: static/js/dist/app/webhooks.min.js  
  ...REDACTED...
  inflating: static/js/dist/app/templates.min.js 
  inflating: templates/reset_password.html  
  inflating: templates/flashes.html  
  inflating: README.md               
 extracting: VERSION                 
  inflating: LICENSE                 
  inflating: config.json             
leonuz@leonuzDojo:~/gophish$ ls
config.json  db  gophish  gophish-v0.11.0-linux-64bit.zip  LICENSE  README.md  static  templates  VERSION
```  
### Paso 4 - Permiso de ejecución
Le asignamos permiso de ejecución al binario gophish.
```
# chmod u+x gophish
```
```
leonuz@leonuzDojo:~/gophish$  chmod u+x gophish
leonuz@leonuzDojo:~/gophish$ ls -la
total 52568
drwxrwxr-x  5 leonuz leonuz     4096 nov 14 18:06 .
drwxr-xr-x 28 leonuz leonuz     4096 nov 14 17:49 ..
-rw-r--r--  1 leonuz leonuz      435 ago 28  2020 config.json
drwxr-xr-x  4 leonuz leonuz     4096 ago 28  2020 db
-rwxr--r--  1 leonuz leonuz 20945544 ago 28  2020 gophish
-rw-rw-r--  1 leonuz leonuz 32841663 ago 16 18:46 gophish-v0.11.0-linux-64bit.zip
-rw-r--r--  1 leonuz leonuz     1115 ago 28  2020 LICENSE
-rw-r--r--  1 leonuz leonuz     3274 ago 28  2020 README.md
drwxrwxr-x  7 leonuz leonuz     4096 nov 14 18:06 static
drwxr-xr-x  2 leonuz leonuz     4096 ago 28  2020 templates
-rw-r--r--  1 leonuz leonuz        7 ago 28  2020 VERSION
```
### Paso 5 - Editamos el archivo config.json 
Nos aseguramos de que el archivo de configuración config.json está correctamente configurado con el puerto y la dirección para establecer una conexión web.  
en este archivo debemos cambiar la dirección 127.0.0.1 a 0.0.0.0 para escuchar cualquier dirección
```
# nano config.json
```
debe de quedar asi:
```
{
        "admin_server": {
                "listen_url": "0.0.0.1:3333",
                "use_tls": true,
                "cert_path": "gophish_admin.crt",
                "key_path": "gophish_admin.key"
        },
        "phish_server": {
                "listen_url": "0.0.0.0:80",
                "use_tls": false,
                "cert_path": "example.crt",
                "key_path": "example.key"
        },
        "db_name": "sqlite3",
        "db_path": "gophish.db",
        "migrations_prefix": "db/db_",
        "contact_address": "",
        "logging": {
                "filename": "",
                "level": ""
        }
}
```
Cerramos el archvio, guardamos los cambios y ya queda configurado y listo para ejecutar el Gophish.  

# Inicializar Gophish  

### Paso 1 - ejecutar la aplicación  

Ejecute el comando `sudo ./gophish` para iniciar el programa.
```
# sudo ./gophish
```
### Paso 2 - Buscar el Password  

Caundo ejecutamos el comando `./gophish` en la consola del sistema aparecera el password de la siguiente manera
```
leonuz@leonuzDojo:~/gophish$ sudo ./gophish 
time="2021-11-14T19:11:58-04:00" level=warning msg="No contact address has been configured."
time="2021-11-14T19:11:58-04:00" level=warning msg="Please consider adding a contact_address entry in your config.json"
goose: no migrations to run. current version: 20200730000000
time="2021-11-14T19:11:58-04:00" level=info msg="Please login with the username admin and the password 6d62d49c09759c48"
time="2021-11-14T19:11:58-04:00" level=info msg="Starting admin server at https://0.0.0.0:3333"
time="2021-11-14T19:11:58-04:00" level=info msg="Background Worker Started Successfully - Waiting for Campaigns"
time="2021-11-14T19:11:58-04:00" level=info msg="Starting IMAP monitor manager"
time="2021-11-14T19:11:58-04:00" level=info msg="Starting new IMAP monitor for user admin"
time="2021-11-14T19:11:58-04:00" level=info msg="Starting phishing server at http://0.0.0.0:80"
```
Aqui vemos que el usuario que debemos utilizar es `admin` y el password es `6d62d49c09759c48`

### Paso 3 - Abrir el Navegador  
Utilizando nuestra dirección ip a través del puerto 3333 mediante el protocolo https accederemos a la web de gophish. Después de hacer clic en avanzado y luego aceptar el riesgo y continuar, nos enviará a la página de inicio de sesión. por ejemplo 
```
https://<DIRECCION_IP>:3333
```
1 - Hacemos click en opciones avanzadas.  
2 - Aceptamos el riesgo y continuamos.  

![cert_warning](img/warning.png)  


Nos pedira introducir las credenciales que conseguimos previamente en la consola (Paso 2 de Instalación)  


![admin](img/admin.png)


 Inmediatamente despues de entrar en el sistema nos pedira realizar un cambio de contraseña.  


![change](img/change_pwd.png)  


Una vez cambiada la contraseña, estaremos dentro de Gophish.  


![dashboard](img/dashboard.png)  







