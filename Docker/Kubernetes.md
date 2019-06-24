# Kubernetes

- > Um `ReplicaSet` só pode gerenciar um tipo de Pod.

- > `Deployments` se baseiam no `ReplicaSets`.

## Understanding Kubernetes Architecture

O Kubernetes permite que você execute seus aplicativos de software em milhares de nós de computador como se todos esses nós fossem um único e enorme computador. Ele abstrai a infra-estrutura subjacente e, ao fazer isso, simplifica o desenvolvimento, a implantação e o gerenciamento para as equipes de desenvolvimento e de operações.

### O que é o Kubernetes?

O Kubernetes é uma plataforma que engloba um grande número de serviços e recursos que continuam crescendo. Sua principal funcionalidade é a capacidade de agendar cargas de trabalho em contêineres em toda a sua infraestrutura, mas isso não para por aí. Aqui estão algumas das outras capacidades que o Kubernetes traz para seu catálogo:

- Mounting storage systems
- Distributing secrets
- Checking application health
- Replicating application instances
- Using horizontal pod autoscaling
- Naming and discovering
- Balancing loads
- Rolling updates
- Monitoring resources
- Accessing and ingesting logs
- Debugging applications
- Providing authentication and authorization

### Kubernetes concepts

#### Deployments

Deployments determinam como as instâncias de pod são criadas, implantadas e mantidas. Você define o Deployment desejado usando um arquivo de configuração e o Kubernetes lida com o processo de implementação do estado desejado. Os Deployments também gerenciam a replicação, o dimensionamento e o reinício dos pods.

Uma Deployment descreve o estado desejado de um cluster. Você define parâmetros como os Pods para criar, o número de réplicas para cada Pod e se os pods devem ser dimensionados durante a alta demanda. O Deployment Controller não apenas implanta os Pods para corresponder à sua configuração, mas também monitora e mantém o cluster para que ele sempre corresponda à sua configuração. Os Deployments também suportam atualizações contínuas e reversões de forma a garantir que pelo menos um Pod esteja on-line a qualquer momento.

#### Cluster

Um cluster é uma coleção de recursos de computação, armazenamento e rede que o Kubernetes usa para executar as várias cargas de trabalho que compõem seu sistema. Observe que todo o seu sistema pode consistir em vários clusters.

#### Node

Um nó é um único host. Pode ser uma máquina física ou virtual. Seu trabalho é executar Pods, que vamos analisar em um momento. Cada nó do Kubernetes executa vários componentes do Kubernetes, como um kubelet e um proxy kube. Os nós são gerenciados por um Master do Kubernetes. Os nós são as abelhas operárias do Kubernetes e suportam todo o trabalho pesado.

#### Master

O mestre é o plano de controle do Kubernetes. Ele consiste em vários componentes, como um servidor de API, um scheduler e um gerenciador de controlador. O mestre é responsável pelo agendamento global de Pods no nível do cluster e pela manipulação de eventos. Geralmente, todos os componentes principais são configurados em um único host. Ao considerar cenários de alta disponibilidade ou clusters muito grandes, você desejará ter redundância no mestre.

#### Pod

Um pod é a unidade de trabalho no Kubernetes. Cada pod contém um ou mais contêineres. Os pods são sempre agendados juntos (ou seja, eles sempre são executados na mesma máquina). Todos os contêineres em um pod têm o mesmo endereço IP e espaço de porta; eles podem se comunicar usando localhost ou comunicação interprocessual padrão. Além disso, todos os contêineres em um pod podem ter acesso ao armazenamento local compartilhado no nó que hospeda o pod. O armazenamento compartilhado pode ser montado em cada contêiner. Os pods são um recurso importante do Kubernetes. É possível executar vários aplicativos dentro de um único contêiner do Docker tendo algo como o `supervisord` como o principal aplicativo do Docker que executa vários processos, mas essa prática costuma ser desaprovada pelos seguintes motivos:

- **Transparência:** Tornar os contêineres dentro do conjunto visível para a infraestrutura permite que a infraestrutura forneça serviços para esses contêineres, como gerenciamento de processos e monitoramento de recursos. Isso facilita várias funcionalidades convenientes para os usuários.
- **Desacoplando dependências de software:** Os contêineres individuais podem ser versionados, reconstruídos e reimplantados de forma independente. O Kubernetes pode até mesmo suportar atualizações ao vivo de contêineres individuais algum dia.
- **Facilidade de uso:** Os usuários não precisam executar seus próprios gerenciadores de processos, se preocupar com propagação de sinal e código de saída e assim por diante.
- **Eficiência:** porque a infra-estrutura assume mais responsabilidade, os containers podem ser mais leves.

Os pods fornecem uma ótima solução para o gerenciamento de grupos de contêineres intimamente relacionados que dependem uns dos outros e precisam cooperar no mesmo host para realizar sua finalidade. É importante lembrar que os pods são considerados entidades efêmeras e descartáveis ​​que podem ser descartadas e substituídas à vontade. Qualquer armazenamento de pods é destruído com o seu pod. Cada pod recebe um ID exclusivo (UID), então você ainda pode distinguir entre eles, se necessário.

-------------

Be sure to take that and mark it in your brain as important - Virtualization does VM’s, Docker does containers, and **Kubernetes does Pods**!

**Pod theory summary**

1. Pods are the smallest unit of scheduling on Kubernetes
2. You can have more than one container in a Pod. Single-container Pods are the most common type, but multi-container Pods are ideal for containers that need to be tightly coupled - maybe they need to share memory or volumes.
3. Pods get scheduled on nodes – you can’t schedule a single Pod to span multiple nodes.
4. They get defined declaratively in a manifest file that we POST to the API server and let the scheduler assign them to nodes.

