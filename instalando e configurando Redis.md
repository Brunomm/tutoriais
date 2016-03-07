Instalado e configurando REDIS
====

1. Instalando a versão mais recente do Redis

		~$ sudo add-apt-repository ppa:chris-lea/redis-server
		~$ sudo apt-get update
		~$ sudo apt-get install build-essential --quiet --yes
		~$ sudo apt-get install redis-server
	Com essa instalação pelo `apt-get` é criado um inicializador automatico do redis em `/etc/inid.d` e também o redis é automaticamente iniciado. É necessário parar o serviço e depois remover o arquivo de inicialização padrão.
		
		~$ sudo /etc/init.d/redis-server stop
		~$ sudo rm /etc/init.d/redis-server
2. Criar arquivo de inicialização
Segundo a documentação do Redis, eles indicam criar o arquivo com o nome da porta que você irá utilizar para acesso ao Redis (padrão 6379)

		~$ sudo nano /etc/init.d/redis-6379
		# Coloque script a baixo
	Arquivo baixado do site do Redis onde é o arquivo de configuração que será mencionado no tutorial como `utils/redis_init_script`:

		#!/bin/sh
		#
		# Simple Redis init.d script conceived to work on Linux systems
		# as it does use of the /proc filesystem.
		
		REDISPORT=6379
		EXEC=/usr/bin/redis-server
		CLIEXEC=/usr/bin/redis-cli
		
		PIDFILE=/var/run/redis-${REDISPORT}.pid
		CONF="/etc/redis/${REDISPORT}.conf"
		
		case "$1" in
		    start)
		        if [ -f $PIDFILE ]
		        then
		                echo "$PIDFILE exists, process is already running or crashed"
		        else
		                echo "Starting Redis server..."
		                $EXEC $CONF
		        fi
		        ;;
		    stop)
		        if [ ! -f $PIDFILE ]
		        then
		                echo "$PIDFILE does not exist, process is not running"
		        else
		                PID=$(cat $PIDFILE)
		                echo "Stopping ..."
		                $CLIEXEC -p $REDISPORT shutdown
		                while [ -x /proc/${PID} ]
		                do
		                    echo "Waiting for Redis to shutdown ..."
		                    sleep 1
		                done
		                echo "Redis stopped"
		        fi
		        ;;
		    restart|force-reload)
		        ${0} stop
		        ${0} start
		        ;;
		    *)
		        echo "Please use start|stop|restart as first argument"
		        ;;
		esac
	É necessário dar permissão de execução para o arquivo
		
		~$ sudo chmod 753 /etc/init.d/redis-6379
3. Copie o arquivo de configuração do modelo que você vai encontrar no diretório raiz da distribuição Redis em  `/etc/redis/` usando o número da porta como o nome, por exemplo:

		~$ sudo cp /etc/redis/redis.conf /etc/redis/6379.conf
4. Crie um diretório dentro de `/var/redis` que funcionarão como os dados e diretório de trabalho para esta instância Redis:

		~$ sudo mkdir /var/redis/6379
5. Editar arquivo de configuração do redis: `/etc/redis/6379.conf`
	- Setar o `daemonize`  para  `yes` (por padrão é setado como `no`).

			daemonize yes
	- Setar o pidfile para `/var/run/redis-6379.pid` (mudar a porta se necessário).
		
			pidfile /var/run/redis-6379.pid
	- Alterar a porta conforme sua configuração. No nosso exemplo, não é necessário, pois a porta padrão já é 6379.
		
			port 6379

	- Definir local para gravar os logs:
		
			logfile /var/log/redis-6379.log
	- Definir o diretório do redis para ` /var/redis/6379` (**Isso émuito importante para que tudo que fizemos funcione**)
			
			dir /var/redis/6379
	- Fazer o redis salvar o banco em disco a cada segundo para segurança dos dados.
			
			appendonly yes
			appendfilename appendonly.aof
			appendfsync everysec
6. Por fim, adicione o script de inicialização do redis, paraque seja iniciado automaticamente quando o servidor ligar.
			
		~$ sudo update-rc.d redis-6379 defaults
7. Você terminou! Agora você pode tentar executar o seu exemplo, com:
		
		~$ sudo /etc/init.d/redis-6379 start

> **IMPORTANTE**
> Caso você queira modificar algum diretório do que foi descrito neste manual, lembre-se que é necessário mudar também no script de inicialização descrito no início deste manual. Veja a baixo o que é necessário modificar caso não seja seguido o padrão deste manual:

    REDISPORT=6379
    EXEC=/usr/local/bin/redis-server
    CLIEXEC=/usr/local/bin/redis-cli
    PIDFILE=/var/run/redis_${REDISPORT}.pid
    CONF="/etc/redis/${REDISPORT}.conf"
	
