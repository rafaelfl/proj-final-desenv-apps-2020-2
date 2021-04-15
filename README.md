# Documentação da API do Projeto da Disciplina Desenvolvimento de Aplicações Móveis

ECP-UFMA (2020-2)

## Descrição do projeto

**Para o projeto final (que valerá para a segunda e a terceira notas), deverá ser desenvolvido um aplicativo escrito em Flutter (web ou móvel) que seja uma loja de e-commerce.**

O aplicativo deverá atender a dois usuários: (a) o cliente (ou comprador) e (b) o administrador (ou lojista).

O aplicativo deverá implementar as seguintes histórias de usuário:

1. Como **comprador** , eu quero realizar o login para acessar o sistema;
2. Como **comprador** , eu quero listar os produtos disponíveis na loja para que eu possa identificar oportunidades de compra;
3. Como **comprador** , eu quero adicionar produtos ao meu carrinho de compras para que eu possa comprar vários produtos em um mesmo pedido;
4. Como **comprador** , eu quero finalizar o pedido a partir do carrinho para que eu possa pagar o meu pedido;
5. Como **comprador** , eu quero pagar o meu pedido para que eu possa receber os meus produtos;
6. Como **comprador** , eu quero cancelar o meu pedido para que eu possa desistir da minha compra;
7. Como **comprador** , eu quero visualizar todos os pedidos que eu já realizei para que eu possa entender como está o meu histórico de compras na loja;
8. Como **lojista** , eu quero realizar o login para acessar o sistema;
9. Como **lojista** , eu quero listar todos os pedidos da minha loja e os seus valores para que eu possa controlar como está a entrada de dinheiro;
10. Como **lojista** , eu quero cadastrar novos produtos para oferecer novas opções de compra para os meus clientes;
11. Como **lojista** , eu apagar produtos para remover produtos que não estejam mais sendo vendidos.

Para realizar essas funções, foi disponibilizado um serviço REST que vocês possam realizar a operação da aplicação de vocês. Desse modo, vocês deverão focar apenas no front-end com Flutter, visto que vou disponibilizar o back-end para vocês.

**OBS:** para a história de usuário de número 10, não é necessário fazer o upload de uma foto para o produto. Basta informar no cadastro o link de uma imagem (mas se quiser implementar, sinta-se à vontade! Isso será valorizado. ;))

## Descrição da API REST

APIs REST são uma forma moderna e flexível de estruturar aplicações web e móveis, tirando parte da complexidade do dispositivo e a migrando para uma infra-estrutura computacional na nuvem, bem como permitindo um acesso simplificado à bases de dados.

