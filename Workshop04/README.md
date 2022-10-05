<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[<- Workshop 3](../Workshop03/README.md)

# Workshop 4

## 1. Crear archivo tar.gz y omitir los "node_modules" y "vendor"

    tar cvfz lfts1.tar.gz --exclude={lfts.isw811.xyz/node_modules,lfts.isw811.xyz/vendor} lfts.isw811.xyz/

## 2 . Configurar base de datos en Debian Bullseye

    sudo apt-get install mariadb-server mariadb-client

    sudo mysql
    show databases;
    create database db_laravel;
    create user user_laravel identified by 'secret';
    grant all privileges on db_laravel.* to user_laravel;
    flush privileges;
    quit;

    sudo mysql -u user_laravel --password="secret"
    show databases;
    quit

## 3. Configurar otro sitio web con Laravel

    cd /vagrant/sites
    tar xvfz laravel.tar.gz
    mv laravel miblog.com
    cd vhosts
    cp lfts.isw811.xyz.conf miblog.com.conf
    sed -i -e 's/lfts.isw811.xyz/miblog.com/g' miblog.com.conf

    sudo cp /vagrant/vhosts/miblog.com.conf /etc/apache2/sites-available/
    sudo apache2ctl -t
    sudo a2ensite /vagrant/vhosts/miblog.com.conf
    sudo systemctl reload apache2.service

## 4. Agregar dominio en el archivo __hosts__ de la maquina anfitriona

    192.168.56.10 miblog.com

## 5. Instalar NVM, Node y NPM

    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    source ~/.bashrc
    nvm ls-remote
    nvm install --lts / nvm install Gallium (Node version)    

## 6. Instalar vistas de autenticacion en Laravel

    cd /vagrant/sites/miblog.com
    composer require laravel/ui
    php artisan ui bootstrap
    php artisan ui bootstrap --auth
    npm i
    npm run dev

## 7. Conectar el proyecto de Laravel a la base de datos, en el .env:

    DB_CONNECTION=mysql
    DB_HOST=localhost
    DB_PORT=3306
    DB_DATABASE=db_laravel
    DB_USERNAME=user_laravel
    DB_PASSWORD=secret

## 8. Ejecutar las migraciones de Laravel, para crear las tablas de la base de datos

    sudo php artisan migrate
    sudo mysql -u user_laravel --password="secret"
    use db_laravel;
    show tables;
    desc users;
    select name, email from users;
    quit