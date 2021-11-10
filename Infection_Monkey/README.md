![InfectionMonkey](img/im1.png)

# Emulación de adversarios utilizando Infection Monkey  

# ¿Qué es Infection Monkey?  
**Infection Monkey** es una herramienta de pruebas de ciberseguridad, capaz de adentrarse por las partes más profundas del centro de datos. Se instala en una máquina virtual en alguna parte del centro de datos, y desde alli, intentara probar posibles fallos de seguridad. Se comporta más como un hacker que como un escáner de vulnerabilidades. Infection Monkey intentara recorrer el centro de datos aprovechando diferentes tecnicas de movimiento lateral típicos de un atacante real que ya ha comprometido un sistema interno. Cuando llega con éxito a otra máquina, significa que hay un fallo de seguridad que debe ser solucionado.  

El funcionamiento de alto nivel de Infection Monkey es sencillo. Está diseñado para enumerar la red, comprobar si hay puertos abiertos y tomar las huellas digitales (fingerprints) de las máquinas, utilizando múltiples protocolos de red. Después de detectar las máquinas accesibles, intentara atacar a cada una de ellas utilizando una variedad de métodos que incluyen el bruteforce de contraseñas hasta exploits básicos. Infection Monkey es un trabajo en curso y aún queda camino por recorrer para aprovechar al máximo sus ventajas.  

# Escenario  
El objetivo de este laboratorio es aprender a instalar, configurar y utilizar la herramienta Infection Monkey. Esta herramienta nos permite realizar ataques a gran escala, utilizando ataques muy efectivos, creando nuestro propio sistema de ataque, todo esto con el fin de mejorar nuestra infraestructura, mejorar el equipo de seguridad y sobre todo mejorar las prácticas utilizadas para la defensa de nuestra organización.  

# Objetivos del laboratorio  
El objetivo de este laboratorio es:  

1 - Aprender a realizar un despliegue exitoso de Infection Monkey  
2 - Aprender a configurar y utilizar Infection Monkey  
3 - Enseñar lo que es posible hacer con Infection Monkey  

# Instalación de Infection Monkey
Vamos a la web oficial de Infection Monkey para solicitar la descargar del programa [Download Infection Monkey]( https://www.guardicore.com/infectionmonkey/). En este sitio nos pide llenamos un formulario para que nos envíen por email el sitio de descarga del programa.  

![Link_download](img/link1.png)  

Una vez recibido el correo electrónico, procederemos a la descarga del programa. Esta descarga es un archivo en formato [AppImage](https://appimage.org/). Una AppImage es un paquete independiente de la distribución y autoejecutable, que contiene una aplicación y todo lo que puede necesitar para ejecutarse.

El paquete AppImage de Infection Monkey debería ejecutarse en la mayoría de las distribuciones modernas de Linux que tienen FUSE instalado, pero las que hemos probado son:
- BlackArch 2020.12.01
- Kali 2021.2
- Parrot 4.11
- Rocky 8
- openSUSE Leap 15.3
- Ubuntu Bionic 18.04
- Ubuntu Focal 20.04
- Ubuntu Hirsute 21.04

Mientras de descarga el paquete, procedemos a actualizar nuestro sistema
```
sudo apt update
sudo apt upgrade
```
# Despliegue

1 - Hacer ejecutable el paquete AppImage
```
chmod u+x Infection_Monkey_v1.12.0.AppImage
```
2 - Inicie Monkey Island ejecutando el paquete Infection Monkey AppImage
```
./Infection_Monkey_v1.11.0.AppImage  
```
3 - Accede a la interfaz web de Monkey Island. Usando nuestra dirección IP, el protocolo https y el puerto 5000, nos conectaremos a la web de Infection Monkey.  
```
https://<DIRECCION_IP>:5000 
```
1 - Hacemos click en opciones avanzadas.  
2 - Aceptamos el riesgo y continuamos.  

![cert_warning](img/warning.png)  

# Configuración  
# Login por Primera Vez  
La primera vez que inicies **Monkey Island** (el servidor de C&C de Infection Monkey), se te pedirá que crees una cuenta y asegures tu isla. Tras la creación de la cuenta, el servidor solo será accesible a través de las credenciales que hayas introducido.  

![login](img/login.png)  


