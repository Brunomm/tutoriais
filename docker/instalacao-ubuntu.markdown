# Instanção do docker

1. Antes de mais nada, remova possíveis versões antigas do Docker:

`sudo apt-get remove docker docker-engine docker.io`


2. Depois, atualize o banco de dados de pacotes:

`sudo apt-get update`


3. Agora, adicione ao sistema a chave GPG oficial do repositório do Docker:

`curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -`

4. Adicione o repositório do Docker às fontes do APT:

```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

5. Atualize o banco de dados de pacotes, pare ter acesso aos pacotes do Docker a partir do novo repositório adicionado:

`sudo apt-get update`

6. Por fim, instale o pacote docker-ce:

`sudo apt-get install docker-ce`

7. Caso você queira, você pode verificar se o Docker foi instalado corretamente verificando a sua versão:

`sudo docker version`

8. E para executar o Docker sem precisar de sudo, adicione o seu usuário ao grupo docker:

`sudo usermod -aG docker $(whoami)`
