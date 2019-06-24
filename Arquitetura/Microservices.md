# Microservices

*However, if you always remember that the most important idea behind microservices is the need to decompose your application into smaller logical pieces, you are already halfway there*.

- Os microsserviços se comunicam por meio de protocolos síncronos, como o HTTP, sobre os quais eles geralmente expõem APIs RESTful (REpresentational State Transfer) ou por meio de protocolos assíncronos, como o AMQP (Advanced Message Queuing Protocol. Esses protocolos são simples, bem compreendidos pela maioria dos desenvolvedores e não estão vinculados a nenhuma linguagem de programação específica. Cada microsserviço pode ser escrito na linguagem mais apropriada para implementar esse microsserviço específico.

- Como cada microsserviço é um processo independente com uma API externa relativamente estática, é possível desenvolver e implantar cada microsserviço separadamente. Uma alteração em um deles não exige alterações ou reimplementação de nenhum outro serviço, desde que a API não seja alterada ou alterada apenas de maneira compatível com versões anteriores.

## What are Microservices?

Microservices are maybe a good approach to solve problems; in the last few years, companies such as Netflix, PayPal, eBay, Amazon, and Spotify have chosen to use microservices in their own development teams because they believed them to be the best solution for their projects. To understand why they chose microservices and understand the kinds of projects you should use them in, it is necessary to know what a microservice is.

Firstly, it is essential to understand what a monolithic application is, but basically, we can define a microservice as an extended Service Oriented Architecture. In other words, it is a way to develop an application by following the required steps to turn it into various little services. Each service will execute itself and communicate with others through requests, usually using APIs on HTTP.

It is important to differentiate between MVC and microservices. MVC is a design pattern and microservices are a way to develop an application;

A microservice is a simple, isolated entity with a concrete proposal. It is independent and works with the rest of the microservices by communicating through an agreed channel.

Another advantage of using microservices is that they are agnostic to the language; in other words, it is possible to use different languages for each microservice. A microservice written in PHP can talk to others written in Python or Ruby because they only give the API to the rest of the microservices, so they just have to share the same interface to communicate with each other.

---------------------

A base da arquitetura de microsserviços é sobre o desenvolvimento de um único aplicativo como um conjunto de serviços pequenos e independentes que estão sendo executados em seus próprios processos, desenvolvidos e implantados de forma independente.

Cada microsserviço na camada de microsserviços oferece uma capacidade de negócios bem definida (preferencialmente com um escopo pequeno), que são projetados, desenvolvidos, implantados e administrados de forma independente.

### Business Capability Oriented

Um dos conceitos-chave da arquitetura de microsserviços é que seu serviço deve ser projetado com base nos recursos de negócios, de modo que um determinado serviço atenda a um propósito comercial específico e tenha um conjunto bem definido de responsabilidades. Um determinado serviço deve se concentrar em fazer apenas uma coisa e fazê-lo bem.

É importante entender que um serviço de baixa granularidade (como um serviço da Web desenvolvido no contexto SOA) ou um serviço refinado (que não é mapeado para um recurso comercial) não é adequado à arquitetura de microsserviços. Em vez disso, o serviço deve ser dimensionado exclusivamente com base no escopo e na funcionalidade comercial. Além disso, lembre-se de que tornar os serviços muito pequenos (isto é, orientados para recursos de baixa granularidade que mapeiam para recursos de negócios) é considerado um antipadrão.

O tamanho do serviço nunca é determinado com base no número de linhas de código ou o número de pessoas que trabalham nesse serviço. The concepts such as Single Responsibility Principle (SRP), Conway's law, Twelve Factor App, Domain Driven Design (DDD), entre outros, são úteis para identificar e projetar o escopo e as funcionalidades de um microsserviço.

### Autonomous: Develop, Deploy, and Scale Independently

Ter serviços autônomos poderia ser a força motriz mais importante por trás da realização da arquitetura de microsserviços. Os microsserviços são desenvolvidos, implantados e escalados como entidades independentes. Ao contrário dos serviços da Web ou de uma arquitetura de aplicativo monolítica, os serviços não compartilham o mesmo tempo de execução. Em vez disso, eles são implantados como tempos de execução isolados, aproveitando tecnologias como contêineres. A adaptação bem sucedida e crescente de contêineres e tecnologias de gerenciamento de contêineres, como Docker, Kubernetes e Mesos, é vital para a realização da autonomia de serviço e contribui para o sucesso da arquitetura de microsserviços como um todo.

Os serviços autônomos garantem a resiliência de todo o sistema, pois isolamos as falhas e isolamos os serviços. Esses serviços são fracamente acoplados por meio de mensagens via comunicação entre serviços pela rede. A comunicação entre serviços pode ser construída sobre vários estilos de interação e formatos de mensagem. Eles expõem suas APIs por meio de contratos de serviços independentes de tecnologia e os consumidores podem usar esses contratos para colaborar com esse serviço. Esses serviços também podem ser expostos como APIs gerenciadas por meio de um _API gateway_.

A implantação independente de serviços fornece a capacidade inata de dimensionar serviços de forma independente. Como o consumo de funcionalidades de negócios varia, podemos dimensionar os microsserviços que obtêm mais tráfego sem dimensionar outros serviços.


### Microservices characteristics

Next, we will look at the key elements of a microservice architecture:

* **Ready to fail**: Microservices are designed to fail. In a web application, microservices communicate with each other and if one of them fails, the rest should work to avoid a cascading failure. Basically, microservices attempt to avoid communicating synchronously using async calls, queues, systems based on actors, and streams instead. This topic will be discussed in the subsequent chapters.

* **Unix philosophy**: Microservices should follow the Unix philosophy. Each microservice must be designed to do one thing only, and should only be small and independent. This allows us as developers to adjust and deploy each microservice independently. The Unix philosophy emphasizes building simple, short, clear, modular, and extensible code that can be easily maintained and repurposed by developers as well, in addition to its creators.

* **Communication layer**: Each microservice communicates with the others through HTTP requests and messages, executing the business logic, querying the database, exchanging messages with the required systems and, at the end, returning a JSON (or HTML/XML) response.

* **Scalability**: The main reason to choose a microservice architecture is that it is possible to scale the application easily. The bigger an application is and the more traffic the application has, the more sense the proper selection of choosing microservices makes. A microservice can scale the required part without any impact on the rest of the application.

### How to focus your development on microservices

#### Disadvantages of microservices

Next, we will look at the disadvantages of the microservices architecture. When asking developers about this, they agree that the major problem with microservices is the debugging on the production server.

Debugging an application based on microservices can be a little tedious when you have hundreds of microservices in your application and you have to find where a problem is; you need to spend time looking for the microservice that is giving you the error. Each microservice works like an individual machine, so to look at a specific log you have to go to the specific microservice. Fortunately, there are some tools to help us out with this subject by getting the logs from all the different microservices in your application and putting them together into a single location. In the subsequent chapters, we will look at these kinds of tools.

Another disadvantage is that it is necessary to maintain every single microservice as an entire server; in other words, every single microservice can have one or more databases, logs, different services, or library versions, and even the code can be in a different language, so if it is difficult to maintain a single server and doing it with hundreds will waste money and time.

Also, the communication between microservices is very important---they have to work like clocks, so communication is essential for the application. To do this, the communication between the development teams will be necessary to tell each other what they need and also to write good documentation for each microservice.

A good practice to work with microservices is having everything automatized or at least everything possible. Maybe the most important part is the deployment. If it is necessary to deploy hundreds of microservices, it can be difficult. So, the best way is to automatize these kinds of tasks.

#### Always create small logical black boxes

As a developer, you always start with the big picture of what you will build. Try to decompose the big picture into small logical blocks that only do one thing. Once the multiple small pieces are ready, you can start building complex systems, ensuring that the foundations of your application are solid.

Each of your microservices is like a black box with a public interface, which is the only way to interact with your software. The main recommendation you need to have always in mind is to build a very stable API. You can change the implementation of an API call without many problems, but if you change the way of calling or even the response of this call, you will be in big trouble. In the case of deep changes to your API, ensure that you use some kind of versioning so that you can support the old and new versions.

#### Always think about scalability

One of the main advantages of a microservice application is that each service can be scaled up or down. This flexibility can be achieved by reducing the number of stateful services to the minimum. A stateful service relies on data persistence, making it difficult to move or share the data without having data consistency problems.

Using autodiscovery and autoregistry techniques, you can build a system that knows which one will deal with each request at all times.

#### Use a lightweight communication protocol

Nobody likes to wait, not even your microservices. Don't try to reinvent the wheel or use an obscure but cool communication protocol, use HTTP and REST. They are known by all web developers, they are fast, reliable, easy to implement, and very easy to debug. If you need to increase the security, implement SSL/TSL.

#### Use queues to reduce a service load or make async executions

As a developer, you want to make your system as fast as possible. Therefore, it makes no sense to increase the execution time of an API call just because it is waiting for some action that can be done in the background. In these cases, the best approach is the use of queues and job runners in charge of this background processing.

Imagine that you have a notification system that sends an e-mail to a customer when placing an order on your microservice e-commerce. Do you think that the customer wants to wait to see the payment successful page only because the system is trying to send an e-mail? In this case, a better approach is to enqueue the message so that the customer will have a pretty instant thank you page. Later, a job runner will pick up the queued notification and the e-mail will be sent to the customer.

#### Each service is different, so keep different repositories and build environments

We are decomposing an application into small parts which we want to scale and deploy independently, so it makes sense to keep the source in different repositories. Having different build environments and repositories means that each microservice has its own lifecycle and can be deployed without affecting the rest of your application.

### Failure Tolerance

Os microsserviços são mais propensos a falhas devido à proliferação dos serviços e suas comunicações de rede entre serviços . Um determinado aplicativo de microsserviço é uma coleção de serviços refinados e, portanto, uma falha de um ou mais desses serviços não deve derrubar todo o aplicativo. Portanto, devemos tratar de forma graciosa uma determinada falha de um microsserviço para que tenha um impacto mínimo nas funcionalidades de negócios do aplicativo. Projetar microsserviços de forma tolerável a falhas requer a adaptação das tecnologias necessárias nas fases de projeto, desenvolvimento e implantação.

O outro aspecto da tolerância a falhas é a capacidade de observar o comportamento dos microsserviços que você executa na produção. Detectar ou prever falhas em um serviço e restaurar esses serviços é muito importante.

### Decentralized Data Management

Em uma arquitetura monolítica, o aplicativo armazena dados em bancos de dados lógicos únicos e centralizados para implementar várias funcionalidades/capacidades do aplicativo. Em uma arquitetura de microsserviços, as funcionalidades estão dispersas em vários microsserviços. Se usarmos o mesmo banco de dados centralizado, os microsserviços não serão mais independentes uns dos outros (por exemplo, se o esquema do banco de dados for alterado por um microsserviço, isso quebrará vários outros serviços). Portanto, cada microsserviço deve ter seu próprio banco de dados e esquema de banco de dados.

Cada microsserviço pode ter um banco de dados privado para persistir os dados que exigem a implementação da funcionalidade comercial oferecida por ele. Um determinado microsserviço só pode acessar o banco de dados privado dedicado e não os bancos de dados de outros microsserviços.

Em alguns cenários de negócios, talvez seja necessário atualizar vários bancos de dados para uma única transação. Nesses cenários, os bancos de dados de outros microsserviços devem ser atualizados somente por meio da API de serviço correspondente (eles não têm permissão para acessar o banco de dados diretamente).

O gerenciamento de dados descentralizado oferece a você os microsserviços totalmente dissociados e a liberdade de escolher técnicas de gerenciamento de dados díspares (SQL ou NoSQL etc., sistemas de gerenciamento de banco de dados diferentes para cada serviço).

### Microservices: Benefits and Liabilities

Como acontece com qualquer arquitetura ou tecnologia, existem benefícios e responsabilidades com a arquitetura de microsserviços. Como você tem uma boa compreensão das principais características dos microsserviços, este é um bom momento para falar sobre eles. Vamos começar com os benefícios dos microservices.

#### Benefícios

Uma das principais razões para a popularidade da arquitetura de microsserviços são os benefícios que ela oferece em relação aos padrões de arquitetura de software convencionais. Vamos dar uma olhada mais de perto nos principais benefícios da arquitetura de microsserviços.

##### Desenvolvimento Ágil e Rápido de Funcionalidades Empresariais

A arquitetura de microsserviços favorece o desenvolvimento de serviços autônomos, o que nos ajuda no desenvolvimento ágil e rápido de funcionalidades de negócios. Em uma arquitetura convencional, a conversão de uma funcionalidade de negócios para uma funcionalidade de aplicativo de software pronta para produção leva muitos ciclos, principalmente devido ao tamanho do sistema, à base de código e às dependências. Com o desenvolvimento de serviços autônomos, você só precisa se concentrar na interface e na funcionalidade do serviço (não na funcionalidade de todo o sistema, que é muito mais complexa), pois todos os outros serviços só se comunicam através das chamadas de rede por meio de interfaces de serviço.

##### Replacibilidade

Devido à sua natureza autônoma, os microservices também são substituíveis. Como estamos construindo serviços como uma entidade independente, que está se comunicando por meio de chamadas de rede e APIs definidas, podemos facilmente substituir essa funcionalidade por outra melhor implementação. Estar focado em uma funcionalidade específica, ter um escopo e tamanho limitados e implantá-lo em um tempo de execução independente, tudo facilita muito a criação de um serviço substituível.

##### Isolamento de falhas e previsibilidade

A capacidade de substituição também nos ajuda a obter isolamento e previsão de falhas. Como discutido, um aplicativo baseado em microsserviços não pode explodir como um aplicativo monolítico convencional devido à falha de qualquer componente ou serviço. Ter os recursos adequados de observabilidade também nos ajuda a identificar ou prever possíveis falhas.

##### Implantação ágil e escalabilidade

Facilidade de implantação e dimensionamento pode muito bem ser a proposição de valor mais importante dos microsserviços. Com as modernas infraestruturas nativas de contêiner baseadas em nuvem, a capacidade de implantar um serviço facilmente e escaloná-lo dinamicamente está se tornando trivial. Como estamos desenvolvendo recursos como serviços autônomos, podemos aproveitar facilmente todas essas tecnologias nativas de contêiner e nuvem para facilitar a implantação e a escalabilidade ágil.

##### Alinhar com a estrutura organizacional

Como os microsserviços são orientados para os negócios, eles podem estar alinhados com a estrutura organizacional/de equipe. Muitas vezes, uma determinada organização é estruturada de maneira a fornecer os recursos de negócios. Portanto, a propriedade de cada serviço pode ser facilmente concedida às equipes que possuem a funcionalidade de negócios. Portanto, dado que um microsserviço se concentra na funcionalidade específica do negócio, você pode escolher uma equipe relativamente pequena para possuir esse microsserviço. Isso tem um impacto positivo na equipe de desenvolvimento, porque o escopo de um determinado serviço é simples e bem definido. Dessa forma, a equipe pode ter todo o ciclo de vida do serviço.

#### Responsabilidades

A maior parte das responsabilidades da arquitetura de microsserviços deve-se principalmente à proliferação de serviços com os quais você precisa lidar.

##### Comunicação entre serviços

A complexidade da comunicação entre serviços poderia ser mais desafiadora do que a implementação dos serviços reais. Como discutido anteriormente, os endpoints inteligentes e o conceito de dumb pipes nos obrigam a ter uma lógica de comunicação entre serviços como parte de nossos microsserviços. Os desenvolvedores de serviços precisam gastar uma quantidade substancial de tempo em microsserviços de plumbing para criar uma funcionalidade de negócios composta.

##### Governança de Serviço

Ter um número de serviços transmitidos pela rede também complica o aspecto de governança e observabilidade dos serviços. Se você não tiver as ferramentas de governança e observabilidade adequadas, será um pesadelo identificar as dependências do serviço e detectar falhas. Por exemplo, o gerenciamento do ciclo de vida do serviço, o teste, a descoberta, o monitoramento, a qualidade do serviço e vários outros recursos de governança de serviços se tornarão mais complexos com uma arquitetura de microsserviços.

##### Depende fortemente de metodologias de implantação

O sucesso de implantar e dimensionar microsserviços depende muito da adoção de contêineres e sistemas de orquestração de contêineres. Se você não tiver essa infraestrutura, precisará investir tempo e energia nela (e nem sequer pensar em ter uma arquitetura de microsserviços bem-sucedida sem contêineres). Por fim, uma arquitetura de microsserviços bem-sucedida também depende das equipes e das pessoas. A propriedade dos serviços, pensando em tornar o serviço leve e nativo para contêineres, não tendo um ponto central para integrar serviços, etc., requer mudanças na cultura de engenharia no nível da organização.

##### Complexidade de dados distribuídos e gerenciamento de transações

Como uma arquitetura de microsservos promove a localização dos dados para um determinado serviço, o gerenciamento de dados distribuídos será bastante assustador. O mesmo se aplica às transações distribuídas. A implementação de limites de transação que abrangem vários microsserviços será bastante desafiadora.

### Como e quando usar microsserviços

Discutimos como a arquitetura de microsserviços evoluiu a partir da arquitetura corporativa convencional centralizada, cobrimos as principais características dela e discutimos os prós e os contras de usá-la. No entanto, precisamos ter um conjunto sólido de diretrizes sobre quando usar a arquitetura de microsserviços e quando evitá-la.

- A arquitetura de microsserviços é ideal quando a arquitetura corporativa atual requer modularidade.
- Se o problema de negócios que você está tentando resolver com seu aplicativo de software é bastante simples, talvez você não precise de microsserviços (ter um aplicativo da Web monolítico simples e um banco de dados geralmente é suficiente).
- Seu aplicativo de software precisa adotar a implantação baseada em contêiner.
- Se o seu sistema for complexo demais para ser segregado em microsserviços, você deverá identificar as áreas nas quais poderá introduzir microsserviços com impacto mínimo. Em seguida, comece a implementar um pequeno caso de uso em microsserviços e crie os componentes do ecossistema necessários em torno disso.
- Compreender os recursos de negócios é bastante crítico para projetar microsserviços. Noções básicas sobre técnicas de projeto de microsserviços, são essenciais antes da implementação do serviço.

## Designing Microservices

### Domain-driven design

DDD é um conjunto de práticas com o objetivo de construir um software de qualidade, que atenda as necessidades do cliente. Junto dessas práticas, temos a importância de trabalharmos próximos aos domain experts (pessoas da área de negócio que possam compartilhar conhecimento com os devs), entendendo a área de negócio do cliente e quebrando o problema em problemas menores. Nesse processo, identificamos o core business do cliente e sub-áreas de negócio de menor importância, e podemos dar maior foco ao core business. Foco no que importa mais para cliente -> Entregamos mais valor -> Melhor retorno sobre o investimento para o cliente! No DDD, o foco está no domínio, que é a área de conhecimento para a qual você está criando uma solução, e na abstração deste domínio em 1 ou mais modelos, que resolvem problemas daquele domínio.

Domain-driven design (DDD from here on) is an approach for the development when it has complex needs. This concept is not new; it was created by Eric Evans in his book with the same title in 2004, but now it is mainstream as microservices are popular among developers and very common in huge projects.

This is happening as there is great compatibility between the microservices concepts (regarding the software architecture, dividing every functionality into services) and DDD concepts (about the bounded contexts).

Before knowing where and how we can use DDD in our microservices project, it is necessary to understand what DDD is and how it works, so let me introduce you to the main concepts as a summary of this approach.

O domain-driven design (DDD) não é um novo conceito introduzido com microsserviços e já existe há algum tempo. Eric Evans, em seu livro, Domain-Driven Design: abordando a complexidade no coração do software, criou o termo Domain-Driven Design. Como os microsserviços se tornaram um padrão de arquitetura mainstream, as pessoas começaram a perceber a aplicabilidade dos conceitos de domain-driven design na criação de microsserviços. Esse design desempenha um papel fundamental no escopo de microsserviços.

O que é domain-driven design? Trata-se principalmente de modelar lógica de negócios complexa ou construir uma abstração sobre lógica de negócios complexa. O domínio está no coração de domain-driven design. Todos os softwares que desenvolvemos estão relacionados a alguma atividade ou interesse do usuário. Eric Evans diz que a área de assunto para a qual o usuário aplica o programa é o domínio do software. Alguns domínios envolvem o mundo físico. No negócio de varejo, você encontra compradores, vendedores, fornecedores, parceiros e muitas outras entidades. Alguns domínios são intangíveis. Por exemplo, no domínio de criptografia, um aplicativo de carteira Bitcoin lida com ativos intangíveis. Seja o que for, o domínio está relacionado ao negócio, não ao software. É claro, o software pode ser um domínio em si quando você cria um software para trabalhar no domínio do software, por exemplo, um programa de gerenciamento de configuração.

Ao longo deste livro, usaremos muitos exemplos para elaborar esses conceitos. Digamos que temos um varejista corporativo que esteja criando um aplicativo de comércio eletrônico. O varejista tem quatro departamentos principais: gerenciamento de estoque e pedidos, gerenciamento de clientes, entrega e faturamento e finanças. Cada departamento pode ter várias seções. A seção de processamento de pedidos do departamento de estoque e gerenciamento de pedidos aceita um pedido, bloqueia os itens no estoque e, em seguida, passa o controle para faturamento e finanças para trabalhar no pagamento. Depois que o pagamento é processado com sucesso, o departamento de entrega torna o pedido pronto para entrega. O departamento de gerenciamento de clientes assume a propriedade de gerenciar todos os dados pessoais do cliente e todas as interações com o cliente.

![Divide and conquer](./Images/Microservices/Divide%20and%20conquer.png "Divide and conquer")

Um princípio fundamental por trás do domain-driven design é dividir e conquistar. O domínio de varejo é o principal domínio de negócios em nosso exemplo. Cada departamento pode ser tratado como um subdomínio. Identificar o domínio principal do negócio e os subdomínios relacionados é extremamente importante. Isso nos ajuda a criar um aplicativo de e-commerce para nosso varejista seguindo os princípios de arquitetura de microsserviços. Um dos principais desafios que muitos arquitetos enfrentam na criação de uma arquitetura de microsserviços é obter o nível certo de granularidade para cada serviço. O domain-driven design ajuda aqui. Como o nome indica, sob domain-driven design, o domínio é o rei!

Vamos dar um passo atrás e olhar a lei de Conway. Ele diz que qualquer organização que projete um sistema produzirá um design cuja estrutura seja uma cópia da estrutura de comunicação da organização. Isso justifica a identificação de subdomínios em uma empresa em termos de departamentos. Um determinado departamento é formado para um propósito. Um departamento possui estrutura própria de comunicação interna, bem como uma estrutura de comunicação entre os departamentos. Mesmo um determinado departamento pode ter várias seções, onde podemos identificar cada seção como um subdomínio.

Vamos ver como podemos mapear essa estrutura de domínio para a arquitetura de microsserviços. Possivelmente, podemos começar a criar nosso aplicativo de e-commerce com quatro microsserviços (consulte a Figura abaixo): Processamento de Pedidos, Cliente, Entrega e Faturamento.

![Communication between microservices](./Images/Microservices/Communication%20between%20microservices.png "Communication between microservices")

Suponha que esses microsserviços se comuniquem entre si por meio de mensagens, que são enviadas como eventos. A solicitação primeiro atinge o microsserviço Processamento de Pedidos e, depois de bloquear os itens no inventário, aciona o evento `ORDER_PROCESSING_COMPLETED`. Eventos são uma maneira de se comunicar entre microsserviços. Pode haver vários outros serviços ouvindo o evento `ORDER_PROCESSING_COMPLETED` e, assim que eles forem notificados, comece a agir de acordo. Conforme a Figura,o microsserviço de Faturamento recebe o evento `ORDER_PROCESSING_COMPLETED` e inicia o processamento do pagamento. A Amazon, por exemplo, não processa pagamentos no momento em que um pedido é feito, mas apenas quando está pronto para envio. Assim como a Amazon, o microsserviço Processamento de Pedidos acionará o evento `ORDER_PROCESSING_COMPLETED` somente quando o pedido estiver pronto para envio. Este evento contém os dados necessários para processar o pagamento pelo microsserviço de Faturamento. Nesse exemplo específico, ele contém o ID do cliente e o método de pagamento. O microsserviço de Faturamento armazena as opções de pagamento do cliente em seu próprio repositório, incluindo as informações do cartão de crédito, para que ele possa agora processar o pagamento de forma independente.

Depois que o microsserviço de Faturamento concluir o processamento do pagamento, ele disparará o evento `PAYMENT_PROCESSING_COMPLETED` e o microsserviço de Entrega o capturará. Este evento contém o ID do cliente, o ID do pedido e a fatura. Agora, o microsserviço de Entrega carrega o endereço de entrega do cliente de seu próprio repositório e prepara o pedido para entrega. Embora o microsserviço do Cliente seja mostrado na Figura, ele não está sendo usado durante o fluxo de processamento de pedidos. Quando novos clientes estão acessando no sistema ou clientes existentes quer atualizar seus dados pessoais, será utilizada a Microservice Cliente.

Cada microsserviço na Figura pertence a um domínio de negócios. O gerenciamento de estoque e pedido é o domínio do microsserviço Processamento de Pedidos; o gerenciamento de clientes é o domínio do microsserviço do Cliente; entrega é o domínio do microsserviço de Entrega; e faturamento e finanças é o domínio do microsserviço de Faturamento. Cada um desses domínios ou departamentos pode ter sua própria estrutura de comunicação internamente, juntamente com sua própria terminologia, para representar as atividades de negócios.

Cada domínio pode ser modelado de forma independente. Por mais que um possa ser independente dos outros, ele ganha mais flexibilidade para evoluir por conta própria. Domain-driven design define as melhores práticas e diretrizes sobre como você pode modelar um determinado domínio. Ele destaca a necessidade de ter uma linguagem onipresente para definir o modelo de domínio. A linguagem onipresente é uma linguagem de equipe compartilhada, que é compartilhada por especialistas em domínio e pelos desenvolvedores. De fato, onipresente significa que a mesma linguagem deve ser usada em todos os lugares dentro de um determinado contexto (ou, para ser preciso, dentro de um contexto delimitado), de conversas para o código. Isso preenche a lacuna na comunicação entre especialistas em domínio e desenvolvedores. Os especialistas de domínio são meticulosos com seus próprios jargões, mas têm pouca ou nenhuma compreensão dos termos técnicos usados ​​no desenvolvimento de software, enquanto os desenvolvedores sabem como descrever um sistema em termos técnicos, mas não possuem perícia de domínio limitada ou limitada. A linguagem onipresente preenche essa lacuna e traz todos para a mesma página.

A terminologia definida pela linguagem onipresente deve ser limitada pelo contexto correspondente. O contexto está relacionado a um domínio. Por exemplo, a linguagem onipresente pode ser usada para definir uma entidade chamada cliente. A definição da entidade do cliente no domínio de gerenciamento de estoque e pedido não precisa necessariamente ser a mesma do domínio de gerenciamento de clientes. Por exemplo, a entidade cliente no domínio de gerenciamento de estoque e pedido pode ter propriedades como histórico de pedidos, pedidos abertos e pedidos agendados, enquanto a entidade cliente no domínio de gerenciamento de clientes tem propriedades como nome, sobrenome, endereço residencial, endereço de e-mail, número de celular, etc. A entidade do cliente no domínio de faturamento e finanças pode ter propriedades como número do cartão de crédito, endereço de faturamento, histórico de faturamento e pagamentos programados. Qualquer termo definido por linguagem onipresente só deve ser interpretado sob o contexto correspondente.

Vamos nos aprofundar neste exemplo. Em nossa arquitetura, um determinado microsserviço pertence a um único domínio de negócios e a comunicação entre microsserviços acontece via passagem de mensagens. A passagem de mensagens pode se basear em uma arquitetura orientada a eventos ou apenas sobre HTTP. Cada mensagem de um microsserviço para outro transporta objetos de domínio.

Por exemplo, o evento `ORDER_PROCESSING_COMPLETED` transporta o objeto do domínio do pedido, enquanto o evento `PAYMENT_PROCESSING_COMPLETED` transporta o objeto do domínio da fatura. A definição desses objetos de domínio deve ser cuidadosamente derivada de domain-driven design com a colaboração entre especialistas e desenvolvedores de domínio.

_Domain-driven design tem seus próprios desafios inerentes. Um desafio é envolver especialistas de domínio no projeto durante sua execução. Também leva uma quantidade considerável de tempo para construir a linguagem onipresente, o que requer uma boa colaboração entre especialistas e desenvolvedores de domínio. Ao contrário do desenvolvimento de um aplicativo monolítico, que se expande em todos os domínios, criar uma solução para um determinado domínio e encapsular a lógica de negócios específica do domínio exige uma mudança na mentalidade do desenvolvedor, o que também é um desafio._

#### Bounded Context

Como discutimos, uma das partes mais desafiadoras de um projeto de microsserviços é a definição de um microsserviço. É aí que a SOA e suas implementações definiram mal o escopo. Na SOA, ao fazer um design, levamos em consideração toda a empresa. Não se preocupará com domínios de negócios individuais, mas sim com a empresa como um todo. Ele não se preocupará com gerenciamento de estoque e pedidos, faturamento e finanças, entrega e gerenciamento de clientes como domínios independentes separados - mas, ao contrário, tratará o sistema completo como um aplicativo de e-commerce corporativo.

A Figura abaixo ilustra a arquitetura em camadas de um aplicativo de comércio eletrônico feito por um arquiteto SOA. Para qualquer pessoa que tenha experiência em SOA, isso deve parecer bastante familiar. O que temos aqui é uma aplicação monolítica. Mesmo que a camada de serviço expõe algumas funcionalidades como serviços, nenhum é dissociado um do outro. O escopo dos serviços não foi feito com base no domínio comercial ao qual pertencem. Por exemplo, o serviço de processamento de pedidos também pode lidar com faturamento e entrega.

![A layered architecture of an e-commerce application](./Images/Microservices/A%20layered%20architecture%20of%20an%20e-commerce%20application.png "A layered architecture of an e-commerce application")

Conforme discutimos na seção anterior, o domain-driven design ajuda a projetar microsserviços. O escopo de um microsserviço é feito em torno de um contexto limitado. O contexto limitado está no centro do design de microsserviços. Eric Evans introduziu pela primeira vez o contexto limitado como um padrão de design em seu livro _Domain-Driven Design: Tackling Complexity in the Heart of Software._ A ideia é que qualquer domínio consiste de vários contextos limitados e cada contexto delimitado encapsula funcionalidades relacionadas em modelos de domínio e define pontos de integração para outros contextos limitados. Em outras palavras, cada contexto limitado possui uma interface explícita, onde define quais modelos compartilhar com outros contextos. Definindo explicitamente quais modelos devem ser compartilhados e não compartilhando a representação interna, podemos evitar as potenciais armadilhas que podem resultar em um alto acoplamento. Esses limites modulares são ótimos candidatos para microsserviços. Em geral, os microsserviços devem se alinhar de forma clara aos contextos limitados. Se os limites de serviço são alinhados aos contextos limitados do domínio correspondente e os microsserviços representam esses contextos limitados, o que é uma ótima indicação de que os microsserviços são fracamente acoplados e fortemente coesos.

_Um contexto limitado é um limite explícito dentro do qual existe um modelo de domínio. Dentro do limite, todos os termos e frases da linguagem onipresente têm um significado específico, e o modelo reflete a linguagem com exatidão._

Vamos estender nosso exemplo anterior com contextos limitados. Lá, identificamos quatro domínios: gerenciamento de estoque e pedidos, faturamento e finanças, entrega e gerenciamento de clientes. Cada microsserviço que projetamos anexado a um desses domínios. Mesmo que tenhamos um para um relação entre um microsserviço e um domínio, sabemos que um domínio pode ter mais de um contexto limitado, portanto, mais de um microsserviço. Por exemplo, se você escolher o domínio de gerenciamento de inventário e pedidos, temos o microsserviço Processamento de Pedidos, mas também podemos ter vários outros microsserviços, com base em diferentes contextos limitados (por exemplo, o microsserviço de Inventário). Para fazer isso, precisamos examinar mais de perto as principais funções fornecidas sob o domínio de gerenciamento de inventário e pedidos, e identificar os contextos limitados correspondentes.

_Recomenda-se que os contextos limitados mantenham sua separação, cada um com sua própria equipe, codebase e o esquema do banco de dados._

O departamento de estoque e gerenciamento de pedidos de uma empresa cuida de gerenciamento de estoques e garante que a demanda do cliente possa ser atendida com os estoques existentes. Também deve saber quando encomendar mais estoques de fornecedores para otimizar as vendas bem como as instalações de armazenamento. Sempre que recebe um novo pedido, ele precisa atualizar o estoque e bloquear os itens correspondentes para entrega. Depois que o pagamento é feito e confirmado pelo departamento de faturamento, o departamento de entrega deve localizar o item em seu depósito e disponibilizá-lo para retirada e entrega. Ao mesmo tempo, sempre que a quantidade disponível de um item na loja atingir algum valor limiar, o departamento de estoque e gerenciamento de pedidos deve entrar em contato com os fornecedores para obter mais e, uma vez recebido, deve atualizar o estoque.

Um dos principais destaques do domain-driven design é a colaboração entre especialistas em domínio e os desenvolvedores. A menos que você tenha uma compreensão adequada de como um departamento de gerenciamento de estoque funciona dentro de uma empresa, você nunca identificará os contextos limitados correspondentes. Com nossa compreensão limitada do gerenciamento de inventário, com base no que foi discutido anteriormente, podemos identificar os três contextos limitados a seguir.

- Processamento de pedidos: Esse contexto delimitado encapsula a funcionalidade relacionada ao processamento de um pedido, que inclui o bloqueio dos itens na ordem no estoque, o registro de pedidos em relação ao cliente, etc.
- Inventário: O inventário em si pode ser tratado como um contexto limitado. Isso cuida da atualização dos estoques ao receber itens de fornecedores e liberar para entrega.
- Gerenciamento de fornecedores: Esse contexto delimitado encapsula a funcionalidade relacionada ao gerenciamento de fornecedores. Ao liberar um item para entrega, o gerenciamento do fornecedor verifica se ele tem estoques suficientes no estoque e, caso contrário, notifica os fornecedores correspondentes.

A Figura abaixo ilustra vários microsserviços no domínio de gerenciamento de inventário e pedido, representando cada um dos contextos limitados. Aqui, os limites de serviço são alinhados aos contextos limitados do domínio correspondente. A comunicação entre contextos limitados acontece somente através da passagem de mensagens contra interfaces bem definidas. Conforme a Figur, o microsserviço Processamento de Pedidos atualiza primeiro o microsserviço de Inventário para bloquear os itens na ordem e, em seguida, aciona o evento `ORDER_PROCESSING_COMPLETED`. O microsserviço de Faturamento que atende ao evento `ORDER_PROCESSING_COMPLETED` executa o processamento de pagamento e dispara o evento `PAYMENT_PROCESSING_COMPLETED`. O microsserviço de Gerenciamento de Fornecedores que atende ao evento `PAYMENT_PROCESSING_COMPLETED` verifica se o número de itens nos estoques está acima do limite mínimo e se não notifica os fornecedores. O microsserviço de entrega que escuta o mesmo evento executa suas operações para pesquisar os itens (provavelmente enviando instruções para bots de armazenamento) e, em seguida, agrupa os itens para criar o pedido e torná-lo pronto para entrega. Feito isso, o microsserviço de Entrega acionará o evento `ORDER_DISPATCHED`, que notifica o microsserviço Processamento de Pedidos e atualizará o status do pedido.

![Communication between microservices](./Images/Microservices/Communication%20between%20microservices.png "Communication between microservices")

Um bom design irá dimensionar um microsserviço para um único contexto limitado. Qualquer microsserviço em expansão em vários contextos limitados se desvia dos objetivos originais. Quando temos um microsserviço, encapsulando a lógica de negócios por trás de uma interface bem definida e representando um contexto limitado, trazer novas alterações não terá impacto mínimo no sistema completo.

Conforme discutimos, a comunicação entre microsserviços pode acontecer por meio de eventos. Em domain-driven design, esses eventos são conhecidos como eventos de domínio. Eventos de domínio são acionados como resultado de alterações de estado em contextos limitados. Então, os outros contextos limitados podem responder a esses eventos de uma maneira fracamente acoplada. Os contextos delimitados que acionam eventos não precisam se preocupar com os comportamentos que devem ocorrer como resultado desses eventos e, ao mesmo tempo, contextos limitados, que lidam com esses eventos, não precisam se preocupar com a origem dos eventos. Os eventos de domínio podem ser usados ​​entre contextos limitados em um domínio ou entre domínios.

#### Relational Patterns

Domain-driven design levou a vários padrões que facilitam a comunicação entre vários contextos limitados. Os mesmos padrões são aplicáveis ​​ao projetar microsserviços, que estão bem alinhados com os contextos limitados. Esses padrões relacionais para contextos limitados foram introduzidos pela primeira vez por Eric Evans em seu livro Domain-Driven Design: Tackling Complexity in the Heart of Software.

##### Anti-Corruption Layer

Vamos revisitar o cenário que discutimos na seção anterior, em que a entidade do pedido tem duas definições diferentes nos contextos limitados Order Processing e Inventory. Para a comunicação entre esses dois contextos limitados, o contrato é definido pelo contexto limitado do Inventário. Quando o Processamento de Ordens atualiza o Inventário, ele precisa traduzir sua própria entidade de pedido para a entidade do pedido, o que é entendido pelo contexto limitado do Inventário. Usamos o padrão de camada anticorrupção (ACL) para resolver essa preocupação. O padrão de camada anticorrupção fornece uma camada de conversão.

Vamos ver como esse padrão é aplicável em uma implantação de microsserviços de nível corporativo. Imagine um cenário em que seu microsserviço precisa invocar um serviço exposto por um aplicativo monolítico. O design do aplicativo monolítico não se preocupa com o design orientado por domínio. A melhor maneira de facilitar a comunicação entre nosso microsserviço e a aplicação monolítica é por meio de uma camada anticorrupção. Isso ajuda a manter o design de microsserviço muito mais limpo (ou menos corrompido).

Existem várias maneiras de implementar a camada anticorrupção. Uma abordagem é construir a tradução no próprio microsserviço. Você usará qualquer linguagem usada para implementar o microsserviço para implementar a camada anticorrupção também. Essa abordagem tem vários inconvenientes. A equipe de desenvolvimento de microsserviço deve possuir a implementação da camada anticorrupção; portanto, eles precisam se preocupar com quaisquer mudanças que ocorram no lado da aplicação monolítica. Se implementarmos essa camada de tradução como outro microsserviço, poderemos delegar sua implementação e a propriedade a uma equipe diferente. Essa equipe só precisa entender a tradução e nada mais. Essa abordagem é comumente conhecida como _sidecar_.

Conforme mostrado na Figura abaixo, o padrão de sidecar é derivado do veículo onde um sidecar é conectado a uma motocicleta. Se desejar, você pode anexar diferentes sidecars (de cores ou desenhos diferentes) à mesma motocicleta, desde que a interface entre os dois esteja inalterada. O mesmo se aplica no mundo de microsserviços, onde nosso microserviço se parece com a motocicleta, enquanto a camada de tradução se parece com o sidecar. Se alguma alteração ocorrer no aplicativo monolítico, precisaremos apenas alterar a implementação do sidecar para estar em conformidade com ela e sem alterar o microsserviço.

![Sidecar](./Images/Microservices/Sidecar.png "Sidecar")

A comunicação entre o microsserviço e o sidecar acontece pela rede (não uma chamada local em processo), mas tanto o microsserviço quanto o sidecar estão co-localizados no mesmo host - portanto, ele não será roteado pela rede. Além disso, lembre-se de que o sidecar em si é outro microsserviço. A Figura abaixo mostra como usar um sidecar como uma camada anticorrupção.

![Sidecar acting as the anti-corruption layer](./Images/Microservices/Sidecar%20acting%20as%20the%20anti-corruption%20layer.png "Sidecar acting as the anti-corruption layer")

Uma camada anticorrupção é uma maneira possível de usar um padrão de sidecar.

##### Shared Kernel

Embora tenhamos discutido como é importante ter limites claros entre contextos delimitados, há casos em que precisamos compartilhar modelos de domínio. Isso pode acontecer em um caso em que temos dois ou mais contextos limitados com certas coisas em comum, e isso adiciona muita sobrecarga para manter modelos de objetos separados em diferentes contextos delimitados. Por exemplo, cada contexto limitado (ou o microsserviço) tem que autorizar o usuário que invoca suas operações. Domínios diferentes podem estar usando suas próprias políticas de autorização, mas em muitos casos compartilham o mesmo modelo de domínio que o serviço de autorização (o próprio serviço de autorização é um microsserviço ou um contexto limitado separado). Nesses casos, o modelo de domínio do serviço de autorização atua como o kernel compartilhado. Como há uma dependência de código compartilhado (provavelmente envolvida em uma biblioteca).

![Shared kernel](./Images/Microservices/Shared%20kernel.png "Shared kernel")

### Design Principles

Domain-driven design nos ajuda a dimensionar microsserviços juntamente com contextos limitados. Em sua essência, em qualquer projeto de microsserviços, o tempo de produção, escalabilidade, localização de complexidade e resiliência são elementos-chave. Aderir aos princípios de design a seguir ajuda um microsserviço a atingir essas metas de design.

#### High Cohesion And Loose Coupling

A coesão é uma medida de quão bem um determinado sistema é autocontido. Gary McLean Hall, em seu livro Adaptive Code, apresenta a coesão como uma medida da relação contextual entre variáveis ​​em um método, métodos em uma classe, classes em um módulo, módulos em uma solução, soluções em um subsistema e subsistemas em um sistema. Essa relação fractal é importante porque a falta de coesão em qualquer escopo é problemática. A coesão pode ser baixa ou alta com base na força das relações contextuais. Em uma arquitetura de microsserviços, se tivermos um microsserviço para tratar dois ou mais problemas não relacionados, ou, em outras palavras, dois ou mais problemas com relacionamentos contextuais fracos, isso resulta em um sistema de baixo coesão. Sistemas coesos baixos são frágeis por natureza. Quando construímos um microsserviço para atender a muitos outros requisitos não relacionados, há grandes chances de termos que alterar a implementação com mais frequência.

Como sabemos, dados dois requisitos, se temos um forte relacionamento contextual? Este é o ponto principal do exercício que realizamos na seção anterior sob o design orientado a domínio. Se os seus requisitos estiverem no mesmo contexto limitado, eles terão um forte relacionamento contextual. Se os limites de serviço de um microsserviço estiverem alinhados ao contexto limitado do domínio correspondente, ele produzirá um microsserviço altamente coeso.

Coesão e acoplamento são duas propriedades relacionadas de um projeto de sistema. Um sistema altamente coeso é naturalmente fracamente acoplado. O acoplamento é uma medida da interdependência entre diferentes sistemas ou, no nosso caso, microsserviços. Quando temos uma alta interdependência entre microsserviços, isso resultará em um sistema fortemente acoplado, enquanto uma baixa interdependência produzirá um sistema fracamente acoplado. Sistemas fortemente acoplados constroem uma arquitetura de sistema frágil. Uma mudança feita em um sistema afetará todos os sistemas relacionados. Se um sistema falhar, todos os sistemas relacionados serão disfuncionais. Quando temos um sistema de alta coesão, agrupamos todas as funcionalidades relacionadas ou interdependentes em um único sistema (ou grupo em um contexto limitado). Portanto, não precisa depender muito de outros sistemas.

_Uma arquitetura de microsserviços deve ser altamente coesa e fracamente acoplada, por definição._

#### Resilience

A resiliência é uma medida da capacidade de um sistema ou componentes individuais em um sistema para se recuperar rapidamente de uma falha. Em outras palavras, é um atributo de um sistema que permite lidar com falhas de uma maneira que não faz com que o sistema inteiro falhe. Uma arquitetura de microsserviços é naturalmente um sistema distribuído. Um sistema distribuído é uma coleção de computações (ou nós) conectadas através da rede, sem memória compartilhada, que aparece para seus usuários como um único sistema coerente. Em um sistema distribuído, as falhas não são estranhas. Agora e para sempre, a rede sempre será pouco confiável. Um cabo de fibra óptica subaquático pode ser danificado por um submarino, um roteador pode ser explodido devido ao calor, um balanceador de carga pode começar a funcionar mal, os computadores podem ficar sem memória, uma falha de energia pode derrubar um servidor de banco de dados,para baixo - e por mil e uma outras razões, a comunicação em um sistema distribuído pode falhar.

A maneira mais comum de combater falhas de ataque em um sistema distribuído é via redundância. Cada componente no sistema distribuído terá uma contrapartida redundante. Se um componente falhar, a contraparte correspondente assumirá. Nem sempre, todos os componentes de um sistema poderão se recuperar de uma falha com tempo de inatividade zero. Além da redundância, ter uma mentalidade de desenvolvedor focada no desenvolvimento orientado para a recuperação ajuda na criação de software mais resiliente. A seguir lista um conjunto de padrões que foram inicialmente introduzidos por Michael T. Nygard em seu livro, Release It,para construir software resiliente. Hoje, esses padrões são parte integrante do desenvolvimento de microsserviços. Muitas estruturas de microsserviços têm suporte de primeira classe para implementar esses padrões.

### 12-Factor App

Uma arquitetura de microsserviços não é construída apenas em torno dos princípios de design. Alguns chamam isso de cultura. É o resultado de muitos outros esforços colaborativos. Sim, o design é um elemento-chave, mas também precisamos nos preocupar com a colaboração entre desenvolvedores e especialistas de domínio, a comunicação entre equipes e membros da equipe, integração e entrega contínuas e muitos outros problemas. O 12-Factor App é um manifesto publicado pela Heroku em 2012. Este manifesto é uma coleção de melhores práticas e diretrizes para criar e manter aplicativos escaláveis, de fácil manutenção e portáteis. Embora essas práticas recomendadas sejam inicialmente derivadas dos aplicativos implantados na plataforma de nuvem Heroku, hoje ela se tornou um mantra para qualquer implantação bem-sucedida de microsserviços. Esses 12 fatores discutidos a seguir são bastante comuns e naturais, então é muito provável que você esteja aderindo a eles, consciente ou inconscientemente.

#### Codebase

O fator de base de código destaca a importância de manter todo o seu código-fonte em um sistema de controle de versão e ter um repositório de código por aplicativo. Aqui o aplicativo pode ser nosso microsserviço. Ter um repositório por microsserviço ajuda a liberá-lo independentemente de outros microsserviços. O microsserviço deve ser implantado em vários ambientes (teste, preparação e produção) do mesmo repositório. Ter repositórios por serviço também ajuda nos aspectos de governança do processo de desenvolvimento.

#### Dependencies

O fator dependências diz que, em seu aplicativo, você deve declarar e isolar explicitamente suas dependências e nunca deve depender de dependências implícitas do sistema. Na prática, se você estiver criando um microsserviço baseado em Java, deverá declarar todas as suas dependências com o Maven em um arquivo pom.xml ou com o Gradle em um arquivo build.gradle. Maven e Gradle são duas ferramentas de automação de construção muito populares, mas com desenvolvimentos recentes, Gradle parece ter vantagem sobre o Maven e é usado pelo Google, Netflix, LinkedIn e muitas outras grandes empresas. A Netflix, em seu processo de desenvolvimento de microsserviços, usa o Gradle com sua própria ferramenta de automação de construção chamada Nebula. Na verdade, o Nebula é um conjunto de plugins Gradle desenvolvidos pela Netflix.

Gerenciar dependências para microsserviços foi além de apenas tê-los declarados em ferramentas de automação de construção. Atualmente, a maioria das implantações de microsserviços dependem de contêineres, como o Docker. Com o Docker, você pode declarar não apenas as principais dependências que seu microsserviço precisa executar, mas também outras dependências externas com versões específicas, como a versão do MySQL, a versão do Java e assim por diante.

#### Configuration

O fator de configuração enfatiza a necessidade de separar configurações específicas do ambiente do código para a configuração. Por exemplo, a URL de conexão do LDAP ou do servidor de banco de dados envolve parâmetros e certificados específicos do ambiente. Estas configurações não devem ser incluídas no código. Além disso, mesmo com os arquivos de configuração, nunca comprometa nenhum tipo de credencial nos repositórios de origem. Este é um erro comum que alguns desenvolvedores fazem; eles acham que, como estão usando repositórios privados do GitHub, apenas eles têm acesso, o que não é verdade. Quando você envia suas credenciais para um repositório privado do GitHub, mesmo sendo privado, essas credenciais são armazenadas em servidores externos em texto não criptografado.

#### Backing Services

Um serviço de suporte é qualquer tipo de serviço que nosso aplicativo consome durante suas operações normais. Pode ser um banco de dados, uma implementação de armazenamento em cache, servidor LDAP, intermediário de mensagem, terminal de serviço externo ou qualquer tipo de serviço externo. O fator de serviços de suporte diz que esses serviços de suporte devem ser tratados como recursos que podem ser anexados. Em outras palavras, eles devem ser plugáveis ​​em nossa implementação de microsserviços. Devemos ser capazes de alterar o banco de dados, o servidor LDAP ou qualquer ponto final externo, apenas editando um arquivo de configuração ou configurando uma variável de ambiente. Esse fator está muito relacionado ao anterior.

![Backing services](./Images/Microservices/Backing%20services.png "Backing services")

#### Build, Release, Run

Esse é o quinto fator e destaca a importância de ter uma separação clara entre as fases de criação, liberação e execução de um aplicativo. Vamos ver como a Netflix constrói seus microservices. Eles têm uma separação clara entre essas fases (veja a Figura abaixo). Começa com o desenvolvedor que constrói e testa localmente usando o Nebula. Nebula é uma ferramenta de automação de build desenvolvida pela Netflix; na verdade, é um conjunto de plugins Gradle. Em seguida, as alterações são confirmadas no repositório Git correspondente. Em seguida, um trabalho do Jenkins executa o Nebula, que cria, testa e empacota o aplicativo para implantação. Para aqueles que são novos no Jenkins, é um servidor de automação de código aberto líder que facilita a integração e a implantação contínuas. Quando a construção estiver pronta, ela será incorporada em uma Amazon Machine Image (AMI).

![Netflix build process](./Images/Microservices/Netflix%20build%20process.png "Netflix build process")

O Spinnaker é outra ferramenta usada no processo de lançamento do Netflix. Na verdade, é uma plataforma de entrega contínua para liberar alterações de software com alta velocidade e confiança, desenvolvida pela Netflix e, posteriormente, feita de código aberto. Quando a AMI estiver pronta para ser implantada, o Spinnaker a disponibilizará para implantação em dezenas, centenas ou milhares de instâncias e a implantará no ambiente de teste. A partir daí, as equipes de desenvolvimento normalmente exercitarão a implantação usando um conjunto de testes de integração automatizados.

#### Processes

O sexto fator afirma que os processos devem ser sem estado e devem evitar o uso de sessões persistentes. Sem estado significa que um aplicativo não deve assumir nenhum dado na memória antes e depois de executar uma operação. Qualquer implantação de microsserviços compatível com o sexto fator deve ser projetada de forma a ser apátrida. Em uma implantação típica de microsserviços de nível corporativo, você descobrirá que várias instâncias de um determinado microsserviço aumentam ou diminuem com base na carga que recebe. Se quisermos manter algum tipo de estado na memória nesses microsserviços, será um processo complicado replicar o estado em todas as instâncias de microsserviço e adicionar muita complexidade. Microsserviços sem estado podem ser replicados sob demanda e nenhuma coordenação é necessária entre cada um durante o bootstrap ou mesmo no tempo de execução, o que nos leva à arquitetura nada compartilhada.

#### Shared Nothing Architecture

#### Port Binding

#### Concurrency

#### Disposability

#### Admin Processes

### How domain-driven design works

Evans introduced some concepts that are necessary to understand to learn how domain-driven design works:

- **Context**:This is the setting in which a word or statement appears that determines its meaning.

- **Domain**: This is a sphere of knowledge (ontology), influence, or activity. The subject area to which the user applies a program is the domain of the software.

- **Model**:This is a system of abstractions that describes selected aspects of a domain and can be used to solve problems related to that domain.

- **Ubiquitous language**:This is a language structured around the domain model and used by all team members to connect all the activities of the team with the software.

The **software domain** is not related to the technical terms, programming or computers in any way. In most projects, the most challenging part is to understand the business domain, so DDD suggests using a **model domain**; this is abstract, ordered, and selective knowledge reproduced in a diagram, code, or just words.

The model domain is like the roadmap to build projects with complex functionalities, and it is necessary to follow five steps to achieve it. These five steps need to be agreed on by the development team and the domain expert:

1. **Brainstorming and refinement**: There should be a communication channel between the development team and the domain expert. So, all the people in the project should be able to talk to everyone because they all need to know how the project should work.
2. **Draft domain model**: During the conversation, it is necessary to start drawing a draft of the domain model, so that it can be checked and corrected by the domain expert until they both agree.
3. **Early class diagram**:Using the draft, we can start building an early version of the class diagram.
4. **Simple prototype**: Using the draft of the early class diagram and domain model, it is possible to build a very simple prototype. Evans suggests avoiding things that are not related to the domain to ensure that the domain business was modeled properly. It can be a very simple program as a trace.
5. **Prototype feedback**: The domain expert interacts with the prototype in order to check whether all the needs are met and then the entire team will improve the model domain and the prototype.

The model, code, and design must evolve and grow together.They cannot be unsynchronized at all. If a concept is updated on the model, it should also be updated on the code and on the design automatically, and the same goes for the rest.

A model is an abstraction system that describes selective concepts of a domain and it can be used to resolve problems related to that domain. If there is a piece of the model that is not reflected in the code, it should be removed.

Finally, the domain model is the base of the common language in a project. This common language in DDD is called **ubiquitous language** and it should have the following things:

- Class names and their functions related to the domain
- Terms to discuss the domain rules included on the model
- Names of analysis and design patterns applied to the domain model

The ubiquitous language should be used by all the members of the project, including developers and domain experts, so the developers should be able to describe all the tasks and functions.

It is absolutely necessary to use this language in all the discussions between the team, such as meetings, diagrams, or documentation, but this language was not born in the first iteration of the process, meaning that it can take many iterations of refactoring having the model, language, and code synchronized. If, for example, the developers discover that a class from the domain should be renamed, they cannot refactor this without refactoring the name on the domain model and the ubiquitous language.

The ubiquitous language, domain model, and code should evolve togetheras a single knowledge block.

There is controversial concept on DDD. Eric Evans says that it is necessary for the domain expert to use the same language as the team, but some people do not like this idea. Usually, the domain experts do not have knowledge of object-oriented concepts or microservices because they are too abstract for non-developers. Anyway, DDD says that if the domain expert does not understand the domain model, it is because there is something wrong with it.

There are diagrams in the domain model, but Evans suggests using text as well, because diagrams do not explain the concepts properly. Also, the diagrams should be superficial; if you want to see more details you have the code for it.

Some projects are affected by the connection between the domain model and the code. This happens because there is a division between analysis and design. The analysts make a model independent of the design and the developers cannot develop the functionalities because some information is missing. In addition, they cannot talk with the domain expert. The development team will not follow the model and, in the end, the domain model will not be updated and it will not work. Therefore, the project will not meet the requirements.

To sum up, DDD works to achieve the software development as an iterative process of refinement of the model, design, and code as a single task in a block.

### Using domain-driver design in microservices

As we said before, DDD meets the microservice's needs perfectly. A common problem with microservices appears because they have decentralized data management; this has advantages but can be problematic sometimes.

The concept model between two services will be different, and it can cause problems in huge companies. For example, a user can differ depending on the service, the attributes for each service regarding the user can differ and, also, the attribute semantic can differ.

It is even more complex when, in a big company, the application evolves a lot and has updates for many years. Each service can have different attributes for the user and, generally, they do not match. So, a great way to solve this is using DDD.

As microservices do, DDD divides a complex domain into different contexts, making relationships between them and asking for the collaboration of all the members to get a ubiquitous language in a particular domain and bounded context, iterating this process until they achieve a real concept regarding the problem.

Evans suggests designing each microservice as a DDD-bounded context so that it will provide a logical boundary for microservices inside a system. Every single microservice (or team working on it) will be responsible for that part of the system, and it will give clearer and maintainable code.

Michael Plöd gave more ideas about how DDD can help microservices. There are four significant areas regarding building microservices:

- **Strategic design**: This is basically bounded context, but context maps and other patterns are important too. A context map should show all the bounded contexts of the project and their relationships with each other; it also describes the contract between them. The context map is very useful for monolithic applications wanting to move into microservices.

- **Internal building blocks**: This refers to using tactical patterns, such as aggregates, entities, or repositories, when designing the inside of a bounded context.

- **Large-scale structures**: This is used to create a structure using evolving order and responsibility layers. This is a concept in microservices too. In huge projects, it is helpful to create large-scale structures into boundary contexts. They should be designed to evolve individually.

- **Distillation**: Distilling a core domain from an already grown system is very useful when migrating a monolithic application into microservices. The most important part should be identifying and extracting the core domain, along with the iteration process of identifying a subdomain, extracting it from the core, and refactoring.

To sum up, microservices and DDD match perfectly, but it is necessary to have a larger scope and understand more than the boundary contexts.

## Inter-Service Communication

Na arquitetura de microsserviços, os serviços são autônomos e se comunicam pela rede para atender a um caso de uso de negócios. Uma coleção de tais serviços forma um sistema e os consumidores geralmente interagem com esses sistemas. Portanto, um aplicativo baseado em microsserviços pode ser considerado um sistema distribuído que executa vários serviços em diferentes locais de rede. Um determinado serviço é executado em seu próprio processo. Portanto, os microsserviços interagem usando estilos de comunicação entre processos ou entre serviços.

### Fundamentals of Microservices Communication

Os serviços são voltados para a capacidade de negócios e as interações entre esses serviços formam um sistema ou um produto, que está relacionado a um conjunto específico de casos de uso de negócios. Portanto , a comunicação entre serviços é um fator chave para o sucesso da arquitetura de microsserviços.

Um aplicativo baseado em microsserviços consiste em um conjunto de serviços independentes que se comunicam entre si por meio de mensagens. Mensagens não é um conceito novo em sistemas distribuídos. Em aplicativos monolíticos, as funcionalidades de negócios de diferentes processadores/componentes são chamadas usando chamadas de função ou chamadas de método em nível da linguagem. Em Arquitetura Orientada a Serviços (SOA), isso foi mudado para mensagens de nível de serviço da Web mais menos acopladas, que são baseadas principalmente em SOAP, além de diferentes protocolos como HTTP, enfileiramento de mensagens etc. Quase todas as interações de serviço são implementadas em camada centralizada do Enterprise Service Bus (ESB).

No contexto de microsserviços, não há essa restrição como com o SOA/Web Services para usar um padrão de comunicação e um formato de mensagem específicos. Em vez disso, a arquitetura de microsserviços prefere selecionar o mecanismo de colaboração de serviço e o formato de mensagem apropriados que são usados ​​para trocar informações, com base no caso de uso.

Os estilos de comunicação do microsserviço são predominantemente sobre como os serviços enviam ou recebem dados de um serviço para o outro. O tipo mais comum de estilos de comunicação usados ​​em microsserviços é síncrono e assíncrono.

### Synchronous Communication

No estilo de comunicação síncrona, o cliente envia uma solicitação e aguarda uma resposta do serviço. Ambas as partes devem manter a conexão aberta até que o cliente receba a resposta. A lógica de execução do cliente não pode prosseguir sem a resposta. Enquanto na comunicação assíncrona, o cliente pode enviar uma mensagem e ser completamente feito sem esperar por uma resposta.

Observe que a comunicação síncrona e de bloqueio são duas coisas diferentes. Existem alguns livros e recursos que interpretam a comunicação síncrona como um cenário de bloqueio puro no qual o encadeamento do cliente basicamente bloqueia até obter a resposta. Isso não está correto. Podemos usar uma implementação de E/S não bloqueada, que registra uma função de retorno de chamada assim que o serviço responde e o encadeamento do cliente pode ser retornado sem bloqueio em uma resposta específica. Portanto, o estilo de comunicação síncrona pode ser construído sobre uma implementação assíncrona sem bloqueio.

#### REST

Representational State Transfer (REST) ​​é um estilo de arquitetura que constrói sistemas distribuídos baseados em hipermídia. O modelo REST usa um esquema de navegação para representar objetos e serviços em uma rede. Estes são conhecidos como recursos. Um cliente pode acessar o recurso usando o URI exclusivo e uma representação do recurso é retornado. O REST não depende de nenhum dos protocolos de implementação, mas a implementação mais comum é o protocolo de aplicativo HTTP. Ao acessar recursos RESTful com o protocolo HTTP, a URL do recurso atua como o identificador de recurso e GET, PUT, DELETE, POST e HEAD são as operações HTTP padrão a serem executadas nesse recurso. O estilo de arquitetura REST é inerentemente baseado em mensagens síncronas.

Para entender como podemos usar o estilo arquitetural REST no desenvolvimento de serviços, vamos considerar o cenário de gerenciamento de pedidos do aplicativo de varejo on-line. Podemos modelar o cenário de gerenciamento de pedidos como um serviço RESTful chamado Processamento de Pedidos. Conforme mostrado na Figura abaixo, você pode definir a ordem dos recursos para este serviço.

![Different operations that you can execute on the Order Processing RESTful service](./Images/Microservices/Different%20operations%20that%20you%20can%20execute%20on%20the%20Order%20Processing%20RESTful%20service.png "Different operations that you can execute on the Order Processing RESTful service")

Para fazer um novo pedido, você pode usar a mensagem HTTP POST com o conteúdo do pedido, que é enviado para o URL (http://xyz.retail.com/order). A resposta do serviço contém uma mensagem HTTP 201 Created com o cabeçalho da localização apontando para o recurso recém-criado (http://xyz.retail.com/order/123456). Agora você pode recuperar os detalhes do pedido desse URL enviando uma solicitação HTTP GET. Da mesma forma, você pode atualizar o pedido e excluir o pedido usando os métodos HTTP apropriados.

Devido ao fato de que o REST é implementado principalmente em HTTP (que é amplamente utilizado, menos complicado e amigável ao firewall), o REST é a forma mais comum de estilo de comunicação de microsserviço adotado por implementações de microsserviços. Vamos considerar algumas práticas recomendadas ao usar microsserviços RESTful.

- Os microsserviços baseados em HTTP RESTful (REST é independente do protocolo de implementação, mas não existem serviços RESTful não HTTP usados ​​na prática) são bons para microsserviços (ou APIs) externos, porque é mais fácil expô-los através de infraestruturas existentes, como firewalls, proxies reversos, balanceadores de carga, etc.
- O escopo de recursos deve ser refinado para que possamos mapeá-lo diretamente para um recurso de negócios relacionado à operação (por exemplo, adicionar pedido).
- Use os modelos de maturidade Richardson e outras práticas recomendadas do serviço RESTful para projetar serviços.
- Use estratégias de versionamento quando aplicável. (A versão geralmente é implementada no nível do API Gateway quando você decide expor seu serviço como APIs.)

Como muitas estruturas de microsserviços usam REST como seus estilos de fato, você pode se sentir tentado a usá-lo para todas as suas implementações de microsserviços. Mas você deve usar REST apenas se for o estilo mais apropriado para seus casos de uso.

### Asynchronous Communication

Na maioria das implementações iniciais da arquitetura de microsserviços, a comunicação síncrona está sendo adotada como o estilo de comunicação inter-serviços de fato. No entanto, a comunicação assíncrona entre microsserviços está ficando cada vez mais popular à medida que torna os serviços mais autônomos.

Na comunicação assíncrona, o cliente não espera por uma resposta em tempo hábil. O cliente pode não receber uma resposta ou a resposta será recebida de forma assíncrona através de um canal diferente.

The asynchronous messaging between microservices is implemented with the use of a lightweight and dumb message broker. Não há lógica de negócios no broker e é uma entidade centralizada com alta disponibilidade. Existem dois tipos principais de estilos de mensagens assíncronas - um único receptor e vários receptores.

#### Single Receiver

No modo de receptor único, uma determinada mensagem é entregue de forma confiável de um produtor para exatamente um consumidor por meio de um intermediário de mensagem (consulte a Figura abaixo). Como esse é um estilo de mensagens assíncrono, o produtor não espera por uma resposta do consumidor nem durante o tempo de produção da mensagem, e o consumidor pode ou não estar disponível. Isso é útil ao enviar comandos baseados em mensagens assíncronas de um microsserviço para o outro. Dado que os microsserviços podem ser implementados usando diferentes tecnologias, devemos implementar a entrega confiável de mensagens entre os microsserviços do produtor e do consumidor de uma maneira agnóstica em termos de tecnologia.

![Single receiver based asynchronous communication with AMQP](./Images/Microservices/Single%20receiver%20based%20asynchronous%20communication%20with%20AMQP.png "Single receiver based asynchronous communication with AMQP")

O protocolo AMQP (Advanced Message Queuing Protocol) é o padrão mais comumente usado com comunicação baseada em um único receptor.

##### AMQP

O AMQP é um protocolo de mensagens que lida com publishers e consumers. Os publishers produzem as mensagens e os consumers as pegam e processam. O trabalho do mediador de mensagens(message broker) é garantir que as mensagens de um publisher atinjam os consumers certos. O AMQP garante confiabilidade na entrega de mensagens, entrega rápida e garantida de mensagens e confirmações de mensagens.

Vejamos um cenário de exemplo de nosso caso de uso de aplicativo de varejo on-line. Suponha que o microsserviço do Checkout coloque um pedido como um comando assíncrono no microsserviço Processamento de Pedidos.

Podemos usar um intermediário de mensagens AMQP (como o RabbitMQ ou o ActiveMQ) como a infraestrutura de mensagens dumb e o microsserviço do Checkout pode produzir uma mensagem para a fila específica no broker. Em um canal independente, o microsserviço Processamento de Pedidos pode se inscrever na fila como consumidor e recebe mensagens de maneira assíncrona. Para este caso de uso, podemos identificar os seguintes componentes principais.

- **Message:** Content of data transferred, which includes the payload and message attributes.

- **AMQP Message Broker** A central application that implements the AMQP protocol, which accepts connections from producers for message queuing and from consumers for consuming messages from queues.

- **Producer:** An application that puts messages in a queue.

- **Consumer:** An application that receives messages from queues.

Os produtores podem especificar vários atributos de mensagem, que podem ser úteis para o broker(intermediário) ou que apenas pretendem ser processados ​​pelo microsserviço consumidor.

Como as redes não são confiáveis, a comunicação entre o broker e os microsserviços pode falhar ao processar mensagens. O AMQP define um conceito chamado confirmações de mensagens, que é útil na implementação confiável da entrega das mensagens. Quando uma mensagem é entregue a um consumidor, o consumidor notifica o broker, automaticamente ou quando o código do aplicativo decide fazê-lo. No modo de confirmação de mensagens, o broker só removerá completamente uma mensagem de uma fila quando receber uma notificação para essa mensagem (ou grupo de mensagens). Além disso, como estamos usando uma fila, podemos garantir a entrega e o processamento ordenados das mensagens. Existe um conjunto de mecanismos de confiabilidade introduzidos pelo AMQP.

As falhas podem ocorrer entre as interações de rede do broker, do publicador e do consumidor ou o tempo de execução do broker e dos aplicativos clientes pode falhar.

As confirmações permitem que o consumidor indique ao servidor que recebeu a mensagem com sucesso. Da mesma forma, o broker pode usar confirmações para informar ao produtor que recebeu a mensagem com sucesso (por exemplo, confirma no RabbitMQ). Portanto, um aviso sinaliza o recebimento de uma mensagem e uma transferência de propriedade, em que o receptor assume total responsabilidade por isso.

#### Multiple Receivers

Quando as mensagens assíncronas produzidas por um produtor devem ir para mais de um consumidor, a comunicação do tipo publisher-subscriber ou receptor múltiplo será útil. Como caso de uso de exemplo, suponha que haja um serviço de gerenciamento de produto, que produz a informação sobre as atualizações de preços dos produtos. Essa informação deve ser propagada para vários microsserviços, como Carrinho de Compras, Detecção de Fraude, Assinaturas, etc. Como mostrado na Figura abaixo, Podemos usar um barramento de eventos, pois a infra-estrutura de comunicação e o serviço Product Management podem publicar os eventos de atualização de preços para um determinado tópico. Os serviços interessados ​​em receber eventos de atualização de preço devem assinar o mesmo tópico. Os serviços interessados ​​receberão eventos sobre tópicos inscritos somente se estiverem disponíveis no momento da transmissão do evento pelo barramento de eventos. Se a implementação do barramento de eventos subjacente oferecer suporte, o assinante pode ter a opção de assinar como um assinante durável, no qual receberá todos os eventos, apesar de estar off-line no momento da transmissão do evento.

![Multiple receivers (pub-sub) based asynchronous communication](./Images/Microservices/Multiple%20receivers%20(pub-sub)%20based%20asynchronous%20communication.png "Multiple receivers (pub-sub) based asynchronous communication")

There are multiple messaging protocols that support pub-sub messaging. Most of the AMQP-based brokers support pub-sub, but Kafka (which has its own messaging protocol) is the most widely used message broker for multiple receiver/pub-sub type messaging between microservices.

### Synchronous versus Asynchronous Communication

Neste ponto, você deve ter uma sólida compreensão das técnicas de mensagens síncronas e assíncronas. É muito importante entender quando usar esses estilos de comunicação na implementação da arquitetura de microsserviços.

A principal diferença entre a comunicação síncrona e assíncrona é que há cenários em que não é possível manter uma conexão aberta entre um cliente e um servidor. Uma comunicação duradoura de mão única é mais prática nesses cenários. A comunicação síncrona é mais adequada em cenários em que você precisa criar serviços que forneçam interações de estilo de solicitação / resposta. Se as mensagens forem orientadas a eventos e o cliente não esperar uma resposta imediatamente, a comunicação assíncrona será mais adequada.

No entanto, em muitos livros e artigos, enfatiza-se que a comunicação síncrona é má, e devemos usar apenas comunicação assíncrona para microsserviços, pois ela permite a autonomia do serviço. Em muitos casos, a comparação entre a comunicação síncrona e a assíncrona está sendo interpretada de tal maneira que, na comunicação síncrona, um dado encadeamento será bloqueado até que ele aguarde uma resposta. Isso é completamente falso, porque a maioria das implementações de mensagens síncronas é baseada em arquiteturas de mensagens totalmente sem bloqueio. (Ou seja, o remetente do lado do cliente envia a solicitação e registra um retorno de chamada, e o encadeamento é retornado. Quando a resposta é recebida, ela é correlacionada com a solicitação original e a resposta é processada.)

Então, voltando ao uso pragmático desses estilos de comunicação, precisamos selecionar o estilo de comunicação baseado em nosso caso de uso.

Em cenários em que interações de curta execução exigem que um cliente e um serviço se comuniquem no estilo de solicitação-resposta, a comunicação síncrona é obrigatória. Por exemplo, em nosso caso de uso de varejo on-line, um serviço de pesquisa de produtos é comunicado de forma síncrona, porque você deseja enviar uma consulta de pesquisa e deseja ver os resultados da pesquisa imediatamente. Cenários como colocação ou processamento de pedidos são inerentemente assíncronos. Portanto, o estilo de mensagens assíncronas é o melhor ajuste para esses cenários.

## Data Management

Na maioria dos casos de uso de negócios, a lógica de serviço é construída sobre uma camada persistente subjacente. Muitas vezes, um banco de dados é usado como camada persistente e está agindo como o sistema de registro de um determinado serviço. Como já discutimos, os microsserviços são construídos como entidades autônomas e devem ter controle sobre a camada de dados em que operam. Isso significa essencialmente que os microsserviços não podem depender de uma camada de dados pertencente ou compartilhada por outra entidade. Portanto, no processo de criação de serviços autônomos, também é necessário ter uma camada persistente isolada para cada microsserviço.

### Monolithic Applications and Shared Databases

No contexto da arquitetura corporativa, que é baseada em aplicativos e serviços monolíticos, geralmente um único banco de dados centralizado (ou alguns) é compartilhado entre vários aplicativos e serviços. Por exemplo, conforme mostrado na Figura abaixo, todos os serviços do sistema de varejo compartilham um banco de dados centralizado (Retail DB). Assim, todas as informações relacionadas a produtos, clientes, pedidos, pagamentos e assim por diante são gerenciadas centralmente por meio do banco de dados Retail.

![The microservices of online retail application share a single database](./Images/Microservices/The%20microservices%20of%20online%20retail%20application%20share%20a%20single%20database.png "The microservices of online retail application share a single database")

Na verdade, um banco de dados compartilhado centralizado facilita a combinação de dados de várias tabelas e a formulação de representações comerciais diferentes. As poderosas linguagens de consulta, como SQL, suportam inatamente todos os recursos necessários para compartilhar os dados e construir diferentes composições de dados. Por exemplo, usando SQL, podemos unir várias tabelas em várias condições complexas e criar uma visão composta de diferentes entidades.

Ao trabalhar com entidades de negócios compartilhadas, a implementação de interações de negócios entre elas de maneira transacional se torna muito importante. Uma transação é uma unidade atômica de trabalho que falha ou é bem-sucedida. As principais características das transações são conhecidas como ACID, que é um acrônimo para atomicidade (todas as alterações nos dados são realizadas como se fossem uma única operação), consistência (os dados estão em um estado consistente quando uma transação inicia e quando termina) , isolamento (o estado intermediário de uma transação é invisível para outras transações) e durabilidade (após uma transação ser concluída com êxito, as alterações nos dados persistem e não são desfeitas, mesmo no caso de uma falha do sistema).

Um banco de dados centralizado torna transações ACID em várias entidades trivialmente fáceis. Em um banco de dados relacional, toda instrução SQL deve ser executada no escopo de uma transação. Por isso, é bastante trivial modelar um cenário transacional complexo que envolve várias tabelas. A maioria dos sistemas de gerenciamento de banco de dados relacional (RDBMS) suporta esses recursos prontos para uso.

Apesar das vantagens da arquitetura de banco de dados compartilhada centralizada, também tem desvantagens. É o único ponto de falha, cria um gargalo de desempenho potencial devido ao tráfego intenso de aplicativos direcionado a um único banco de dados e tem dependências restritas entre os aplicativos, pois compartilham as mesmas tabelas de banco de dados. Portanto, você não pode criar microsserviços autônomos e independentes se estiver usando uma camada ou banco de dados persistente compartilhado. Assim, com microsserviços você precisa descentralizar o gerenciamento de dados e cada microsserviço deve possuir os dados em que opera.

### A Database per Microservice

A arquitetura de microsserviços incentiva os microsserviços a serem proprietários dos dados em que operam e os bancos de dados não devem ser compartilhados com nenhum outro serviço. Portanto, um determinado microsserviço terá um armazenamento de dados isolado ou usará um serviço persistente isolado (por exemplo, de um provedor de nuvem).

Ter um banco de dados por microsserviço nos dá muita liberdade quando se trata de autonomia de microsserviços. Por exemplo, os proprietários de microserviços podem modificar o esquema do banco de dados de acordo com os requisitos de negócios, sem se preocupar com os consumidores externos do banco de dados. Não há ninguém dos aplicativos externos que possa acessar o banco de dados diretamente. Isso também dá ao desenvolvedor de microserviços a liberdade de selecionar a tecnologia a ser usada como a camada persistente de microsserviços. Microservices diferentes podem usar diferentes tecnologias de armazenamento persistentes, como RDBMS, NoSQL ou outros serviços em nuvem.

No entanto, ter um banco de dados por serviço apresenta um novo conjunto de desafios. Quando se trata da realização de qualquer cenário de negócios, o compartilhamento de dados entre microsserviços e a implementação de transações dentro e fora dos limites do serviço torna-se bastante desafiador.

### Sharing Data Between Microservices

Em um banco de dados monolítico, é muito fácil fazer qualquer composição de dados arbitrária porque compartilhamos um único banco de dados monolítico. No entanto, no contexto de microsserviços, cada parte dos dados é de propriedade de um único serviço (sistema único de registro). Um sistema de registro (ou camada persistente) não pode ser acessado diretamente de qualquer outro serviço ou sistema. A única maneira de acessar os dados pertencentes a outro microsserviço é por meio de uma interface de serviço ou API. Outros sistemas, que acessam os dados por meio da API publicada, possivelmente poderiam usar um cache local somente leitura para manter os dados localmente.

Para atender a esses requisitos, precisamos desenvolver técnicas adequadas para compartilhar dados entre microsserviços, como a maioria dos cenários de negócios exigiria.

#### Eliminating Shared Tables

O compartilhamento de tabelas entre vários serviços/aplicativos é um padrão bastante comum em um banco de dados monolítico. Como discutido anteriormente, quando compartilhamos uma tabela entre dois ou mais microsserviços, uma alteração no esquema dessa tabela pode afetar todos os microsserviços dependentes. Por exemplo, conforme mostrado na Figura abaixo, os serviços de `Order Processing` and `Shipping` compartilham a mesma tabela `TRACKING_INFO`, que acompanha o status do pedido. Ambos os serviços podem ler ou gravar na mesma tabela e o banco de dados central subjacente fornece todas as funcionalidades necessárias (como transações ACID). No entanto, se precisarmos alterar o esquema da tabela `TRACKING_INFO`, isso afetará os serviços de `Order Processing` and `Shippings`. Além disso, não é possível ter dados específicos do serviço (que o serviço não gostaria de compartilhar) na tabela compartilhada. Esses tipos de cenários de tabela compartilhada não são compatíveis com os fundamentos do gerenciamento de dados de microsserviços. O armazenamento/banco de dados persistente de um serviço deve ser independente e apenas um microsserviço deve operá-lo. Portanto, com uma arquitetura de microservices, precisamos nos livrar dessas tabelas compartilhadas.

![Data management between two services using a shared table](./Images/Microservices/Data%20management%20between%20two%20services%20using%20a%20shared%20table.png "Data management between two services using a shared table")

Então, como nos livramos das tabelas compartilhadas? Se você pensar em termos de princípios de manipulação de dados de microserviço que discutimos anteriormente, um determinado dado deve pertencer a um único serviço. Portanto, neste exemplo (descrito na Figura abaixo), as informações de rastreamento devem ser divididas em duas tabelas. Uma tabela deve ter os dados, o que é relevante para o microsserviço de processamento de pedidos, e a outra tabela deve conter os dados relevantes para o microsserviço de envio. Pode haver dados compartilhados duplicados nessas duas tabelas e os serviços são responsáveis ​​por manter os dados em sincronia usando as APIs publicadas desses serviços (sem acesso direto ao banco de dados). Discutimos essas técnicas de sincronização em detalhes mais adiante.

![Splitting shared data and managing it as independent entities](./Images/Microservices/Splitting%20shared%20data%20and%20managing%20it%20as%20independent%20entities.png "Splitting shared data and managing it as independent entities")

Outra variação de tabelas compartilhadas é quando os dados compartilhados são representados como uma entidade comercial separada. No exemplo anterior, os dados compartilhados (informações de rastreamento) não representam uma entidade comercial. Então, vamos dar um exemplo diferente de compartilhamento de dados do cliente entre os serviços de `Order Processing` and `Product Management` services. Nesse caso, esses dois serviços usam dados de uma tabela de dados compartilhada (tabela `CUSTOMER`) em sua lógica de negócios. Podemos agora identificar que as informações dos clientes não são apenas uma tabela, mas também uma entidade comercial completamente diferente. Podemos simplesmente tratá-lo como uma entidade orientada para o recurso de negócios e modelar isso como um microsserviço. Conforme mostrado na Figura abaixo, podemos introduzir o microsserviço do Cliente e deixá-lo possuir os dados do cliente, e os outros serviços podem consumir os dados do cliente por meio de uma API exposta pelo serviço de `Customer service`.

![Services share the customer information through a service build on top of customer database](./Images/Microservices/Services%20share%20the%20customer%20information%20through%20a%20service%20build%20on%20top%20of%20customer%20database.png "Services share the customer information through a service build on top of customer database")

Assim, podemos identificar as principais etapas envolvidas na eliminação de tabelas de dados, que compartilham dados entre vários microsserviços.

1. Identifique a tabela compartilhada e identifique o recurso de negócios dos dados armazenados nessa tabela compartilhada.
2. Mova a tabela compartilhada para um banco de dados dedicado e, no topo desse banco de dados, crie um novo serviço (recurso comercial) identificado na etapa anterior.
3. Remova todos os acessos diretos ao banco de dados de outros serviços e permita somente que eles acessem os dados por meio da API publicada do serviço

Com esse design, precisamos ter um proprietário dedicado do serviço compartilhado recém-criado que possa modificar a interface de serviço ou o esquema desse serviço. Isso também nos ajuda a descobrir os novos limites de negócios entre esses serviços, o que tornará nossa aplicação baseada em microsserviços à prova de futuro com quaisquer novos requisitos.

#### Shared Data

Armazenar dados em várias tabelas e conectá-los por meio de chaves estrangeiras (FK) é uma técnica muito comum em bancos de dados relacionais. Uma chave estrangeira é uma coluna ou combinação de colunas que é usada para estabelecer e impor um link entre os dados em duas tabelas. Você pode criar uma chave estrangeira definindo uma restrição FOREIGN KEY ao criar ou modificar uma tabela. As chaves estrangeiras permitem a integridade referencial entre os dados armazenados em várias tabelas, o que significa que, se uma chave estrangeira contiver um valor, esse valor se refere a um registro existente na tabela relacionada.

Por exemplo, a Figura abaixo ilustra os serviços de `Order Processing` and `Product Management`, que usam as tabelas `ORDER` e `PRODUCT`. Um determinado pedido contém vários produtos e a tabela de pedidos refere-se a esses produtos usando uma chave estrangeira, que aponta para a chave primária da tabela `PRODUCT`. Com a restrição de chaves estrangeiras, você só pode adicionar um valor à chave estrangeira da tabela `ORDER` de uma entidade `ORDER` existente.

![Shared database - Foreign key relationship between tables](./Images/Microservices/Shared%20database%20-%20Foreign%20key%20relationship%20between%20tables.png "Shared database - Foreign key relationship between tables")

Com bancos de dados compartilhados monolíticos, usar uma chave estrangeira e unir dados é bastante trivial. Mas quando você deseja ter serviços independentes e usar um banco de dados por serviço, ter esse tipo de link para a integridade referencial é praticamente impossível. Portanto, com uma arquitetura de microsserviços, precisamos encontrar maneiras diferentes de lidar com esse cenário. Vamos dar uma olhada em algumas das técnicas mais usadas para atingir esses requisitos.

##### Synchronous Lookups

Quando você tem um banco de dados dedicado para cada microsserviço, se um serviço precisar acessar os dados do outro, ele pode simplesmente acessar a API publicada desse microsserviço e recuperar os dados necessários. Por exemplo, conforme mostrado na Figura abaixo, o serviço de order mantém os IDs de produto necessários que fazem parte de uma determinada order. Se o serviço de `Order Processing` exigir as informações detalhadas dos produtos, a partir da lógica do aplicativo, ele deverá chamar o serviço de `Product Management` e recuperar as informações do produto.

![Using synchronous lookups to the service interface to access the data owned by other services](./Images/Microservices/Using%20synchronous%20lookups%20to%20the%20service%20interface%20to%20access%20the%20data%20owned%20by%20other%20services.png "Using synchronous lookups to the service interface to access the data owned by other services")

Essa técnica é bastante trivial de entender e, no nível da implementação, você precisa escrever uma lógica extra para fazer uma chamada de serviço externo. Precisamos ter em mente que, ao contrário dos bancos de dados, não temos mais a integridade referencial de uma restrição de chave estrangeira. Isso significa que os desenvolvedores de serviços devem cuidar da consistência dos dados que eles colocam na tabela. Por exemplo, quando você cria um pedido, é necessário certificar-se (possivelmente ligando para o serviço do produto) de que os produtos mencionados nesse pedido realmente existem na tabela `PRODUCT`.

##### Using Asynchronous Events

O compartilhamento de dados usando synchronous lookups de outros microsserviços pode ser caro em determinados cenários de negócios. Como alternativa, podemos aproveitar a arquitetura orientada a eventos (padrão de publisher-subscriber) para compartilhar dados entre serviços. Por exemplo, para o mesmo cenário da `Order Processing` and `Product Management` services (veja a Figura abaixo), podemos introduzir um event-driven padrão de comunicação, no qual temos um barramento de eventos, que é usado como a infraestrutura de mensagens. Se houver uma atualização em um produto, o serviço de `Product Management` (publicador) atualizará sua tabela de produtos e publicará um evento no barramento de eventos. O serviço de `Order Processing` (assinante) assinou o tópico de atualizações de produtos interessado e, portanto, como o serviço de `Product Management` publica eventos de atualização de produto para esse tópico, o serviço de `Product Management` os receberá. Em seguida, ele pode atualizar seu cache local de informações sobre o produto e usar o cache para implementar a lógica de negócios do serviço `Product Management`.

![Using asynchronous events to share data between microservices](./Images/Microservices/Using%20asynchronous%20events%20to%20share%20data%20between%20microservices.png "Using asynchronous events to share data between microservices")

Quanto ao event bus você pode selecionar qualquer tecnologia de mensagens assíncrona, como Kafka ou AMQP Broker (tais como RabbitMQ), e você pode usar técnicas de assinatura diferentes para garantir a entrega do evento ao assinante (como durable subscriptions).

Com essa abordagem, você pode eliminar chamadas de serviço síncronas de um serviço para outro, mas, como estamos usando um cache local, os dados podem ficar obsoletos. Portanto, o compartilhamento de dados baseado em eventos assíncronos é um modelo de consistência eventual. A consistência eventual garante que os dados de cada serviço sejam consistentes eventualmente (você pode obter dados desatualizados por um determinado período de tempo). O tempo gasto pelos serviços para obter dados consistentes pode ou não ser definido. Portanto, precisamos usar esse padrão para casos de uso que não são afetados por uma eventual natureza de consistência.

##### Shared Static Data

Quando se trata de armazenar e compartilhar os metadados imutáveis somente leitura, os bancos de dados monolíticos convencionais são frequentemente usados ​​e os dados são compartilhados através de uma tabela compartilhada. Por exemplo, dados como estados dos EUA, lista de países, etc., costumam ser usados ​​como dados estáticos compartilhados. Com uma abordagem de microsserviços, uma vez que não queremos compartilhar os bancos de dados, precisamos pensar em como manter os dados estáticos compartilhados.

Alguém poderia pensar que ter outros microsserviços com os dados estáticos resolveria esse problema, mas é um exagero ter um serviço apenas para obter algumas informações estáticas que não mudam com o tempo. Por isso, compartilhar dados estáticos é feito frequentemente com bibliotecas compartilhadas. Por exemplo, se um determinado serviço quiser usar os metadados estáticos, ele precisará importar a biblioteca compartilhada para o código de serviço.

#### Data Composition

Compor dados de várias entidades e criar visualizações diferentes é um requisito muito comum no gerenciamento de dados. Com bancos de dados monolíticos (RDBMS em particular), é trivialmente fácil construir a composição de várias tabelas usando junções em instruções SQL. Assim, você pode compor diferentes visualizações de dados das entidades existentes e usá-las em seus serviços.

No entanto, no contexto de microsserviços, quando você introduz o banco de dados por método de microsserviço, a criação de composições de dados se torna muito complexa. Você não pode mais usar as construções internas, como junções, para compor dados, que estão dispersos entre vários bancos de dados pertencentes a diferentes serviços.

Vamos dar uma olhada em algumas das técnicas comumente usadas para fazer a composição de dados com microsserviços.

##### Composite Services or Client-Side Mashups

Quando você precisa criar uma junção de dados de vários microsserviços, só tem permissão para acessar as APIs de serviço. Portanto, para criar uma composição de dados a partir de vários microsserviços, você pode criar um serviço composto sobre os microsserviços existentes. O serviço composto é responsável por invocar os serviços de recebimento de dados(downstream) e a composição de tempo de execução dos dados recuperados por meio de chamadas de serviço.

Por exemplo, vamos considerar o exemplo mostrado na Figura abaixo. Suponha que precisamos criar uma composição dos pedidos feitos e ter os detalhes dos clientes que fizeram esses pedidos. Aqui temos dois serviços - `Order Processing` e `Customer` - que possuem seus próprios bancos de dados para armazenar os pedidos e informações do cliente.

![Data composition using a composite service that calls the downstream services and aggregates the data](./Images/Microservices/Data%20composition%20using%20a%20composite%20service%20that%20calls%20the%20downstream%20services%20and%20aggregates%20the%20data.png "Data composition using a composite service that calls the downstream services and aggregates the data")

O requisito que temos em mãos é criar uma junção de pedidos e clientes. Com a abordagem de serviços compostos, podemos criar um novo serviço - o serviço composto `Customer-Order` - e chamar os microsserviços `Order Processing` and `Customer` a partir dele. Você precisa implementar a lógica de composição de dados de tempo de execução, bem como a lógica de comunicação (por exemplo, para invocar microsserviços de `Order Processing` and `Customer` via RESTful) dentro do serviço composto.

Uma outra alternativa é implementar a mesma composição de dados de tempo de execução no lado do cliente. Basicamente, em vez de ter um serviço composto, os aplicativos consumidores/clientes podem chamar os serviços de downstream necessários e criar a composição eles mesmos. Isso geralmente é conhecido como um _client-side mashup_.

Serviços compostos ou mashups do lado do cliente são adequados quando os dados que você ingressou são relativamente pequenos. Como essa é uma composição de tempo de execução, se você for carregar muitos dados na memória, o tempo de execução do serviço composto exigirá muita memória. Portanto, precisamos selecionar essa abordagem com base no cenário de composição de dados que precisamos implementar.

_A composição de dados com serviços compostos ou mashup do lado do cliente é adequada para junções do tipo 1:m, em que uma linha de uma tabela pode ter várias linhas correspondentes em outra tabela._

##### Joins with Materialize View Using Asynchronous Events

Existem determinados cenários de composição de dados em que você precisa materializar a exibição com dados pré-agrupados provenientes de vários microsserviços. Por exemplo, considere o cenário ilustrado na Figura abaixo. Aqui temos os serviços de `Order Processing` and `Customer`, e precisamos materializar a junção/visualização de customer-order. A visão materializada será usada para uma função de negócios específica, que requer a união entre pedidos e clientes.

![Joins with materialized view using asynchronous events](./Images/Microservices/Joins%20with%20materialized%20view%20using%20asynchronous%20events.png "Joins with materialized view using asynchronous events")

Os serviços de `Order Processing` and `Customer` publicam os eventos de pedidos e atualizações de clientes para o barramento/intermediário de eventos. Existe um serviço que se inscreveu nesses eventos e, em seguida, materializa a junção de pedidos e clientes. Esse serviço (`Customer-OrderView-Sync`) mantém uma junção desnormalizada entre os pedidos e os clientes, o que é feito antes do tempo e não em tempo real. Conforme mostrado na Figura acima, o serviço `Customer-OrderView-Sync` também possui um componente que opera no Cache de exibição do `Customer-Order` que atende a todas as consultas externas.

_A composição de dados com visão de materialização é adequada para composições com grande número de linhas (m:n une-se com alta cardinalidade) em cada lado._

Os dados desnormalizados podem ser mantidos em um cache ou outro tipo de armazenamento e podem ser consumidos por outros microsserviços como um armazenamento de dados somente leitura.

### Transactions with Microservices

As transações são um conceito importante em aplicativos de software. Eles permitem que você agrupe um conjunto de operações que devem ser executadas juntas em um cenário de tudo ou nada (ou seja, elas são executadas ou todas são revertidas no caso de uma falha). As transações são bastante usadas no contexto de um banco de dados, mas não limitadas a ele. Neste capítulo, nos concentramos principalmente nas transações do banco de dados.

ACID (Atomicidade, Consistência, Isolamento e Durabilidade) é um conjunto de propriedades de transações de banco de dados destinadas a garantir a validade mesmo em caso de falha. Portanto, uma seqüência de operações de banco de dados que satisfaz as propriedades do ACID pode ser considerada uma transação.

Os aplicativos monolíticos geralmente são construídos sobre um único banco de dados relacional centralizado e as transações são usadas para manter os dados em um estado consistente em várias tabelas. Usar as propriedades ACID no aplicativo fornece a capacidade de iniciar uma transação para executar mudanças como inserir, atualizar e excluir e permite consolidar ou reverter a transação. Com aplicativos monolíticos e bancos de dados centralizados, é bastante simples iniciar uma transação, alterar os dados em várias linhas (que podem se estender por várias tabelas) e, finalmente, confirmar a transação.

Com microservices, os requisitos de negócios relacionados a transações não mudariam drasticamente. No entanto, ao contrário de um banco de dados centralizado, os microsserviços têm seu próprio banco de dados e os limites transacionais que abrangem vários serviços e bancos de dados. Portanto, a implementação de tais cenários transacionais não é mais tão direta quanto é com aplicativos monolíticos e bancos de dados centralizados.

#### Avoiding Distributed Transactions with Two-Phase Commit

#### Publishing Events Using Local Transactions

O gerenciamento assíncrono de dados baseado em eventos é bastante comum no gerenciamento de dados de microsserviços. Existem determinados comportamentos transacionais que você pode implementar na arquitetura orientada a eventos que permitem atingir a atomicidade. Por exemplo, suponha que o serviço de `Processamento de Pedidos` ilustrado na Figura abaixo seja responsável por atualizar a tabela `ORDER` e publicar um evento no barramento de eventos de maneira transacional. (ou seja, atualize o pedido e publique o evento de uma só vez).

![Using local transactions to update a database table and create an event](./Images/Microservices/Using%20local%20transactions%20to%20update%20a%20database%20table%20and%20create%20an%20event.png "Using local transactions to update a database table and create an event")

Aqui, usamos uma tabela de eventos para armazenar os eventos de atualização do pedido. Portanto, o serviço de processamento de pedidos pode iniciar uma transação local, que inclui operações de atualização de pedidos e adicionar um evento à tabela `ORDER_EVENT`. Ambas as operações serão executadas no mesmo limite de transação local.

Existe um serviço/processo dedicado que é responsável por consumir a tabela `ORDER_EVENT` e publicar o evento no barramento de eventos. Ele também pode usar transações locais para ler os eventos, publicá-los em um barramento de eventos/intermediário de mensagens e atualizar a tabela de eventos do pedido. O serviço consumidor do evento é responsável por ler os eventos do barramento de eventos e manipulá-los no limite da transação desse serviço.

Essa abordagem evita o uso de transações distribuídas com 2PC, mas tem algumas limitações, como a dependência de um banco de dados que suporta transações (ou seja, a maioria dos bancos de dados NoSQL não suporta transações).

#### Database Log Mining

#### Event Sourcing

Usando as técnicas que discutimos anteriormente (como publicar eventos usando transações locais), podemos persistir cada evento de mudança de estado de uma entidade como uma seqüência de eventos. Todos esses eventos são armazenados em um barramento de eventos e os assinantes podem derivar o estado dessa entidade processando a sequência do evento que ocorreu naquela entidade. Por exemplo, conforme mostrado na Figura abaixo, o serviço de `Order Processing` publica as mudanças que ocorrem na entidade `Order` como eventos (em vez de atualizar a tabela do banco de dados com o status do pedido). Os eventos que mudam de estado - como order created, updated, paid, shipped, etc. - são publicados no barramento de eventos.

![Event sourcing](./Images/Microservices/Event%20sourcing.png "Event sourcing")

Os aplicativos e serviços do assinante podem recriar o status de um pedido simplesmente reproduzindo os eventos que estão ocorrendo em um pedido.

### Polyglot Persistence

Com o gerenciamento de dados descentralizado, você pode aproveitar a técnica persistente mais apropriada para seu caso de uso. Com base no seu caso de uso, um microsserviço pode usar um banco de dados SQL, enquanto outro serviço pode aproveitar um banco de dados NoSQL.

Por exemplo, um microsserviço de um aplicativo de mídia social pode usar um banco de dados relacional / SQL para armazenar suas informações de usuário, enquanto o armazenamento de multimídia é baseado em um banco de dados NoSQL.

### Caching

Como parte das técnicas de gerenciamento de dados para microsserviços, o armazenamento em cache desempenha um papel fundamental, pois melhora a disponibilidade, a escalabilidade e o desempenho de um determinado microsserviço. Em cada nível de microsserviço, podemos armazenar em cache as entidades de negócios nas quais um determinado serviço opera. Geralmente, essas entidades de negócios (ou objetos) não são alteradas com frequência (por exemplo, o serviço de informações do produto armazena em cache o nome e os detalhes do produto em um cache, que será usado com frequência para pesquisa de produtos). Esses dados geralmente podem ser armazenados em cache sob demanda (quando acessamos primeiro as informações do produto a partir do armazenamento de dados subjacente). Além disso, você pode armazenar em cache quaisquer metadados de nível de serviço (configurações ou dados estáticos) durante a inicialização do serviço.

Um dos aspectos mais importantes do armazenamento em cache é não usar uma camada central de armazenamento em cache compartilhada entre microsserviços. No entanto, as instâncias de um determinado microsserviço terão todos os mesmos requisitos de dados, portanto, faz sentido compartilhar uma camada de armazenamento em cache nessas instâncias.


Existem algumas soluções de armazenamento em cache, mas Redis, Ehcache, Hazelcast e Coherence são algumas das implementações de cache mais populares. O Redis, em particular, tem sido amplamente utilizado em cenários de armazenamento em cache de microsserviço nativo e de código aberto. O Redis é um armazenamento de estrutura de dados na memória de software livre, usado como banco de dados, cache e intermediário de mensagens. Ele suporta estruturas de dados como strings, hashes, listas, conjuntos, conjuntos classificados com consultas de intervalo, bitmaps, hiperloglogs e índices geoespaciais com consultas radius. Embora o Redis seja usado principalmente para armazenamento em cache no contexto de microsserviços, ele também pode ser usado como um banco de dados ou intermediário de mensagens(message broker) (publisher-subscriber messaging).

## Integrating Microservices

A arquitetura de microsserviços promove a construção de uma aplicação de software como um conjunto de serviços independentes. Quando temos que realizar um determinado caso de uso de negócios, geralmente precisamos ter a comunicação e a coordenação entre vários microsserviços. Portanto, a integração de microsserviços e a construção de comunicação entre serviços tornou-se uma das tarefas mais desafiadoras necessárias para realizar a arquitetura de microsserviços.

### Why We Have to Integrate Microservices

Os microsserviços são projetados para tratar de funcionalidades específicas de negócios refinadas . Portanto, quando você está construindo um aplicativo de software usando microsserviços, é necessário construir uma estrutura de comunicação entre esses serviços. Ao migrar para a arquitetura de microsserviços, você ainda precisará integrar seus microsserviços para criar casos de uso de negócios significativos. Existem vários requisitos importantes em uma arquitetura de microsserviços que tornam a integração com microsserviços bastante crítica.

- **Composição de microsserviço:** Criando um serviço composto a partir dos microsserviços existentes e expondo isso como uma funcionalidade de negócios aos consumidores é um dos casos de uso mais comuns na arquitetura de microsserviços. A composição pode ser construída usando comunicação síncrona (ativa) ou usando padrões de comunicação assíncrona (reativa).
- **Building resilient inter-service communication:** Todas as chamadas de microsserviço ocorrem na rede e estão sujeitas a falhas. Portanto, precisamos implementar os padrões de estabilidade e resiliência quando fizermos chamadas entre serviços.
- **Granular services and APIs:** A maioria dos microsserviços é muito granular para ser publicada como uma funcionalidade / API de negócios para os consumidores.
- **Microservices in brownfield enterprises:** Os microsserviços em aplicativos corporativos precisam de integração entre sistemas legados existentes, sistemas proprietários (por exemplo, sistemas ERP), bancos de dados e APIs da Web (por exemplo, Salesforce).
