<p align="center">
    <img src="https://d0.awsstatic.com/product-marketing/DevOps/continuous_integration.png">
</p>

# Continuous Integration (Integração Contínua)

- É uma prática de desenvolvimento de software de DevOps em que os desenvolvedores, com frequência, juntam suas alterações de código em um repositório central.

- Os principais objetivos da integração contínua são encontrar e investigar bugs mais rapidamente, melhorar a qualidade do software e reduzir o tempo que leva para validar e lançar novas atualizações de software.

- Fornece um meio de automatizar processos manuais e repetitivos, evitando que os processos sejam esquecidos ou que algum passo importante não seja executado. Assim, você poderá realmente ter confiança de que essas tarefas serão realizadas com precisão e consistência todas as vezes que for preciso.

- É o processo que permite integrar frequentemente alterações parciais em desenvolvimento ao código principal, sem intervenções manuais e com garantias de que funcionem como esperado para um trabalho conjunto e contínuo.

- A integração contínua é a prática de integrar e testar automaticamente uma peça de software cada código de tempo é cometido por um desenvolvedor.

- A ideia é justamente automatizar o processo para que com bastante frequência, uma ou mais vezes ao dia, seja possível integrar todas as alterações de todos os desenvolvedores envolvidos no projeto e realizar um teste geral.

> “Integração Contínua é uma pratica de desenvolvimento de software onde os membros de um time integram seu trabalho frequentemente, geralmente cada pessoa integra pelo menos diariamente – podendo haver multiplas integrações por dia. Cada integração é verificada por um build automatizado (incluindo testes) para detectar erros de integração o mais rápido possível. Muitos times acham que essa abordagem leva a uma significante redução nos problemas de integração e permite que um time desenvolva software coeso mais rapidamente.” *Martin Fowler*

> Em outras palavras, podemos descrever Integração Contínua como integração automática com processo de build automático e que roda testes de forma automática e automaticamente detecta falhas em cada pedaço.

- *Integração contínua* é uma prática que implica uma fusão contínua de ramos em um ramo comum. Faz parte da metodologia Extreme Programming (XP), um método ágil, focado no aspecto de realização de uma aplicação. A implementação da integração contínua exige que a base do código seja construída e testada para cada commit, para evitar qualquer regressão, vinculada a problemas de integração. Esta nova versão da base de código pode então ser implantada em um servidor de integração, de modo que testes manuais adicionais possam ser realizados.

- *Integração contínua* é uma prática de desenvolvimento de software de DevOps em que os desenvolvedores, com frequência, juntam suas alterações de código em um repositório central. Depois disso, criações e testes são executados. Geralmente, a integração contínua se refere ao estágio de criação ou integração do processo de lançamento de software, além de originar um componente de automação (ex.: uma CI ou serviço de criação) e um componente cultural (ex.: aprender a integrar com frequência). Os principais objetivos da integração contínua são encontrar e investigar bugs mais rapidamente, melhorar a qualidade do software e reduzir o tempo que leva para validar e lançar novas atualizações de software.

- É uma distribuição de atualizações mais rápida para os clientes.

- Um serviço de integração contínua detecta confirmações para o repositório compartilhado, além de criar e executar automaticamente testes de unidade nas novas alterações de código, para que qualquer erro funcional ou de integração apareça imediatamente.

- A integração contínua é referente aos estágios de criação e teste de unidade do processo de lançamento de software. Cada revisão confirmada aciona criação e teste automatizados.

# Continuous Delivery (Entrega|Distribuição Contínua)

- É um conjunto de práticas com o objetivo de garantir que um novo código esteja apto para ser disponibilizado em ambiente de produção.

- A entrega contínua dá um passo adiante para empacotar o software, realizar testes de regressão e garantir que o software esteja pronto para a liberação.

- Automatizar o processo na entrega de software.

- Encontrar e resolver bugs automaticamente.

- Fornecer atualizações com mais rapidez.

- Entrega contínua permite que você forneça novos recursos de forma rápida e freqüente sem qualquer regressão de sua aplicação. Para isso, esta prática reduz as tarefas manuais, executa automaticamente testes e cria compilações.

- A entrega contínua vai um passo além da integração contínua. Tudo o que se aplica a esta prática também é válido para este. Um conceito adicional deve ser adicionado: packaging. Cada commit irá desencadear um fluxo de trabalho (a compilação) que irá construir a base do código, testá-lo e criar o pacote que pode ser entregue. Todos os commit podem ser entregues em qualquer momento.

- Com a distribuição contínua, as alterações de código são criadas, testadas e preparadas automaticamente para que a produção seja liberada. A distribuição contínua expande com base na integração contínua ao implantar todas as alterações de código em um ambiente de teste e/ou ambiente de produção, após o estágio de criação.

- A distribuição contínua automatiza o processo de lançamento de software completo. Cada revisão confirmada aciona um fluxo automático que cria, testa e prepara a atualização. A decisão final de implantar em um ambiente de produção ativo é acionada pelo desenvolvedor.

# Continuous Deployment (Implantação Contínua)

- é mais relevante para aplicativos hospedados na Web e SaaS e automatiza o processo até a implantação em produção.

- a fase de deployment que é responsável por publicar o código em produção de forma automatizada.

- Com a implantação contínua, as revisões são implantadas em um ambiente de produção automaticamente, sem aprovação explícita de um desenvolvedor, automatizando todo o processo de lançamento de software.
