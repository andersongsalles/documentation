#### Escopo Inicial do Projeto
------------------------------------------------------------------------------------------
Montar um Servidor para Hospedar vários sites/sistemas ;
Este sistema tem que possuir controle de usuarios/clientes ;
Os sites/sistemas hospedados tem que ter suporte a SSL, E-mail, Backup,Banco de dados;
Ser um sistema leve e de fácil administração;
Poder executar configurações adicionais definidas para cada site/sistema;
------------------------------------------------------------------------------------------

# Passos 3,2,1 estão no vídeo 01 
# Continuação do vídeo Instalando ISPConfig 3 no Debian - Parte 01 em >>> https://youtu.be/fK5zAh4ULHM



### Se for máquina virtual
nano /etc/amavis/conf.d/05-node_id 

#Comenta a linha
#chomp($myhostname = `hostname --fqdn`);

#Descomenta a linha e coloca o nome do host.local
$myhostname = "isp.local";
### Se for VPS ignore etapa acima

4 - Instalando o PHP 7.0 e 7.1 (Compilando/Instalando)

*** >>> Consultar a ultima versão em http://php.net/downloads.php

# Instalando dependencias 

apt-get install build-essential autoconf libfcgi-dev libicu-dev libfcgi0ldbl libjpeg62-turbo-dbg libmcrypt-dev libssl-dev libc-client2007e libc-client2007e-dev libxml2-dev libbz2-dev libcurl4-openssl-dev libjpeg-dev libpng12-dev libfreetype6-dev libkrb5-dev libpq-dev libxml2-dev libxslt1-dev

### Instalando o PHP 7.0 

mkdir -p /opt/php-7.0
mkdir /usr/local/src/php7-build
cd /usr/local/src/php7-build
wget http://br2.php.net/get/php-7.0.30.tar.bz2/from/this/mirror -O php-7.0.30.tar.bz2
tar jxf php-7.0.30.tar.bz2 
cd php-7.0.30

### Criando link simbólico para a lib 
ln -s /usr/lib/libc-client.a /usr/lib/x86_64-linux-gnu/libc-client.a

./configure --prefix=/opt/php-7.0 --with-pdo-pgsql --with-zlib-dir --with-freetype-dir --enable-mbstring --with-libxml-dir=/usr --enable-soap --enable-calendar --with-curl --with-mcrypt --with-zlib --with-gd --with-pgsql --disable-rpath --enable-inline-optimization --with-bz2 --with-zlib --enable-sockets --enable-sysvsem --enable-sysvshm --enable-pcntl --enable-mbregex --enable-exif --enable-bcmath --with-mhash --enable-zip --with-pcre-regex --with-pdo-mysql --with-mysqli --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --with-openssl --with-fpm-user=www-data --with-fpm-group=www-data --with-libdir=/lib/x86_64-linux-gnu --enable-ftp --with-imap --with-imap-ssl --with-kerberos --with-gettext --with-xmlrpc --with-xsl --enable-opcache --enable-fpm --enable-intl

make && make install

cp /usr/local/src/php7-build/php-7.0.19/php.ini-production /opt/php-7.0/lib/php.ini
cp /opt/php-7.0/etc/php-fpm.conf.default /opt/php-7.0/etc/php-fpm.conf
cp /opt/php-7.0/etc/php-fpm.d/www.conf.default /opt/php-7.0/etc/php-fpm.d/www.conf

### Editar o arquivo abaixo removendo o “;” na linha onde diz pid = run/php-fpm.pid
nano /opt/php-7.0/etc/php-fpm.conf

### Editar o arquivo abaixo alterando onde diz listen = 127.0.0.1:9000 para 8999
nano /opt/php-7.0/etc/php-fpm.d/www.conf


### Criando script para inicialização 
cd /etc/init.d/
wget https://www.dropbox.com/s/zz2zbbo2196qioe/php-7.0-fpm
chmod 755 php-7.0-fpm 
insserv php-7.0-fpm
/etc/init.d/php-7.0-fpm start

cd /lib/systemd/system/
wget https://www.dropbox.com/s/cf29anj5jv52o3q/php-7.0-fpm.service
chmod 755 php-7.0-fpm.service 
systemctl enable php-7.0-fpm.service
systemctl start php-7.0-fpm.service