#### Label

Os Labels são pares de chave-valor que são usados ​​para agrupar conjuntos de objetos, muitas vezes, pods. Isso é importante para vários outros conceitos, como controladores de replicação, conjuntos de réplicas e serviços que operam em grupos dinâmicos de objetos e precisam identificar os membros do grupo. Existe um relacionamento NxN entre objetos e Labels. Cada objeto pode ter vários Labels e cada Label pode ser aplicado a objetos diferentes. Há determinadas restrições nos Labels por design. Cada Label em um objeto deve ter uma chave exclusiva. A chave do Label deve obedecer a uma sintaxe estrita. Tem duas partes: prefixo e nome. O prefixo é opcional. Se existir, ele será separado do nome por uma barra (/) e deve ser um subdomínio DNS válido. O prefixo deve ter no máximo 253 caracteres. O nome é obrigatório e deve ter no máximo 63 caracteres. Os nomes devem começar e terminar com um caractere alfanumérico (az, AZ, 0-9) e conter apenas caracteres alfanuméricos, pontos, traços e sublinhados. Os valores seguem as mesmas restrições dos nomes. Observe que os Labels são dedicados à identificação de objetos e não à anexação de metadados arbitrários a objetos. Isto é o que as anotações são para.

#### Annotations

As anotações permitem associar metadados arbitrários a objetos do Kubernetes. O Kubernetes apenas armazena as anotações e disponibiliza seus metadados. Ao contrário dos Labels, eles não têm restrições rígidas sobre os caracteres permitidos e os limites de tamanho.

Na minha experiência, você sempre precisa desses metadados para sistemas complicados, e é bom que o Kubernetes reconheça essa necessidade e ofereça-a para que você não tenha que criar seu próprio repositório de metadados e mapear objetos para seus metadados.

#### Replication controllers and replica sets

Controladores de replicação e conjuntos de réplicas gerenciam um grupo de pods identificados por um seletor de Label e garantem que um determinado número esteja sempre ativo e em execução. A principal diferença entre eles é que os controladores de replicação testam a associação por igualdade de nome e os conjuntos de réplica podem usar a seleção baseada em conjunto. Conjuntos de réplicas são o caminho a percorrer, pois são um superconjunto de controladores de replicação. Espero que os controladores de replicação sejam descontinuados em algum momento.

O Kubernetes garante que você sempre terá o mesmo número de pods em execução que você especificou em um controlador de replicação ou em um conjunto de réplicas. Sempre que o número cair devido a um problema com o nó de hospedagem ou com o próprio pod, o Kubernetes irá disparar novas instâncias. Observe que, se você iniciar manualmente os pods e exceder o número especificado, o controlador de replicação eliminará os pods extras.

Os controladores de replicação costumavam ser centrais em muitos fluxos de trabalho, como atualizações contínuas e execução de tarefas únicas. Com a evolução do Kubernetes, ele introduziu suporte direto para muitos desses fluxos de trabalho, com objetos dedicados, como Deployment, Job e DaemonSet.

-------------

Um ReplicaSet é um objeto de nível superior do Kubernetes que envolve um Pod e adiciona recursos. Como os nomes sugerem, eles pegam um modelo de Pod e implantam um número desejado de réplicas dele. Eles também instanciam um loop de reconciliação em background que verifica se o número correto de réplicas está sempre em execução - estado desejado versus estado real.

Os ReplicaSets podem ser implantados diretamente. Mas, na maioria das vezes, eles são implantados indiretamente por meio de objetos de nível superior, como Deployments.

-------------

The Pod template tells the ReplicaSet what type of Pods to deploy. Things like; container image, network ports, and even labels. The desired number of replicas tell the ReplicaSet how many of these Pods to deploy.

#### Services

Pods são mortais e podem morrer. Se eles são implantados via ReplicaSets ou Deployments, quando eles falham, eles são substituídos por novos Pods em algum outro lugar no cluster - esses Pods têm IPs totalmente diferentes! Isso também acontece quando dimensionamos um aplicativo. Os novos Pods chegam com seus próprios novos IPs. Isso também acontece ao executar atualizações contínuas - o processo de substituir Pods antigos por novos Pods resulta em muita rotatividade de IP.

A moral desta história é que não podemos confiar nos Pod IPs. Mas isso é um problema. Suponhamos que temos um aplicativo de microsserviço com um back-end de armazenamento persistente que outras partes do aplicativo usam para armazenar e recuperar dados. Como isso funcionará se não pudermos confiar nos endereços IP dos pods de back-end?

É aqui que os Serviços entram para jogar. Os serviços fornecem um ponto de extremidade de rede confiável para um conjunto de pods.

Uma última coisa sobre serviços. Eles só enviam tráfego para Pods Saudáveis. Isso significa que, se um Pod falhar nas verificações de integridade, ele não receberá tráfego do Serviço. Então, sim, os serviços trazem endereços IP estáveis e nomes DNS para o mundo instável do Pods!

_For a Service to match a set of Pods, and therefore provide stable networking and load-balance, it only needs to match some of the Pods labels. However, for a Pod to match a Service, the Pod must match all of the values in the Service’s label selector._

_Services are all about providing a stable networking endpoint for Pods. They also provide load-balancing and a cluster-wide port that the Service can be accessed on from outside of the cluster. We associate them with Pods using labels and label selectors._

