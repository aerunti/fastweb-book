# php-5.6 com Docker Ubuntu 22.04


às veses é necessário para dar suporte em sistemas legados configurar versões antigas do php com mysql 

## instale o docker e o package 

    docker pull javanile/lamp:5.6

Depois inicie o container mapeando portas, rede e pasta do seu projeto. configure a rede primeiro

    docker network create -d bridge my-net

Depois inicie o container, ajustando para suas configurações

    docker run --network=world -itd -d --mount type=bind,source=/<yourprojectpath>,target=/var/www/<yourproject> -p 127.0.0.1:9080:80 -p 127.0.0.1:3307:3306 --name=<yourproject> javanile/lamp:5.6

Caso seja necessário, configure o apache ou nginx do host para a porta 9080 ou outra que você escolher para o seu projeto para um subdominio e crie um certificado ssl com certbot. 

