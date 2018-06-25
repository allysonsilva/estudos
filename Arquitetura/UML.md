# UML

## Introdução à UML

A UML – Unified Modeling Language ou Linguagem de Modelagem Unificada – é uma linguagem visual utilizada para modelar softwares baseados no paradigma de orientação a objetos. É uma linguagem de modelagem de propósito geral que pode ser aplicada a todos os domínios de aplicação. Essa linguagem é atualmente a linguagem-padrão de modelagem adotada internacionalmente pela indústria de engenharia de software.

Deve ficar bem claro, porém, que a UML não é uma linguagem de programação, e sim uma linguagem de modelagem, ou seja, uma notação cujo objetivo é auxiliar os engenheiros de software a definirem as características do sistema, como seus requisitos, seu comportamento, sua estrutura lógica, a dinâmica de seus processos e até mesmo suas necessidades físicas em relação ao equipamento sobre o qual o sistema deverá ser implantado. Tais características podem ser definidas por meio da UML antes de o software começar a ser realmente desenvolvido. Além disso, cumpre destacar que a UML não é um processo de desenvolvimento de software e tampouco está ligada a um de forma exclusiva, sendo totalmente independente, assim, ela pode ser utilizada por qualquer processo de desenvolvimento ou mesmo da forma que o engenheiro de software considerar mais adequada.

A fase de elicitação de requisitos deve identificar dois tipos de requisitos: os funcionais e os não funcionais. Os requisitos funcionais correspondem ao que o cliente quer que o sistema realize, ou seja, as funcionalidades do software. Já os requisitos não funcionais correspondem a restrições, condições, consistências e validações que devem ser levadas a efeito sobre os requisitos funcionais. Por exemplo, em um sistema bancário, deve ser oferecida a opção de abrir novas contas-correntes, o que é um requisito funcional. Já determinar que somente pessoas maiores de idade possam abrir contas-correntes é um requisito não funcional.

Alguns requisitos não funcionais identificam regras de negócio, ou seja, políticas, normas e condições estabelecidas pela empresa que devem ser seguidas na execução de uma funcionalidade. Por exemplo, estabelecer que depois de abrir uma conta é necessário depositar um valor mínimo inicial é uma regra de negócio adotada por um determinado banco e que não necessariamente é seguida por outras instituições bancárias. Outro exemplo de regra de negócio seria determinar que, em um sistema de biblioteca, só se poderia realizar um novo empréstimo para um sócio se o seu limite máximo de exemplares para locação ainda não tivesse sido atingido, caso contrário ele deveria devolver algum exemplar antes de realizar um novo empréstimo.

Existem ainda diversos tipos de requisitos não funcionais, como de usabilidade, desempenho, confiabilidade, segurança ou interface, que se referem ao sistema como um todo e não estão atrelados a um requisito funcional específico.

Logo após a elicitação de requisitos, passa-se à fase em que as necessidades apresentadas pelo cliente são analisadas. Essa etapa é conhecida como análise de requisitos. Aqui, o engenheiro examina os requisitos enunciados pelos usuários-chave ou stakeholders (profissionais com experiência que têm conhecimento profundo sobre o funcionamento de determinados processos na empresa) verificando se estes foram especificados corretamente e se foram realmente bem compreendidos. A partir da etapa de análise de requisitos são determinadas as reais necessidades do sistema de informação.

### Por que Modelar Software?

Qual a real necessidade de se modelar um software? Muitos “profissionais” podem afirmar que conseguem determinar todas as necessidades de um sistema de informação de cabeça e que sempre trabalharam assim. Qual a real necessidade de se projetar uma casa? Um pedreiro experiente não é capaz de construí-la sem um projeto? Isso pode ser verdade, mas a questão é muito mais ampla, envolvendo fatores complexos, como elicitação e análise de requisitos, projeto, prazos, custos, documentação, manutenção e reusabilidade, entre outros.