### Instalando o PHP 7.1

cd /usr/local/src/php7-build
wget http://br2.php.net/get/php-7.1.5.tar.bz2/from/this/mirror -O php-7.1.5.tar.bz2
tar jxf php-7.1.5.tar.bz2 
cd php-7.1.5
./configure --prefix=/opt/php-7.1 --with-pdo-pgsql --with-zlib-dir --with-freetype-dir --enable-mbstring --with-libxml-dir=/usr --enable-soap --enable-calendar --with-curl --with-mcrypt --with-zlib --with-gd --with-pgsql --disable-rpath --enable-inline-optimization --with-bz2 --with-zlib --enable-sockets --enable-sysvsem --enable-sysvshm --enable-pcntl --enable-mbregex --enable-exif --enable-bcmath --with-mhash --enable-zip --with-pcre-regex --with-pdo-mysql --with-mysqli --with-mysql-sock=/var/run/mysqld/mysqld.sock --with-jpeg-dir=/usr --with-png-dir=/usr --enable-gd-native-ttf --with-openssl --with-fpm-user=www-data --with-fpm-group=www-data --with-libdir=/lib/x86_64-linux-gnu --enable-ftp --with-imap --with-imap-ssl --with-kerberos --with-gettext --with-xmlrpc --with-xsl --enable-opcache --enable-fpm --enable-intl

make && make install

cp /usr/local/src/php7-build/php-7.1.5/php.ini-production /opt/php-7.1/lib/php.ini
cp /opt/php-7.1/etc/php-fpm.conf.default /opt/php-7.1/etc/php-fpm.conf
cp /opt/php-7.1/etc/php-fpm.d/www.conf.default /opt/php-7.1/etc/php-fpm.d/www.conf

### Editar o arquivo abaixo removendo o “;” na linha onde diz pid = run/php-fpm.pid
nano /opt/php-7.1/etc/php-fpm.conf

### Editar o arquivo abaixo alterando onde diz listen = 127.0.0.1:9000 para 8998
nano /opt/php-7.1/etc/php-fpm.d/www.conf

### Criando script para inicialização 
cd /etc/init.d/
wget https://www.dropbox.com/s/95q649ip85wfa9t/php-7.1-fpm
chmod 755 php-7.1-fpm 
insserv php-7.1-fpm
/etc/init.d/php-7.1-fpm start

cd  /lib/systemd/system/
wget https://www.dropbox.com/s/jtzm3gx0jh7zm9w/php-7.1-fpm.service
chmod 755 php-7.1-fpm.service 
systemctl enable php-7.1-fpm.service
systemctl start php-7.1-fpm.service

5 - Habilitar no ISP Config PHP 7.0

Ir em System > Additional PHP Versions
Adicionar Nova Versão 
Server: Selecionar o Servidor
Client: Deixar em  branco, isso para todos os clientes terem a versão disponível
PHP Name: PHP 7.0

Ir na aba FastCGI Settings:
Path to the PHP FastCGI binary: /opt/php-7.0/bin/php-cgi
Path to the php.ini directory: /opt/php-7.0/lib

Ir na aba PHP-FPM Settings:
Path to the PHP-FPM init script: /etc/init.d/php-7.0-fpm
Path to the php.ini directory: /opt/php-7.0/lib
Path to the PHP-FPM pool directory: /opt/php-7.0/etc/php-fpm.d

6 - Habilitar no ISP Config 7.1
Ir em System > Additional PHP Versions
Adicionar Nova Versão 
Server: Selecionar o Servidor
Client: Deixar em  branco, isso para todos os clientes terem a versão disponível
PHP Name: PHP 7.1

Ir na aba FastCGI Settings:
Path to the PHP FastCGI binary: /opt/php-7.1/bin/php-cgi
Path to the php.ini directory: /opt/php-7.1/lib

Ir na aba PHP-FPM Settings:
Path to the PHP-FPM init script: /etc/init.d/php-7.1-fpm
Path to the php.ini directory: /opt/php-7.1/lib
Path to the PHP-FPM pool directory: /opt/php-7.1/etc/php-fpm.d