_A maneira como um Serviço sabe quais Pods para balancear a carga é através de Labels._

![Labels in services](./Images/Kubernetes/Labels%20in%20services.png "Labels in services")

Figure shows an example that does work. It works because the Service is selecting on two labels and the Pods have both. It doesn’t matter that the Pods have additional labels that the Service is not selecting on. The Service selector is looking for Pods with two labels, it finds them, and ignores the fact that the Pods have additional labels - all that is important is that the Pods have the labels the Service is looking for.

![Labels in services and pods](./Images/Kubernetes/Labels%20in%20services%20and%20pods.png "Labels in services and pods")

--------------

- Um Serviço do Kubernetes é uma abstração que define um conjunto lógico de Pods e uma política pela qual acessá-los do mundo externo.
- Vamos dizer que há um pod executando seu aplicativo da web, para expor este aplicativo da web você precisa de serviços.
- Dá-lhe uma maneira de acessar seus Pods, balanceamento de carga entre seus Pods.
- Ele esconde como gerencia os Pods internamente.

Os serviços são usados ​​para expor uma certa funcionalidade aos usuários ou outros serviços. Eles geralmente englobam um grupo de pods, geralmente identificado por - você adivinhou - um Label. Você pode ter serviços que fornecem acesso a recursos externos ou a pods que você controla diretamente no nível de IP virtual. Os serviços do Kubernetes Nativo são expostos através de endpoints convenientes. Observe que os serviços operam na camada 3 (TCP/UDP). O Kubernetes 1.2 adicionou o objeto `Ingress`, que fornece acesso a objetos HTTP. Os serviços são publicados ou descobertos por meio de um dos dois mecanismos: DNS ou variáveis ​​de ambiente. Os serviços podem ser load balanced pelo Kubernetes, mas os desenvolvedores podem optar por gerenciar o balanceamento de carga no caso de serviços que usam recursos externos ou exigem tratamento especial.

#### Volume

O armazenamento local no pod é efêmero e desaparece com o pod. Às vezes, isso é tudo que você precisa, se o objetivo for apenas trocar dados entre os contêineres do nó, mas às vezes é importante que os dados sobrevivam ao pod ou é necessário compartilhar dados entre os pods. O conceito de volume suporta essa necessidade. Observe que, embora o Docker tenha um conceito de volume, ele é bastante limitado (embora esteja ficando mais poderoso). O Kubernetes usa seus próprios volumes separados. O Kubernetes também suporta tipos adicionais de contêineres.

Existem muitos tipos de volume. Atualmente, o Kubernetes suporta diretamente muitos tipos de volume, mas a abordagem moderna para estender o Kubernetes com mais tipos de volume é através da **Container Storage Interface (CSI)**, que discutirei em detalhes posteriormente. O tipo de volume `emptyDir` monta um volume em cada contêiner que é suportado por padrão pelo que estiver disponível na máquina de hospedagem. Você pode solicitar um meio de memória, se quiser. Esse armazenamento é excluído quando o pod é encerrado por qualquer motivo. Existem muitos tipos de volume para ambientes de nuvem específicos, vários sistemas de arquivos em rede e até mesmo repositórios Git. Um tipo de volume interessante é o `persistentDiskClaim`, que abstrai os detalhes um pouco e usa o armazenamento persistente padrão em seu ambiente (normalmente em um provedor de nuvem).

#### StatefulSet

#### Secrets

Segredos são pequenos objetos que contêm informações confidenciais, como credenciais e tokens. Eles são armazenados no etcd, podem ser acessados ​​pelo servidor da API do Kubernetes e podem ser montados como arquivos em pods (usando volumes secretos dedicados que utilizam volumes de dados regulares) que precisam de acesso a eles. O mesmo segredo pode ser montado em vários pods. O próprio Kubernetes cria segredos para seus componentes e você pode criar seus próprios segredos. Outra abordagem é usar segredos como variáveis ​​de ambiente. Observe que os segredos em um pod são sempre armazenados na memória (`tmpfs`, no caso de segredos mounted) para melhor segurança.

#### Names

Cada objeto no Kubernetes é identificado por um UID e um nome. O nome é usado para se referir ao objeto em chamadas de API. Os nomes devem ter até 253 caracteres e usar caracteres alfanuméricos minúsculos, traços (-) e pontos (.). Se você excluir um objeto, poderá criar outro objeto com o mesmo nome do objeto excluído, mas os UIDs devem ser exclusivos em todo o ciclo de vida do cluster. Os UIDs são gerados pelo Kubernetes, então você não precisa se preocupar com isso.

#### Namespaces

Um namespace é um cluster virtual. Você pode ter um único cluster físico que contenha vários clusters virtuais segregados por namespaces. Cada cluster virtual é totalmente isolado dos outros clusters virtuais e só pode se comunicar por meio de interfaces públicas. Observe que objetos de node e volumes persistentes não vivem em um Namespaces. O Kubernetes pode agendar pods de diferentes namespaces para serem executados no mesmo nó. Da mesma forma, os pods de diferentes namespaces podem usar o mesmo armazenamento persistente.

Ao usar namespaces, você deve considerar as diretivas de rede e as cotas de recursos para garantir o acesso e a distribuição adequados dos recursos físicos do cluster.

## PODs: running containers in Kubernetes

### Introducing pods

You’ve already learned that a pod is a co-located group of containers and represents the basic building block in Kubernetes. Instead of deploying containers individually, you always deploy and operate on a pod of containers. We’re not implying that a pod always includes more than one container—it’s common for pods to contain only a single container. The key thing about pods is that when a pod does contain multiple containers, all of them are always run on a single worker node—it never spans multiple worker nodes, as shown in figure.