Para implementar as funcionalidades do aplicativo que vocês estarão desenvolvendo, foi disponibilizado para vocês uma API para o controle e armazenamento de dados. Essa API está acessível a partir do endereço: [https://restful-ecommerce-ufma.herokuapp.com/](https://restful-ecommerce-ufma.herokuapp.com/)

Os seguintes serviços são providos pela API:

1. Autenticação
2. Listagem e administração de produtos
3. Criação de carrinhos de compra
4. Finalização de pedidos
5. Restrições de acesso

O código fonte da API (para quem se interessar, apenas), está disponível em: [https://github.com/rafaelfl/restful-ecommerce](https://github.com/rafaelfl/restful-ecommerce)

## Interface de acesso da API REST

Para acessar a API do serviço, você deverá utilizar métodos HTTP por meio de pacotes Flutter que forneçam o acesso ao serviço, como o pacote http ([https://pub.dev/packages/http](https://pub.dev/packages/http)) ou dio ([https://pub.dev/packages/dio](https://pub.dev/packages/dio)).

Para acessar a API REST, você deverá inicialmente realizar o login no serviço. Dois usuários já estão disponíveis para que possam ser utilizados:

| **Email** | **Senha** | **Access** |
| --- | --- | --- |
| admin@example.com | secret | Admin Access |
| user@example.com | secret | User Access |

A API sempre vai retornar um JSON no seguinte formato:

```
{
    success: true | false,

    data: { dados em formato JSON },

    message: <string> [opcional, retorna principalmente informações de erros]
}
```

A seguir são detalhadas as funcionalidades disponíveis na API e suas interfaces de acesso.

**Registrar um novo usuário (opcional no app, mas recomendado para evitar que diferentes alunos tenham problemas acessando com o mesmo usuário)**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/register](https://restful-ecommerce-ufma.herokuapp.com/register)

_Método:_ POST

_Corpo da mensagem:_

firstName - primeiro nome do usuário

lastName - último nome do usuário

email - email do usuário

password - senha a ser registrada

passwordConfirmation - confirmação de senha - deve ser igual à password

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;id&quot;: 3,

&quot;firstName&quot;: &quot;Teste&quot;,

&quot;lastName&quot;: &quot;de Teste&quot;,

&quot;email&quot;: &quot;teste@teste.com&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T02:26:06.587Z&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T02:26:06.587Z&quot;,

&quot;isAdmin&quot;: false,

&quot;token&quot;: &quot;…&quot;

}

}

**Login de um usuário (comprador ou lojista)**

Para que as funcionalidades possam ser acessadas, é necessário realizar o login com um usuário válido. Após o login (ou mesmo após o registro) é retornado um token para o acesso ao serviço (que deverá ser incorporado em TODAS as requisições à API). Esse token é gerado quando é realizado o login e ele tem um tempo de expiração de 12 horas (bastante tempo para testar, não ;)).

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/login](https://restful-ecommerce-ufma.herokuapp.com/login)

_Método:_ POST

_Corpo da mensagem:_

email - email do usuário

password - senha do usuário

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;id&quot;: 2,

&quot;firstName&quot;: &quot;John&quot;,

&quot;lastName&quot;: &quot;Doe&quot;,

&quot;email&quot;: &quot;user@example.com&quot;,

&quot;isAdmin&quot;: null,

&quot;createdAt&quot;: &quot;2021-04-14T21:39:01.943Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-14T21:39:01.943Z&quot;,

&quot;token&quot;: &quot;…&quot;

}

}

**ATENÇÃO! TODAS AS DEMAIS REQUISIÇÕES A SEGUIR DEVERÃO TER DEFINIDAS UM CABEÇALHO QUE INCORPORE O TOKEN GERADO NO LOGIN!**

Cabeçalho das mensagens a seguir:

**Authorization**** : **** Bearer **** \<SEU TOKEN AQUI\>**

Isso vale para todas as requisições!!!

—— **LISTA DE OPERAÇÕES REALIZADAS PELO COMPRADOR**

**Listar produtos disponíveis**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/api/v1/products](https://restful-ecommerce-ufma.herokuapp.com/api/v1/products)

_Método:_ GET

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: [

{

&quot;id&quot;: 1,

&quot;title&quot;: &quot;Awesome Fresh Gloves&quot;,

&quot;description&quot;: &quot;Praesentium asperiores expedita eligendi commodi a. Qui nostrum quia quo sint cupiditate ea repellat occaecati qui. Minus praesentium quasi architecto. Ab eius qui non suscipit.&quot;,

&quot;price&quot;: 634,

&quot;imageUrl&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;createdAt&quot;: &quot;2021-04-14T21:39:02.785Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-14T21:39:02.785Z&quot;

},

….

]

}

**Consultar um produto**

_Endpoint:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/products/\<CÓDIGO DO PRODUTO\>

_Exemplo:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/products/2

_Método:_ GET

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;id&quot;: 1,

&quot;title&quot;: &quot;Awesome Fresh Gloves&quot;,

&quot;description&quot;: &quot;Praesentium asperiores expedita eligendi commodi a. Qui nostrum quia quo sint cupiditate ea repellat occaecati qui. Minus praesentium quasi architecto. Ab eius qui non suscipit.&quot;,

