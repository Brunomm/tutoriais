Instalando NGIX com Passenger
========

###Comandos
1. Instalar dependências	

		~$ sudo apt-get install libcurl4-openssl-dev
2. Instalar gem passsenger

		~$ gem install passenger
3. Trapaça: digo que o dono da pasta /opt é o meu usuário de deploy

		~$ sudo chown -R `nome_do_usuario` /opt
4. Instalo o NGINX pelo Passenger

		
		~$ passenger-install-nginx-module --auto-download --auto
5. Configurando o NGINX
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

6. Desfazendo a trapaça
Na instalação eu trapeceei um pouco: como instalei o RVM em single-mode não dá para
instalar o nginx usando o script do Passenger via sudo. Por isso mudei o dono do diretório
/opt. Precisamos mudar de volta:

	    ~$ sudo chown -R root /opt

7. Fazer NGINX iniciar automaticamente
Agora queremos que o NGINX inicie automaticamente sempre que o servidor reiniciar, para
isso podemos usar da ajuda do script de inicialização feito pelo Linode:
		
		~$ wget -O init-deb.sh https://www.linode.com/docs/assets/660-init-deb.sh
		
		~$ sudo mv init-deb.sh /etc/init.d/nginx
		
		~$ sudo chmod +x /etc/init.d/nginx

		~$ sudo /usr/sbin/update-rc.d -f nginx defaults
Caso o link da Linode não esteja mais disponível, pode ser baixado [aqui.](https://raw.githubusercontent.com/Brunomm/tutoriais/master/NGINX-com-passenger/660-init-deb.sh)
8. Inicie ou pare o NGINX como qualquer outro serviço no Ubuntu:

	    ~$ sudo service nginx start
	    ~$ sudo service nginx stop
	    ~$ sudo service nginx restart


####Pronto, servidor configurado
