#### Escopo Inicial do Projeto
------------------------------------------------------------------------------------------
Montar um Servidor para Hospedar vários sites/sistemas ;
Este sistema tem que possuir controle de usuarios/clientes ;
Os sites/sistemas hospedados tem que ter suporte a SSL, E-mail, Backup,Banco de dados;
Ser um sistema leve e de fácil administração;
Poder executar configurações adicionais definidas para cada site/sistema;
------------------------------------------------------------------------------------------

1 - Preparação do Server

### Remover do Server o Source CD
nano /etc/apt/sources.list

### Atualizar o Sistema
apt-get update && apt-get upgrade && apt-get dist-upgrade

### Ajustar os arquivos Hostname e hosts
nano /etc/hostname
# para máquina virtual
isp

# para VPS
server



nano /etc/hosts
# para máquina virtual
192.168.XX.XX isp.local isp

# para VPS
XXX.XXX.XXX.XXX server.dominio.com.br server


### Instalar o unzip e configurar a região (Locales para PT_BR UTF8)
apt-get install unzip 
dpkg-reconfigure locales

2 - Download do ISPConfig
cd /tmp/
wget --no-check-certificate https://github.com/servisys/ispconfig_setup/archive/master.zip
unzip master.zip 
cd ispconfig_setup-master/

3 - Instalação do ISPConfig
./install.sh 

Configurações solicitadas via instalação por script:
Banco: mariadb
Password: https://www.random.org/passwords/
password ispconfig

Apache (*)
HHVM (Yes)
Xcache (Yes)
PhpMyadmin (Yes)
MailServer (dovecot)
Antivirus (Yes)
Setup User Quota (Yes)
IspConfig (Standard)
JailKit (Yes)
RoundCube (*)
SSL Conf - (BR)/(RS)/(Cidade)/meusite/TI