![All containers of a pod run on the same node. A pod never spans two nodes](./Images/Kubernetes/All%20containers%20of%20a%20pod%20run%20on%20the%20same%20node.%20A%20pod%20never%20spans%20two%20nodes.png "All containers of a pod run on the same node. A pod never spans two nodes")

#### Compreendendo os pods

Como você não deve agrupar vários processos em um único contêiner, é óbvio que precisa de outra construção de nível superior que permita vincular contêineres e gerenciá-los como uma única unidade. Esse é o raciocínio por trás dos pods.

Um conjunto de contêineres permite que você execute processos intimamente relacionados juntos e forneça-os com (quase) o mesmo ambiente, como se estivessem todos em um único contêiner, mantendo-os um pouco isolados. Dessa forma, você obtém o melhor dos dois mundos. Você pode aproveitar todos os recursos oferecidos pelos contêineres e, ao mesmo tempo, dar aos processos a ilusão de estarem juntos.

Uma coisa a ser ressaltada aqui é que, como os contêineres em um pod são executados no mesmo namespace de rede, eles compartilham o mesmo endereço IP e espaço de porta. Isso significa que os processos que estão sendo executados em contêineres do mesmo pod precisam tomar cuidado para não se vincular aos mesmos números de porta ou se encontrarão em conflitos de porta. Mas isso só diz respeito a contêineres no mesmo pod. Contêineres de diferentes pods nunca podem entrar em conflito de portas, porque cada pod tem um espaço de porta separado. Todos os contêineres em um pod também têm a mesma interface de rede de loopback, portanto, um contêiner pode se comunicar com outros contêineres no mesmo pod por meio do host local.

Para resumir: os pods são hosts lógicos e se comportam de maneira muito semelhante a hosts físicos ou VMs no mundo sem contêineres. Processos em execução no mesmo pod são como processos em execução na mesma máquina física ou virtual, exceto pelo fato de cada processo ser encapsulado em um contêiner.

#### Organizando contêineres nos pods corretamente

Você deve pensar nos pods como máquinas separadas, mais onde cada um hospeda apenas um determinado aplicativo. Ao contrário dos velhos tempos, quando costumávamos empilhar todos os tipos de aplicativos no mesmo host, não fazemos isso com os pods. Como os pods são relativamente leves, você pode ter quantos você precisar sem incorrer em quase nenhuma sobrecarga. Em vez de colocar tudo em um único pod, você deve organizar os aplicativos em vários pods, onde cada um contém apenas componentes ou processos altamente relacionados.

Dito isto, você acha que um aplicativo multicamadas que consiste em um servidor de aplicativos front-end e um de back-end deve ser configurado como um único pod ou como dois pods?

##### SPLITTING MULTI-TIER APPS INTO MULTIPLE PODS

Embora nada o impeça de executar o servidor front-end e o banco de dados em um único conjunto com dois contêineres, esse não é o modo mais apropriado. Dissemos que todos os contêineres do mesmo pod sempre são executados no mesmo local, mas o servidor da web e o banco de dados realmente precisam ser executados na mesma máquina? A resposta é obviamente não, então você não quer colocá-los em um único pod. Mas é errado fazer isso independentemente? De certa forma, é.

Se o frontend e o backend estiverem no mesmo pod, ambos serão sempre executados na mesma máquina. Se você tiver um cluster Kubernetes de dois nós e apenas esse único conjunto, você estará usando apenas um único nó de trabalho e não aproveitando os recursos computacionais (CPU e memória) que você tem à sua disposição no segundo nó. Dividir o pod em dois permitiria que o Kubernetes agendasse o frontend para um nó e o backend para o outro nó, melhorando assim a utilização de sua infraestrutura.

##### SPLITTING INTO MULTIPLE PODS TO ENABLE INDIVIDUAL SCALING

Outra razão pela qual você não deve colocá-los em um único pod é o dimensionamento. Um pod também é a unidade básica de dimensionamento. O Kubernetes não pode dimensionar horizontalmente contêineres individuais; em vez disso, dimensiona pods inteiros. Se o seu pod consiste em um frontend e um contêiner backend, quando você aumenta o número de instâncias do pod para, digamos, dois, você acaba com dois contêineres front-end e dois contêineres backend.

Normalmente, os componentes do frontend têm requisitos de dimensionamento completamente diferentes dos backends, portanto, tendemos a escalá-los individualmente. Sem mencionar o fato de que os backends, como os bancos de dados, são geralmente muito mais difíceis de escalar em comparação com os servidores Web frontend (stateless). Se você precisar dimensionar um contêiner individualmente, essa é uma clara indicação de que ele precisa ser implantado em um pod separado.

##### UNDERSTANDING WHEN TO USE MULTIPLE CONTAINERS IN A POD

A principal razão para colocar vários contêineres em um único pod é quando o aplicativo consiste em um processo principal e um ou mais processos complementares, conforme mostrado na figura abaixo.

![Pods should contain tightly coupled containers, usually a main container and containers that support the main one](./Images/Kubernetes/Pods%20should%20contain%20tightly%20coupled%20containers,%20usually%20a%20main%20container%20and%20containers%20that%20support%20the%20main%20one.png "Pods should contain tightly coupled containers, usually a main container and containers that support the main one")

Por exemplo, o contêiner principal em um pod pode ser um servidor da web que exibe arquivos de um determinado diretório de arquivos, enquanto um contêiner adicional (um contêiner sidecar) faz o download de conteúdo de uma fonte externa e o armazena no diretório do servidor.