Existe uma diferença gritante entre construir uma pequena casa e construir um prédio de vários andares. Obviamente, para se construir um edifício, é necessário, em primeiro lugar, desenvolver um projeto muito bem elaborado, cujos cálculos têm de estar corretos e precisos. Além disso, é preciso fornecer uma estimativa de custos, determinar em quanto tempo a construção estará concluída, avaliar a quantidade de profissionais necessária à execução da obra, especificar a quantidade de material a ser adquirida para a construção, escolher o local onde o prédio será erguido etc. Grandes projetos não podem ser modelados de cabeça, nem mesmo a maioria dos pequenos projetos pode sê-lo, exceto, talvez, aqueles extremamente simples.

Na realidade, por mais simples que seja, todo sistema deve ser modelado antes de se iniciar sua implementação, entre outras coisas, porque os sistemas de informação frequentemente costumam ter tendência a “crescer”, isto é, aumentar em tamanho, complexidade e abrangência. Alguns profissionais costumam afirmar que sistemas de informação são “vivos” porque nunca estão completamente finalizados. Na verdade, o termo correto seria “dinâmicos”, pois normalmente os sistemas de informação estão em constante mudança. Tais mudanças são devidas a diversos fatores, como:

- Os clientes desejam constantemente modificações ou melhorias no sistema.
- O mercado está sempre mudando, o que força a adoção de novas estratégias pelas empresas e, consequentemente, de seus sistemas.
- O governo frequentemente promulga novas leis e cria novos impostos e alíquotas ou, ainda, modifica as leis, os impostos e as alíquotas já existentes, o que acarreta a manutenção no software.

Assim, um sistema de informação precisa ter uma documentação detalhada, precisa e atualizada para ser mantido com facilidade, rapidez e correção, sem produzir novos erros ao corrigir os antigos. Modelar um sistema é uma forma bastante eficiente de documentá-lo, mas a modelagem não serve apenas para isso: a documentação é apenas uma das vantagens fornecidas pela modelagem.

### Rápido Resumo dos Diagramas da UML

#### Diagrama de Casos de Uso

O diagrama de casos de uso tem por objetivo apresentar uma visão externa geral das funcionalidades que o sistema deverá oferecer aos usuários, sem se preocupar muito com a questão de como tais funcionalidades serão implementadas. Costuma ser utilizado principalmente nas fases de elicitação e análise de requisitos do sistema, embora venha a ser consultado durante todo o processo de modelagem e possa servir de base para diversos outros diagramas. Procura apresentar uma linguagem simples e de fácil compreensão para que os usuários possam ter uma ideia geral de como o sistema vai se comportar. Procura identificar os atores (usuários, outros sistemas ou até mesmo algum hardware especial) que utilizarão, de alguma forma, o software, bem como os serviços, ou seja, as funcionalidades que o sistema disponibilizará a esses atores, conhecidas nesse diagrama como casos de uso.

#### Diagrama de Classes

O diagrama de classes é um dos mais importantes e utilizados da UML. Seu principal enfoque está em permitir a visualização das classes que comporão o sistema com seus respectivos atributos e métodos, bem como em demonstrar como as classes do diagrama se relacionam, complementam e transmitem informações entre si. Esse diagrama apresenta uma visão estática de como as classes estão organizadas, preocupando-se em como definir a estrutura lógica delas. O diagrama de classes serve ainda como apoio para a construção da maioria dos outros diagramas da linguagem UML.

#### Diagrama de Objetos

O diagrama de objetos está amplamente associado ao diagrama de classes. Na verdade, o diagrama de objetos é praticamente um complemento do diagrama de classes e bastante dependente deste. O diagrama fornece uma visão dos valores armazenados pelos objetos de um diagrama de classes em um determinado momento da execução de um processo do software.

#### Diagrama de Pacotes

