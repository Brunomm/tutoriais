## Dependências###
 1. image-magic
	
		~$ sudo apt-get install imagemagick --fix-missing
 2. redis

		~$ sudo apt-get install redis-server
 3. Libs

		~$ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libxml2-dev autoconf libc6-dev ncurses-dev automake libtool libgmp-dev libcurl4-openssl-dev build-essential openssl libssl-dev zlib1g libpq-dev --quiet --yes

 4. Instalar gems manualmente

	    ~$ gem install bundle
	    ~$ gem install cairo -v '1.14.1'
	    ~$ gem install RedCloth -v '4.2.9'

##Instalando##

2. **Instalando RVM e Ruby**
			
		~$ echo 'rvm_path="$HOME/.rvm"' >> ~/.rvmrc
		~$ \curl -sSL get.rvm.io | bash -s stable 
		# Com o comando a cima, vai pedir para adicionar uma chave com uma msg +/- assim: "gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3"
		~$ gpg --keyserver hkp://keys.gnupg.net --recv-keys CHAVE-EXIBIDA-NO-TERMINAL

		#Agora vamos instalar o RVM
		~$  \curl -sSL get.rvm.io | bash -s stable
		# Agora vamos carregar o RVM e fazer com que sempre seja carregado ao fazer o login
		~$ export PATH="$PATH:$HOME/.rvm/bin"
		~$ source ~/.rvm/scripts/rvm
	
		# Agora vamos instalar o ruby
		~$ rvm install 2.3.0
		~$ rvm use 2.3.0 --default # Definir a versão 2.3.0 como padrão
3. **Instalando o bundle**

		~$ gem install bundle

4. **Instalando o postgreSQL**
	
		~$ sudo apt-get install postgresql postgresql-contrib libpq-dev
		~$ sudo -u postgres psql
		~$ \password #ctrl D para sair


		
