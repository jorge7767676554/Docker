-docker compose up para levantar el contenedor con el cliente y el servidor
-apt get update
apt get-install net-tools
- sudo nano /etc/resolv.conf 
-nameserver ip_del_servidor_dns
-domain zeppelinux.es para establecer el dominio predeterminado


-----------------------X-------------------------------------
**Docker compose para crear el contenedor servidor y el contenedor cliente**
version: "3.3"
services:
  asir_bind9:
    container_name: asir_bind9VVVVVVVVVV
    image: internetsystemsconsortium/bind9:9.16
    networks:
       br02:
        ipv4_address: 10.1.0.254
    ports:
      - 53:53
    volumes:
      - conf:/etc/bind
  asir_cliente:
    container_name: asir_clienteVVVVVVVVVV
    image: clienteimaxe
    networks:
      - br02
    stdin_open: true  # docker run -i
    tty: true         # docker run -t
    dns:
      - 10.1.0.254  # el contenedor dns server
volumes:
  conf:
    external: true # tenemos que tenerlo creado previamente
networks:
  br02: 
    external: true

**Para crear configuracion por separado**
en volumes con la opcion de conf:external(creado previamente)

**Red propia interna** 
con la opcion de network , y de nombre la interfaz que se haya creado
networks:
  br02: 
    external: true

**ip fija en el servidor**
En el apartado de networks, y con el nombre de la interfaz que se haya creado
 networks:
       br02:
        ipv4_address: 10.1.0.254

**Configurar Forwarders**       
En el apartado de etc/bind , se añaden las direcciones en el fichero named.conf.options

**Crear Zona propia**
    **Registros a configurar: NS, A, CNAME, TXT, SOA**
En el fichero etc/bind db.example.com
<!-- ;
; BIND data file for example.com
;
$TTL    604800
@       IN      SOA     example.com. root.example.com. (
                              2         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                         604800 )       ; Negative Cache TTL

@       IN      NS      ns.example.com.
@       IN      A       192.168.1.10
@       IN      AAAA    ::1
ns      IN      A       172.18.0.3
songoku       IN      A 172.18.0.11   -->      

**Cliente con herramientas de red**
Lo mas fácil es hacer un tag con una imagen con las herramientas instaladas, y en el docker compose image , utilizar esa imagen ya con todo preparado.
asir_cliente:
    container_name: asir_clienteVVVVVVVVVV
    image: clienteimaxe

**procedimiento de creacion de servicios**
Explicado con comentarios en el readme

**Para añadir un txt en la zona**
En db.example.com añadimos por ej:
<!-- songohanda IN TXT "hijo de songoku"  -->

Comprobamos con dig desde el cliente