O diagrama de pacotes é um diagrama estrutural que tem por objetivo representar como os elementos do modelo estão divididos logicamente. Tais elementos podem ser, por exemplo, subsistemas ou componentes englobados por um sistema ou as camadas que o compõem, entre outras possibilidades. Essas divisões lógicas são denominadas Pacotes. Esse diagrama pode ser utilizado de maneira independente ou associado a outros diagramas.

#### Diagrama de Sequência

O diagrama de sequência é um diagrama comportamental que se preocupa com a ordem temporal em que as mensagens são trocadas entre os objetos envolvidos em um determinado processo. Em geral, baseia-se em um caso de uso definido pelo diagrama de mesmo nome e apoia-se no diagrama de classes para determinar os objetos das classes envolvidas em um processo. Um diagrama de sequência costuma identificar o evento gerador do processo modelado, bem como o ator responsável por esse evento, e determina como o processo deve se desenrolar e ser concluído por meio da chamada de métodos disparados por mensagens enviadas entre os objetos.

#### Diagrama de Comunicação

O diagrama de comunicação era conhecido como de colaboração até a versão 1.5 da UML, tendo seu nome modificado para diagrama de comunicação a partir da versão 2.0. Está amplamente associado ao diagrama de sequência: na verdade, um complementa o outro. As informações mostradas no diagrama de comunicação com frequência são praticamente as mesmas apresentadas no de sequência, porém com um enfoque distinto, visto que esse diagrama não se preocupa com a temporalidade do processo, concentrando-se em como os elementos do diagrama estão vinculados e quais mensagens trocam entre si durante o processo.

#### Diagrama de Máquina de Estados

O diagrama de máquina de estados demonstra o comportamento de um elemento por meio de um conjunto finito de transições de estado. Esse diagrama pode ser utilizado para expressar o comportamento de uma parte do sistema, quando é chamado de máquina de estado comportamental, ou o protocolo de uso de parte de um sistema, quando identifica uma máquina de estados de protocolo. Uma máquina de estados comportamental pode ser usada para especificar o comportamento de vários elementos do modelo. O elemento modelado muitas vezes é uma instância de uma classe. No entanto, pode-se usar esse diagrama para modelar o comportamento de um caso de uso, por exemplo.

#### Diagrama de Atividade

O diagrama de atividade preocupa-se em descrever os passos a serem percorridos para a conclusão de uma atividade específica, podendo esta ser representada por um método com certo grau de complexidade, um algoritmo, ou mesmo um processo completo. O diagrama de atividade concentra-se na representação do fluxo de controle e de objetos de uma atividade.

#### Diagrama de Visão Geral de Interação

O diagrama de visão geral de interação é uma variação do diagrama de atividade que fornece uma visão geral dentro de um sistema ou processo de negócio, podendo englobar vários subprocessos. Esse diagrama passou a existir apenas a partir da UML 2.

#### Diagrama de Componentes

O diagrama de componentes, como seu próprio nome indica, identifica os componentes que fazem parte de um sistema, um subsistema ou mesmo os componentes ou classes internas de um componente individual. Um componente pode representar tanto um componente lógico (um componente de negócio ou de processo) quanto um componente físico, como arquivos contendo código-fonte, arquivos de ajuda (help), bibliotecas, arquivos executáveis etc.

#### Diagrama de Implantação

O diagrama de implantação determina as necessidades de hardware do sistema, como servidores, estações, topologias e protocolos de comunicação, ou seja, todo o aparato físico sobre o qual o sistema deverá ser executado. Esse diagrama permite demonstrar também como se dará a distribuição dos módulos do sistema, em situações em que estes forem executados em mais de um servidor.

#### Diagrama de Estrutura Composta

O diagrama de estrutura composta descreve a estrutura interna de um classificador, como uma classe ou componente, detalhando as partes internas que o compõem, como estas se comunicam e colaboram entre si. Também é utilizado para descrever uma colaboração em que um conjunto de instâncias coopera entre si para realizar uma tarefa.

## Introdução a Objetos

### Herança