##### DECIDING WHEN TO USE MULTIPLE CONTAINERS IN A POD

Para recapitular como os contêineres devem ser agrupados em pods - ao decidir colocar dois contêineres em um único pod ou em dois pods separados, você sempre precisa se fazer as seguintes perguntas:

- Eles precisam ser executados juntos ou podem ser executados em hosts diferentes?
- Eles representam um todo ou são componentes independentes?
- Eles devem ser escalados juntos ou individualmente?

Basicamente, você deve sempre preferir executar contêineres em pod separados, a menos que um motivo específico exija que eles façam parte do mesmo pod.

![A container shouldn’t run multiple processes. A pod shouldn’t contain multiple containers if they don’t need to run on the same machine](./Images/Kubernetes/A%20container%20shouldn’t%20run%20multiple%20processes.%20A%20pod%20shouldn’t%20contain%20multiple%20containers%20if%20they%20don’t%20need%20to%20run%20on%20the%20same%20machine.png "A container shouldn’t run multiple processes. A pod shouldn’t contain multiple containers if they don’t need to run on the same machine")

##### INTRODUCING THE MAIN PARTS OF A POD DEFINITION

- **Metadata** includes the name, namespace, labels, and other information about the pod.
- **Spec** contains the actual description of the pod’s contents, such as the pod’s containers, volumes, and other data.
- **Status** contains the current information about the running pod, such as what condition the pod is in, the description and status of each container, and the pod’s internal IP and other basic info.

### Creating pods from YAML or JSON descriptors

#### Using kubectl create to create the pod

To create the pod from your YAML file, use the kubectl create command:

```
kubectl create -f pod-manifest.yaml
```

_O comando `kubectl create -f` é usado para criar qualquer recurso (não apenas pods) de um arquivo YAML ou JSON._

##### RETRIEVING THE WHOLE DEFINITION OF A RUNNING POD

Depois de criar o pod, você pode pedir ao Kubernetes o YAML completo do pod:

```
kubectl get po pod-name -o yaml
```

Se você estiver mais interessado em JSON, também pode dizer ao `kubectl` para retornar JSON em vez de YAML assim (isso funciona mesmo se você usou YAML para criar o pod):

```
kubectl get po pod-name -o json
```

##### SEEING YOUR NEWLY CREATED POD IN THE LIST OF PODS

Your pod has been created, but how do you know if it’s running? Let’s list pods to see their statuses:

```
kubectl get pods
```

#### Viewing application logs

##### RETRIEVING A POD’S LOG WITH KUBECTL LOGS

Para ver o log do seu pod (mais precisamente, o log do contêiner) você executa o seguinte comando em sua máquina local:

```
kubectl logs pod-name
```

##### SPECIFYING THE CONTAINER NAME WHEN GETTING LOGS OF A MULTI-CONTAINER POD

Se o seu pod inclui vários contêineres, você deve especificar explicitamente o nome do contêiner incluindo a opção `-c <container name>`ao executar `kubectl logs`.

```
kubectl logs pod-name -c container-name
```

Observe que você só pode recuperar logs de contêiner de pods ainda existentes. Quando um pod é excluído, seus logs também são excluídos. Para disponibilizar os registros de um pod mesmo depois de excluí-lo, você precisa configurar o registro centralizado em todo o cluster, que armazena todos os registros em um repositório central.

#### Sending requests to the pod

##### FORWARDING A LOCAL NETWORK PORT TO A PORT IN THE POD

Quando você quiser falar com um pod específico sem passar por um serviço (por depuração ou outros motivos), o Kubernetes permite configurar o encaminhamento de porta para o pod. Isso é feito através do comando `kubectl port-forward`. O seguinte comando encaminhará a porta local `8888` da sua máquina para a porta `8080` do seu pod:

```
kubectl port-forward pod-name 8888:8080
```

O encaminhador de porta está em execução e agora você pode se conectar ao seu pod através da porta local.

##### CONNECTING TO THE POD THROUGH THE PORT FORWARDER

In a different terminal, you can now use curl to send an HTTP request to your pod through the `kubectl port-forward` proxy running on localhost:8888:

```sh
curl localhost:8888
```

A Figura abaixo mostra uma visão excessivamente simplificada do que acontece quando você envia a solicitação. Na realidade, alguns componentes adicionais ficam entre o processo `kubectl` e o pod, mas eles não são relevantes no momento.

![A simplified view of what happens when you use curl with kubectl port-forward](./Images/Kubernetes/A%20simplified%20view%20of%20what%20happens%20when%20you%20use%20curl%20with%20kubectl%20port-forward.png "A simplified view of what happens when you use curl with kubectl port-forward")

Usando o encaminhamento de porta como este é uma maneira eficaz de testar um pod individual.

### Organizing pods with labels

#### Modifying labels of existing pods

_Para mostrar todos os PODs com seus LABELS_

```
kubectl get po --show-labels
```

Em vez de listar todos os LABELS, se estiver interessado apenas em determinados LABELS, você poderá especificá-los com a opção `-L` e exibir cada um deles em sua própria coluna.

```
kubectl get po -L label_name_1,label_name_1
```

Os LABELS também podem ser adicionados e modificados em PODs existentes. Vamos adicionar o label `creation_method=manual` a um pod como exemplo:

```
kubectl label po pod-name creation_method=manual
```