&quot;price&quot;: 634,

&quot;imageUrl&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;createdAt&quot;: &quot;2021-04-14T21:39:02.785Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-14T21:39:02.785Z&quot;

}

}

**Adicionar produto no carrinho**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/api/v1/cart/add](https://restful-ecommerce-ufma.herokuapp.com/api/v1/cart/add)

_Método:_ POST

_Corpo da mensagem:_

productId - código do produto

qty - quantidade do produto comprado

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;cartItem&quot;: {

&quot;items&quot;: [

{

&quot;id&quot;: 2,

&quot;title&quot;: &quot;Sleek Fresh Chips&quot;,

&quot;description&quot;: &quot;Omnis porro minus sit molestias dolore eius corrupti cum. Veritatis repudiandae iure dolore et fugiat. Quis ea est est rerum inventore et ex esse. Sunt recusandae iure dolorum dolor quia placeat molestiae.&quot;,

&quot;price&quot;: 492,

&quot;image&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;qty&quot;: 1

}

],

&quot;totalAmount&quot;: 492

},

&quot;id&quot;: 1,

&quot;userId&quot;: 2,

&quot;createdAt&quot;: &quot;2021-04-15T00:04:12.803Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T02:44:31.751Z&quot;

}

}

**Listar produtos no carrinho**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/api/v1/cart](https://restful-ecommerce-ufma.herokuapp.com/api/v1/cart)

_Método:_ GET

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;cartItem&quot;: {

&quot;items&quot;: [

{

&quot;id&quot;: 2,

&quot;title&quot;: &quot;Sleek Fresh Chips&quot;,

&quot;description&quot;: &quot;Omnis porro minus sit molestias dolore eius corrupti cum. Veritatis repudiandae iure dolore et fugiat. Quis ea est est rerum inventore et ex esse. Sunt recusandae iure dolorum dolor quia placeat molestiae.&quot;,

&quot;price&quot;: 492,

&quot;image&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;qty&quot;: 1

}

],

&quot;totalAmount&quot;: 492

},

&quot;id&quot;: 1,

&quot;userId&quot;: 2,

&quot;createdAt&quot;: &quot;2021-04-15T00:04:12.803Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T02:44:31.751Z&quot;

}

}

**Finalizar o pedido (transformar o carrinho em pedido)**

_Endpoint:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders

_Método:_ POST

_Corpo da mensagem:_

Não há

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;items&quot;: [

{

&quot;id&quot;: 2,

&quot;title&quot;: &quot;Sleek Fresh Chips&quot;,

&quot;description&quot;: &quot;Omnis porro minus sit molestias dolore eius corrupti cum. Veritatis repudiandae iure dolore et fugiat. Quis ea est est rerum inventore et ex esse. Sunt recusandae iure dolorum dolor quia placeat molestiae.&quot;,

&quot;price&quot;: 492,

&quot;image&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;qty&quot;: 1

}

],

&quot;status&quot;: &quot;pending&quot;,

&quot;id&quot;: 7,

&quot;userId&quot;: 2,

&quot;grandTotal&quot;: 492,

&quot;updatedAt&quot;: &quot;2021-04-15T02:48:19.509Z&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T02:48:19.509Z&quot;

}

}

**Listar pedidos**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders](https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders)

_Método:_ GET

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: [

{

&quot;items&quot;: [

{

&quot;id&quot;: 4,

&quot;title&quot;: &quot;Sleek Granite Shoes&quot;,

&quot;description&quot;: &quot;Non quam magni. Ad delectus aut et. Nisi repellat et non optio beatae voluptatem accusamus corporis. Ullam quis ipsam. Asperiores reiciendis atque dolorem omnis suscipit non provident ipsa assumenda.&quot;,

&quot;price&quot;: 170,

&quot;image&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;qty&quot;: 2

}

],

&quot;id&quot;: 1,

&quot;userId&quot;: 2,

&quot;grandTotal&quot;: 340,

&quot;status&quot;: &quot;cancelled&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T00:07:18.083Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T00:10:54.906Z&quot;

},

…

]

}

