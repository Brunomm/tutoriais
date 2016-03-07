### NO SERVIDOR
1. Instalando git e pacotes necessários
		
		~$ sudo apt-get update
		~$ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev autoconf libc6-dev ncurses-dev automake libtool libgmp-dev libcurl4-openssl-dev libpq-dev --quiet --yes
		~$ sudo apt-get install imagemagick --fix-missing  --quiet --yes
2. Instalando RVM Ruby
		
		~$ echo 'rvm_path="$HOME/.rvm"' >> ~/.rvmrc
		~$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
		~$ \curl -sSL get.rvm.io | bash -s stable 
		# Se ocorrer algum erro, execute: `echo ipv4 >> ~/.curlrc` no terminal e tente baixar novamente o rvm
		~$ export PATH="$PATH:$HOME/.rvm/bin"
		~$ source ~/.rvm/scripts/rvm
		~$ rvm install 2.3.0
		~$ rvm use 2.3.0 --default
		~$ gem install bundle
	Caso utilize as gems RedCloth e/ou cairo, instale-as manualmente para evitar erros:
	
		~$ gem install RedCloth -v 4.2.9
		~$ gem install cairo -v 1.14.1
3. Configurando permissões do usuário
		
		~$ sudo groupadd deployers
		~$ sudo usermod -a -G deployers NOME-DO-USUARIO
4.Criando o diretório da app e adicionado permissoes

		~$ mkdir  /caminho/da/aplicacao
		~$ sudo chown -R duobr:deployers /caminho/da/aplicacao
		~$ sudo chmod -R g+w  /caminho/da/aplicacao
		~$ ssh-keygen -t rsa -C "email@email.com.br"
		# Copiar a chave gerada para o github/gitlab
		~$ cat ~/.ssh/id_rsa.pub 
5. Instalar gem passsenger

		~$ gem install passenger
6. Trapaça: digo que o dono da pasta /opt é o meu usuário de deploy

		~$ sudo chown -R NOME-DO-USUÁRIO /opt
7. Instalo o NGINX pelo Passenger
		
		~$ passenger-install-nginx-module --auto-download --auto
8. Configurando o NGINX
Quando ele terminar de instalar, você lembre-se que no arquivo `/opt/nginx/conf/nginx.conf`
haverá o seguinte:

		http {
			...
			passenger_root /home/nome_do_usuario/.rvm/gems/ruby-2.0.0-p245/gems/passenger-
			4.0.17;
			passenger_ruby /home/nome_do_usuario/.rvm/wrappers/ruby-2.0.0-p245/ruby;
			...
		}
Sempre que atualizar o passenger, atualize este trecho com a versão mais recente. Além disso,
para configurar novas aplicações Rails, no mesmo arquivo configure da seguinte forma:

		server {
			listen 80;
			erver_name localhost;
			root /somewhere/public; # <--- Caminho do public da aplicação!
			passenger_enabled on;
		}
Onde /somewhere é onde está o código da aplicação Rails, sempre completando com /public
para o Passenger saber o que fazer.

9. Desfazendo a trapaça
Na instalação eu trapeceei um pouco: como instalei o RVM em single-mode não dá para
instalar o nginx usando o script do Passenger via sudo. Por isso mudei o dono do diretório
/opt. Precisamos mudar de volta:

		~$ sudo chown -R root /opt

10. Fazer NGINX iniciar automaticamente
Agora queremos que o NGINX inicie automaticamente sempre que o servidor reiniciar, para
isso podemos usar da ajuda do script de inicialização feito pelo Linode:
		
		~$ wget -O init-deb.sh https://www.linode.com/docs/assets/660-init-deb.sh
		
		~$ sudo mv init-deb.sh /etc/init.d/nginx
		
		~$ sudo chmod +x /etc/init.d/nginx

		~$ sudo /usr/sbin/update-rc.d -f nginx defaults
Caso o link da Linode não esteja mais disponível, pode ser baixado [aqui.](https://raw.githubusercontent.com/Brunomm/tutoriais/master/NGINX-com-passenger/660-init-deb.sh)
11. Inicie ou pare o NGINX como qualquer outro serviço no Ubuntu:

		~$ sudo service nginx start
		~$ sudo service nginx stop
		~$ sudo service nginx restart


## EM MEU COMPUTADOR 
 Para poder acessar a aplicação em um servidor local ou em uma vm
 Fazer o seguinte:

1. Instalar o dnsmasq para poder acessar a app por um endereço fisico

		#como por exemplo: vm-myapp.com
		~$ sudo apt-get install dnsmasq

2. Editar o arquivo de configuração do dnsmasq para redirecionar o meu endereço para o IP que desejar

		~$ sudo nano /etc/dnsmasq.conf

3. Adicionar no fim do arquivo: 

		address=/vm-myapp.com/IP.DA.MINHA.APP
		~$ sudo /etc/init.d/dnsmasq restart



