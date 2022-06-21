---
Projeto: ansible-zabbix
Descrição: O objetivo desse projeto é instalar a ferramenta de monitoramento Zabbix,
           configurar o server, com banco de dados MariaDB, com front e bankend all in one.
Autor: Glauber GF (mcnd2)
Atualização: 2022-06-20
---

![Image](https://github.com/glaubergf/ansible-zabbix/blob/main/pictures/start_zabbix.jpg)

# Instalar e Configurar o Zabbix com o Ansible

O **Zabbix** é um software de nível corporativo ideal para o monitoramento em tempo real de milhões de métricas coletadas de milhares de servidores, máquinas virtuais e dispositivos de rede, além de ser de código aberto é também gratuito.

O **Ansible** trabalha com os conceitos de inventário (_lista de máquinas que serão gerenciadas_), playbooks (_comandos ou passo-a-passo a ser executado_) e roles (_modularização do código_). Atualmente o Ansible pertence a **Red Hat**.

## Comandos

* Para checar cada role, execute o Ansible com o parâmetro "-t" seguida da tag da role e "-C" para apenas checar sem executar.

Exemplo para a role 'config_initial':
```
ansible-playbook -i hosts main.yml -t conf_in -C
```

* Executando o projeto completo:

```
ansible-playbook -i hosts main.yml
```

## A Automação

Uma breve descrição das tasks dentro de cada role:

Na role **config_initial** é executado o ajuste do timezone, alterando o idioma do sistema, alterando o layout do teclado, instalando, configurando e habilitando o ntp.

* tasks:

```
Ajustando o timezone do sistema;
Alterando o idioma do sistema;
Alterando o layout do teclado do sistema;
Removendo arquivo '/var/lib/dpkg/lock-frontend' se existir;
Executando 'apt-get update';
Instalando o ntp / ntpdate;
Parando o ntp;
Copiando o arquivo de configuração do ntp;
Ajustando a hora e habilitando o ntp;
Iniciando o ntp.
```

Na role **db-mariadb** é executado a instalação e configuração necessária para o banco de dados MariaDB.

* tasks:

```
Instalando o MariaDB Database Server;
Verificando se o MariaDB está em execução e inicia no boot;
Conectando ao servidor usando 'login_unix_socket';
Removendo contas de usuários 'anônimos' para localhost;
Removendo usuários 'zabbix' para o localhost se existir;
Removendo o MySQL 'test database';
Altera o plugin de autenticação do usuário 'root' do MariaDB para mysql_native_password;
Privilégios de liberação para usuário 'root';
Criando usuário 'zabbix' para o MariaDB;
Criando banco de dados com nome 'zabbix'.
```

Na role **mon-zabbix** é executado a adição do repositório zabbix, instalando e configurando sendo necessárias para que o zabbix carregue sem problemas.

* tasks:

```
Adicionando o repositório do Zabbix Server;
Executando o update nos repositórios;
Instalando o Zabbix com frontend e suporte ao banco de dados MariaDB/MySQL;
Importando arquivo.sql similar para mariadb/mysql -u <user> -p <password> < arquivo.sql;
Configuração do zabbix /etc/zabbix/zabbix_server.conf e inserindo os dados de conexão do banco;
Configurando timezone do Nginx para os parâmetros que o Zabbix usa;
Reiniciando o Zabbix Server, Zabbix Agente, Nginx e PHP;
Configurando o Nginx;
Mudando a porta default do Nginx para não colidir com a do Zabbix;
Alterando permissões no diretório raiz do Zabbix;
Reiniciando o Nginx.
```


## Licença

**GNU General Public License** (_Licença Pública Geral GNU_), **GNU GPL** ou simplesmente **GPL**.

[GPLv3](https://www.gnu.org/licenses/gpl-3.0.html)

------

Copyright (c) 2022 Glauber GF (mcnd2)

Este programa é um software livre: você pode redistribuí-lo e/ou modificar
sob os termos da GNU General Public License conforme publicada por
a Free Software Foundation, seja a versão 3 da Licença, ou
(à sua escolha) qualquer versão posterior.

Este programa é distribuído na esperança de ser útil,
mas SEM QUALQUER GARANTIA; sem mesmo a garantia implícita de
COMERCIALIZAÇÃO ou ADEQUAÇÃO A UM DETERMINADO FIM. Veja o
GNU General Public License para mais detalhes.

Você deve ter recebido uma cópia da Licença Pública Geral GNU
junto com este programa. Caso contrário, consulte <https://www.gnu.org/licenses/>.

*

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>