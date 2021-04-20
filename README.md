Montando um Ambiente Local com recursos da AWS

Através de alguns recursos e API’s conseguimos criar um ambiente de testes local bem semelhante à um de produção, os pré-requisitos são:
    • Docker
    • Python
    • PIP

Primeiro passo é a verificar/instalar o docker:
sudo apt-get install docker.io -y
sudo apt-get install docker-compose -y

Segundo passo é verificar/instalar Python e PIP
sudo apt-get install python3 -y
sudo apt-get install python3-pip -y

Terceiro é instalar o AWS CLI e configurar as credencias, é recomendado usar o user e pass como ‘test’:
pip3 install awscli
aws cofigure –profile default

Também tem a opção de usar o awscli-local mas não testei ainda, optei pelo o awscli convecional pois seriam os mesmos comandos usados para usar com a AWS diretamente.

O ambiente de teste da AWS vai ser baseado do projeto da LocalStack, um projeto open-source que simula os recursos da AWS, a maneira que eles recomendam a instalação é através do:
pip3 install localstack

E para começar a usar as API’s basta usar o comando:
localstack start

Com isso será baixada uma imagem com todos os recursos necessários para começar a montar sua nuvem local.

Obs: é possível que seja necessário adicionar o local de instalação do localstack ao PATH do sistema para usá-lo em qualquer nível no terminal, o comando é o:
PATH=”$PATH:caminhoDoLocalStack”

Para testar seu ambiente de nuvem local com outros serviços, nifi por exemplo, é necessário criar os containers na mesma network, a maneira que fiz foi criar separadamente uma network:
docker network create nomeDaSuaNetwork

Para verificar os ips de cada serviço pode usar o comando:
docker network inspect nomeDaSuaNetwork

Com esse docker-compose tenho recursos da aws e o apache nifi rodando na mesma rede e se comunicando. 

Com isso podemos criar recursos da aws mocados com os comandos do cli, um exemplo é criar um bucket S3 e salvar arquivos neles através do nifi:

aws –endpoint-url=http://localhost:4566 s3 mb s3://nomeDoBucket


Configurações necessárias para usar o nifi listS3 por exemplo e acessar um banco postgres:

cd lib && wget https://jdbc.postgresql.org/download/postgresql-42.2.19.jar
