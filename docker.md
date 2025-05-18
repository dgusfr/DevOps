# Contêineres e Docker

Contêineres são uma tecnologia de virtualização que permite empacotar uma aplicação e todas as suas dependências em uma unidade isolada, garantindo que ela funcione de maneira consistente em diferentes ambientes.

Isso resolve problemas comuns de incompatibilidade entre sistemas operacionais, bibliotecas ou configurações de ambiente.

O **Docker** é uma plataforma que utiliza contêineres para facilitar o desenvolvimento, implantação e execução de aplicações. Ele simplifica a criação, distribuição e gerenciamento de contêineres, proporcionando agilidade e consistência em todo o ciclo de vida do software.

![ambientes-dev-test-prod](images/ambientes-dev-test-prod.png)

---

## O Processo de Desenvolvimento

O desenvolvimento de software segue um fluxo típico:

- **Identificação de Necessidades**: Definição dos requisitos funcionais e não funcionais da aplicação.
- **Especificação Técnica**: Documentação detalhada do que será desenvolvido.
- **Documentação e Desenvolvimento**: Implementação das funcionalidades seguindo as especificações.
- **Testes e Revisão**: Validação das funcionalidades para garantir qualidade.
- **Homologação e Produção**: Publicação da solução para que os usuários possam acessá-la.

![fluxo-processo-desenvolvimento](images/fluxo-processo-desenvolvimento.png)

Esse processo envolve transições entre diferentes ambientes (desenvolvimento, teste e produção), onde dependências e bibliotecas podem variar, gerando problemas de compatibilidade.

---

## Desafios na Transição Entre Ambientes

O principal desafio na transição entre ambientes é a **inconsistência causada por diferenças em**:

- Versões de bibliotecas.
- Configurações do sistema.
- Sistemas operacionais.

Um software pode funcionar no ambiente de desenvolvimento, mas falhar no de produção devido a essas diferenças.  
É essencial garantir que o ambiente seja **reproduzível e consistente** em todas as fases do ciclo de vida do software.

---

## Solução com Contêineres

Os contêineres encapsulam o software e suas dependências, garantindo que ele funcione da mesma forma em qualquer ambiente. Eles fornecem:

- **Isolamento**: Cada contêiner funciona de forma independente, como se fosse uma aplicação em sua própria máquina.
- **Eficiência**: Não é necessário executar sistemas operacionais completos, reduzindo o consumo de recursos.
- **Reprodutibilidade**: Asseguram que a aplicação tenha o mesmo comportamento em desenvolvimento, teste e produção.

---

## Diferenças Entre Máquinas Virtuais e Contêineres

### Máquinas Virtuais (VMs):

- Utilizam um **hypervisor** para criar ambientes virtuais.
- Cada VM executa um **sistema operacional completo**, consumindo mais recursos.
- Oferecem maior isolamento, mas são menos eficientes para execução de aplicações leves.

### Contêineres:

- Utilizam o **kernel do sistema operacional hospedeiro**, sem precisar de um hypervisor.
- Compartilham o sistema operacional, consumindo **menos recursos**.
- São mais **leves e rápidos**, ideais para escalar aplicações.

### Comparação Estrutural:

![comparacao-container-vm](images/comparacao-container-vm.png)

---

## Mecanismos de Isolamento dos Contêineres

Os contêineres utilizam **namespaces** e **cgroups** para isolar processos e gerenciar recursos.

### Namespaces

Isolam diferentes aspectos dos contêineres:

- **PID**: Isolamento dos processos.
- **NET**: Isolamento de redes e interfaces.
- **IPC**: Isolamento da comunicação entre processos.
- **MNT**: Isolamento do sistema de arquivos.
- **UTS**: Permite que o contêiner tenha seu próprio nome de host.

### Cgroups

Controlam o uso de recursos (CPU, memória, I/O) de cada contêiner.

Esses mecanismos permitem que os contêineres funcionem como **processos isolados dentro de uma máquina**, proporcionando **segurança e eficiência**.