a herança é uma das características mais poderosas e importantes da orientação a objetos. Isso se deve ao fato de permitir o reaproveitamento de atributos e métodos, otimizando o tempo de desenvolvimento, além de permitir a diminuição de linhas de código, bem como facilitar futuras manutenções.

A herança na orientação a objetos trabalha com os conceitos de superclasses e subclasses. Uma superclasse, também chamada de classe-mãe, contém classes derivadas dela, chamadas subclasses, também conhecidas como classes-filha. As subclasses, ao serem derivadas de uma superclasse, herdam suas características, ou seja, seus atributos e métodos.

A vantagem do uso da herança é óbvia: ao declararmos uma classe com atributos e métodos específicos e, depois disso, derivarmos uma subclasse da classe já criada, não precisamos redeclarar os atributos e métodos previamente definidos: a subclasse herda-os automaticamente, permitindo reutilização do código já pronto. Assim, só precisamos nos preocupar em declarar os atributos ou métodos exclusivos da subclasse, o que torna muito mais ágil o processo de desenvolvimento, além de facilitar igualmente futuras manutenções, sendo necessário apenas alterar o método da superclasse para que todas as subclasses estejam também atualizadas imediatamente.

Além disso, a herança permite trabalhar com especializações. Podemos criar classes gerais, com características compartilhadas por muitas classes, mas que tenham pequenas diferenças entre si. Assim, criamos uma classe geral com as características comuns a todas as classes e diversas subclasses a partir dela, detalhando apenas os atributos ou métodos exclusivos destas. Uma subclasse pode se tornar uma superclasse a qualquer momento, bastando para tanto que se derive uma subclasse dela.

### Polimorfismo

O conceito de polimorfismo está associado à herança. O polimorfismo trabalha com a redeclaração de métodos previamente herdados por uma classe. Esses métodos, embora semelhantes, diferem de alguma forma da implementação utilizada na superclasse, sendo necessário, portanto, reimplementá-los na subclasse.

Porém, para evitar ter de modificar o código-fonte, inserindo uma chamada a um método com um nome diferente, redeclara-se o método com o mesmo nome declarado na superclasse. Assim, podem existir dois ou mais métodos com a mesma nomenclatura, diferenciando-se na maneira como foram implementados, sendo o sistema responsável por verificar se a classe da instância em questão contém o método declarado nela própria ou se o herda de uma superclasse.

## Diagrama de Casos de Uso

O diagrama de casos de uso procura possibilitar a compreensão do comportamento externo do sistema (em termos de funcionalidades oferecidas por ele) por qualquer pessoa com algum conhecimento sobre o problema enfocado, tentando apresentar o sistema por intermédio de uma perspectiva dos usuários.

Esse diagrama costuma ser utilizado, sobretudo, no início da modelagem do sistema, principalmente nas etapas de elicitação e análise de requisitos, embora venha a ser consultado e possivelmente modificado durante todo o processo de engenharia e sirva de base para a modelagem de outros diagramas. Esse diagrama tem por objetivo apresentar uma visão externa geral das funcionalidades que o sistema deverá oferecer aos usuários, sem se preocupar em profundidade com a questão de como tais funcionalidades serão implementadas. O diagrama de casos de uso é de grande auxílio para a identificação e compreensão dos requisitos funcionais do sistema, ajudando a especificar, visualizar e documentar as funções e serviços do software desejados pelos clientes e stakeholders (usuários com conhecimento sobre como a informação é gerida e modificada em determinados setores da empresa e que possuem interesse no software). O diagrama de casos de uso tenta identificar os tipos de usuários que interagirão com o sistema, quais papéis eles assumirão e quais funções um usuário específico poderá requisitar.

De acordo com diversos autores, casos de uso são de interesse especial na engenharia de requisitos, uma vez que se mostraram valiosos para elicitar, documentar e analisar requisitos, sobretudo os funcionais, ou seja, que identificam as funções que devem ser fornecidas pelo software. Além disso, casos de uso também permitem a rastreabilidade de requisitos, ou seja, qual a origem de um requisito e por que foi considerado importante nas fases de projeto, implementação e verificação e validação.

