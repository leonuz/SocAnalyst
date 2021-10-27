``````
![graylog](img/graylog.png)
---

#Graylog Server on Ubuntu 20.04
##Guia de Instalación
###Sobre Graylog


Graylog es una herramienta de código abierto utilizada para la agregación y gestión de registros (logs), se utiliza tambien para almacenar, analizar y enviar alertas de los registros recopilados. Con Graylog se pueden analizar registros estructurados y no estructurados utilizando ElasticSearch y MongoDB.  Esto incluye una variedad de sistemas, incluyendo sistemas Windows, sistemas Linux, diferentes aplicaciones y microservicios, etc.

**Graylog tiene los siguientes componentes**

1. [Servidor Graylog](https://www.graylog.org/products/open-source)
1. [MongoDB](https://www.mongodb.com/es)
1. [ElasticSearch](https://www.elastic.co/es/what-is/elasticsearch)

**Requisitos de hardware**
la instalación de Grylog requiere de las siguientes caracteristicas minimas de hardware:

-4 CPU Cores  
-8 GB RAM  
-Ubuntu 20.04 LTS actualizado.  

##Instalación##

###Prerequisitos de software  

La instalación de Graylog requiere de una version de Java 8 o superior. Se puede utilizar openJDk 11

```
sudo apt update && apt upgrade -y
```
```
sudo apt install -y apt-transport-https openjdk-11-jre-headless uuid-runtime pwgen curl dirmngr
```
Se puede verificar la version de Java recien instalada utilizando el siguiente comando:

```
user@Dojo:~/greylog$java -version
```
```
user@Dojo:~/greylog$ java -version
openjdk version "11.0.11" 2021-04-20
OpenJDK Runtime Environment (build 11.0.11+9-Ubuntu-0ubuntu2.20.04)
OpenJDK 64-Bit Server VM (build 11.0.11+9-Ubuntu-0ubuntu2.20.04, mixed mode, sharing)
```

##Paso 1 – Elasticsearch 

Descargar la llave de autenticacion GPG del repositorio de elsticksearch e instalarla:  
```
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
```

Agregar el repositorio de elasticsearch a las sources.list de Ubuntu:

```
echo "deb https://artifacts.elastic.co/packages/oss-7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list

```
**Instalar Elasticsearch**
```
sudo apt update
sudo apt install -y elasticsearch-oss
```

Editamos el archivo elasticsearch.yml:

```
sudo vim /etc/elasticsearch/elasticsearch.yml
```
Cambiamos el nombre del cluster a graylog: 

```
cluster.name: graylog
````

Agregamos la siguiente linea al final del archivo:

```
action.auto_create_index: false
```

Despues de editar el archivo de elasticsearch.yml, se inician los servicios:

```
sudo systemctl daemon-reload
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
```
```
user@Dojo:~/greylog$ sudo systemctl daemon-reload
user@Dojo:~/greylog$ sudo systemctl start elasticsearch
user@Dojo:~/greylog$ sudo systemctl enable elasticsearch
Synchronizing state of elasticsearch.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable elasticsearch
Created symlink /etc/systemd/system/multi-user.target.wants/elasticsearch.service → /lib/systemd/system/elasticsearch.service.
```

Se puede comprobar el estado del servicio de elasticsearch con el siguiente comando:

```
$ systemctl status elasticsearch
```
```
user@Dojo:~/greylog$ systemctl status elasticsearch
● elasticsearch.service - Elasticsearch
     Loaded: loaded (/lib/systemd/system/elasticsearch.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2021-10-26 23:43:10 -04; 34s ago
       Docs: https://www.elastic.co
   Main PID: 14728 (java)
      Tasks: 40 (limit: 4651)
     Memory: 1.2G
     CGroup: /system.slice/elasticsearch.service
             └─14728 /usr/share/elasticsearch/jdk/bin/java -Xshare:auto -Des.networkaddress.cache.ttl=60 -Des.networkaddress.cache.negative.t>

oct 26 23:42:42 Dojo systemd[1]: Starting Elasticsearch...
oct 26 23:43:10 Dojo systemd[1]: Started Elasticsearch.
```

Elasticsearch se anuncia en el puerto 9200 y se puede verificar con el comando `curl` de la siguiente manera:
```
curl -X GET http://localhost:9200
```

La salida del comando mostrara el nombre del cluster de elasticsearch:
```
user@Dojo:~/greylog$ curl -X GET http://localhost:9200
{
  "name" : "Dojo",
  "cluster_name" : "graylog",
  "cluster_uuid" : "6hGzbcSWTG-NgaJlkxIBCg",
  "version" : {
    "number" : "7.10.2",
    "build_flavor" : "oss",
    "build_type" : "deb",
    "build_hash" : "747e1cc71def077253878a59143c1f785afa92b9",
    "build_date" : "2021-01-13T00:42:12.435326Z",
    "build_snapshot" : false,
    "lucene_version" : "8.7.0",
    "minimum_wire_compatibility_version" : "6.8.0",
    "minimum_index_compatibility_version" : "6.0.0-beta1"
  },
  "tagline" : "You Know, for Search"
}
```
**Paso 2 – MongoDB  

**Instalación de MongoDB**

MongoDB se descarga y se instala directamente de las fuentes de los repositorios de Ubuntu

```
sudo apt update
```
```
sudo apt install -y mongodb-server
```

Luego de finalizada la instalación, se inician los servicios:

```
sudo systemctl start mongodb
sudo systemctl enable mongodb
sudo systemctl status mongodb
```
```
user@Dojo:~/greylog$ sudo systemctl start mongodb
user@Dojo:~/greylog$ sudo systemctl enable mongodb
Synchronizing state of mongodb.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable mongodb
user@Dojo:~/greylog$ sudo systemctl status mongodb
● mongodb.service - An object/document-oriented database
     Loaded: loaded (/lib/systemd/system/mongodb.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2021-10-26 23:51:28 -04; 1min 53s ago
       Docs: man:mongod(1)
   Main PID: 15671 (mongod)
      Tasks: 23 (limit: 4651)
     Memory: 42.1M
     CGroup: /system.slice/mongodb.service
             └─15671 /usr/bin/mongod --unixSocketPrefix=/run/mongodb --config /etc/mongodb.conf

oct 26 23:51:28 Dojo systemd[1]: Started An object/document-oriented database.
```

**Paso 3 – Graylog


Download and configure Graylog repository.***
```
wget https://packages.graylog2.org/repo/packages/graylog-4.1-repository_latest.deb
```
```
sudo dpkg -i graylog-4.1-repository_latest.deb
```
**Install Graylog server:**
```
sudo apt update
```
```
sudo apt install -y graylog-server
```
Generate a secret to secure user passwords using pwgen command

```
pwgen -N 1 -s 96
```
The output should look like:

FFP3LhcsuSTMgfRvOx0JPcpDomJtrxovlSrbfMBG19owc13T8PZbYnH0nxyIfrTb0ANwCfH98uC8LPKFb6ZEAi55CvuZ2Aum
Edit the graylog config file to add the secret we just created:
```
sudo vim /etc/graylog/server/server.conf
```
Locate the password_secret = line and add the above created secret after it.

password_secret = FFP3LhcsuSTMgfRvOx0JPcpDomJtrxovlSrbfMBG19owc13T8PZbYnH0nxyIfrTb0ANwCfH98uC8LPKFb6ZEAi55CvuZ2Aum

Also add the following lines to the /etc/graylog/server/server.conf file
```
rest_listen_uri = http://127.0.0.1:9000/api/
web_listen_uri = http://127.0.0.1:9000/
```
The next step is to create a hash sha256 pasword for the administrator. This is the password you will need to login to the web interface.

```
echo -n Str0ngPassw0rd | sha256sum
```
Replace ‘Str0ngPassw0rd’ with a password of your choice.

Alternatively use the command below:

```
$ echo -n "Enter Password: " && head -1 </dev/stdin | tr -d '\n' | sha256sum | cut -d" " -f1
```
Enter Password: password
You will get an output of this kind:

e3c652f0ba0b4801205814f8b6bc49672c4c74e25b497770bb89b22cdeb4e951
Edit the /etc/graylog/server/server.conf file then place the hash password at root_password_sha2 =

```
sudo vi /etc/graylog/server/server.conf
```
root_password_sha2 = e3c652f0ba0b4801205814f8b6bc49672c4c74e25b497770bb89b22cdeb4e951
Graylog is now configured and ready for use.

**Start Graylog service:**
```
sudo systemctl daemon-reload
sudo systemctl restart graylog-server
sudo systemctl enable graylog-server

```
user@Dojo:~/greylog$ sudo systemctl daemon-reload
user@Dojo:~/greylog$ sudo systemctl restart graylog-server
user@Dojo:~/greylog$ sudo systemctl enable graylog-server
Synchronizing state of graylog-server.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable graylog-server
Created symlink /etc/systemd/system/multi-user.target.wants/graylog-server.service → /lib/systemd/system/graylog-server.service.
```


You can check if the service has started successfully from the logs:

```
sudo tail -f /var/log/graylog-server/server.log
```


Graylog server up and running.
If you would like to Graylog interface with Server IP Address and port, then set http_bind_address to the public host name or a public IP address of the machine you can connect to

```
http_bind_address = 0.0.0.0:9000
```
And restart graylog-server service

```
sudo systemctl restart graylog-server
```
You can then access graylog web dashboard on:

```
http://serverip_hostname:9000
```
Updated:12/09/2021

Referencias
1. https://www.graylog.org/

1. https://computingforgeeks.com/install-graylog-on-ubuntu-with-lets-encrypt/#comments
``````