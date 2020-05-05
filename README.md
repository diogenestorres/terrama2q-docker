# TerraMA2Q Docker

Instruções de instalação e configuração do projeto TerraMA2Q com Docker.

## Requisitos

- Docker
- Docker-compose
- Git

## Instalação e configuração

Obtenha o repositório:
```bash
svn checkout https://svn.cptec.inpe.br/qmd-terrama2q/qmd-terrama2q-docker
```

Defina o arquivo `.env`:
```bash
# Para uso local
cp .env.example-local .env

# Para uso externo
cp .env.example-external .env
```

As configurações padrões são carregadas a partir do arquivo `.env.default`. Para sobreescrever tais configurações, defina as variáveis no arquivo `.env`, por exemplo:
```bash
# Para definir uma nova senha para o banco de dados.
POSTGRES_PASSWORD="novasenha"
```

Com isso, a nova senha do banco será, `novasenha`.

### Configuração local

Caso a inteção seja apenas uso na máquina local, é necessários a configuração de hostnames.
No Windows, edite o arquivo `C:\Windows\system32\drivers\etc\hosts`, no linux , o arquivo `/etc/hosts`:
```bash
127.0.0.1       terrama2_geoserver
127.0.0.1       terrama2_webapp_1
127.0.0.1       terrama2_webmonitor_1
```

### Configuração externa

Para acesso via rede, configure um endereço IP ou nome de domínio válido no arquivo `.env`:
```bash
# EXEMPLO
# Por exemplo, caso seu endereço IP seja 192.168.15.13
PUBLIC_HOSTNAME=192.168.15.13
```

## Iniciando as instâncias

Para subir o projeto, execute:
```bash
./up-terrama2q.sh
```

Assim, todas as configurações serão executadas.

Para remover os _containers_:
```bash
# Esse comando NÃO APAGA os dados
./wipe-terrama2q.sh
```
O script `./wipe-terrama2q.sh` não remove os dados que já foram inseridos no banco de dados, servidor de mapas ou salvos pelo TerraMA². Para apagá-los, deve-se remover os volumes criados, o comando `docker volume ls` permite que você saiba quais são esses volumes.

Para resetar os _containers_:
```bash
# Esse comando NÃO APAGA os dados
./reset-terrama2q.sh
```

## Variáveis

Essas variáveis são definições padrões por parte destes scripts e são passíveis de alterações. Para altera-los, edite o arquivo `.env`.

### Configurações compartilhadas

| Variável | Descrição | Default |
| --- | --- | --- |
| SHARED_VOLUME | Nome do volume para compartilhamento de dados entre as instãncias do TerraMA² (Serviços) | terrama2_shared_vol |
| SHARED_NETWORK | Nome da rede docker criada para comunição dos containers utilizados | terrama2_net |

### Configurações do TerraMA²

| Variável | Descrição | Default |
| --- | --- | --- |
| TERRAMA2_REPO_URL | URL para projeto terrama2/docker com a configuração das imagens Terrama² | https://github.com/terrama2/docker.git|
| TERRAMA2_DOCKER_DIR | Caminho do diretório do projeto terrama2/docker | terrama2-docker |
| TERRAMA2_CONF_DIR | Caminho do diretório com os arquivos de configuração necessários para levantar os containers TerraMA² | terrama2-conf|
| TERRAMA2_VOLUME | Nome do volume de dados das instâncias TerraMA² | terrama2_data_vol|

### Configurações do WebApp

| Variável | Descrição | Default |
| --- | --- | --- |
| WEBAPP_PATH | Caminho da aplicação WebApp (Admin) | /admin |
| WEBAPP_PORT_EXTERNAL | Porta utilizada para exposição externa da aplicação WebApp<br>__Obs.:__ Em caso de execução local, não alterar o valor padrão. | 36000 |

### Configurações do WebMonitor

