# SeniorSmart

*O nosso projeto é focado em pessoas a partir de 50 anos que possuem dificuldade em utilizar computadores, smartphones e outros eletroeletrônicos. Ele consiste em um aplicativo que utiliza um chatbot com inteligência artificial, capaz de responder perguntas personalizadas para cada problema ou dúvida do usuário.*

<hr>

## Protótipo funcional da aplicação

[Vídeo do protótipo](https://www.youtube.com/watch?v=-tCpLz5HO1Q)

<br>

*Para rodar adicionar seu endereço de ip no properties da Api e colocar username e password da oracle ou outras configurações se necessário (O ip no properties deve ser usado para ligar com o react native). Faça dowload da dependências e faça um reload no mavem. Possivel vizualizar o swagger quando estiver rodando.*

```
# Configurações do banco de dados Oracle
spring.datasource.url=jdbc:oracle:thin:@oracle.fiap.com.br:1521:ORCL
spring.datasource.username=username
spring.datasource.password=password
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.Oracle12cDialect
spring.jpa.hibernate.ddl-auto=update

spring.profiles.active=dev //Caso utilize o react native nessa solução, comente essa linha. Esse código tira a necessidade do token.
// O programa no native foi criado com a utilização do token. Não comentar antes de rodar daria conflito.

server.error.include-stacktrace=never

server.address=seu ip //Caso utilize o react native coloque o endereço de ip da sua maquina tanto aqui quando na parte do react native.
// Caso utilize o next.js, pode comentar essa parte pois o mesmo roda no localhost.
```

<br>

<h2 style="font-style: italic; color: #F5FFFA" >Ideia da aplicação</h2>

<img src="image/Seniorsmart_arquitetura_solucao.png" width="auto">

<br>

Legenda da solução:

1. API do chatGPT que irá receber as perguntas do usuário e responde-las.
2. O aplicativo móvel feito em React Native que faz a ligação com a API do ChatGPT configurando o chatbot para responder as perguntas que o usuário fizer.
3. O aplicativo web feito em React, para que o usuário possa utilizar o chatbot também pelo site que ficará na nuvem em "Serviços de Aplicativo" na Azure.
4. A API da SeniorSmart que vai persistir os dados no Banco de Dados Oracle, que virá tanto do aplicativo móvel quanto do aplicativo web com seu CRUD e endpoints.
5. Banco de Dados Oracle que irá armazenar, gerenciar e recuperar informações de forma eficiente e segura.
6. Serviços de Aplicativo, irá armazenar tanto a API da SeniorSmart, quanto o aplicativo web na nuvem da Azure.

<br><hr>

## Testes

Você pode testar o front com o Back, basta usar o código de Hybrid e fazer as devidas mudanças que quiser
- Link do Front: https://github.com/biancabt1102/SeniorSmart-Hybrid

<hr>

## Endpoints

- Usuário:
    - [Cadastrar](#cadastrar-usuário)
    - [Login](#login-usuário)
    - [Listar todos](#listar-usuários)
    - [Alterar](#alterar-usuário)
    - [Detalhes](#detalhes-usuário)
    - [Excluir](#excluir-usuário)

- Plano:
    - [Cadastrar](#cadastrar-plano)
    - [Listar todos](#listar-planos)
    - [Alterar](#alterar-plano)
    - [Detalhes](#detalhes-plano)
    - [Excluir](#excluir-plano)

- Pagamento:
    - [Cadastrar](#cadastrar-pagamento)
    - [Listar todos](#listar-pagamentos)
    - [Alterar](#alterar-pagamento)
    - [Detalhes](#detalhes-pagamento)
    - [Excluir](#excluir-pagamento)

- Pergunta:
    - [Cadastrar](#cadastrar-pergunta)
    - [Listar todos](#listar-perguntas)
    - [Detalhes](#detalhes-pergunta)

- Resposta:
    - [Cadastrar](#cadastrar-resposta)
    - [Listar todos](#listar-respostas)
    - [Detalhes](#detalhes-resposta)

<hr>

### Cadastrar Usuário

`POST` /api/usuarios/cadastro

*Campos da requisição*

| Campo                | Tipo   | Obrigatório | Descrição                                                          |
|----------------------|--------|:-----------:|--------------------------------------------------------------------|
|nomeUsuario           |String  |sim          | Nome completo do usuário.                                          |
|emailUsuario          |String  |sim          | Endereço de e-mail do usuário.                                     |
|senhaUsuario          |String  |sim          | Senha do usuário.                                                  |
|confirmacaoSenha      |String  |sim          | Confirmação da senha do usuário.                                   |
|dataNascimentoUsuario |String  |sim          | Data de nascimento do usuário. (No formato "DD-MM-YYYY")           |
|telefoneUsuario       |String  |sim          | Número de celular do usuário.                                      |
|tipoPlano             |String  |sim          | O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|

```
{
    "nome": "Matheus Araujo",
    "email": "matheusaraujo@hotmail.com",
    "senha": "03121985",
    "confirmarSenha": "03121985",
    "data": "1860-05-15",
    "telefone": "(11) 94002-8922",
    "plano": {
        "id": 1,
        "tipoPlano": "Teste grátis",
        "planoMensal": null,
        "planoAnual": null
    }
}
```

*Corpo da resposta*

| Código | Descrição                                    |
|:------:|----------------------------------------------|
|201     | Usuário criada com sucesso.                  |
|400     | Usuário inválido.                            |
|409     | Já existe um usuário com o e-mail fornecido. |

<hr>

### Login Usuário

`POST` /api/usuarios/login

*Campos da requisição*  

|Campo|Tipo|Obrigatório|Descrição|
|:----:|----|:-------:|---------|
|email|String|sim|Email cadastrado|
|senha|String|não|Senha cadastrada|

```
{
    "email": "matheusaraujo@hotmail.com",
    "senha": "03121985"
}
```

*Exemplo de resposta*

|Campo|Tipo|Descrição|
|-----|----|-------|
|token|String|Usado para autenticar e permitir o acesso controlado a recursos ou funcionalidades da API.
|type|String|Tipo de token utilizado.
|prefix|String|Prefixo que indica como o token deve ser formatado

```
{
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJtYXRoZXVzYXJhdWpvQGhvdG1haWwuY29tIiwiaXNzIjoiTmFvRmFsaW5kbyIsImV4cCI6MTY5MzYxNzczOX0.bVNi2o2xZqv3QxWZdTIxaAKNcs0gDJv8r7vPP2gyiOw",
    "type": "JWT",
    "prefix": "Bearer"
}
```

<hr>

### Listar Usuários

`GET` /api/usuarios

*Exemplo de resposta*

| Campo                | Tipo   | Descrição                                                                  |
|----------------------|--------|----------------------------------------------------------------------------|
|idUsuario             |Long    | O id de um usuário previamente criado.                                     |
|nomeUsuario           |String  | Nome completo do usuário.                                                  |
|emailUsuario          |String  | Endereço de e-mail do usuário.                                             |
|senhaUsuario          |String  | Senha do usuário.                                                          |
|dataNascimentoUsuario |Date    | Data de nascimento do usuário. (No formato "DD-MM-YYYY")                   |
|telefoneUsuario       |String  | Número de celular do usuário.                                              |
|tipoPlano             |String  | O valor do tipo de pagamento escolhido pelo usuário (teste grátis, anual ou mensal)|

```
[
    {
        "idUsuario": "1"
        "nomeUsuario": "Mateus Araujo",
        "emailUsuario": "matheusaraujo@hotmail.com",
        "senhaUsuario": "03121865",
        "dataNascimentoUsuario": "15-05-1860",
        "telefoneUsuario": "11940028922",
        "tipoPlano": "Teste Grátis"
    },

    {
        "idUsuario": "2"
        "nomeUsuario": "José Aquino",
        "emailUsuario": "seuze@bol.com",
        "senhaUsuario": "11091922",
        "dataNascimentoUsuario": "02-12-1922",
        "telefoneUsuario": "11989224002",
        "tipoPlano": "Anual"
    },

    {
        "idUsuario": "3"
        "nomeUsuario": "Pedro de Alcântara João Carlos Leopoldo Salvador Bibiano Francisco Xavier de Paula Leocádio Miguel Gabriel Rafael Gonzaga de Bragança e Bourbon",
        "emailUsuario": "dompedro2@bol.com.br",
        "senhaUsuario": "07091822",
        "dataNascimentoUsuario": "15-05-1825",
        "telefoneUsuario": "11911111111",
        "tipoPlano": "Mensal"
    }
]
```

*Corpo da resposta*

| Código | Descrição                   |
|:------:|-----------------------------|
|200     | Listagem feita com sucesso. |
|404     | Lista não encontrada.       |

<hr>

### Alterar Usuário

`PUT` /api/usuarios/{id}

*Campos da requisição*

| Campo                | Tipo   | Obrigatório | Descrição                                                          |
|----------------------|--------|:-----------:|--------------------------------------------------------------------|
|idUsuario             |Long    |sim          | O id de um usuário previamente criado.                             |
|nomeUsuario           |String  |não          | Nome completo do usuário.                                          |
|emailUsuario          |String  |não          | Endereço de e-mail do usuário.                                     |
|senhaUsuario          |String  |não          | Senha do usuário.                                                  |
|dataNascimentoUsuario |String  |não          | Data de nascimento do usuário. (No formato "DD-MM-YYYY")           |
|telefoneUsuario       |String  |não          | Número de celular do usuário.                                      |
|tipoPlano             |String  |não          | O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|

```
{
    "nome": "Math",
    "email": "matheusaraujo@hotmail.com",
    "senha": "03121985",
    "confirmarSenha": "03121985",
    "data": "1860-05-15",
    "telefone": "(11) 94002-8922",
    "plano": {
        "id": 2,
        "tipoPlano": "Mensal",
        "planoMensal": 35,
        "planoAnual": null
    }
}
```

*Corpo de resposta*

| Código | Descrição                        |
|:------:|----------------------------------|
|200     | Alteração realizada com sucesso. |
|400     | Alteração inválida.              |
|404     | Avaliação não encontrada.        |

<hr>

### Detalhes Usuário

`GET` /api/usuarios/{id}

*Exemplo de resposta*

| Campo                | Tipo | Descrição                                                |
|----------------------|------|----------------------------------------------------------|
|idUsuario             |Long   | O id de um usuário previamente criado.                   |
|nomeUsuario           |String | Nome completo do usuário.                                |
|emailUsuario          |String | Endereço de e-mail do usuário.                           |
|senhaUsuario          |String | Senha do usuário.                                        |
|dataNascimentoUsuario |Date   | Data de nascimento do usuário. (No formato "DD-MM-YYYY") |
|telefoneUsuario       |String | Número de celular do usuário.                            |
|tipoPlano             |String | O valor do tipo de pagamento escolhido pelo usuário (teste grátis, anual ou mensal)|

```
{
    "idUsuario": "1",
    "nomeUsuario": "Matheus Araujo",
    "emailUsuario": "matheusaraujo@hotmail.com",
    "senhaUsuario": "03121865",
    "confirmacaoSenha": "03121865",
    "dataNascimentoUsuario": "15-05-1860",
    "telefoneUsuario": "11940028922",
    "tipoPlano": "Anual"
}
```

*Corpo da resposta*

| Código | Descrição                                  |
|:------:|--------------------------------------------|
|200     | Os dados foram retornados com sucesso.     |
|404     | Não foi encontrado um usuário com esse ID. |

<hr>

### Excluir Usuário

`DELETE` /api/usuarios/{id}

*Corpo da resposta*

| Código | Descrição                |
|:------:|--------------------------|
|204     | Requisição bem-sucedida. |
|404     | Conteúdo não encontrado. |

<hr>

### Cadastrar Plano

`POST` /api/planos/cadastro

*Campos da requisição*

| Campo             | Tipo      | Obrigatório    | Descrição                                                   |
|-------------------|-----------|:--------------:|-------------------------------------------------------------|
|tipoPlano          |String     |sim             |O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|
|planoMensal        |BigDecimal |não             |Valor do plano mensal.                                   |
|planoAnual         |BigDecimal |não             |Valor do plano anual.                                 |

```
{
    "tipoPlano": "Anual",
    "planoMensal": null,
    "planoAnual": 378
}
```

*Corpo da resposta*

| Código | Descrição                                                      |
|:------:|----------------------------------------------------------------|
|201     | Informação de plano criada com sucesso.                        |
|400     | Informação de plano inválida.                                  |

<hr>

### Listar Planos

`GET` /api/planos

*Campos da requisição*

| Campo             | Tipo      | Descrição                                                   |
|-------------------|-----------|-------------------------------------------------------------|
|tipoPlano          |String     |O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|
|planoMensal        |BigDecimal |Valor do plano mensal.                                   |
|planoAnual         |BigDecimal |Valor do plano anual.                                 |

```
[
    {
        "idPlano": 1,
        "tipoPlano": "Teste Gratis",
        "planoMensal": null,
        "planoAnual": null
    },
    {
        "idPlano": 2,
        "tipoPlano": "Mensal",
        "planoMensal": 35,
        "planoAnual": null
    },
    {
        "idPlano": 3,
        "tipoPlano": "Anual",
        "planoMensal": null,
        "planoAnual": 378
    }
]
```

*Corpo da resposta*

| Código | Descrição                   |
|:------:|-----------------------------|
|200     | Listagem feita com sucesso. |
|404     | Lista não encontrada.       |

<hr>

### Alterar Plano

`PUT` /api/planos/{id}

*Campos da requisição*

| Campo             | Tipo      | Obrigatório    | Descrição                                                   |
|-------------------|-----------|:--------------:|-------------------------------------------------------------|
|idPlano            |Long       |sim             | O id de um plano previamente criado.                        |
|tipoPlano          |String     |não             |O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|
|planoMensal        |BigDecimal |não             |Valor do plano mensal.                                   |
|planoAnual         |BigDecimal |não             |Valor do plano anual.                                 |

```
{
    "idPlano": 1,
    "tipoPlano": "Mensal",
    "planoMensal": 35
}
```

*Corpo de resposta*

| Código | Descrição                        |
|:------:|----------------------------------|
|200     | Alteração realizada com sucesso. |
|400     | Alteração inválida.              |
|404     | Plano não encontrado.            |

<hr>

### Detalhes Plano

`GET` /api/planos/{id}

*Campos da requisição*

| Campo             | Tipo      | Descrição                                                   |
|-------------------|-----------|-------------------------------------------------------------|
|idPlano            |Long       |sim             | O id de um plano previamente criado.                        |
|tipoPlano          |String     |O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|
|planoMensal        |BigDecimal |Valor do plano mensal.                                   |
|planoAnual         |BigDecimal |Valor do plano anual.                                 |

```
{
    "idPlano": 1,
    "tipoPlano": "Mensal",
    "planoMensal": 35,
    "planoAnual": null
}
```

*Corpo da resposta*

| Código | Descrição                                    |
|:------:|----------------------------------------------|
|200     | Os dados foram retornados com sucesso.       |
|404     | Não foi encontrado um plano com esse ID.     |

<hr>

### Excluir Plano

`DELETE` /api/planos/{id}

*Corpo da resposta*

| Código | Descrição                |
|:------:|--------------------------|
|204     | Requisição bem-sucedida. |
|404     | Conteúdo não encontrado. |

<hr>

### Cadastrar Pagamento

`POST` /api/pagamentos/cadastro

*Campos da requisição*

| Campo             | Tipo   | Obrigatório | Descrição                                                   |
|-------------------|--------|:-----------:|-------------------------------------------------------------|
|nomeTitular        |String  |sim          |Nome do titular do cartão.                                   |
|numeroCartao       |String  |sim          |Número do cartão de crédito.                                 |
|dataValidadeCartao |String  |sim          |Data de validade do cartão de crédito. (No formato "MM/YYYY")|
|codigoSeguranca    |String  |sim          |Código de segurança do cartão de crédito.                    |
|tipoPlano          |String  |sim          | O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|

```
{
    "nomeNoCartao": "MATHEUS ARAUJO",
    "numeroDoCartao": "1234 5678 9012 3456",
    "validadeDoCartao": "2024-10-01",
    "codigoDoCartao": "123",
    "plano": {
        "id": 1,
        "tipoPlano": "Teste grátis",
        "planoMensal": null,
        "planoAnual": null
    }
}
```

*Corpo da resposta*

| Código | Descrição                                                      |
|:------:|----------------------------------------------------------------|
|201     | Informação de pagamento criada com sucesso.                    |
|400     | Informação de pagamento inválida.                              |
|409     | Já existe uma informação de pagamento para o usuário informado.|

<hr>

### Listar Pagamentos

`GET` /api/pagamentos

*Campos da resposta*

| Campo             | Tipo   | Descrição                                                   |
|-------------------|--------|-------------------------------------------------------------|
|idPagamento        |Long    |O id de um pagamento previamente criado.                     |
|nomeTitular        |String  |Nome do titular do cartão.                                   |
|numeroCartao       |String  |Número do cartão de crédito.                                 |
|dataValidadeCartao |String  |Data de validade do cartão de crédito. (No formato "MM/YYYY")|
|codigoSeguranca    |String  |Código de segurança do cartão de crédito.                    |
|tipoPlano          |String  | O valor do tipo de pagamento escolhido pelo usuário (teste grátis, anual ou mensal)|

```
[
    {
        "idPagamento": 1,
        "idUsuario": 1,
        "nomeTitular": "MATHEUS ARAUJO",
        "numeroCartao": "**** **** **** 3456",
        "dataValidadeCartao": "10/2024",
        "codigoSeguranca": "***",
        "tipoPlano": "Anual"
    },

    {
        "idPagamento": 2,
        "idUsuario": 2,
        "nomeTitular": "JOSÉ A.",
        "numeroCartao": "**** **** **** 1234",
        "dataValidadeCartao": "03/2025",
        "codigoSeguranca": "***",
        "tipoPlano": "Mensal"
    },

    {
        "idPagamento": 3,
        "idUsuario": 3,
        "nomeTitular": "PEDRO ALVARES C.",
        "numeroCartao": "**** **** **** 1111",
        "dataValidadeCartao": "01/2024",
        "codigoSeguranca": "***",
        "tipoPlano": "Teste Grátis"
    }
]
```

*Corpo da resposta*

| Código | Descrição                   |
|:------:|-----------------------------|
|200     | Listagem feita com sucesso. |
|404     | Lista não encontrada.       |

<hr>

### Alterar Pagamento

`PUT` /api/pagamentos/{id}

*Campos da requisição*

| Campo             | Tipo   | Obrigatório | Descrição                                                   |
|-------------------|--------|:-----------:|-------------------------------------------------------------|
|idPagamento        |Long    |sim          |O id de um pagamento previamente criado.                     |
|nomeTitular        |String  |não          |Nome do titular do cartão.                                   |
|numeroCartao       |String  |não          |Número do cartão de crédito.                                 |
|dataValidadeCartao |String  |não          |Data de validade do cartão de crédito. (No formato "MM/YYYY")|
|codigoSeguranca    |String  |não          |Código de segurança do cartão de crédito.                    |
|tipoPlano          |String  |sim          | O tipo de pagamento escolhido pelo usuário. (teste grátis, anual ou mensal)|
```
{
    "nomeNoCartao": "MATH ARAUJO",
    "numeroDoCartao": "1234 5678 9012 3456",
    "validadeDoCartao": "2024-10-01",
    "codigoDoCartao": "321",
    "plano": {
        "id": 1,
        "tipoPlano": "Teste grátis",
        "planoMensal": null,
        "planoAnual": null
    }
}
```

*Corpo de resposta*

| Código | Descrição                        |
|:------:|----------------------------------|
|200     | Alteração realizada com sucesso. |
|400     | Alteração inválida.              |
|404     | Pagamento não encontrado.        |

<hr>

### Detalhes Pagamento

`GET` /api/pagamentos/{id}

*Campos da resposta*

| Campo             | Tipo   | Descrição                                                   |
|-------------------|--------|-------------------------------------------------------------|
|idPagamento        |Long    |O id de um pagamento previamente criado.                     |
|nomeTitular        |String  |Nome do titular do cartão.                                   |
|numeroCartao       |String  |Número do cartão de crédito.                                 |
|dataValidadeCartao |String  |Data de validade do cartão de crédito. (No formato "MM/YYYY")|
|codigoSeguranca    |String  |Código de segurança do cartão de crédito.                    |
|tipoPlano          |String  | O valor do tipo de pagamento escolhido pelo usuário (teste grátis, anual ou mensal)|

```
{
    "idPagamento": 1,
    "idUsuario": 1,
    "nomeTitular": "MATHEUS ARAUJO",
    "numeroCartao": "5678 9012 3456 1234",
    "dataValidadeCartao": "07/2025",
    "codigoSeguranca": "321",
    "tipoPlano": "Mensal"
}
```

*Corpo da resposta*

| Código | Descrição                                    |
|:------:|----------------------------------------------|
|200     | Os dados foram retornados com sucesso.       |
|404     | Não foi encontrado um pagamento com esse ID. |

<hr>

### Excluir Pagamento

`DELETE` /api/pagamentos/{id}

*Corpo da resposta*

| Código | Descrição                |
|:------:|--------------------------|
|204     | Requisição bem-sucedida. |
|404     | Conteúdo não encontrado. |

<hr>

### Cadastrar Pergunta

`POST` /api/pergunta

*Campos da requisição*

| Campo    | Tipo  | Obrigatório | Descrição                          |
|----------|-------|:-----------:|------------------------------------|           
|pergunta  |String |sim          |A pergunta feita pelo idoso.        |


```
{
    "pergunta": "Como faço para tirar uma foto com o meu celular?"
}
```

*Corpo da resposta*

| Código | Descrição                |
|:------:|--------------------------|
|201     | Requisição bem-sucedida. |
|400     | Requisição inválida.     |

<hr>

### Listar Perguntas

`GET` /api/pergunta

*Campos da resposta*

| Campo     | Tipo  | Descrição                                    |
|-----------|-------|----------------------------------------------|
|idPergunta |Long   |O id da pergunta à qual a resposta se refere. |
|pergunta   |String |A pergunta feita pelo idoso.                  |

```
[   
    {        
        "idPergunta": 1,
        "idUsuario": 1,        
        "pergunta": "Como faço para tirar uma foto com o meu celular?"
    },    
    {        
        "idPergunta": 2,
        "idUsuario": 1,
        "pergunta": "Como faço para enviar uma mensagem no WhatsApp?"
    },
    {
        "idPergunta": 3,
        "idUsuario": 2,
        "pergunta": "Como faço para adicionar um contato na minha lista de contatos do celular?"
    }
]
```

*Corpo da resposta*

| Código | Descrição                   |
|:------:|-----------------------------|
|200     | Listagem feita com sucesso. |
|404     | Lista não encontrada.       |

<hr>

### Detalhes Pergunta

`GET` /api/pergunta/{id}

*Campos da resposta*

| Campo     | Tipo  | Descrição                                    |
|-----------|-------|----------------------------------------------|
|idPergunta |Long   |O id da pergunta à qual a resposta se refere. |
|pergunta   |String |A pergunta feita pelo idoso.                  |


```
{        
    "idPergunta": 1,
    "idUsuario": 1,        
    "pergunta": "Como faço para tirar uma foto com o meu celular?"
}
```

*Corpo da resposta*

| Código | Descrição                                    |
|:------:|----------------------------------------------|
|200     | Os dados foram retornados com sucesso.       |
|404     | Não foi encontrado uma pergunta com esse ID. |

<hr>

### Cadastrar Resposta

`POST` /api/resposta

*Campos da requisição*

| Campo     | Tipo  | Obrigatório | Descrição                                    |
|-----------|-------|:-----------:|----------------------------------------------|
|idPergunta |Long   |sim          |O id da pergunta à qual a resposta se refere. |
|resposta   |String |sim          |A resposta do chatbot.                        |

```
{
    "resposta": "Abra o aplicativo Contatos ou Pessoas em seu dispositivo. Esse aplicativo pode ter nomes diferentes dependendo da marca do seu smartphone. Toque no ícone de adição                    (+) ou no botão Novo contato (geralmente localizado na parte inferior da tela). Você será direcionado para uma tela onde poderá inserir as informações do contato.                     Normalmente, você pode adicionar: Nome: Digite o nome completo do contato. Número de telefone: Insira o número de telefone do contato. Endereço de email: Adicione                     o endereço de email do contato (opcional). Foto do contato: Você pode adicionar uma foto do contato, se desejar. Outras informações: Dependendo do aplicativo, você                    também pode adicionar informações como endereço, empresa, aniversário, etc. Após preencher as informações necessárias, toque em Salvar ou no ícone de confirmação                      para salvar o contato.",
    "pergunta": {
        "id": 6,
        "pergunta":"Como cadastro um contato?"
    }
}
```

| Código | Descrição                |
|:------:|--------------------------|
|201     | Requisição bem-sucedida. |
|400     | Requisição inválida.     |

<hr>

### Listar Respostas

`GET` /api/resposta

*Campos da resposta*

| Campo     | Tipo  | Descrição                                    |
|-----------|-------|----------------------------------------------|
|idResposta |Long   |O id da resposta.                             |
|idPergunta |Long   |O id da pergunta à qual a resposta se refere. |
|resposta   |String |A resposta do chatbot.                        |

```
[
    {
        "idResposta": 1,
        "idPergunta": 1,
        "resposta": "Para tirar uma foto com o seu celular, siga os passos abaixo:
        1. Abra o aplicativo de câmera do seu celular.
        2. Enquadre o objeto ou a cena que deseja fotografar.
        3. Toque no botão de disparo da câmera para capturar a foto.
        4. Você pode acessar a foto tirada na galeria de imagens do seu celular.
        Espero que estas instruções sejam úteis!"
        
    },
    {
        "idResposta": 2,
        "idPergunta": 2,
        "resposta": "Para enviar uma mensagem no WhatsApp, siga os passos abaixo:
        5. Abra o aplicativo do WhatsApp no seu celular.
        6. Toque na opção de bate-papo ou no nome do contato para quem deseja enviar a mensagem.
        7. Digite a mensagem no campo de texto.
        8. Toque no botão de envio (geralmente representado por um ícone de avião de papel).
        9. A mensagem será enviada ao contato selecionado.
        Espero que estas instruções sejam úteis!"
    },
    {
        "idResposta": 3,
        "idPergunta": 3,
        "resposta": "Para adicionar um contato na lista de contatos do seu celular, siga os passos abaixo:
        10. Abra o aplicativo de contatos no seu celular.
        11. Selecione a opção de adicionar um novo contato.
        12. Preencha os campos de nome, número de telefone e outras informações relevantes do contato.
        13. Salve o contato.
        O novo contato será adicionado à sua lista de contatos do celular e estará disponível para acesso e uso."
    }
]
```

*Corpo da resposta*

| Código | Descrição                   |
|:------:|-----------------------------|
|200     | Listagem feita com sucesso. |
|404     | Lista não encontrada.       |

<hr>

### Detalhes Resposta

`GET` /api/resposta/{id}

*Campos da resposta*

| Campo     | Tipo  | Descrição                                    |
|-----------|-------|----------------------------------------------|
|idResposta |Long   |O id da resposta.                             |
|idPergunta |Long   |O id da pergunta à qual a resposta se refere. |
|resposta   |String |A resposta do chatbot.                        |

```
{
    "idResposta": 1,
    "idPergunta": 1,
    "resposta": "Para tirar uma foto com o seu celular, siga os passos abaixo:
    1. Abra o aplicativo de câmera do seu celular.
    2. Enquadre o objeto ou a cena que deseja fotografar.
    3. Toque no botão de disparo da câmera para capturar a foto.
    4. Você pode acessar a foto tirada na galeria de imagens do seu celular.
    Espero que estas instruções sejam úteis!" 
}
```

*Corpo da resposta*

| Código | Descrição                                    |
|:------:|----------------------------------------------|
|200     | Os dados foram retornados com sucesso.       |
|404     | Não foi encontrado uma resposta com esse ID. |

<hr>

## Contribuição

Se você deseja contribuir para o projeto, sinta-se à vontade para enviar pull requests ou relatar problemas no repositório do projeto. Esse projeto ainda se encontra em construção.

## Licença

Este projeto é licenciado sob a Licença MIT - consulte o arquivo [LICENSE](./LICENSE) para obter mais detalhes.

