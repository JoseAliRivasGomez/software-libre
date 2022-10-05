<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[<- Volver](./README.md)

## 1. Abrir el archivo __hosts__

    cd \
    cd Windows\System32\drivers\etc
    nodepad hosts

    sudo nano /etc/hosts

## 2. Agregar 3 dominios en el archivo __hosts__

    192.168.56.10 webserver
    192.168.56.10 mibanco.com
    192.168.56.10 lfts.isw811.xyz

    CTRL+O, ENTER, CTRL+X

## 3. Hacer ping a los 3 dominios (levantar la maquina virtual)

    ping webserver
    ping mibanco.com
    ping lfts.isw811.xyz