# Redirección de puertos (Port Forwarding)

La redirección de puertos, a veces llamado tunelado de puertos, es la acción de redirigir un puerto de red de un nodo de red a otro. Esta técnica puede permitir que un usuario externo tenga acceso a un puerto en una dirección IP privada (dentro de una LAN) desde el exterior vía un router o máquina con NAT activado.


## Realizar una conexión entre mi máquina (localhost) y un servidor Apache
- Acceder al servidor a través del puerto 8080 en lugar del 80 (reservado para xampp)
- Entrar a la maquina virtual del apache a través del puerto virtual 80


## Instalar y configurar nuestro servidor Apache en un Ubuntu-Server
* https://www.digitalocean.com/community/tutorials/como-instalar-el-servidor-web-apache-en-ubuntu-18-04-es

## Configuración de puertos en VMware Workstation 16

Edit >>  VirtualNetwork editor >> Change Settings >> Elegir el adaptador NAT >> NAT Settings... >> Add >> Host Port: 8080 // Type: TCP // Virtual Machine IP address: (IP del ubuntu) // Virtual Machine Port: 80
-----------------
![Captura](https://user-images.githubusercontent.com/75449797/144110242-2d638850-a98e-4da0-a94b-b86de5d56969.PNG)

## Acceder al interfaz del Apache desde nuestra maquina (localhost)

http://127.0.0.1:8080/