Os pedidos podem ter um dos dois três estados: &quot;pending&quot; (aguardando pagamento), &quot;cancelled&quot; (pedido cancelado) e &quot;completed&quot; (pedido pago). Para consultar os pedidos por tipo, basta realizar a mesma requisição GET acima, porém com a seguintes URLs (respectivamente):

[https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/pending](https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/pending)

[https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/cancelled](https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/cancelled)

[https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/completed](https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/completed)

**Cancelar um pedido**

_Endpoint:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/\<CÓDIGO DO PEDIDO\>/cancel

_Exemplo_: https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/1/cancel

_Método:_ POST

_Corpo da mensagem:_

Não há

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;items&quot;: [

{

&quot;id&quot;: 4,

&quot;title&quot;: &quot;Sleek Granite Shoes&quot;,

&quot;description&quot;: &quot;Non quam magni. Ad delectus aut et. Nisi repellat et non optio beatae voluptatem accusamus corporis. Ullam quis ipsam. Asperiores reiciendis atque dolorem omnis suscipit non provident ipsa assumenda.&quot;,

&quot;price&quot;: 170,

&quot;image&quot;: &quot;http://lorempixel.com/640/480&quot;,

&quot;qty&quot;: 2

}

],

&quot;id&quot;: 1,

&quot;userId&quot;: 2,

&quot;grandTotal&quot;: 340,

&quot;status&quot;: &quot;cancelled&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T00:07:18.083Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T00:10:54.906Z&quot;

}

}

**Pagar um pedido**

_Endpoint:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/\<CÓDIGO DO PEDIDO\>/pay

_Exemplo_: https://restful-ecommerce-ufma.herokuapp.com/api/v1/orders/2/pay

_Método:_ POST

_Corpo da mensagem:_

Não há

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;items&quot;: [],

&quot;id&quot;: 2,

&quot;userId&quot;: 2,

&quot;grandTotal&quot;: 0,

&quot;status&quot;: &quot;completed&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T00:15:00.174Z&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T01:40:33.801Z&quot;

}

}

—— **LISTA DE OPERAÇÕES EXCLUSIVAS DO LOJISTA**

**Adicionar um novo produto à loja**

_Endpoint:_[https://restful-ecommerce-ufma.herokuapp.com/api/v1/products](https://restful-ecommerce-ufma.herokuapp.com/api/v1/products)

_Método:_ POST

_Corpo da mensagem:_

title - nome do produto

description - descrição do produto

price - valor inteiro do preço

imageUrl - endereço de uma foto do produto

_Exemplo de resultado retornado:_

{

&quot;success&quot;: true,

&quot;data&quot;: {

&quot;id&quot;: 6,

&quot;title&quot;: &quot;Produto de Teste&quot;,

&quot;description&quot;: &quot;Decrição do produto de teste&quot;,

&quot;price&quot;: 20,

&quot;imageUrl&quot;: &quot;https://sequelize.org/master/image/brand\_logo.png&quot;,

&quot;updatedAt&quot;: &quot;2021-04-15T03:09:42.462Z&quot;,

&quot;createdAt&quot;: &quot;2021-04-15T03:09:42.462Z&quot;

}

}

Código de status 201

**Remover um produto da loja**

_Endpoint:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/products/\<CÓDIGO DO PRODUTO\>

_Exemplo:_https://restful-ecommerce-ufma.herokuapp.com/api/v1/products/6

_Método:_ DELETE

_Corpo da mensagem:_

Não há

_Exemplo de resultado retornado:_

Mensagem vazia com o código de status 204 (sem conteúdo)