Agora, vamos também alterar o LABEL `env=prod` para `env=debug`, para ver como os LABELs existentes podem ser alterados. É necessário usar a opção `--overwrite` ao alterar os LABELS existentes.

```
kubectl label po pod-name env=debug --overwrite
```

### Listando subconjuntos de pods por meio de seletores de labels

Anexar labels a recursos para que você possa ver os labels ao lado de cada recurso ao listá-los não é tão interessante. Mas os labels andam de mãos dadas com os _label selectors_. Os label selectors permitem que você selecione um subconjunto de pods marcados com determinadas labels e realize uma operação nesses pods. Um label selector é um critério, que filtra recursos com base no fato de incluir um determinado label com um determinado valor.

Um label selector pode selecionar recursos com base em se o recurso

- Contém (ou não contém) um label com uma determinada chave
- Contém um label com uma determinada chave e valor
- Contém um label com uma determinada chave, mas com um valor diferente do que você especifica

#### Listing pods using a label selector

Vamos usar label selectors nos pods. Para ver todos os pods criados manualmente:

```
kubectl get po -l creation_method=manual
```

Para listar todos os pods que incluem o label `env`, seja qual for seu valor:

```
kubectl get po -l env
```

E aqueles que não têm o label `env`:

```
kubectl get po -l `!env`
```

Da mesma forma, você também pode combinar pods com os seguintes label selectors:

- `creation_method!=manual` para selecionar pods com o label `creation_method` com qualquer valor diferente de manual
- `env in (prod, devel) `para selecionar pods que tenha o label `env` com um dos valores `prod` ou `devel`
- `env notin (prod, devel)` para selecionar pods com o label `env` configurado para qualquer valor diferente de `prod` ou `devel`

Voltando aos pods no exemplo da arquitetura orientada a microsserviços, você pode selecionar todos os pods que fazem parte do microsserviço do catálogo de produtos usando o seletor de label `app=pc` (mostrado na figura a seguir).

![Selecting the product catalog microservice pods using the “app=pc” label selector](./Images/Kubernetes/Selecting%20the%20product%20catalog%20microservice%20pods%20using%20the%20“app=pc”%20label%20selector.png "Selecting the product catalog microservice pods using the “app=pc” label selector")

#### Usando várias condições em um label selectors

Um seletor também pode incluir vários critérios separados por vírgulas. Os recursos precisam corresponder a todos eles para corresponder ao seletor. Se, por exemplo, você quiser selecionar apenas os pods que executam a versão beta do microsserviço do catálogo de produtos, use o seguinte seletor: `app=pc`, `rel=beta` (visualizado na figura a seguir).

Os label selectors não são úteis apenas para listar pods, mas também para executar ações em um subconjunto de todos os pods.

![Selecting pods with multiple label selectors](./Images/Kubernetes/Selecting%20pods%20with%20multiple%20label%20selectors.png "Selecting pods with multiple label selectors")

### Usando labels e seletores para restringir o agendamento de pods

Todos os pods que você criou até agora foram agendados aleatoriamente nos seus nós de trabalho. Essa é a maneira correta de trabalhar em um cluster do Kubernetes. Como o _Kubernetes expõe todos os nós no cluster como uma única plataforma de implantação_, não importa para qual nó um pod está agendado. Como cada pod obtém a quantidade exata de recursos computacionais solicitados (CPU, memória e assim por diante) e sua acessibilidade de outros pods não é de forma alguma afetada pelo nó no qual o pod está programado, geralmente não deve haver necessidade para você dizer ao Kubernetes exatamente onde agendar seus pods.

Certos casos existem, no entanto, onde você vai que especificar onde um pod deve ser agendado. Um bom exemplo é quando sua infraestrutura de hardware não é homogênea. Se parte de seus nós de trabalho tiver unidades de disco rígido sem SSD, enquanto outras têm SSDs, você poderá programar determinados pods para um grupo de nós e o restante para o outro. Outro exemplo é quando você precisa programar pods que realizam computação intensiva baseada em GPU apenas para nós que fornecem a aceleração de GPU necessária.

Você nunca quer dizer especificamente para qual nó um pod deve ser programado, porque isso acoplaria o aplicativo à infraestrutura, enquanto toda a ideia do Kubernetes está ocultando a infraestrutura real dos aplicativos executados nela. Mas se você quiser programar sobre onde um pod deve ser agendado, em vez de especificar um nó exato, você deve descrever os requisitos do nó e deixar o Kubernetes selecionar um nó que corresponda a esses requisitos. Isso pode ser feito por meio de labels de nó e label selectors de nó.

#### Using labels for categorizing worker nodes

Os pods não são o único tipo de recurso do Kubernetes ao qual você pode anexar um label. Os labels podem ser anexados a qualquer objeto do Kubernetes, incluindo nodes(nós). Geralmente, quando a equipe de infra adiciona um novo nó ao cluster, eles categorizam o nó anexando labels especificando o tipo de hardware que o nó fornece ou qualquer outra coisa que possa ser útil ao agendar pods.

Vamos imaginar que um dos nós no seu cluster contenha uma GPU destinada a ser usada para computação de GPU de propósito geral. Você deseja adicionar um label ao nó que mostra esse recurso. Você vai adicionar o label `gpu=true` a um de seus nós.

```
kubectl label node node-name gpu=true
```

Agora você pode usar um label selectors ao listar os nós, como fez antes com os pods. Listar apenas nós que incluam o label `gpu=true`:

```
kubectl get nodes -l gpu=true
```

#### Scheduling pods to specific nodes

