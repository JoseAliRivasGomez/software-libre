<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[<- Workshop 5](../Workshop05/README.md)

[Workshop 7 ->](../Workshop07/README.md)

# Workshop 6

## 1. Crear archivo hello.sh en maquina anfitriona

    touch hello.sh

## 2. En hello.sh:

    #!/bin/bash 

    # Hola mundo en Bash

    clear
    echo "Hola mundo"

## 3. En la maquina virtual:

    echo "Hola mundo" > confidencial.txt
    ls
    ls -l
    chmod 600 confidencial.txt
    ls -l
    echo "Hola mundo publico" > publico.txt
    ls -l

    sudo adduser tony
    Ingresar la informacion de tony, password 12345678

    su tony
    cd
    pwd
    cd ..
    ls -l
    cd /vagrant
    ls -l
    cat publico.txt
    cat confidencial.txt
    exit

    sudo addgroup isw811
    sudo gpasswd -a tony isw811
    ls -l
    chown vagrant:isw811 confidencial.txt
    ls -l

    sudo adduser tommy
    Ingresar la informacion de tommy, password 12345678

    su tommy
    ls -l
    cat confidencial.txt
    exit

    su tony
    ls -l
    cat confidencial.txt
    exit

    chmod 640 confidencial.txt

    su tony
    ls -l
    cat confidencial.txt
    exit

    cat /etc/group 
    cat /etc/group | grep isw811

## 4. En hello.sh:

    #!/bin/bash 

    # Hola mundo en Bash

    clear
    printf "Ingrese su nombre: "
    read name
    echo

    if [ ${#name} -eq 0 ]; then
        echo "Me hubiera gustado conocerte..."
    else
        echo "Hola $name"
    fi

    while [ true ]; do
        printf "Ingrese su año de nacimiento (yyyy): "
        read birthyear
        echo

        if [ -z `echo $birthyear | grep -E ^-\?[0-9]*$`  ]; then
            echo "Debe ingresar un numero"
        else
            if [ $birthyear -ge 1930 ] && [ $birthyear -lt 2023 ]; then
                break
            else
                echo "Debe ingresar un numero entre 1930 y 2022"
            fi
        fi
    done

    if [ $birthyear -ge 2010 ]; then
        echo "Eres de la generacion alfa"
    elif [ $birthyear -ge 1995 ] && [ $birthyear -lt 2010 ]; then
        echo "Eres un nativo digital (Z)"
    elif [ $birthyear -ge 1982 ] && [ $birthyear -lt 1995 ]; then
        echo "Eres un millenial"
    elif [ $birthyear -ge 1965 ] && [ $birthyear -lt 1982 ]; then
        echo "Eres de la generacion X"
    elif [ $birthyear -ge 1945 ] && [ $birthyear -lt 1965 ]; then
        echo "Eres de la generacion de los Baby Boomers"
    elif [ $birthyear -lt 1945 ]; then
        echo "Eres de la generacion silenciosa"
    fi

## 5. En la maquina virtual:

    cd /vagrant
    ls
    ls -l
    chmod 744 hello.sh
    ./hello.sh

## 6. En la maquina virtual:

    cd ~
    mysqldump northwind > northwind.sql -u user_laravel -p
    ls -lh
    tar cvfz northwind.tar.gz northwind.sql
    rm northwind.sql

## 7. Crear archivo backup-northwind.sh en maquina anfitriona

    touch backup-northwind.sh

## 8. En backup-northwind.sh:

    #!/bin/bash

    site="northwind.isw811.xyz"
    directory="/home/vagrant/backups/"$site
    datetime=$(date +"%Y%m%d_%H%M%S")
    database="northwind"
    username="user_laravel"
    password="secret"

    if [ ! -z "$1" ]; then
        filename="$site"_"$1.sql"
    else
        filename="$site"_"$datetime.sql"
    fi

    mkdir -p $directory
    cd $directory

    echo "$filename" >> "$directory/backup_$site.log"
    echo "Inicio: "$(date +"%d/%m/%Y %H:%M:%S") >> "$directory/backup_$site.log"

    echo "Iniciando respaldo..."

    mysqldump northwind > "$directory/$filename" -u $username --password=$password

    echo "Comprimiendo respaldo..."

    tar cvfz "$filename.tar.gz" $filename

    echo "Eliminando archivo temporal..."

    rm $filename

    echo "Fin: "$(date +"%d/%m/%Y %H:%M:%S") >> "$directory/backup_$site.log"
    echo >> "$directory/backup_$site.log"

## 9. En la maquina virtual:

    cd /vagrant
    ls
    ls -l
    chmod 744 backup-northwind.sh
    ./backup-northwind.sh
    cd /home/vagrant/backups/northwind.isw811.xyz
    ls -lh
    cat backup_northwind.isw811.xyz.log

    cd /vagrant
    ./backup-northwind.sh martes
    cd /home/vagrant/backups/northwind.isw811.xyz
    ls -lh
    cat backup_northwind.isw811.xyz.log

    cd /vagrant
    date
    crontab -e
    Seleccionar nano
    Agregar debajo del ultimo comentario:
        29 03 * 11 * /vagrant/backup-northwind.sh
        * * * * * /vagrant/backup-northwind.sh
    CTRL+O, ENTER, CTRL+X

    cd /home/vagrant/backups/northwind.isw811.xyz
    ls

    cd ~
    ls -la
