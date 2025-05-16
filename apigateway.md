# API Gateway

O **API Gateway** é um componente fundamental em arquiteturas modernas, especialmente em aplicações baseadas em microsserviços. Ele atua como um ponto de entrada único para todas as requisições externas que precisam acessar múltiplas APIs ou serviços dentro de um sistema.

## O que é um API Gateway?

Um **API Gateway** é um servidor que recebe, gerencia, autentica, roteia e processa todas as solicitações de clientes (aplicações web, mobile, sistemas de terceiros etc.) para as APIs internas de uma organização. 

Em vez de o cliente se comunicar diretamente com cada serviço individual, todas as requisições passam pelo API Gateway, que centraliza o controle e oferece funcionalidades extras.

### Esquema Simplificado:

```
Cliente (App/Web) → API Gateway → Diversos Serviços Internos (APIs)
```

## Principais Funções do API Gateway

1. **Roteamento de Requisições**

   * Direciona cada solicitação recebida para o serviço interno correto, com base na URL, no método ou em regras de negócio.

2. **Autenticação e Autorização**

   * Verifica se o cliente possui permissão para acessar o recurso solicitado, podendo exigir tokens (como JWT), chaves de API, ou integração com sistemas de login.

3. **Agregação de Respostas**

   * Junta dados de múltiplos serviços internos em uma única resposta, facilitando para o cliente que receberá tudo de uma só vez.

4. **Rate Limiting e Throttling**

   * Limita o número de requisições que um usuário pode fazer em determinado período, protegendo a infraestrutura de sobrecarga e abusos.

5. **Cache de Requisições**

   * Armazena temporariamente respostas para evitar consultas repetidas aos serviços internos, melhorando a performance.

6. **Transformação de Dados**

   * Pode modificar ou adaptar dados das requisições e respostas para garantir compatibilidade entre clientes e serviços internos (ex: converter formatos de dados).

7. **Monitoramento e Logging**

   * Centraliza o registro de todas as requisições e respostas, facilitando auditoria, monitoramento e diagnóstico de problemas.

8. **Segurança**

   * Atua como uma camada extra de proteção, filtrando tráfego malicioso, aplicando regras de segurança e criptografando dados.

## Exemplo Prático

Imagine um aplicativo de e-commerce. Ao fazer login, buscar produtos ou finalizar uma compra, o aplicativo se comunica apenas com o API Gateway. O gateway, então, encaminha cada solicitação para o serviço interno correto: autenticação, catálogo de produtos, pagamento, etc. O cliente nunca acessa diretamente esses serviços.

```
App Mobile/Web
     │
     ▼
API Gateway
     ├─► Serviço de Autenticação
     ├─► Serviço de Produtos
     ├─► Serviço de Pagamento
     └─► Serviço de Entregas
```

## Diferença Entre API Gateway e Proxy Reverso

* **Proxy Reverso:** Faz apenas o encaminhamento de requisições externas para servidores internos, com funções como balanceamento de carga e cache, mas não entende o contexto das APIs.
* **API Gateway:** Vai além do proxy reverso, oferecendo funcionalidades específicas para APIs, como autenticação, agregação de respostas, limitação de requisições, transformação de dados, entre outros.

## Arquitetura de uma API Gateway

![alt text](images/image6.png)

A imagem a cima ilustra como um API Gateway gerencia as requisições que chegam de diversos clientes (aplicações web, mobile, PCs) até os microsserviços internos de uma aplicação.

### **Fluxo Passo a Passo do API Gateway**

1. **O Cliente faz uma requisição HTTP**
   Aplicações Web, Mobile ou PC enviam solicitações para o sistema, como `/cadastro`.

2. **Validação de Parâmetros (Parameter Validation)**
   O API Gateway verifica se os parâmetros da requisição são válidos (formato, obrigatoriedade etc.).

3. **Lista de Permissões/Bloqueios (Allow-list/Deny-list)**
   Verifica se o cliente tem permissão ou está bloqueado para acessar o recurso.

4. **Autenticação e Autorização (Authentication/Authorization)**
   Confirma a identidade do cliente e verifica se ele tem permissão para a ação solicitada.

5. **Limitação de Taxa (Rate Limit)**
   Controla o número de requisições permitidas por cliente em um período de tempo.

6. **Roteamento Dinâmico (Dynamic Routing)**
   Encaminha a requisição ao microsserviço correto com base na URL ou regras internas.

7. **Descoberta de Serviços (Service Discovery)**
   Identifica dinamicamente onde os microsserviços estão executando na rede.

8. **Conversão de Protocolo (Protocol Conversion)**
   Converte o protocolo da requisição, se necessário, para compatibilidade com o serviço interno.

9. **Tratamento de Erros (Error Handling)**
   Garante que falhas sejam tratadas de forma controlada e com retorno adequado ao cliente.

10. **Circuit Break (Interruptor de Circuito)**
    Desativa temporariamente o envio de requisições para serviços que estão falhando repetidamente.

11. **Registro e Monitoramento (Logging/Monitoring)**
    Armazena logs e métricas das requisições para análise e auditoria.

12. **Cache**
    Respostas a requisições frequentes são armazenadas temporariamente para agilizar futuras chamadas.

## Desvantagens

Uma desvantagem importante do uso de um **API Gateway** é que ele pode se tornar um **ponto único de falha** na arquitetura do sistema. Isso significa que, se o servidor responsável pelo gateway apresentar lentidão, instabilidade ou falhar completamente, toda a aplicação pode ser afetada, já que todas as requisições passam por ele. 

Como o API Gateway centraliza o tráfego e a lógica de encaminhamento das requisições para os microsserviços, qualquer sobrecarga ou falha nesse ponto pode resultar em **downtime generalizado** da aplicação. Por isso, é fundamental monitorar constantemente o desempenho do gateway e adotar estratégias de alta disponibilidade para garantir a resiliência da infraestrutura.


## Resumindo

O **API Gateway** é o responsável por gerenciar e proteger o tráfego de dados entre clientes e os serviços internos de uma aplicação. Ele proporciona segurança, organização, escalabilidade e simplifica a comunicação em ambientes complexos, sendo indispensável em arquiteturas modernas como microservices.

