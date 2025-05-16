**Proxy de Encaminhamento**

Um **proxy de encaminhamento** (em inglês, **forward proxy**) é um tipo de servidor intermediário que atua entre usuários finais (clientes) e a internet. O principal objetivo desse tipo de proxy é encaminhar solicitações feitas pelos usuários a servidores externos, como sites, aplicativos web ou outros recursos na internet.

### Como Funciona o Proxy de Encaminhamento

Quando um usuário deseja acessar um site, ao invés de se conectar diretamente ao servidor desse site, ele envia sua solicitação para o **proxy de encaminhamento**. O proxy então repassa essa solicitação ao destino desejado. Quando o servidor responde, o proxy recebe essa resposta e a entrega ao usuário.

Esse processo ocorre de forma transparente para o servidor de destino, que “enxerga” apenas o proxy, e não o usuário original.

#### Esquema Simplificado:

![alt text](images/image4.png)

### Finalidades do Proxy de Encaminhamento

* **Privacidade**: O servidor externo vê apenas o endereço IP do proxy, não do usuário, ajudando a ocultar a identidade do cliente.
* **Controle de acesso**: Empresas e organizações podem usar proxies para restringir o acesso a determinados sites ou serviços na internet, bloqueando conteúdos indesejados.
* **Cache de conteúdo**: Proxies podem armazenar páginas e arquivos acessados com frequência, agilizando o carregamento para vários usuários.
* **Segurança**: O proxy pode filtrar tráfego, bloquear ameaças e impedir que softwares maliciosos atinjam os dispositivos dos usuários.
* **Monitoramento**: Permite registrar e analisar o tráfego dos usuários, importante para auditoria e conformidade.

### Exemplo Prático

Em uma empresa, todos os computadores estão configurados para acessar a internet somente por meio de um proxy de encaminhamento. Quando um colaborador tenta acessar um site, o pedido passa pelo proxy, que pode bloquear páginas proibidas (como redes sociais) ou liberar o acesso se estiver de acordo com as políticas internas.


### Resumindo

O **proxy de encaminhamento** é um servidor intermediário voltado para proteger, filtrar e controlar o acesso dos clientes à internet, tornando a navegação mais segura, eficiente e conforme as políticas da organização. Ele é um dos principais componentes de segurança e gerenciamento de redes modernas.

