# Instalación de LDAP

# Configurar FQDN

En el primer paso, debe configurar el fqdn (nombre de dominio completo) adecuado de su servidor Debian. Esto se puede lograr usando la utilidad hostnamectl y modificando el archivo /etc/hosts.

Ejecute el siguiente comando hostnamectl para configurar el fqdn de su servidor Debian. En este caso, el fqdn que se utilizará para el servidor OpenLDAP es ldap.midominio.local.

```
sudo hostnamectl set-hostname ldap.mydomain.local
```
Usando el comando del editor nano, abra el archivo /etc/hosts.
```
sudo nano /etc/hosts
```
Agregue la dirección IP, el nombre de host y el fqdn de su servidor OpenLDAP de esta manera:
```
192.168.10.15  ldap.mydomain.local ldap
```
Una vez terminado, guarde el archivo y salga del editor.

Por último, ejecute el siguiente comando para asegurarse de tener el fqdn adecuado. Luego, asegúrese de que el fqdn apunte a la dirección IP correcta.
```
sudo hostname -f
```
```
sudo ping -c3 ldap.mydomain.local
```

# Instalación del servidor OpenLDAP

Después de configurar el fqdn, puede instalar el paquete del servidor OpenLDAP a través de APT desde el repositorio oficial de Debian. 
Luego, debe configurar cierta información básica sobre su servidor OpenLDAP, como un nombre de dominio y la contraseña de administrador.

Ejecute el comando apt install a continuación para instalar el paquete del servidor OpenLDAP. Cuando se le solicite, ingrese y para confirmar y continuar con la instalación.
```
sudo apt install slapd ldap-utils
```
Durante la instalación, se le pedirá que configure la contraseña de administrador para el servidor OpenLDAP. Ingrese su contraseña y repita.
Una vez instalado el servidor OpenLDAP, ejecute el siguiente comando para configurar su instalación OpenLDAP.
```
sudo dpkg-reconfigure slapd
```
> [!NOTE]
> Seleccione NO cuando se le solicite omitir la configuración predeterminada de OpenLDAP.
> 
> Ingrese el nombre de dominio para su servidor OpenLDAP y seleccione Aceptar.
> 
> Ingrese el nombre de la organización y seleccione Aceptar.
> 
> Ingrese su contraseña de administrador para el servidor OpenLDAP y repita la contraseña.
> 
> Cuando se le solicite eliminar la base de datos anterior del servidor OpenLDAP, seleccione NO.
> 
> Para finalizar la configuración del servidor OpenLDAP, seleccione SÍ cuando se le solicite mover la antigua base de datos OpenLDAP a una nueva ubicación.

Después de configurar el servidor OpenLDAP. ejecute el comando systemctl a continuación para reiniciar el servicio OpenLDAP slapd y aplicar los cambios. Luego, verifique el servicio slapd para asegurarse de que se esté ejecutando.
```
sudo systemctl restart slapd
```
```
sudo systemctl status slapd
```
```
sudo apt install apache2 php php-cgi libapache2-mod-php php-mbstring php-common php-pear -y

sudo apt -y install ldap-account-manager

sudo a2enconf php*-cgi

sudo systemctl restart apache2

sudo systemctl enable apache2

http://ip/lam
```
# Recursos
```
https://www.youtube.com/watch?v=LzRK_8zwqxY
https://www.youtube.com/watch?v=yGJERaeZmKc
https://www.youtube.com/watch?v=3OIjXgyJlBs
https://www.youtube.com/watch?v=mL7g35tTZY8
```