Agora imagine que você deseja implantar um novo pod que precisa de uma GPU para realizar seu trabalho. Para pedir ao agendador para escolher apenas entre os nós que fornecem uma GPU, você adicionará um seletor de nó ao YAML do pod.

```
apiVersion: v1
kind: Pod
metadata:
  name: pod-name
spec:
  nodeSelector: # nodeSelector tells Kubernetes to deploy this pod only to nodes containing the `gpu=true` label.
    gpu: "true"
  containers:
  - name: container-name
    image: polinux/stress
```

Você adicionou um campo `nodeSelector` na seção `spec`. Quando você cria o pod, o planejador só escolhe entre os nós que contêm o label `gpu=true`.

### Usando namespaces para agrupar recursos

Vamos voltar aos labels por um momento. Vimos como eles organizam pods e outros objetos em grupos. Como cada objeto pode ter vários labels, esses grupos de objetos podem se sobrepor. Além disso, ao trabalhar com o cluster (por meio do kubectl, por exemplo), se você não especificar explicitamente um label selector, sempre verá todos os objetos.

Mas e quanto às vezes em que você deseja dividir objetos em grupos separados e sem sobreposição? Você pode querer operar somente dentro de um grupo de cada vez. Por essa e outras razões, o Kubernetes também agrupa objetos em namespaces. Estes não são os namespaces do Linux que são usados ​​para isolar processos uns dos outros. Namespaces do Kubernetes fornecem um escopo para nomes de objetos. Em vez de ter todos os seus recursos em um único namespace, é possível dividi-los em vários namespaces, o que também permite usar os mesmos nomes de recursos várias vezes (em namespaces diferentes).

#### Compreendendo a necessidade de namespaces

O uso de vários namespaces permite dividir sistemas complexos com vários componentes em grupos distintos menores. Eles também podem ser usados ​​para separar recursos em um ambiente de vários locatários, dividindo recursos em ambientes de produção, desenvolvimento e controle de qualidade ou de qualquer outra maneira que você possa precisar. Os nomes de recursos precisam ser exclusivos em um namespace. Dois namespaces diferentes podem conter recursos com o mesmo nome. Mas, enquanto a maioria dos tipos de recursos é namespaced, alguns não são. Um deles é o recurso Node, que é global e não está vinculado a um único namespace.

#### Discovering other namespaces and their pods

Primeiro, vamos listar todos os namespaces em seu cluster:

```
kubectl get ns
```

Até este ponto, você operou apenas no namespace `default`. Ao listar recursos com o comando `kubectl get`, você nunca especificou o namespace explicitamente, portanto, o kubectl sempre assumiu o `default` para o namespace `default`, mostrando apenas os objetos nesse namespace. Mas, como você pode ver na lista, os namespaces `kube-public` e `kube-system` também existem. Vejamos os pods que pertencem ao namespace `kube-system`, dizendo ao kubectl para listar os pods apenas nesse namespace:

_Você pode usar `-n` em vez de `--namespace`_

```
kubectl get po --namespace kube-system
```

Os namespaces permitem separar recursos que não pertencem juntos a grupos não sobrepostos. Se vários usuários ou grupos de usuários estiverem usando o mesmo cluster do Kubernetes, e cada um deles gerencia seu próprio conjunto distinto de recursos, cada um deve usar seu próprio namespace. Dessa forma, eles não precisam ter nenhum cuidado especial para não modificar ou excluir inadvertidamente os recursos dos outros usuários e não precisam se preocupar com conflitos de nome, porque os namespaces fornecem um escopo para nomes de recursos, como já foi mencionado.

_Além de isolar recursos, os namespaces também são usados ​​para permitir que apenas certos usuários acessem recursos específicos e até mesmo para limitar a quantidade de recursos computacionais disponíveis para usuários individuais._

#### Criando um namespace

Um namespace é um recurso do Kubernetes como qualquer outro, portanto, você pode criá-lo publicando um arquivo YAML no servidor da API do Kubernetes. Vamos ver como fazer isso agora.

##### CREATING A NAMESPACE FROM A YAML FILE

![CREATING A NAMESPACE FROM A YAML FILE](./Images/Kubernetes/CREATING%20A%20NAMESPACE%20FROM%20A%20YAML%20FILE.png "CREATING A NAMESPACE FROM A YAML FILE")

##### CREATING A NAMESPACE WITH KUBECTL CREATE NAMESPACE

Apesar de escrever um arquivo como o anterior não é um grande negócio, ainda é um aborrecimento. Felizmente, você também pode criar namespaces com o comando dedicado de criação de namespace `kubectl`, que é mais rápido do que gravar um arquivo YAML. Ao criar um programa YAML para o namespace, eu queria reforçar a ideia de que tudo no Kubernetes tem um objeto de API correspondente que você pode criar, ler, atualizar e excluir postando um manifesto YAML no servidor da API.

```
kubectl create namespace custom-namespace
```

_Embora a maioria dos nomes de objetos deva obedecer às convenções de nomenclatura especificadas no RFC 1035 (nomes de domínio), o que significa que eles podem conter apenas letras, dígitos, traços e pontos, namespaces (e alguns outros) não podem contém pontos._

#### Managing objects in other namespaces

Para criar recursos no namespace que você criou, adicione `namespace: custom-namespace` à seção de `metadata` ou especifique o namespace ao criar o recurso com o comando `kubectl create`:

```
kubectl create -f pod-name.yaml -n custom-namespace
```

