# Webproxy

El servicio de proxy en VyOS se basa en Squid y algunos módulos relacionados.

Squid es un proxy web HTTP de almacenamiento en caché y reenvío. Tiene una amplia variedad de usos, incluida la aceleración de un servidor web al almacenar en caché solicitudes repetidas, almacenar en caché web, DNS y otras búsquedas de redes informáticas para un grupo de personas que comparten recursos de red y ayudar a la seguridad al filtrar el tráfico. Aunque se usa principalmente para HTTP y FTP, Squid incluye soporte limitado para varios otros protocolos, incluidos Internet Gopher, SSL, [6] TLS y HTTPS. Squid no es compatible con el protocolo SOCKS.

El filtrado de URL es proporcionado por SquidGuard.

# Configuración
```
set service webproxy append-domain <domain>
```

Utilice este comando para especificar un nombre de dominio que se agregará a los nombres de dominio dentro de las URL que no incluyen un punto . se adjunta el dominio.

Ejemplo: para agregar se establece en vyos.net y la URL recibida es www/foo.html, el sistema usará la URL final generada de www.vyos.net/foo. html.
```
set service webproxy append-domain vyos.net
```
```
set service webproxy cache-size <size>
```
El tamaño de la memoria caché del proxy en disco es configurable por el usuario. El tamaño de caché predeterminado de Proxies está configurado en 100 MB.

La unidad de este comando es MB.
```
set service webproxy cache-size 1024
```
```
set service webproxy default-port <port>
```
Especifique el puerto utilizado en el que el servicio de proxy escucha las solicitudes. Este puerto es el puerto predeterminado utilizado para la dirección de escucha especificada.

El puerto predeterminado es 3128.
```
set service webproxy default-port 8080
```
```
set service webproxy domain-block <domain>
```
Se utiliza para bloquear dominios específicos por parte del Proxy. Especificar &quot;vyos.net&quot; bloqueará todo acceso a vyos.net, y especificar &quot;.xxx&quot; bloqueará todo acceso a las URL que tengan una URL que termine en .xxx.
```
set service webproxy domain-block vyos.net
```
```
set service webproxy domain-noncache <domain>
```
Permita el acceso a los sitios de un dominio sin recuperarlos de la memoria caché del proxy. Especificar &quot;vyos.net&quot; permitirá el acceso a vyos.net pero las páginas a las que se acceda no se almacenarán en caché. Es útil para resolver problemas con la verificación &quot;Si se modifica desde&quot; en ciertos sitios.
```
set service webproxy domain-noncache vyos.net
```
```
set service webproxy listen-address <address>
```
Especifica la dirección de escucha del servicio proxy. La dirección de escucha es la dirección IP en la que el servicio de proxy web escucha las solicitudes de los clientes.

Por seguridad, la dirección de escucha solo debe usarse en redes internas/de confianza.
```
set service webproxy listen-address 192.0.2.1
```
```
set service webproxy listen-address <address> disable-transparent
```
Deshabilita el modo transparente de proxy web en una dirección de escucha.

En el modo de proxy transparente, todo el tráfico que llega al puerto 80 y tiene como destino Internet se reenvía automáticamente a través del proxy. Esto permite el reenvío de proxy inmediato sin configurar los navegadores de los clientes.

El proxy no transparente requiere que los navegadores de los clientes estén configurados con la configuración del proxy antes de que se redirijan las solicitudes. La ventaja de esto es que el navegador web del cliente puede detectar que se está utilizando un proxy y puede comportarse en consecuencia. Además, el malware transmitido por la web a veces puede ser bloqueado por un proxy web no transparente, ya que no conocen la configuración del proxy.
```
set service webproxy listen-address 192.0.2.1 disable-transparent
```
```
set service webproxy listen-address <address> port <port>
```
Establece el puerto de escucha para una dirección de escucha. Esto anula el puerto predeterminado de 3128 en la dirección de escucha específica.
```
set service webproxy listen-address 192.0.2.1 port 8080
```
```
set service webproxy reply-block-mime <mime>
```
Se utiliza para bloquear un tipo de mimo específico.

# block all PDFs
```
set service webproxy reply-block-mime application/pdf
```
```
set service webproxy reply-body-max-size <size>
```
Especifica el tamaño máximo del cuerpo de una respuesta en KB, que se usa para limitar el tamaño de la respuesta.

