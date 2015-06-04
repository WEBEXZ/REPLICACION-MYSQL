# REPLICACION-MYSQL
COMANDOS

#!/bin/bash

 mkdir /etc/mysql_r #NOMBRE GENEREAL
  echo "SE CREO EL DIRECTORIO BASE";
 mkdir /etc/mysql_r/maestro #INSTANCIA MAESTRO
  echo "SE CREO EL DIRECTORIO MAESTRO";
 mkdir /etc/mysql_r/esclavo #INSTANCIA ESCLAVO
  echo "SE CREO EL DIRECTORIO ESCLAVO";
chown -R mysql:mysql /etc/mysql_r #CAMBIAR USARIO Y GRUPO

  mysql_install_db --user=mysql --datadir=/etc/mysql_r/maestro --server-id=1 --socket=/etc/mysql_r/maestro/mysql.sock ;
  mysql_install_db --user=mysql --datadir=/etc/mysql_r/esclavo --server-id=2 --socket=/etc/mysql_r/esclavo/mysql.sock ;
 
 echo "ARRANCANDO INSTANCIAS MAESTRO Y ESCLAVO DE MARIADB........";
 
 mysqld_safe --datadir=/etc/mysql_r/maestro --user=mysql --log_error=/etc/mysql_r/maestro/error.txt --log-bin=/etc/mysql_r/maestro/log-bin.log --port=3306 --pid-file    =/etc/mysql_r/maestro/mysql.pid --server-id=1 --socket=/etc/mysql_r/maestro/mysql.sock &
 mysqld_safe --datadir=/etc/mysql_r/esclavo --user=mysql --log_error=/etc/mysql_r/esclavo/error.txt --log-bin=/etc/mysql_r/esclavo/log-bin.log --port=3308 --pid-file    =/etc/mysql_r/esclavo/mysql.pid --server-id=2 --read-only=1 --socket=/etc/mysql_r/esclavo/mysql.sock &
 exit