Ao listar, descrever, modificar ou excluir objetos em outros namespaces, você precisa passar o sinalizador `--namespace` (ou `-n`) para `kubectl`. Se você não especificar o namespace, o kubectl executa a ação no namespace `default` configurado no contexto atual do `kubectl`. O namespace do contexto atual e o próprio contexto atual podem ser alterados através dos comandos `kubectl config`.

_Para alternar rapidamente para um namespace diferente, você pode configurar o seguinte alias: `alias kcd='kubectl config set-context $(kubectl config currentcontext) --namespace '`. Você pode alternar entre namespaces usando `kcd some-namespace`._

#### Understanding the isolation provided by namespaces

### Stopping and removing pods

#### Excluindo um pod por nome

```
kubectl delete po pod-name_1 pod-name_2
```

#### Deleting pods using label selectors

Em vez de especificar cada pod para excluir por nome, você usará agora o que aprendeu sobre os label selectors para interromper o pods que contenham os labels no comando.

```
kubectl delete po -l creation_method=manual
```

No exemplo anterior de microservices, onde você tinha dezenas (ou possivelmente centenas) de pods, você poderia, por exemplo, excluir todos os pods canary de uma vez especificando o label selector `rel=canary`.

```
kubectl delete po -l rel=canary
```

#### Deleting pods by deleting the whole namespace

Você pode excluir o namespace inteiro (os pods serão excluídos juntamente com o namespace automaticamente), usando o seguinte comando:

```
kubectl delete ns custom-namespace
```

![Selecting and deleting all canary pods through the rel=canary label selector](./Images/Kubernetes/Selecting%20and%20deleting%20all%20canary%20pods%20through%20the%20rel=canary%20label%20selector.png "Selecting and deleting all canary pods through the rel=canary label selector")

#### Deleting all pods in a namespace, while keeping the namespace

Em vez de excluir o pod específico, diga ao Kubernetes para excluir todos os pods no namespace atual usando a opção `--all`:

```
kubectl delete po --all
```

#### Deleting (almost) all resources in a namespace

Você pode excluir o ReplicationController e os pods, bem como todos os Serviços que você criou, excluindo todos os recursos no namespace atual com um único comando:

```
kubectl delete all --all
```

O primeiro `all` no comando especifica que você está excluindo recursos de todos os tipos, e a opção `--all` especifica que você está excluindo todas as instâncias de recursos em vez de especificá-las pelo nome (você já usou essa opção quando executou o comando comando delete anterior).

## Replication and other controllers: deploying managed pods

Como você aprendeu até agora, os pods representam a unidade implantável básica no Kubernetes. Você sabe criar, supervisionar e gerenciá-los manualmente. Mas, em casos de uso do mundo real, você deseja que suas implantações permaneçam funcionando automaticamente e permaneçam saudáveis ​​sem qualquer intervenção manual. Para fazer isso, você quase nunca cria pods diretamente. Em vez disso, você cria outros tipos de recursos, como ReplicationControllers ou Deployments, que criam e gerenciam os pods reais.

Quando você cria pods não gerenciados (como aqueles que você criou no capítulo anterior), um nó de cluster é selecionado para executar o pod e, em seguida, seus contêineres são executados nesse nó. Neste capítulo, você aprenderá que o Kubernetes monitora esses contêineres e os reinicia automaticamente se falharem. Mas se o nó inteiro falhar, os pods no nó serão perdidos e não serão substituídos por novos, a menos que esses pods sejam gerenciados pelos ReplicationControllers mencionados anteriormente ou similares. Neste capítulo, você aprenderá como o Kubernetes verifica se um contêiner ainda está ativo e o reinicia, se não estiver. Você também aprenderá como executar pods gerenciados, tanto os que são executados indefinidamente quanto os que executam uma única tarefa e depois param.

### Keeping pods healthy

_Um dos principais benefícios de usar o Kubernetes é a capacidade de fornecer uma lista de contêineres e permitir que ele mantenha esses contêineres em algum lugar do cluster. Você faz isso criando um recurso Pod e permitindo que o Kubernetes escolha um nó de trabalho para ele e execute os contêineres do pod nesse nó._ Mas e se um desses containers morrer? E se todos os containers de um pod morrerem?

Assim que um pod é programado para um nó, o Kubelet nesse nó executará seus contêineres e, a partir de então, os manterá em execução enquanto o Pod existir. Se o processo principal do contêiner falhar, o Kubelet irá reiniciar o contêiner. Se o seu aplicativo tiver um bug que causa uma falha de vez em quando, o Kubernetes irá reiniciá-lo automaticamente, portanto, mesmo sem fazer nada de especial no próprio aplicativo, rodar o aplicativo no Kubernetes automaticamente lhe dá a habilidade de se curar.

Mas às vezes os apps param de funcionar sem que o processo falhe. Por exemplo, um aplicativo Java com um vazamento de memória começará a gerar OutOfMemoryErrors, mas o processo da JVM continuará em execução. Seria ótimo ter uma maneira de um aplicativo sinalizar para o Kubernetes que ele não está mais funcionando adequadamente e que o Kubernetes deve reiniciá-lo.

Dissemos que um contêiner que falha é reiniciado automaticamente, então talvez você esteja pensando que poderia detectar esses tipos de erros no aplicativo e sair do processo quando eles ocorrem. Você certamente pode fazer isso, mas ainda não resolve todos os seus problemas.

Por exemplo, e as situações em que seu aplicativo para de responder porque cai em um loop infinito ou em um deadlock? Para garantir que os aplicativos sejam reiniciados em tais casos, você deve verificar a integridade de um aplicativo de fora e não depender do aplicativo que faz isso internamente.

#### Introducing liveness probes