Todos los tamaños de respuesta se aceptan de forma predeterminada.
```
set service webproxy reply-body-max-size 2048
```
```
set service webproxy safe-ports <port>
```
Agregar nuevo puerto a Safe-ports acl. Puertos incluidos por defecto en Safe-ports acl: 21, 70, 80, 210, 280, 443, 488, 591, 777, 873, 1025-65535
```
set service webproxy ssl-safe-ports <port>
```
Agregue un nuevo puerto a SSL-ports acl. Puertos incluidos por defecto en SSL-ports acl: 443

#Autenticación

El proxy Squid incorporado puede usar LDAP para autenticar a los usuarios en un directorio de toda la empresa. La siguiente configuración es un ejemplo de cómo usar Active Directory como backend de autenticación. Las consultas se realizan vía LDAP.
set service webproxy authentication children <number>

Número máximo de procesos de autenticación para generar. Si comienza con muy pocos Squid, tendrá que esperar a que procesen una acumulación de verificaciones de credenciales, lo que lo ralentizará. Cuando las verificaciones de contraseña se realizan a través de una red (lenta), es probable que necesite muchos procesos de autenticación.

Esto por defecto es 5.
```
set service webproxy authentication children 10
```
```
set service webproxy authentication credentials-ttl <time>
```
Especifica durante cuánto tiempo squid asume que un par de nombre de usuario:contraseña validado externamente es válido; en otras palabras, con qué frecuencia se llama al programa auxiliar para ese usuario. Configure este valor bajo para forzar la revalidación con contraseñas de corta duración.

El tiempo es en minutos y el valor predeterminado es 60.
```
set service webproxy authentication credentials-ttl 120
```
```
set service webproxy authentication method <ldap>
```
Método de autenticación de proxy, actualmente solo se admite LDAP.
```
set service webproxy authentication method ldap
```
```
set service webproxy authentication realm
```
Especifica el ámbito de protección (también conocido como nombre de dominio) que se debe informar al cliente para el esquema de autenticación. Por lo general, es parte del texto que el usuario verá cuando se le solicite su nombre de usuario y contraseña.
```
set service webproxy authentication realm "VyOS proxy auth"
```

# LDAP
```
set service webproxy authentication ldap base-dn <base-dn>
```
Especifica el DN base bajo el cual se ubican los usuarios.
```
set service webproxy authentication ldap base-dn DC=vyos,DC=net
```
```
set service webproxy authentication ldap bind-dn <bind-dn>
```
El DN y la contraseña para enlazar mientras se realizan búsquedas.
```
set service webproxy authentication ldap bind-dn CN=proxyuser,CN=Users,DC=vyos,DC=net
```
```
set service webproxy authentication ldap filter-expression <expr>
```
Filtro de búsqueda LDAP para localizar el DN del usuario. Obligatorio si los usuarios están en una jerarquía por debajo del DN base, o si el nombre de inicio de sesión no es lo que crea la parte específica del usuario del DN de los usuarios.

El filtro de búsqueda puede contener hasta 15 ocurrencias de %s que serán reemplazadas por el nombre de usuario, como en &quot;uid=%s&quot; para los directorios RFC 2037. Para obtener una descripción detallada de la sintaxis del filtro de búsqueda LDAP, consulte RFC 2254.
```
set service webproxy authentication ldap filter-expression (cn=%s)
```
```
set service webproxy authentication ldap password <password>
```
El DN y la contraseña para enlazar mientras se realizan búsquedas. Como la contraseña debe imprimirse en texto sin formato en su configuración de Squid, se recomienda encarecidamente utilizar una cuenta con privilegios asociados mínimos. Esto para limitar el daño en caso de que alguien pueda obtener una copia de su archivo de configuración de Squid.
```
set service webproxy authentication ldap password vyos
```
```
set service webproxy authentication ldap persistent-connection
```
Utilice una conexión LDAP persistente. Normalmente, la conexión LDAP solo se abre mientras se valida un nombre de usuario para preservar los recursos en el servidor LDAP. Esta opción hace que la conexión LDAP se mantenga abierta, lo que permite reutilizarla para posteriores validaciones de usuarios.

Recomendado para instalaciones más grandes.
```
set service webproxy authentication ldap persistent-connection
```
```
set service webproxy authentication ldap port <port>
```
Especifique un puerto TCP alternativo en el que escuche el servidor ldap si no es el puerto LDAP predeterminado 389.
```
set service webproxy authentication ldap port 389
```
```
set service webproxy authentication ldap server <server>
```
Especifique el servidor LDAP al que conectarse.
```
set service webproxy authentication ldap server ldap.vyos.net
```
```
set service webproxy authentication ldap use-ssl
```
Utilice el cifrado TLS.
```
set service webproxy authentication ldap use-ssl
```
```
set service webproxy authentication ldap username-attribute <attr>
```
Especifica el nombre del atributo DN que contiene el nombre de usuario/inicio de sesión. Combinado con el DN base para construir el DN de los usuarios cuando no se especifica ningún filtro de búsqueda (expresión-filtro).

