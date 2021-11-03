-docker compose up para levantar el contenedor con el cliente y el servidor
-apt get update
apt get-install net-tools
- sudo nano /etc/resolv.conf 
-nameserver ip_del_servidor_dns
-domain zeppelinux.es para establecer el dominio predeterminado


-----------------------X-------------------------------------
**Docker compose para crear el contenedor servidor y el contenedor cliente**
version: "2.2"
services:
  asir_bind9:
    image: internetsystemsconsortium/bind9:9.16
    ports:
      - 53:53
    volumes:
      - conf:/etc/bind
  asir_cliente:
    image: ubuntu
    stdin_open: true  # docker run -i
    tty: true         # docker run -t
volumes:
  conf:






