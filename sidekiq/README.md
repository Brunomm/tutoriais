Configuração para o Sidekiq iniciar automaticamente com o ubuntu utilizando o Upstart.

1. **Criar arquivo de inicialização do Sidekiq**

	Criar arquivo `/etc/init/sidekiq.conf` e coloque o conteúdo [deste arquivo](https://github.com/Brunomm/tutoriais/blob/master/sidekiq/sidekiq.conf). Certifique-se de mudar o caminho do HOME e o caminho do APP_PATH.
	Verifique que é carregado o arquivo `sidekiq.yml` que se encontra na pasta `config` da aplicação. Caso não utilize o arquivo remova o seguinte pedaço do arquivo `-C $APP_PATH/config/sidekiq.yml`.

2. **Criar arquivo de inicialização de processos do Sidekiq**

	Criar arquivo `/etc/init/workers.conf` e coloque o conteúdo [deste arquivo](https://github.com/Brunomm/tutoriais/blob/master/sidekiq/workers.conf).
	Modifique a variável `NUM_WORKERS` para o numero de processos que você deseja executar. Normalmente é executado 1 processo para cada core da CPU.
	
---------------

Fonte: https://github.com/mperham/sidekiq/wiki/Deploying-to-Ubuntu
  
