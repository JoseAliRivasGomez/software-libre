<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo-small.svg" width="200" alt="Nest Logo" /></a>
</p>

[<- Workshop 4](../Workshop04/README.md)

# Workshop 5

## 0. Instalar MySQL Workbench en Debian (anfitriona)

    sudo apt-get install mysql-workbench-community

## 1. Crear vagrantfile en la carpeta database

    vagrant init debian/bullseye64

## 2. Cambiar la IP de private_network a 192.168.56.11

## 3. Configuar maquina virtual y la base de datos

    vagrant up
    vagrant ssh
    sudo apt-get update
    sudo apt-get install mariadb-server mariadb-client vim vim-nox curl git

    sudo mysql
    create database northwind;
    create user user_laravel identified by 'secret';
    grant all privileges on northwind.* to user_laravel;
    flush privileges;
    use mysql;
    select user, host from user;

    sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf     (COMENTAR BIND-ADDRESS)
    sudo systemctl restart mysql

## 4. Ir a la carpeta donde estan los backups y resturar backups de la base de datos

    mysql northwind < northwind.sql -u user_laravel -p
    mysql northwind < northwind-data.sql -u user_laravel -p

## 5. Challenge #1: Recupere el código (id) y la descripción (type_name) de los tipos de movimiento de inventario (inventory_transaction_types).

    desc inventory_transaction_types;
    select id `codigo`, type_name `description` from inventory_transaction_types;

![Challenge #1](./images/1.png)

## 6. Challenge #2: Recupere la cantidad de ordenes (orders) registradas por cada vendedor (employees).

    desc orders;
    desc employees;
    select concat(e.first_name, ' ', e.last_name) as `Vendedor`,
        count(1) as `Cantidad`
        from orders o
        join employees e
            on e.id = o.employee_id
        group by `Vendedor`
        order by e.id;

![Challenge #2](./images/2.png)

## 7. Challenge #3: Recupere la lista de los 10 productos más ordenados (order_details), y la cantidad total de unidades ordenadas para cada uno de los productos. Deberá incluir como mínimo los campos de código (product_code), nombre del producto (product_name) y la cantidad de unidades.

    desc order_details;
    desc products;
    select p.product_code as `Codigo`,
        p.product_name as `Producto`,
        round(sum(od.quantity),2) as `Cantidad`
        from products p
        join order_details od
            on od.product_id = p.id
        group by p.id
        order by `Cantidad` DESC
        limit 10;

![Challenge #3](./images/3.png)

## 8. Challenge #4: Recupere el monto total (invoices, orders, order_details, products) y la cantidad de facturas (invoices) por vendedor (employee). Debe considerar solamente las ordenes con estado diferente de 0 y solamente los detalles en estado 2 y 3, debe utilizar el precio unitario de las lineas de detalle de orden, no considere el descuento, no considere los impuestos, porque la comisión a los vendedores se paga sobre el precio base.

    desc employees;
    desc invoices;
    desc orders;
    desc order_details;
    select count(1) as `Cantidad`,
		concat(e.first_name, ' ', e.last_name) as `Vendedor`,
        round(sum(od.unit_price * od.quantity),2) as `Monto`
        from employees e
        join orders o
        	on o.employee_id = e.id
        join order_details od
        	on od.order_id = o.id
        where o.status_id <> 0 
        	and od.status_id in (2,3)
        group by e.id
        order by `Cantidad` DESC, e.first_name ASC;

![Challenge #4](./images/4.png)

## 9. Challenge #5: Recupere los movimientos de inventario del tipo ingreso. Tomando como base todos los movimientos de inventario (inventory_transactions), considere unicamente el tipo de movimiento 1 (transaction_type) como ingreso. Debe agrupar por producto (inventory_transactions.product_id) y deberá incluir como mínimo los campos de código (product_code), nombre del producto (product_name) y la cantidad de unidades ingresadas.

    select p.product_code as `Codigo`,
        p.product_name as `Producto`,
        sum(it.quantity) as `Cantidad`
        from products p
        join inventory_transactions it
            on it.product_id = p.id
        where it.transaction_type in (1)
        group by p.id;

![Challenge #5](./images/5.png)

## 10. Challenge #6: Recupere los movimientos de inventario del tipo salida. Tomando como base todos los movimientos de inventario (inventory_transactions), considere unicamente los tipos de movimiento (transaction_type) 2, 3 y 4 como salidas. Debe agrupar por producto (products) y deberá incluir como mínimo los campos de código (product_code), nombre del producto (product_name) y la cantidad de unidades que salieron.

    select p.product_code as `Codigo`,
        p.product_name as `Producto`,
        sum(it.quantity) as `Cantidad`
        from products p
        join inventory_transactions it
            on it.product_id = p.id
        where it.transaction_type in (2,3,4)
        group by p.id;

![Challenge #6](./images/6.png)

## 11. Challenge #7: Genere un reporte de movimientos de inventario (inventory_transactions) por producto (products), tipo de transacción y fecha, entre las fechas 22/03/2006 y 24/03/2006 (incluyendo ambas fechas). Debe incluir como mínimo el código (product_code), el nombre del producto (product_name), la fecha truncada (transaction_created_date), la descripción del tipo de movimiento (type name) y la suma de cantidad (quantity) .

    select p.product_code as `Codigo`,
        p.product_name as `Producto`,
        itt.type_name as `Tipo`,
        date_format(it.transaction_created_date, "%d/%m/%Y") as `Fecha`,
        sum(it.quantity) as `Cantidad`
        from inventory_transactions it
        join products p
            on it.product_id = p.id
        join inventory_transaction_types itt
            on it.transaction_type = itt.id
        where it.transaction_created_date between '2006-03-22' and '2006-03-24'
        group by it.id
		order by p.product_name asc;

![Challenge #7](./images/7.png)

## 12. Challenge #8: Genere la consulta SQL para un reporte de inventario, tomando como base todos los movimientos de inventario (inventory_transactions), considere los tipos de movimiento (transaction_type) 2, 3 y 4 como salidas y el tipo 1 como ingreso. Este reporte debe estar agrupado por producto (products) y deberá incluir como mínimo los campos de código (product_code), nombre del producto (product_name) y la sumarización de ingresos, salidas y la cantidad disponible en inventario (diferencia de ingresos - salidas).

    select p.product_code as `Codigo`,
        p.product_name as `Producto`,
        sum(if(it.transaction_type = 1, it.quantity, 0)) as `Ingresos`,
        sum(if(it.transaction_type > 1, it.quantity, 0)) as `Salidas`,
        sum(if(it.transaction_type = 1, it.quantity, 0)) - sum(if(it.transaction_type > 1, it.quantity, 0)) as `Disponible`
        from products p
        join inventory_transactions it
            on it.product_id = p.id
        group by p.id;

![Challenge #8](./images/8.png)