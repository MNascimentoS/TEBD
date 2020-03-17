# TEBD

## Nivelamento 1 e 2

No primeiro nivelamento foi realizada uma atividade para entender e aplicar os conceitos de mapeamento Objeto-Relacional utilizando o framework Hibernate. Os procedimentos realizados nesse nivelamento foram:
1. Download do Hibernate
2. Adição biblioteca do Hibernate no projeto base oferecido pelo professor no Eclipse
3. Criação da arquivo de configuração do Hibernate, o Persistence.xml
4. Adaptação da classe Aluno e Nota para realizar o mapeamento

## Nivelamento 3

O projeto de Nivelamento III é uma atividade requisitada no currículo da matéria de Tópicos Especiais em Bancos de Dados do Curso de Sistema de Informação na Universidade do Estado da Bahia.

Esta atividade tem como objetivo nivelar os conhecimentos entre os membros do grupo de trabalho, a fim de facilitar o desenvolvimento da atividade final.

O projeto é criar um aplicativo android que simule superficialmente um sistema de faculdade. É necessário criar um método de cadastro de disciplinas, armazenamento em banco local, listagem, edição e remoção.

Tecnologias utilizadas:
- Java
- SqLite

## Nivelamento 4

Uma das grandes vantagens do MongoDB (Banco NoSQL) é estar preparado para escalonar horizontalmente através do conceito de réplicas e particionamento de dados. Este nivelamento consistiu em criar um cluster MongoDB com vários nós e foi baseado no tutorial Criando um cluster MongoDB com ReplicaSet e Sharding com Docker de Gustavo Leitão (link nas referências).
Após a instalação do Docker e do MongoDB, foi criada uma rede de comunicação para os containers executando o comando a seguir:
$ docker network create mongo-shard
Após a criação da rede de comunicação foi necessário criar os containers. Foram criados três nós de configuração (ConfigServer), três shards cada um como uma réplica (totalizando seis nós) e um roteador. A figura abaixo mostra a arquitetura:

![Alt text](docs/img1.jpg?raw=true "Imagem 1")

Após a configuração, rodamos o projeto java do quarto nivelamento que realiza a inserção de dados na tabela VENDAS. Para realizar o teste, paramos um dos shards utilizando o Docker Desktop e rodamos novamente o projeto inserindo mais uma vez, verificando a funcionalidade da proposta do nivelamento.

## Desafio Final

O desafio da disciplina foi dividido em cinco etapas, de forma que em cada etapa um tópico de banco de dados fosse estudado e aplicado, como por exemplo: OLTP, ETL, OLAP. A seguir serão descritos os procedimentos realizados em cada etapa.

### Etapa 1

O objetivo dessa etapa é carregar um banco relacional com 1GB de dados e criar um programa para fazer uma inserção utilizando controle de transação e mapeamento de dados. Para isso foi realizado os seguintes procedimentos:

1. Script NodeJS
   Foi desenvolvido uma aplicação em NodeJS que gera o script com os dados que serão inseridos no banco. O script foi executado com sucesso MySql, populando o banco.
2. Camada Orientada a Objeto
   Foi desenvolvido um programa em Java utilizando o EntityManager para fazer o controle de transações e framework Hibernate para realizar o mapeamento objeto-Relacional.
   O programa permite realizar a inserção de Autores no banco criado anteriormente e o código do controle da transação pode ser encontrado no arquivo AutorBD.java, onde foi utilizada técnicas como de commit e rollback .
   Para realizar o mapeamento com o Hibernate, foi criado o arquivo de configuração xml com os atributos necessários para o funcionamento do projeto. A classe Autor.java na pasta Model foi adaptada adicionando os elementos necessários para o mapeamento.

### Etapa 2

Essa etapa consiste em criar uma solução ETL para efetuar a extração do banco de dados relacional e realizar a carga nos componentes das etapas 4 e 5.
O projeto foi feito em NodeJS e tem dois módulo:

- Android
- MongoDB

Para o primeiro módulo foi necessário consultar todos os dados do banco relacional realizado na Etapa 1 e gerar uma api com esses dados em formato JSON para que o aplicativo Android possa acessá-lo e realizar os devidos procedimentos da etapa 4.

Já para o segundo módulo, o MongoDB, foi necessário primeiramente modelar o banco de dados. Após a modelagem realizada na etapa 5, o programa Node consulta os dados do banco relacional e realiza o tratamento deles para que possam ser inseridos no banco MongoDB. A carga neste banco é realizada a cada 10000 registros, criando um log com os dados que foram inseridos. Resumindo, os procedimentos realizados nessa etapa foram:

1. Conexão com o banco relacional para consultar os dados
2. Criação das queries de consulta
3. Geração do json para a api que o aplicativo Android vai consultar
4. Transformação dos dados para NoSQL
5. Carga de dados no MongoDB em lotes de 10000 registros

### Etapa 3

Para essa etapa é necessário ter todos os dados que serão trabalhos, já armazenados no banco de dados para assim podermos aplicar a ferramenta de BI em cima deles e termos os gráficos que auxiliam nas tomadas de decisões.
Com os dados no banco de dados, é necessário que primeiramente configuremos o Knowage para que o mesmo tenha acesso a base.
Para a configuração é necessário acessar a opção Data Source e informar os seguintes dados: Label, Description, Dialect, Permission, Type, URL, User, Password e Driver.

- **Label** – é o nome que será definido para a conexão e será utilizado no DataSet;
- **Description** – como o nome já diz é a descrição da conexão;
- **Dialect** – é a informação que qual banco de dados está sendo utilizado. No caso deste trabalho, o banco utilizado será o Mysql, então a opção escolhida é MySQL/MariaDB;
- **Permission** – é referente a permissão que terá ao acessar o banco de dado, sendo ela apenas leitura ou leitura e escrita;
- **Type** – é o tipo de conexão, neste caso é utilizado a conexão JDBC;
- **URL** – é referente ao caminho para o banco de dados;
- **User** – é o usuário do banco de dados;
- **Password** – é a senha do banco de dados;
- **Driver** – é o drive utilizado para a conexão que no nosso caso como é utilizado o mysql ele será o com.mysql.jdbc.Driver.

A configuração ficará da seguinte forma:

![Alt text](docs/img2.jpg?raw=true "Imagem 2")

Após essa configuração é necessário definir os DataSets. Eles nada mais são do que as querys que deverão retornar os dados para serem trabalhos. No nosso foi definido três consultas, pois não foi possível pegar tudo os dados necessários é apenas uma.
Ao acessar a opção de DataSet, será dado a opção de criar uma, clicando no “+” que irá aparecer no topo. Ao clicar será aberto lateralmente a página de detalhes da consulta. Nela é necessário informar o Label da consulta, que é o campo nome que será exibido em todo o KnowAge que identifica esse DataSet, Name que é nome do DataSet, Description que é a descrição do mesmo, Scope, Category que é a categoria do Dataset e normalmente encontrada só uma opção.
Os campos deverão ficar preenchidos mais ou menos dessa maneira:

![Alt text](docs/img3.jpg?raw=true "Imagem 3")

Após essa primeira configuração é necessário passar para a aba TYPE. Nela temos três campos, onde o primeiro é o DataSet Type que é o referente ao tipo de consulta, que em nosso caso será definido a consulta do tipo query, que é referente a escrever a consulta SQL. O segundo tempo é o Data Source que é o campo que deverá ser escolhido o banco de dados que deverá ser conectado. Neste caso a opção é o de nome escolhido para o Data Source que criamos no passo anterior. E por fim temos o campo query que aparece quando escolhemos o tipo de DataSet, neste caso iremos informar a seguinte query:

![Alt text](docs/img4.jpg?raw=true "Imagem 4")

Após definido é necessário testar para ver se os dados estão sendo retornado. Para isso temos a opção PREVIEW que mostra os dados sendo retornado caso a consulta esteja correta, como podemos ver na imagem a seguir:

![Alt text](docs/img5.jpg?raw=true "Imagem 5")

Porém ainda tem um passo importante que é definir o que cada campo representa, para isso existe uma opção que tem uma varinha mágica que ao clicar retornar os campos, dizendo que cada um deles representam, pendendo ser um campo mensurável ou atributo. No nosso caso os campos deverão seguir a seguinte imagem:

![Alt text](docs/img6.jpg?raw=true "Imagem 6")

Após isso é necessário criar os demais DataSets. Eles deverão seguir as mesmas configurações, porém mudando apenas a consulta e os dados que serão mensuráveis ou atributos. Seguindo a seguinte ordem:

Consulta 2:

![Alt text](docs/img7.jpg?raw=true "Imagem 7")

![Alt text](docs/img8.jpg?raw=true "Imagem 8")

Consulta 3:

![Alt text](docs/img9.jpg?raw=true "Imagem 9")

![Alt text](docs/img10.jpg?raw=true "Imagem 10")

