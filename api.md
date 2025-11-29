# API

## Sumário Interativo
- [Application Programming Interface](#application-programming-interface)
  - [Modelo Cliente-Servidor](#modelo-cliente-servidor)
  - [Arquiteturas](#arquiteturas)
    - [REST](#rest)
    - [Seis Restrições REST](#seis-restrições-rest)
    - [RESTful](#restful)
    - [Idempotência](#idempotência)
  - [Parâmetros de Caminho e de Consulta](#parâmetros-de-caminho-e-de-consulta)
  - [JSON API](#json-api)
  - [GraphQL](#graphql)
  - [WebSocket](#websocket)
  - [cURL](#curl)
  - [CORS](#cors)
  - [Autenticação de API](#autenticação-de-api)
    - [JWT](#jwt-json-web-token)
  - [Padrões de Integração](#padrões-de-integração)
    - [Polling](#polling)
    - [Webhooks (Pushing)](#webhooks-pushing)

---

# Application Programming Interface

Uma API é um conjunto de regras que define como diferentes sistemas podem conversar entre si. Essas regras indicam como trocar dados de forma precisa e segura.

## Modelo Cliente-Servidor

- **Cliente:** sistema que faz a solicitação (ex.: navegador).  
- **Servidor:** sistema que recebe a solicitação e devolve dados (ex.: vídeos do YouTube).

![clientserverside](images/clientserverside.png)
<br>
*Fonte: [https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/](https://www.cloudflare.com/learning/serverless/glossary/client-side-vs-server-side/)*

Exemplo de fluxo:
1. Cliente solicita um vídeo ao servidor do YouTube.  
2. Servidor processa e envia o vídeo.  
3. Cliente exibe o vídeo ao usuário.


---

## Arquiteturas

Consumidores utilizam APIs como clientes, mas é importante entender seu funcionamento e desenvolvimento. As arquiteturas mais relevantes são **REST** e **RESTful**.

### REST

- **REST (Representational State Transfer)** é um estilo arquitetural para sistemas distribuídos.  
- Utiliza HTTP e métodos **GET, POST, PUT, DELETE**.  
- Cada recurso é acessado por um **endpoint** (URL).  
- **Stateless:** cada requisição contém todas as informações necessárias.  
- Códigos de status: `200` (OK), `404` (Not Found), `500` (Server Error).  
- Dados, em geral, em **JSON** (ou XML).

#### Seis Restrições REST

| Restrição | Descrição |
|-----------|-----------|
| Interface uniforme | Facilita comunicação e desacoplamento. |
| Stateless | Servidor não mantém estado entre requisições. |
| Cacheável | Respostas indicam se podem ser armazenadas. |
| Cliente-Servidor | Separação de responsabilidades. |
| Sistema em camadas | Possibilidade de intermediários transparentes. |
| Código sob demanda | Opcional – servidor envia código executável. |

### RESTful

APIs **RESTful** seguem todas as restrições REST, resultando em serviços simples, escaláveis e padronizados.

### Idempotência

- **PUT:** Substitui ou cria recurso; múltiplas execuções mantêm mesmo estado final.  
- **DELETE:** Remove recurso; chamadas subsequentes retornam `404`.

---

## Parâmetros de Caminho e de Consulta

### 1. Path Parameters

- Parte fixa da URL.  
- Identificam recurso único.  
- `GET /usuarios/{usuarioId}` → `/usuarios/123`.

### 2. Query Parameters

- Após `?`, separados por `&`.  
- Usados para filtros, ordenação, paginação.  
- `GET /usuarios?status=ativo&idade=30`.

**Diferenças:** path é obrigatório; query é opcional.

Outros parâmetros: **Header** (ex.: autenticação) e **Cookie** (sessão).

---

## JSON API

Formato leve de troca de dados baseado em pares chave-valor.

```json
{
  "nome": "Diego",
  "idade": 27,
  "cidades": ["São Paulo", "Rio de Janeiro"],
  "ativo": true
}
````

### Exemplo de criação de usuário

```http
POST /api/usuarios
Content-Type: application/json

{
  "nome": "João Silva",
  "email": "joao.silva@example.com",
  "idade": 30,
  "cidade": "São Paulo"
}
```

---

Existem diferentes protocolos de API, cada um com características próprias que influenciam no desempenho, segurança e facilidade de implementação. 

---

### **1. REST (Representational State Transfer)** 

* Baseado no protocolo **HTTP**, é o padrão mais popular atualmente.
* Utiliza **recursos** identificados por **URLs** e operações baseadas em **métodos HTTP** (GET, POST, PUT, DELETE).
* É conhecido por ser **simples**, **escalável** e amplamente suportado.

---

### **2. SOAP (Simple Object Access Protocol)** 

* Mais antigo e formal, utiliza **XML** para troca de mensagens.
* É orientado a **contratos (WSDL)** e conhecido pela **robustez** em ambientes corporativos.
* Geralmente, apresenta maior **complexidade** em comparação ao REST.

---

### **3. GraphQL**

* Criado pelo Facebook em 2015.
* Permite ao cliente definir **exatamente quais dados deseja receber**, evitando *overfetching* (trazer dados em excesso) ou *underfetching* (trazer dados insuficientes).
* É altamente **flexível** para aplicações modernas.

---

### **4. gRPC (Google Remote Procedure Call)** 

* Desenvolvido pelo Google.
* Utiliza **Protocol Buffers (Protobuf)** para serialização de **dados binários**, garantindo **alta performance** e baixo consumo de rede.
* Muito usado em arquiteturas de **microsserviços** e comunicação interna de alta velocidade.

---

### **5. WebSockets** ⚡

* Não é um protocolo de API no sentido clássico, mas um padrão para comunicação **bidirecional** e **em tempo real** entre cliente e servidor.
* Ideal para **chats**, **jogos online** e sistemas de monitoramento que exigem baixa latência.

---

### **🔗 Referências**

* Documentação oficial do **REST**: [https://restfulapi.net](https://restfulapi.net)
* Guia da W3C sobre **SOAP**: [https://www.w3.org/TR/soap](https://www.w3.org/TR/soap)
* **GraphQL** oficial: [https://graphql.org](https://graphql.org)
* **gRPC** oficial: [https://grpc.io](https://grpc.io)
* **WebSockets** MDN: [https://developer.mozilla.org/docs/Web/API/WebSockets_API](https://developer.mozilla.org/docs/Web/API/WebSockets_API)

---

## cURL

cURL é uma ferramenta de linha de comando para testar e consumir APIs HTTP.

---

### Principais comandos

- **GET:** Obtém dados.

  ```bash
  curl https://api.github.com/users/username/repos
```

* **GET explícito:**

  ```bash
  curl -X GET https://api.example.com/resource
  ```

* **POST:** Envia dados para criar um recurso.

  ```bash
  curl -X POST -d "userId=5&title=PostTitle&body=PostContent" https://jsonplaceholder.typicode.com/posts
  ```

* **POST com autenticação e JSON:**

  ```bash
  curl -X POST https://some-web-url/api/v1/users \
  -H 'Accept: application/json' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer {ACCESS_TOKEN}' \
  -d '{
    "username": "myusername",
    "email": "myusername@gmail.com",
    "password": "PasswOrd123!"
  }'
  ```

* **PUT:** Atualiza um recurso por completo.

  ```bash
  curl -X PUT -H "Content-Type: application/json" \
  -d '{"userId": 5, "title": "New Post Title", "body": "New post content."}' \
  https://jsonplaceholder.typicode.com/posts/5
  ```

* **PATCH:** Atualiza parte de um recurso.

  ```bash
  curl -X PATCH -H "Content-Type: application/json" \
  -d '{"title": "Updated Title"}' \
  https://jsonplaceholder.typicode.com/posts/5
  ```

* **DELETE:** Remove um recurso.

  ```bash
  curl -X DELETE https://jsonplaceholder.typicode.com/posts/5
  ```

---

## CORS (Cross-Origin Resource Sharing)

CORS é um mecanismo de segurança dos navegadores que controla o acesso a recursos entre diferentes origens (combinação de protocolo, domínio e porta).


### Origem

Diferença em protocolo, domínio ou porta já define origens diferentes.


### Restrição

Navegadores bloqueiam por padrão requisições AJAX (fetch, XMLHttpRequest) de uma origem para outra, exceto se o servidor responder com os cabeçalhos CORS corretos.


### Cabeçalhos CORS

O servidor deve liberar explicitamente acesso, usando cabeçalhos como:

- **Access-Control-Allow-Origin:** Domínios permitidos.
- **Access-Control-Allow-Methods:** Métodos HTTP liberados.
- **Access-Control-Allow-Headers:** Cabeçalhos aceitos.
- **Access-Control-Allow-Credentials:** Permite cookies/autenticação.


### Tipos de Requisições

- **Simples:** Usam métodos GET, HEAD ou POST com cabeçalhos padrões. Só funcionam se o servidor liberar a origem.
- **Complexas:** Usam outros métodos (PUT, DELETE, etc.) ou cabeçalhos personalizados. O navegador faz uma pré-requisição (OPTIONS) antes da requisição real. O servidor deve liberar métodos e cabeçalhos via resposta OPTIONS.


### Importância

- **Segurança:** Bloqueia requisições não autorizadas de outros sites.
- **Controle:** Permite liberar acesso somente para origens confiáveis.


### Implementação

Configuração é feita no backend (ex: ExpressJS usa o middleware `cors`).


### Observação

Ferramentas como **Postman** ou **curl** não aplicam CORS, por isso testes nelas não refletem a segurança do navegador.


---

## Autenticação de API

### Métodos Comuns

| Método              | Descrição                                    |
| ------------------- | -------------------------------------------- |
| Chave de API        | Token simples, porém menos seguro.           |
| OAuth 2.0           | Padrão robusto de autenticação/ autorização. |
| JWT                 | Token assinado; autenticação stateless.      |
| Autenticação Básica | Usuário e senha em Base64 + HTTPS.           |
| OpenID Connect      | Extensão do OAuth 2.0 para login federado.   |

### JWT (JSON Web Token)

Token compacto que carrega informações do usuário e permissões. Muito usado em APIs REST por permitir autenticação sem estado (stateless).


![jwt-token](images/jwt-token.png)
<br>
*Fonte: [https://jwt.io/](https://jwt.io/)*

* Token composto por três partes: cabeçalho, payload (dados/claims) e assinatura.
* Enviado em `Authorization: Bearer <token>`.
* Assinado digitalmente; tokens expiram.

Fluxo básico:

![jwt-flow](images/jwt-flow.png)
<br>
*Fonte: [https://roadmap.sh/guides/jwt-authentication](https://roadmap.sh/guides/jwt-authentication)*

1. Cliente faz login enviando credenciais.

2. Servidor valida e gera um JWT.

3. Cliente armazena o JWT e envia nas próximas requisições.

4. Servidor valida o JWT a cada requisição protegida.

---

# Padrões de Integração

## Polling

No polling, o cliente faz requisições periódicas para a API para saber se há dados novos ou atualizações. Esse método é simples, mas pode sobrecarregar o servidor com requisições desnecessárias, principalmente se a frequência for alta. É mais adequado quando a atualização em tempo real não é essencial ou quando o servidor não suporta notificações automáticas.

---

## Webhooks

Webhooks são uma solução baseada em **pushing**. Nesse modelo, o servidor envia uma requisição HTTP automaticamente para um endpoint do cliente assim que um evento ocorre. O cliente precisa disponibilizar uma URL pública para receber essas notificações. Esse mecanismo reduz o tráfego desnecessário, fornece atualizações em tempo real e é ideal para integrações que precisam de notificações automáticas, como sistemas de pagamento ou notificações de status.

---

## Pushing

O termo **pushing**, no contexto de APIs, se refere ao servidor enviando informações de forma ativa para o cliente, sem que o cliente precise perguntar repetidamente. Webhooks são a forma mais comum de implementar pushing em APIs HTTP.

---

![webhook-vs-polling](images/webhookpolling.gif)
<br>
*Fonte: [https://x.com/LevelUpCoding\_/status/1809118819966988592](https://x.com/LevelUpCoding_/status/1809118819966988592)*

---

## Exemplo Prático

Imagine um sistema de e-commerce que deseja saber quando o pagamento de um pedido foi aprovado:

- **Polling:** O sistema de e-commerce faz requisições constantes para a API do gateway de pagamentos perguntando se o pagamento do pedido foi aprovado.

- **Webhook (Pushing):** Assim que o pagamento é aprovado, o gateway de pagamentos faz uma requisição HTTP (POST) automaticamente para uma URL do sistema de e-commerce, enviando os dados da aprovação. Não é necessário que o sistema de e-commerce faça verificações repetidas.

---

## Resumo das diferenças

- **Polling:** O cliente pergunta várias vezes ao servidor por novidades.
- **Pushing (Webhooks):** O servidor avisa o cliente automaticamente quando há novidades.

A escolha entre polling e pushing (webhooks) depende da necessidade de atualização em tempo real, da infraestrutura e dos requisitos do sistema.


---