El valor predeterminado es 'uid'

> [!NOTE]
> Esto solo se puede hacer si todos sus usuarios están ubicados directamente debajo de la misma posición en el árbol LDAP y el nombre de inicio de sesión se usa para nombrar cada objeto de usuario. Si su árbol LDAP no coincide con estos criterios o si desea filtrar quiénes son usuarios válidos, debe usar un filtro de búsqueda para buscar el DN de sus usuarios (expresión de filtro).

```
set service webproxy authentication ldap username-attribute uid
```
```
set service webproxy authentication ldap version <2 | 3>
```
Versión del protocolo LDAP. El valor predeterminado es 3 si no se especifica.
```
set service webproxy authentication ldap version 2
```

# Filtrado de URL

> [!WARNING]
> Call for Contributions
> Esta sección necesita mejoras, ejemplos y explicaciones. Por favor, eche un vistazo a la Guía de contribución para nuestra documentación.
> Deshabilita el filtrado web sin descartar la configuración.
> ```
> set service webproxy url-filtering disable
> ```

# Actualizar


Si desea utilizar las listas negras existentes, primero debe crear/descargar una base de datos. De lo contrario, no podrá confirmar los cambios de configuración.
update webproxy blacklists

Descargar/Actualizar lista negra completa
```
vyos@vyos:~$ update webproxy blacklists
Warning: No url-filtering blacklist installed
Would you like to download a default blacklist? [confirm][y]
Connecting to ftp.univ-tlse1.fr (193.49.48.249:21)
blacklists.gz        100% |*************************************************************************************************************| 17.0M  0:00:00 ETA
Uncompressing blacklist...
Checking permissions...
Skip link for   [ads] -> [publicite]
Building DB for [adult/domains] - 2467177 entries
Building DB for [adult/urls] - 67798 entries
Skip link for   [aggressive] -> [agressif]
Building DB for [agressif/domains] - 348 entries
Building DB for [agressif/urls] - 36 entries
Building DB for [arjel/domains] - 69 entries
...

Building DB for [webmail/domains] - 374 entries
Building DB for [webmail/urls] - 9 entries

The webproxy daemon must be restarted
Would you like to restart it now? [confirm][y]

[ ok ] Restarting squid (via systemctl): squid.service.
vyos@vyos:~$
```
```
update webproxy blacklists category <category>
```
Descargar/Actualizar lista negra parcial.

Utilice la función de completar con tabulación para obtener una lista de categorías.

    Para actualizar automáticamente los archivos de la lista negra

    set service webproxy url-filtering squidguard auto-update update-hour 23

    Para configurar el bloqueo agregue lo siguiente a la configuración

    set service webproxy url-filtering squidguard block-category ads

    set service webproxy url-filtering squidguard block-category malware

Omitir el webproxy
> [!WARNING]
> Call for Contributions
> Esta sección necesita mejoras, ejemplos y explicaciones. Por favor, eche un vistazo a la Guía de contribución para nuestra documentación.
> Algunos servicios no funcionan correctamente cuando se manejan a través de un proxy web. Entonces, a veces es útil omitir un proxy transparente:
> Para omitir el proxy para cada solicitud que se dirige a un destino específico:
> ```
> set service webproxy whitelist destino-dirección 198.51.100.33
> set service webproxy whitelist destino-dirección 192.0.2.0/24
> ```
> Para omitir el proxy para cada solicitud que proviene de una fuente específica:
> ```
> set service webproxy whitelist source-address 192.168.1.2
> set service webproxy whitelist source-address 192.168.2.0/24
> ```
> (Esto puede ser útil cuando un servicio al que se llama tiene muchas direcciones de destino que cambian con frecuencia, por ejemplo, Netflix).

# Ejemplos
```
vyos@vyos# show service webproxy
 authentication {
     children 5
     credentials-ttl 60
     ldap {
         base-dn DC=example,DC=local
         bind-dn CN=proxyuser,CN=Users,DC=example,DC=local
         filter-expression (cn=%s)
         password Qwert1234
         server ldap.example.local
         username-attribute cn
     }
     method ldap
     realm "VyOS Webproxy"
 }
 cache-size 100
 default-port 3128
 listen-address 192.168.188.103 {
     disable-transparent
 }
```
