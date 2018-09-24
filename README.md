# Appmoove: Teste prático para back-end developer 

## Pré-requisitos

- JavaScript
- NodeJS
- MySQL
- REST


## Instruções

1. Faça o clone deste repositório.
2. Rode o `dump.sql` em uma base de dados local. Utilize estas tabelas e estes dados.
2. Implemente o desafio.
3. Após término envie seu projeto <nome_candidato>.zip no e-mail contato@appmoove.com.br, adicione a pasta do projeto um dump do seu BD após finalizar o teste.

    **Atenção: Remova a pasta `node_modules` antes de zipar o projeto.**


## Como iniciar

Abra o diretório do projeto no terminal e digite os comandos abaixo

```bash
npm install
npm start
```


### Bibliotecas pré instaladas
1. [axios](#https://github.com/axios/axios)
2. [bluebird](#http://bluebirdjs.com/docs/getting-started.html)
3. [body-parser](#https://github.com/expressjs/body-parser)
3. [express](#https://expressjs.com/pt-br/4x/api.html)
4. [knex](#https://knexjs.org/)
5. [mysql2](#https://github.com/sidorares/node-mysql2)



## Desafio

Você deve desenvolver uma API REST para compra de produtos utilizando a forma de pagamento cartão de crédito. A API deve possibilitar todo o gerenciamento de estoque e venda de produtos, para isso os seguintes endpoints devem ser implementados:

1. Adicionar produtos ao estoque

    Esta rota insere um novo registro na tabela _produtos_.
    - Request
        ```bash
        POST http://localhost:8080/api/produtos 
        ```
    - Body
        ```json
        {
            "nome": "Macbook 13\" 8GB|256SSD|2.7Ghz",
            "valor_unitario": 8450.0,
            "qtde_estoque": 5
        }
        ```
    - Retornos possíveis

        Código | Resposta
        ------------ | -------------
        `201 (Criado)` | `Produto cadastrado ` 
        `400 (Requisição inválida)` | `Ocorreu um erro desconhecido`
        `412 (Pré-condição falhou)` | `Os valores informados não são    válidos.`

2. Listar produtos

    Esta rota lista todos os produtos *disponíveis* para venda.
    - Request
        ```bash
        GET http://localhost:8080/api/produtos
        ```
    - Retornos possíveis

        Retornar um array com todos os produtos cadastrados. Atributos: nome, valor unitário, quantidade em estoque.

        Código | Resposta
        ------------ | -------------
        `200 (OK)` | [] 
        `400 (Requisição inválida)` | `Ocorreu um erro desconhecido`

3. Detalhar um produto

    Esta rota obtém um produto específico por seu id, os dados de retorno são id, nome, valor unitário, quantidade em estoque, data e valor da última venda deste produto, caso o produto não possua vendas a data da última venda deve retornar null;
    - Request 
        ```bash
        GET http://localhost:8080/api/produtos/:produto_id
        ```
    - Retornos possíveis

        Retornar um objeto com atributos: nome, valor unitário, quantidade em estoque, data e valor da última venda.

        Código | Resposta
        ------------ | -------------
        `200 (OK)` | {}
        `400 (Requisição inválida)` | `Ocorreu um erro desconhecido`

4. Comprar um produto

    Esta rota realiza a compra de um produto, ela deve: requisitar a rota [Gateway](#gateway) de pagamentos, caso a compra seja aprovada: realizar a baixa do produto no estoque e retornar a resposta de sucesso. Qualquer erro que ocorra durante a compra dever ser retornado a resposta de erro.
    `Não esqueça de validar o número do cartão de crédito antes de enviá-lo ao gateway.`
    - Request 
        ```bash
        POST http://localhost:8080/api/compras
        ```
    - Body
        ```json
        {
            "produto_id": 1,
            "qtde_comprada": 1,
            "cartao": {
                "titular": "John Doe",
                "numero": "4111111111111111",
                "data_expiracao": "12/2018",
                "bandeira": "VISA",
                "cvv": "123",
            }
        }
        ```
    - Retornos possíveis

        Código | Resposta
        ------------ | -------------
        `201 (Criado)` | `Venda realizada com sucesso` 
        `400 (Requisição inválida)` | `Ocorreu um erro desconhecido`
        `412 (Pré-condição falhou)` | `Os valores informados não são    válidos.`

5. Remover um produto do estoque

    Esta rota remove um produto da base de dados.
    - Request 
        ```bash
        DELETE http://localhost:8080/api/produtos/produto_id
        ```
    - Retornos possíveis

        Código | Resposta
        ------------ | -------------
        `204 (Nenhum conteúdo) ` | `Produto excluído com sucesso` 
        `400 (Requisição inválida)` | `Ocorreu um erro desconhecido`


### O que nós esperamos no seu teste 
1. Uso de validadores na rota.
2. Testes unitários.
3. Documentação
4. Utilização adequada das bibliotecas fornecidas [**dica: sempre ler a doc ;)**].
5. Código escrito da maneira mais semântica possível.


### O que nos impressionaria
- Alguma metodologia para definição e organização do seu código.
- Conteinerização da aplicação.

### O que nós não gostaríamos
- Descobrir que não foi você quem fez seu teste.
- Ver commits gigantes, sem mensagens ou com `-m` sem pé nem cabeça.

## O que avaliaremos de seu teste
1. Funcionamento e método de resolução do problema.
2. Organização do código.
3. Performance do código.
4. Documentação da API.
5. Arquitetura do projeto.
6. Semântica, estrutura, legibilidade, manutenibilidade, escalabilidade do seu código e suas tomadas de decisões.
7. Históricos de commits do git.

## Gateway
Gateway de pagamentos é um serviço destinado a lojas virtuais, por ele é possível autorizar transações de pagamentos online.
Em nosso teste você irá utilizar um gateway de pagamentos em mock, nós criamos uma rota para simular a transação de compra, o que você precisa fazer é fazer um *POST* nesta rota:
```bash
POST http://sv-dev-01.pareazul.com.br:8080/api/gateways/compras
```
como este body:
```json
{
    "valor": 100.0,
    "cartao": {
        "titular": "John Doe",
        "numero": "4111111111111111",
        "data_expiracao": "12/2018",
        "bandeira": "VISA",
        "cvv": "123"
    }
}
```
possíveis respostas do gateway:

- Quando a compra for aprovada
    ```json
    {
        "valor": 100.0,
        "estado": "APROVADO"
    }
    ```
- Quando a compra for rejeitada
    ```json
    {
        "valor": 100.0,
        "estado": "REJEITADO"
    }
    ```

### Dúvidas
Mande um e-mail para nós: contato@appmoove.com.br
