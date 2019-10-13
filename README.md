# notes

Projeto referente a vaga: Analista de Infra Sênior (N3)

## Arquivos 

Os arquivos foram divididos em diretórios de acordo com o seu uso, abaixo, uma breve descrição referente aos mesmos. 

### playbook

Aqui temos os playbooks do Ansible referentes a criação da EC2, instalação do Docker e todas as demais configurações dos containers. 

### srv 

Diretório que contém os arquivos da aplicação e também o schema da base de dados: 
O mesmo é subdivido em: 

#### app 

Arquivos da aplicação em NodeJS. 

#### nginx

Arquivos de configuração do NGinx. 

#### .my.cnf

Arquivos com os dados de acesso ao container MySQL. 

#### database_schema.sql

Schema da base de dados utilizada pela aplicação. 

______

## Custos

Os custos podem ser visualizados através do arquivo "calc-02CE32A6-80AD-41C0-8DC5-738566E1B868.csv" na pasta "adicionais" ou através da URL: 
https://calculator.s3.amazonaws.com/index.html#r=IAD&key=files/calc-ab1eb37ecb253c8655bf30407c72d254250058e5&v=ver20190926fD

______

## A estrutura 

O mesmo é composto de uma EC2 com os containers e um Load Balancer que encaminha as requisições para a EC2. 
O security group é configurado para permitir apenas as portas 80 e 443 (esta em caso de uso de SSL) para o container. 
Toda a administração será realizada através de um VPN entre a rede da Mobicare com a AWS. 

______

## Como criar

A ação é simples, após alterar o playbook com a chave, basta: 
ansbile-playbook create_ec2.yml 

Aguarde a finalização da criação e atualize o arquivo /etc/ansible/hosts com o IP, usuário e a chave SSH ou senha da EC2: 
ansible-playbook mobicare.yml

______ 

## Testes

### Adição de Nota
 ~]$ curl -X POST http://172.16.0.102/notes -d 'Note=Teste1' && echo  
Ok
 ~]$ curl -X POST http://172.16.0.102/notes -d 'Note=Teste2' && echo  
Ok
 ~]$ curl -X POST http://172.16.0.102/notes -d 'Note=Teste3' && echo  
Ok
 ~]$ curl -X POST http://172.16.0.102/notes -d 'Note=Teste4' && echo  
Ok

### Listagem das Notas 
 ~]$ curl -X GET http://172.16.0.102/notes && echo  
[{"Id":4,"Text":"`Note` = 'Teste4'","CreateDate":"2019-10-13T22:12:59.000Z"},{"Id":3,"Text":"`Note` = 'Teste3'","CreateDate":"2019-10-13T22:12:57.000Z"},{"Id":2,"Text":"`Note` = 'Teste2'","CreateDate":"2019-10-13T22:12:54.000Z"},{"Id":1,"Text":"`Note` = 'Teste1'","CreateDate":"2019-10-13T22:06:46.000Z"}]

### Deleção de Nota
 ~]$ curl -X DELETE 'http://172.16.0.102/notes/4' && echo
Ok

 ~]$ curl -X GET http://172.16.0.102/notes && echo  
[{"Id":3,"Text":"`Note` = 'Teste3'","CreateDate":"2019-10-13T22:12:57.000Z"},{"Id":2,"Text":"`Note` = 'Teste2'","CreateDate":"2019-10-13T22:12:54.000Z"},{"Id":1,"Text":"`Note` = 'Teste1'","CreateDate":"2019-10-13T22:06:46.000Z"}]