Por utilizar uma linguagem menos formal e apresentar uma visão geral do comportamento do sistema a ser desenvolvido, o diagrama de casos de uso pode e deve ser apresentado durante as reuniões iniciais com os stakeholders para ilustrar as funcionalidades e o comportamento do sistema de informação, facilitar a compreensão dos stakeholders do que se pretende desenvolver e auxiliar na identificação de possíveis falhas de especificação, verificando se os requisitos do sistema foram bem compreendidos. É bastante útil e recomendável que um diagrama de casos de uso seja apresentado aos clientes com um protótipo, o que permitirá que um complemente o outro.

### Atores

O diagrama de casos de uso concentra-se em dois itens principais: atores e casos de uso. Os atores costumam representar os papéis desempenhados pelos diversos usuários que poderão utilizar, de alguma maneira, os serviços e funções do sistema. Eventualmente, um ator pode representar algum hardware especial ou mesmo outro software que interaja com o sistema, como no caso de um sistema integrado, por exemplo.

Assim, um ator pode ser qualquer elemento externo que interaja com o software, porém, na maioria das vezes, um ator representará uma pessoa que utilizará o sistema. Esse elemento recebe o nome de ator porque um usuário pode representar mais de um papel no sistema. Por exemplo, um usuário pode se logar em um software com um nível de permissões menor, sendo capaz de utilizar poucas funções do sistema, ou pode se logar com um nível de permissão mais alto e utilizar mais recursos, ou pode ainda se logar como administrador e ter acesso a todas ou à maioria das funcionalidades do sistema. Em cada situação, o papel representado pelo usuário será diferente, por isso o ator não representa um usuário propriamente dito, mas sim um papel que pode vir a ser desempenhado por um ou mais usuários.

Os atores são representados por símbolos de “bonecos magros”, contendo uma breve descrição logo abaixo de seu símbolo que identifica o papel que o ator em questão assume dentro do diagrama.

*Estereótipo serve para destacar um componente ou associação, atribuindo-lhe características especiais em relação a seus iguais.*

### Casos de Uso

Os casos de uso são utilizados para capturar os requisitos funcionais do sistema, ou seja, referem-se a serviços, tarefas ou funcionalidades identificados como necessários ao software e que podem ser utilizados de alguma maneira pelos atores que interagem com o sistema. Assim, casos de uso expressam e documentam os comportamentos pretendidos para as funções do software.

Casos de uso podem ser classificados em casos de uso primários ou secundários. Um caso de uso é considerado primário quando se refere a um processo importante, que enfoca um dos requisitos funcionais do software, como realizar um saque ou emitir um extrato em um sistema de controle bancário. Já um caso de uso secundário se refere a um processo periférico, como a manutenção de um cadastro ou a emissão de um relatório simples. Em situações em que há um grande número de casos de uso, é recomendável limitar a representação de casos de uso secundários, representando-os de maneira geral ou mesmo não os representando de forma alguma, para evitar poluir demais o diagrama.

Os casos de uso costumam ser documentados. A documentação de um caso de uso fornece instruções em linhas gerais de como será seu funcionamento, quais atores poderão utilizá-lo, quais atividades deverão ser executadas pelos atores e pelo sistema, qual evento forçará sua execução e quais suas possíveis restrições, entre outras informações.

Normalmente, as ações contidas na documentação de um caso de uso são descritas em termos gerais, embora nada impeça que o engenheiro de software insira detalhes mais técnicos, até mesmo de implementação na documentação, o que não é recomendado. A documentação deve representar o comportamento de um caso de uso da maneira o mais completa possível, mas sem detalhes técnicos. Um detalhamento maior será acrescido por outros diagramas em fases posteriores.
