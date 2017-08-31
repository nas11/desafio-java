# Desafio Java

Quer jogar Nas11? Você aguenta o tranco? Siga as instruções abaixo:

## Instruções

1. Faça o fork deste projeto para a sua conta no Github.
1. Execute o script SQL no diretorio bd/ para inserir os dados necessários à aplicação. É altamente recomendado o uso de MySQL como banco de dados deste projeto.
1. Cumpra os objetivos abaixo e teste o projeto em um servidor Wildfly, versão 10.1, utilizando o arquivo de configuração "standalone.xml".
1. Testes unitários são fortemente encorajados.
1. **Não faça tudo em um único commit!**

## Objetivo 1 - Configuração inicial do projeto

* Crie um projeto Maven no Eclipse e configure o POM para utilizar Java 8, encoding UTF-8 e empacotar o projeto em um WAR. Você pode usar qualquer nome nas tags <groupId> e <artifactId>, assim como nos pacotes java. (ex: joga.nas11.meunome)
* Crie o arquivo persistence.xml e configure o acesso aos dados utilizando Hibernate 5 e JPA 2.x. 
* Todas as tabelas do banco de dados devem ter classes anotadas com @Entity, assim como suas colunas e relacionamentos.
* Todas as consultas ao banco devem ser feitas através do EntityManager, em queries JPA.

## Objetivo 2 - Consulta com filtros

Crie um Endpoint REST que receba a seguinte requisição: (utilize o protocolo que julgar mais eficiente: GET, POST, etc)

```
http://localhost/desafio-java/clientes/listar?name=ANA
```

E retorne **em formato JSON** todas as clientes que contém "Ana" no primeiro nome. **Traga somente os dados contidos na tabela de Clientes.** 

Exemplo:

```
"clientes": [{
        "id":"1",
        "nome":"Ana Cláudia",
        "sobrenome":"Oliveira",
        "nascimento":"2000-01-25",
        "cadastro":"2015-05-22T14:56:29"
        },
        {
        "id":"2",
        "nome":"Mariana",
        "sobrenome":"Pereira",
        "nascimento":"2000-01-25",
        "cadastro":"2015-05-22T14:56:29"
        }]
```

## Objetivo 3 - Consulta a dados completos de um cliente

Crie um Endpoint REST que receba a seguinte requisição:

```
http://localhost/desafio-java/clientes/perfil/1
```

E retorne, **em formato JSON**, todas as informações sobre o cliente de ID: "1", incluindo:

* Dados de cadastro.
* Dados da loja que o cadastrou.
* Dados de todas as vendas que ele realizou.
* Dados de todos os itens dessas vendas.

## Objetivo 4 - Inserção de dados no banco relacional

Crie um Endpoint REST que receba uma solicitação **em formato JSON** contendo dados de VENDAS e ITENS_VENDIDOS no seguinte endereço:

```
http://localhost/desafio-java/vendas/cadastro
```

Exemplo do JSON de requisição:

```
"vendas": [{
        "id":"100",
        "cliente_id":"1",
        "loja_id":"2",
        "data":"2015-05-22T14:56:29"
        "valor_bruto":"20.59",
        "valor_desconto":"1.59",
        "valor_final":"19.00",
        "itens":[{
            "produto_id":"1",
            "valor_custo_und":"9.90",
            "valor_desconto_und":"1.59",
            "valor_venda_und":"20.59",
            "quantidade":"1"
            }]
        },
        {
        "id":"101",
        "cliente_id":"2",
        "loja_id":"1",
        "data":"2015-07-11T18:56:29"
        "valor_bruto":"99.99",
        "valor_desconto":"0.99",
        "valor_final":"99.00",
        "itens":[{
            "produto_id":"2",
            "valor_custo_und":"11.00",
            "valor_desconto_und":"0.33",
            "valor_venda_und":"33.00",
            "quantidade":"2"
            },
            {
            "produto_id":"4",
            "valor_custo_und":"11.00",
            "valor_desconto_und":"0.33",
            "valor_venda_und":"33.00",
            "quantidade":"1"
            }]
      }]
```

O Endpoint deverá realizar as seguintes validações, e retornar uma mensagem de erro personalizada para o usuário em cada uma delas:

* Impedir o cadastro de uma venda cujo código da venda já esteja cadastrado no banco de dados.
* Impedir o cadastro de uma venda com valor total nulo ou negativo.
* Impedir o cadastro de uma venda com um código de cliente que não existe.
* Impedir o cadastro de uma venda com um código de loja que não exista ou que tenha o status "INATIVA".
* Impedir o cadastro de uma venda sem dados dos itens da venda.
* Informar o cadastro da venda com sucesso, caso ela passe todas as validações acima.
