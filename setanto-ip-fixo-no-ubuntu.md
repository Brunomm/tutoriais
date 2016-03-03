Setando IP fixo no ubuntu 14.04
===========================
Descobrindo seus dados de rede 
-----------
	# Para descobrir o gateway
	~$ route -n
	
	# Para descobrir os demais dados
	~$ ifconfig

Setando IP
------------
	# Editar o arquivo /etc/network/interfaces
	~$ sudo nano /etc/network/interfaces
	
	auto eth0
	iface eth0 inet static
	address 192.168.0.X
	netmask 255.255.255.0
	network 192.168.0.0
	broadcast 192.168.0.255
	gateway 192.168.0.X
Após isso reiniciar o computador para garantir.

-----------------
####Fontes
[http://askubuntu.com/questions/338442/how-to-set-static-ip-address](http://askubuntu.com/questions/338442/how-to-set-static-ip-address)
[http://www.unixmen.com/how-to-find-default-gateway-in-linux/](http://www.unixmen.com/how-to-find-default-gateway-in-linux/)
