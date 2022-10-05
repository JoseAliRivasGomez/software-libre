<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

# Workshop 1

## 1. Instalar VirtualBox

<p align="left">
  <img src="./images/Virtualbox_logo.png" width="200" alt="VirtualBox Logo" />
</p>

## 2. Instalar Vagrant

<p align="left">
  <img src="./images/Vagrant_logo.png" width="200" alt="Vagrant Logo" />
</p>

## 3. Instalar Cmder (si esta en Windows)

<p align="left">
  <img src="./images/cmder_logo.png" width="200" alt="Cmder Logo" />
</p>


## 4. Abrir la terminal

    vagrant init debian/bullseye64

## 5. Cambiar la IP de private_network a 192.168.56.10

    vagrant up

## 6. Conectarse a la instancia

    vagrant ssh

    OR

    eval "$(ssh-agent -s)"

    ssh-add .vagrant/machines/default/virtualbox/private_key
    
    ssh vagrant@192.168.56.10

## 7. Crear el tar.gz del Workshop

    cd ..

    tar cvfz Workshop01-Jose-Ali.tar.gz Workshop01

## 8. Desconectarse y apagar la maquina virtual

    exit

    vagrant halt