| Variável | Descrição | Default |
| --- | --- | --- |
| WEBMONITOR_PATH | Caminho da aplicação WebMonitor (Admin) | /monitor |

### Configurações do Geoserver

| Variável | Descrição | Default |
| --- | --- | --- |
| GEOSERVER_BY_DOCKER | Flag que define se será utilizado um container ou não | true |
| GEOSERVER_IMAGE | Imagem docker utilizada | terrama2/geoserver:2.11 |
| GEOSERVER_VOLUME | Nome no volume docker | terrama2_geoserver_vol |
| GEOSERVER_CONTAINER | Nome do container docker | terrama2_geoserver |
| GEOSERVER_PROTOCOL | Protocólo utilizado na requisição Geoserver | http |
| GEOSERVER_HOST | Hostname geoserver | terrama2_geoserver |
| GEOSERVER_PORT | Porta Geoserver | 8080 |
| GEOSERVER_URL | Caminho da aplicação Geoserver | /geoserver |
| GEOSERVER_CONF_DIR | Caminho para o diretório de com arquivos de configuração Docker | geoserver-conf |
| GEOSERVER_FILE_SETENV | Caminho para o arquivo que define o ambiente da instância docker | ${GEOSERVER_CONF_DIR}/terrama2_geoserver_setenv.sh |
| GEOSERVER_CSRF_DISABLED | Variável de ambiente que desabilita CSRF. https://docs.geoserver.org/stable/en/user/security/webadmin/csrf.html | false |

### Configurações do BDQueimadas Light

| Variável | Descrição | Default |
| --- | --- | --- |
| BDQLIGHT_REPO_URL | URL para repositório Git do BDQueimadas-Light | https://github.com/jonatasleon/bdqueimadas-light.git |
| BDQLIGHT_IMAGE | Nome da imagem utilizada para criar o container | jonatasleon/bdqlight:1.0.1 |
| BDQLIGHT_VOLUME | Nome do volume disponível para dados do BDQueimadas-Light | terrama2_bdq_vol |
| BDQLIGHT_CONTAINER | Nome do container | terrama2_bdq |
| BDQLIGHT_DOCKER_DIR | Diretório para build da imagem docker do BDQueimadas-Light | bdqlight |
| BDQLIGHT_CONF_DIR | Caminho do diretório com arquivos de configuração do BDQueimadas-Light | bdqlight-conf |

### Configurações do Nginx

| Variável | Descrição | Default |
| --- | --- | --- |
| NGINX_IMAGE | Nome da imagem do Nginx | nginx:latest |
| NGINX_CONTAINER | Nome do container | terrama2_nginx |
| NGINX_CONF_DIR | Caminho para o arquivo de configuração do Nginx | nginx-conf |
| NGINX_UP | Flag que define se o Nginx é levantado ou não | true |
| NGINX_PORT | Porta utilizada pelo Nginx | 80 |

### Configurações do Postgres/Postgis

| Variável | Descrição | Default |
| --- | --- | --- |
| POSTGRES_IMAGE | Nome da imagem Docker | mdillon/postgis |
| POSTGRES_VOLUME | Nome do volume de dados Docker | terrama2_pg_vol |
| POSTGRES_CONTAINER | Nome do container | terrama2_pg |
| POSTGRES_HOST | Hostname do banco de dados | terrama2_pg |
| POSTGRES_PORT | Porta de conexão do banco de dados | 5432 |
| POSTGRES_USER | Nome do usuário | postgres |
| POSTGRES_PASSWORD | Senha do banco de dados | mysecretpassword |
| POSTGRES_DB | Nome do banco de dados do TerraMA² | terrama2 |

### Flags extras

| Variável | Descrição | Default |
| --- | --- | --- |
| FORCE_LOCAL_SERVICE_CONFIG | Flag que força configuração de serviço local | true |
| FORCE_RESTART_AFTER_CONFIG | Reinicia container após script de configuração | false |