Ao fim dessa configuração, estará tudo pronto para criarmos os gráficos, para isso devemos ir a opção WorkSpace e clicar na opção Analysis. Ao chegar na página aberta deve clicar na para adicionar um novo Cockpit. Então a página será aberta e vemos informar quais Datasets devemos utilizar nessa página. No botão de Menu devemos clicar em Data Configuration e adicionar os três DataSets que criamos e ficará da seguinte forma:

![Alt text](docs/img11.jpg?raw=true "Imagem 11")

Ao fim devemos ainda criar associação entre os três, para isso devemos clicar na opção ASSOCIATIONS e teremos a seguinte visão:

![Alt text](docs/img12.jpg?raw=true "Imagem 12")

Então devemos clicar nos campos de artigo_id de TEBD1 e clicar no mesmo campo de TEBD2, na parte de baixo da página irá aparecer a relação, então é só clicar no botão em formato de disquete para salvar e repetir o processo para TEBD2 -> TEBD3 e TEBD1 -> TEBD3 e salvar.
Ao fim desse processo iremos retornar a página do CockPit e devemos clicar no menu novamente e clicar no “+” e abrirá o seguinte modal:

![Alt text](docs/img13.jpg?raw=true "Imagem 13")

Nesse momento iremos criar os gráficos, para isso devemos clicar em CHART. E definir qual DataSet iremos utilizar, para exemplo do primeiro gráfico iremos utilizar o TEBD1, o selecionamos e clicar no texto CHART ENGINE DESIGNER. Ao clicar nele devemos escolher qual tipo de gráfico queremos, no primeiro vamos utilizar o gráfico de pizza. Clicamos nele e no menu iremos clicar em STRUCTURE. Nessa parte vamos clicar para mensurável em cartao_id e para atributo em marca_cartao. Porém devemos fazer uma configuração e isso deverá ser feito em todos os demais gráficos que é definir o tipo agregação deverá ser feito, então na parte de Series(Y) que está o dado cartao_id, devemos clicar para edita-lo e iremos configurar seguinte forma:

![Alt text](docs/img14.jpg?raw=true "Imagem 14")

Feito isso é só clicar em SAVE e o primeiro gráfico de pizza irá aparecer, como podemos ver na imagem a seguir:

![Alt text](docs/img15.jpg?raw=true "Imagem 15")

Esse gráfico mostra quando autores do sistema utilizam a determinada marca para pagar a inscrição. Seguindo esse processo para os demais dados, ao fim terá algo parecido com isso:

![Alt text](docs/img16.jpg?raw=true "Imagem 16")

Esses dados tempos as datas de vencimento do pagamento, cartões utilizados, quantos voluntários, quantidade de artigos avaliados por quantas pessoas e as notas obtidas.

### Etapa 4

Nesta etapa foi criada toda a parte da interface do usuário. Foi criado um aplicativo na plataforma Android que, consumindo a API de dados, lista todos os artigos salvos e autores, além de salvar no banco local do aparelho.

1. Recuperação dos dados
 Para essa etapa foi utilizada a biblioteca Retrofit para realizar as calls na API, apontando para o link dela e os caminhos dos dados.
2. Modelagem do banco local
 A modelagem do banco local foi feita utilizando o Banco SQLite, onde a partir de todas as informações recuperadas da API ele salva no aparelho.


### Etapa 5

Neste componente foi criado um banco de dados seguindo os conceitos de BigData e uma camada de interface para consulta.

1. Modelagem
 A princípio foi realizada a modelagem do banco de forma que tem-se apenas dois Schemas: registrations e submitions.
 O Schema Registration contém todas as inscrições sejam de autores voluntários ou não. Já o Schema Submitions contém os artigos, possuindo dados como: resumo, array dos ids dos autores, array de avaliações (cada avaliação tem o id do revisor, a nota e o comentário realizado).
2. 
 O MongoAtlas gera dois replicaSets, pra caso o principal venha a ter algum problema, esse outros nós sejam utilizados.
3. Programa
 Foi desenvolvido um programa em Java para fazer a consulta ao banco e retornar todos os documentos do Schema de Registrations (ver em /ETAPA 5/TEBD_ETAPA5).
 
 ## Referências
 
 [https://docs.docker.com/install/](https://docs.docker.com/install/)
 [https://medium.com/@gustavo.leitao/criando-um-cluster-mongodb-com-replicaset-e-sharding-com-docker-9cb19d456b56](https://medium.com/@gustavo.leitao/criando-um-cluster-mongodb-com-replicaset-e-sharding-com-docker-9cb19d456b56)
 [https://docs.mongodb.com/manual/mongo/](https://docs.mongodb.com/manual/mongo/)

 
