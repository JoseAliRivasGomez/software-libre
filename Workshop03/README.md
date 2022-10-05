<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[<- Workshop 2](../Workshop02/README.md)

[Workshop 4 ->](../Workshop04/README.md)

# Workshop 3

## 1. En la maquina virtual, configurar el sitio mibanco.com

    cd /vagrant
    cp lfts.isw811.xyz.conf mibanco.com.conf
    sudo nano mibanco.com.conf (cambiar a mibanco.com X4)
    mkdir vhosts
    mv *.conf vhosts
    tree .

    mkdir -p sites/mibanco.com/public
    touch sites/mibanco.com/public/index.php
    echo "<h1>Hola soy tu banco</h1>" > sites/mibanco.com/public/index.php

    sudo cp /vagrant/vhosts/mibanco.com.conf /etc/apache2/sites-available/
    sudo apache2ctl -t
    sudo a2ensite /vagrant/vhosts/mibanco.com.conf
    sudo systemctl reload apache2.service

## 2. Desactivar sitio por defecto

    sudo a2dissite 000-default.conf
    sudo systemctl reload apache2.service

## 3. Instalar Composer

    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
    php composer-setup.php
    sudo mkdir -p /opt/composer
    sudo mv composer.phar /opt/composer/
    sudo ln -s /opt/composer/composer.phar /usr/bin/composer 
    composer

## 4. Crear proyecto de Laravel

    cd /vagrant/sites
    composer create-project laravel/laravel:8.6.12
    tar cvfx laravel.tar.gz laravel
    du -h laravel.tar.gz
    rm lfts.isw811.xyz -rf
    mv laravel lfts.isw811.xyz