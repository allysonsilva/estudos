# Docker

> O Docker é um sistema de gerenciamento de containers que nos ajuda a gerenciar facilmente o Linux Containers (LXC) de maneira mais fácil e universal. As ações executadas nos containers que você executa nesses ambientes localmente em sua própria máquina serão os mesmos comandos ou operações executados contra eles quando estiverem em execução em ambiente de produção. Isso ajuda a não ter que fazer as coisas de maneira diferente quando você passa de um ambiente de desenvolvimento como o da sua máquina local para um ambiente de produção em seu servidor.

## Anotações gerais

> In general, ***Docker containers are ephemeral***, running just as long as it takes for the command issued in a container to complete. By default, any data created inside the container is only available from within the container and only while the container is running.

> O **Docker** aproveita a infraestrutura do *kernel* do seu sistema operacional para não precisar instalar outro `SO` completo. Ao invés disso ele utiliza-se de uma engine (chamado de **Docker Engine**) para abstrair as chamadas de `SO` das suas aplicações e utilizar as libraries e binários já existentes. Para criar e executar os seus containers, o *Docker* cria processos (como os programas normais) mantendo isolados em cada um deles as dependências da sua aplicação, garantindo que estejam instaladas nela apenas as bibliotecas (libs) necessárias para fazer a sua aplicação funcionar.

> **Docker Compose** é uma abstração do Docker para gerenciamento de containers.

> O **Container** é uma *instância* de uma imagem em execução naquele momento. É uma instância executável de uma imagem. Você pode criar, iniciar, parar, mover ou excluir um container usando o Docker API ou CLI.

> O ambiente Docker tem images, containers e volumes. As images são as instalações "puras", como se fossem ISO’s dos serviços. Os containers são instâncias das images com adaptações e modificações que acontecem conforme vão sendo usados. Já os volumes são opções de armazenamento que podem ser reutilizadas entre os containers.

> Containers são **stateless**.

> Uma **Imagem** é um pacote leve e executável que engloba tudo que é preciso para executar um pedaço da aplicação, incluindo código, bibliotecas, variáveis de ambiente, arquivos de configuração e um ambiente de execução. Atua como gerador de *container*, é a partir dela que o seu container vai ser criado, ou seja, atua como um template para criação do *container*.

> **Imagem** é um modelo, template de somente leitura com instruções para criar um container do Docker.

> Um **Container** é uma instancia desta imagem, isto é, dada a configuração da imagem, é o que todo o conjunto anterior se torna em memória quando é de fato executado. Um *container* roda de modo isolado do host por padrão, apenas acessando arquivos do host caso haja configuração para tal.

> Fazendo um paralelo com o conceito de orientação a objeto, a imagem é a classe e o container o objeto, ou seja, a imagem é a abstração da infraestrutura em um estado somente leitura, que é de onde será instanciado o container.

> Todo container é iniciado a partir de uma imagem, dessa forma podemos concluir que nunca teremos uma imagem em execução.

> Somente o **KERNEL** é compartilhado com o **HOST**, Libs e Binários apenas entre os containers.

> Todos os processos que estão rodando nos containers, é possível visualizar no host.

> O Docker não é uma máquina, é um processo em execução. E, como todo processo, deve ser descartado para que outro possa tomar seu lugar na reinicialização do mesmo.

> De forma bem resumida, podemos dizer que o **Docker** é uma plataforma aberta, criada com o objetivo de facilitar o desenvolvimento, a implantação e a execução de aplicações em ambientes isolados. Foi desenhada especialmente para disponibilizar uma aplicação da forma mais rápida possível.

> O modelo de isolamento utilizado no Docker é a *virtualização a nível do sistema operacional*, um método de virtualização onde o kernel do sistema operacional permite que múltiplos processos sejam executados isoladamente no mesmo host. Esses processos isolados em execução são denominados no Docker de container.

> Um ponto interessante no Docker é a velocidade para viabilizar o ambiente desejado; como é basicamente o início de um processo e não um sistema operacional inteiro, o tempo de disponibilização é, normalmente, medido em segundos.

> Para criar o isolamento necessário entre processos, o **Docker** usa a funcionalidade do *Kernel* denominada **namespaces**, que cria ambientes isolados entre containers, ou seja, os processos em execução de um container não terão acesso aos recursos de outros containers, a não ser que isso seja expressamente liberado em configuração de cada ambiente.

> Para evitar a exaustão dos recursos da máquina por apenas um ambiente isolado, o **Docker** usa a funcionalidade **cgroups** do kernel, que é responsável por criar limites de uso do hardware a disposição. Com isso é possível coexistir no mesmo host diferentes containers, sem que um afete diretamente o outro por uso exagerado dos recursos compartilhados.

## Arquitetura Docker

Quando você inicia um container do Docker, o *Docker Engine* está fazendo muito trabalho nos bastidores. Duas das tarefas que ele realiza ao ativar seus containers são configurar `namespaces` e `cgroups`. O que isso significa? Ao configurar **namespaces**, o Docker mantém os processos isolados em cada container, não apenas de outros containers, mas também do sistema host. Os **cgroups** garantem que cada container receba sua própria parcela de itens, como CPU, memória e E/S de disco. Mais importante, eles garantem que um container não esgote todos os recursos em um determinado host do Docker.

Os containers do Docker não possuem um *kernel* separado, como faz uma VM. Comandos executados a partir de um container Docker aparecem na tabela de processos no host e, na maioria das vezes, se parecem muito com qualquer outro processo em execução no sistema. A diferença entre uma aplicação executada nesses dois ambientes, no entanto, tem mais a ver com a visão diferente do mundo que essas duas aplicações têm em vista:

- **Sistema de arquivos**: O container tem seu próprio sistema de arquivos e não pode ver o sistema de arquivos do sistema host por padrão. Uma exceção a essa regra é que os arquivos (como `/etc/hosts` e `/etc/resolv.conf`) podem ser vinculados automaticamente dentro do container. Outra exceção é que você pode montar explicitamente diretórios do host dentro do container ao executar uma imagem de container.

- **Tabela de processos**: Centenas de processos podem estar sendo executados em um computador host Linux. No entanto, por padrão, os processos dentro de um container não podem ver a tabela de processos do host, mas possuem sua própria tabela de processos. Portanto, o processo do aplicativo executado quando você inicia o container é atribuído ao `PID` 1 no container. Dentro do container, um processo não pode ver nenhum outro processo em execução no host que não foi lançado dentro do container.

- **Interfaces de rede**: Por padrão, o `daemon` do *Docker* define um endereço **IP** via **DHCP** a partir de um conjunto de endereços IP privados. Em vez de usar o DHCP, o Docker suporta outros modos de rede, como permitir que containers usem interfaces de rede de outro container, interfaces de rede do host diretamente ou nenhuma interface de rede. Se você escolher, poderá expor uma porta de dentro do container para o mesmo ou diferente número de porta no host.

- **Recurso do IPC**: Os processos em execução dentro dos containers não podem interagir diretamente com o recurso de comunicações entre processos (`IPC`) em execução no sistema host. Você pode expor o recurso IPC no host para o container, mas isso não é feito por padrão. Cada container tem sua própria instalação `IPC`.

- **Dispositivos**: Os processos dentro do container não podem ver diretamente os dispositivos no sistema host. Novamente, uma opção de privilégio especial pode ser definida quando o container é executado para conceder esse privilégio.

### Qual é a relação entre o sistema operacional host e o Docker?

> O sistema operacional host e o container compartilham o mesmo Kernel.

Se você estiver executando o Ubuntu como host, o Kernel do container usará o mesmo Kernel que o sistema Ubuntu , mas você poderá usar o CentOs ou qualquer outra imagem do sistema operacional dentro do container. É por isso que a principal diferença entre uma máquina virtual e um container do Docker é a ausência de uma parte intermediária entre o Kernel e o guest, o Docker ocorre diretamente no Kernel do seu host. Portanto, o Docker é um ambiente de isolamento de processo e não um ambiente de isolamento do SO (como máquinas virtuais).

## Dockerfile

*Dockerfile* é um recurso feito para automatizar o processo de execução de tarefas no Docker. Com este recurso, podemos descrever o que queremos inserir em nossa imagem. Desta forma, quando geramos um build, o Docker cria um snapshot com toda a instalação que elaboramos no **Dockerfile**.

Quando se utiliza **Dockerfile** para gerar uma imagem, basicamente é apresentado uma lista de instruções que serão aplicadas em uma determinada imagem para que outra seja gerada com base nessas modificações.

Podemos resumir que o arquivo **Dockerfile** na verdade representa a exata diferença entre uma determinada imagem, que aqui chamamos de base, e a imagem que se deseja criar, ou seja, nesse modelo temos total rastreabilidade sobre o que será modificado essa nova imagem.

###  Formato do Dockerfile

```
#  Comment
INSTRUCTION arguments
```

A instrução não faz distinção entre maiúsculas e minúsculas. No entanto, a convenção é que eles sejam MAIÚSCULAS para distingui-los dos argumentos mais facilmente.

O Docker executa as instruções Dockerfile em ordem. Um Dockerfile deve começar com uma instrução `FROM` . A instrução `FROM` especifica a imagem base a partir da qual você está construindo. `FROM` só pode ser precedido por uma ou mais instruções `ARG`, que declaram argumentos que são usados ​​em linhas `FROM` no Dockerfile.

*As variáveis ​​de ambiente são suportadas pela seguinte lista de instruções no Dockerfile:*

`ADD`
`COPY`
`ENV`
`EXPOSE`
`FROM`
`LABEL`
`STOPSIGNAL`
`USER`
`VOLUME`
`WORKDIR`

Antes que a janela de encaixe CLI envie o contexto para o daemon do docker, ele procura por um arquivo chamado `.dockerignore` no diretório raiz do contexto. Se este arquivo existir, o CLI modificará o contexto para excluir arquivos e diretórios que correspondam a padrões nele. Isso ajuda a evitar o envio desnecessário de arquivos e diretórios grandes ou confidenciais para o daemon e potencialmente adicioná-los às imagens usando `ADD` ou `COPY`.

Se uma linha no `.dockerignore` arquivo começar `#` na coluna 1, essa linha será considerada como um comentário e será ignorada antes de ser interpretada pela CLI.

Aqui está um arquivo `.dockerignore` de exemplo:

```
# comment
*/temp*
*/*/temp*
temp?
```

### Boas práticas

**Sempre combine `RUN`  `apt-get update` com `apt-get install` na mesma declaração.**

A utilização `RUN apt-get update && apt-get install -y` garante que o Dockerfile instale as versões mais recentes do pacote, sem nenhuma codificação adicional ou intervenção manual. Essa técnica é conhecida como **cache busting**.

*A fixação de versão força o build a recuperar uma versão específica, independentemente do que está no cache. Essa técnica também pode reduzir falhas devido a mudanças imprevistas nos pacotes necessários*.

Abaixo está uma instrução `RUN` bem formada que demonstra todas as recomendações de `apt-get`.

```bash
RUN apt-get update && apt-get install -y \
    aufs-tools \
    automake \
    build-essential \
    curl \
    dpkg-sig \
    libcap-dev \
    libsqlite3-dev \
    mercurial \
    reprepro \
    ruby1.9.1 \
    ruby1.9.1-dev \
    s3cmd=1.1.* \
 && rm -rf /var/lib/apt/lists/*
```

Além disso, quando você limpa o cache do `apt` removendo `/var/lib/apt/lists`, reduz o tamanho da imagem, já que o cache do `apt` não é armazenado em uma camada. Desde o início da instrução `RUN apt-get update`, o cache de pacotes é sempre atualizado antes `apt-get install`.

**`CMD` deve quase sempre ser usado na forma de `CMD ["executable", "param1", "param2"...]`**

**Embora `ADD` e `COPY` sejam funcionalmente semelhantes, de um modo geral, `COPY` é preferido. Isso é porque é mais transparente que `ADD`. `COPY` suporta apenas a cópia básica de arquivos locais no container, enquanto `ADD` suporta e extração de `tar` e também URL remoto.**

O melhor uso do `ENTRYPOINT` é para definir o comando principal da imagem, permitindo que a imagem seja executada como se fosse esse comando (e, em seguida, usar `CMD` como sinalizadores padrão).

O principal processo de execução de um container é o `ENTRYPOINT` e/ou `CMD` no final do arquivo Dockerfile. Geralmente, é recomendável separar as áreas de interesse usando um serviço por container. Esse serviço pode bifurcar-se em vários processos (por exemplo, o servidor da web Apache inicia vários processos de trabalho). Não há problema em ter vários processos, mas, para aproveitar ao máximo o Docker, evite que um container seja responsável por vários aspectos do aplicativo em geral. Você pode conectar vários containers usando redes definidas pelo usuário e volumes compartilhados.

### Comandos

- `FROM`: Informa qual imagem deve ser usado como base.
- `RUN`: Informa quais comandos serão executados nesse ambiente para efetuar as mudanças necessárias na infraestrutura do sistema. São como comandos executados no shell do ambiente, igual ao modelo por commit. É onde interagimos com nossa imagem para instalar software e executar scripts, comandos e outras tarefas.
- `CMD`: Informa qual comando será executado quando o container é iniciado, caso nenhum seja informado na inicialização de um container a partir dessa imagem.
- `COPY`: Usado para copiar arquivos do host onde está executando o *build* para dentro da imagem.
- `EXPOSE`: Permite ao Docker saber que, quando a imagem é executada, a porta e o protocolo definidos serão expostos no tempo de execução. Esse comando não mapeia a porta para a máquina host, mas abre a porta para permitir o acesso ao serviço na rede do container.
- `USER`: Permite que você especifique o nome de usuário a ser usado quando um comando é executado. A instrução `USER` pode ser usada na instrução `RUN`, na instrução `CMD` ou na instrução `ENTRYPOINT` no *Dockerfile*. Além disso, o usuário definido no comando `USER` deve existir ou sua imagem falhará no `build`. O uso da instrução `USER` também pode introduzir problemas de permissão, não apenas no próprio container, mas também se você montar volumes.
- `WORKDIR`: Define o diretório de trabalho para o mesmo conjunto de instruções que a instrução `USER` pode usar (`RUN`, `CMD` e `ENTRYPOINT`). Isso permitirá que você use as instruções `CMD` e `ADD` também. Para maior clareza e confiabilidade, você deve sempre usar caminhos absolutos para o seu `WORKDIR`.

Após construir seu *Dockerfile* basta executar o comando abaixo:

```bash
docker build -t HUBUSER/REPOSITORY .
```

```bash
$ docker image build --file <path_to_Dockerfile> --tag <REPOSITORY>:<TAG> .
```

O comando acima tem a opção `-t` que serve para informar o nome da imagem que será criada, e o `.` no final informa qual o contexto que deve ser usado nessa construção de imagem, ou seja, todos os arquivos da sua pasta atual serão enviados para o serviço do docker e apenas eles podem ser usados para manipulações do *Dockerfile* (Exemplo do uso do `COPY`).

Para marcar a imagem em vários repositórios após a construção, adicione vários `-t` parâmetros ao executar o `build` comando:

```
$ docker build -t HUBUSER/myapp:1.0.2 -t HUBUSER/myapp:latest .
```

#### CMD/ENTRYPOINT

A instrução `CMD` tem três formas:

```docker
CMD ["executable","param1","param2"](exec form, esta é a forma preferida)
CMD ["param1","param2"](define parâmetros padrão adicionais para ENTRYPOINT no formato exec)
CMD command param1 param2(shell form)
```

A segunda forma é usado em conjunto com a instrução `ENTRYPOINT` no formato *exec*. Ele define parâmetros padrão que serão incluídos após os parâmetros `ENTRYPOINT` se o container for executado sem argumentos de linha de comando.

A instrução `ENTRYPOINT` tem duas formas:

```docker
ENTRYPOINT ["executable", "param1", "param2"] (forma exec, preferida)
ENTRYPOINT command param1 param2 (shell form)
```

1. O Dockerfile deve especificar pelo menos um dos `CMD` ou `ENTRYPOINT` comandos.
2.  `ENTRYPOINT` deve ser definido ao usar o container como um executável.
3.  `CMD` deve ser usado como uma maneira de definir argumentos padrão para um `ENTRYPOINT` comando ou para executar um comando ad-hoc em um container.
4.  `CMD` será substituído ao executar o container com argumentos alternativos.

A instrução `ENTRYPOINT` permite configurar um container que será executado como um executável. Parece semelhante ao CMD, porque também permite especificar um comando com parâmetros. A diferença é o comando `ENTRYPOINT` e os parâmetros não são ignorados quando o container Docker é executado com parâmetros de linha de comando.

*Só pode haver uma CMD instrução em um Dockerfile. Se você listar mais de uma CMD , somente a última CMD terá efeito.*

O principal objetivo de um CMD é fornecer padrões para um container em execução. Esses padrões podem incluir um executável ou eles podem omitir o executável; nesse caso, você deve especificar uma ENTRYPOINT instrução também.

*Se CMD for usado para fornecer argumentos padrão para a `ENTRYPOINT` instrução, as instruções `CMD` e `ENTRYPOINT` devem ser especificadas com o formato de matriz JSON.*

*O formulário exec é analisado como uma matriz JSON, o que significa que você deve usar aspas duplas (“) em torno de palavras e não de aspas simples (').*

*Quando usada nos formatos shell ou exec, a instrução `CMD` define o comando a ser executado ao executar a imagem.*

*Ambos as instruções `CMD` e `ENTRYPOINT` definem qual comando é executado ao executar um container.*

Se você quiser que seu container execute o mesmo executável todas as vezes, considere usá-lo `ENTRYPOINT` em combinação com o `CMD`.

Não confunda `RUN` com `CMD`. `RUN` na verdade, executa um comando e confirma o resultado; `CMD` não executa nada no tempo de compilação, mas especifica o comando pretendido para a imagem.

O `ENTRYPOINT` ou `CMD` que você especifica no Dockerfile identifica o executável padrão da sua imagem.

Dado o quanto é mais fácil substituir o `CMD`, a recomendação é usar o `CMD` no Dockerfile quando você quiser que o usuário da sua imagem tenha a flexibilidade de executar o executável que escolher ao iniciar o container.

Por outro lado, o `ENTRYPOINT` deve ser usado em cenários em que você deseja que o container se comporte exclusivamente como se fosse o executável em que está empacotando. Ou seja, quando você não deseja ou espera que o usuário substitua o executável especificado.

Quando a forma exec da instrução CMD é usada, o comando será executado sem um shell.

A melhor opção é usar o formulário exec das instruções `ENTRYPOINT`/`CMD` que se parece com isto:

```
CMD ["executable","param1","param2"]
```

Observe que o conteúdo que aparece após a instrução CMD nesse caso é formatado como um array JSON.

Quando a forma exec da instrução CMD é usada, o comando será executado sem um shell.

A instrução CMD permite que você defina um comando padrão , que será executado somente quando você executar o container sem especificar um comando. Se o container Docker for executado com um comando, o comando padrão será ignorado. Se o Dockerfile tiver mais de uma instrução CMD, todas, exceto as últimas instruções do CMD, serão ignoradas.

Esteja você usando `ENTRYPOINT` ou `CMD` (ou ambos), a recomendação é sempre usar a *exec form*, de modo que fique óbvio qual comando está sendo executado como PID 1 dentro do container.

A combinação de `ENTRYPOINT` e `CMD` permite que você especifique o executável padrão para sua imagem enquanto também fornece argumentos padrão para esse executável, que podem ser substituídos pelo usuário.

Quando um `ENTRYPOINT` e o `CMD` são especificados, as cadeias de caracteres do `CMD` serão anexadas ao `ENTRYPOINT` para gerar a cadeia de comandos do container. Lembre-se de que o valor do `CMD` pode ser facilmente substituído fornecendo um ou mais argumentos para o `docker run` após o nome da imagem.

Ao usar `ENTRYPOINT` e `CMD` juntos, é importante que você sempre use o formulário exec de ambas as instruções. Tentar usar o formulário de shell ou misturar e combinar os formulários shell e exec quase nunca lhe dará o resultado desejado.

Se você quer que sua imagem realmente faça alguma coisa quando for executada, você deve definitivamente configurar algum tipo de ENTRYPOINT ou CMD em seu Dockerfile. No entanto, lembre-se de que eles não são mutuamente exclusivos. Em muitos casos, você pode melhorar a experiência do usuário da sua imagem usando-os em combinação.

Não importa como você usa essas instruções, você deve sempre usar a forma exec.

O `CMD` define o comando e/ou os parâmetros padrão, que podem ser sobrescritos na linha de comando quando o container do docker é executado.
`ENTRYPOINT` configura um container que será executado como um executável.

Esta é a forma preferida para as instruções `CMD` e `ENTRYPOINT`.

```
<instruction> ["executable", "param1", "param2", ...]
```

Quando a instrução é executada em forma de shell(`ENTRYPOINT echo "Hello world"`), ela chama `/bin/sh -c <command>` sob o capô e o processamento normal do shell acontece.

Quando a instrução é executada no formato exec(`ENTRYPOINT ["/bin/echo", "Hello world"]`), ela chama executável diretamente e o processamento do shell não acontece.

Se você precisar executar bash (ou qualquer outro interpretador), use o formulário exec com um `/bin/bash` executável. Nesse caso, o processamento normal do shell ocorrerá.

```docker
ENTRYPOINT ["/bin/bash", "-c", "echo Hello, $name"]
```

```docker
# exec form, preferred way
CMD ["executable","param1","param2"]

# (sets additional default parameters for ENTRYPOINT in exec form)
CMD ["param1","param2"]

# shell form
CMD command param1 param2
```

Novamente, a primeira e a terceira formas devem parecer familiares a você, como já foram abordadas acima. O segundo é usado em conjunto com a instrução ENTRYPOINT no formato exec. Ele define parâmetros padrão que serão incluídos após os parâmetros ENTRYPOINT se o container for executado sem argumentos de linha de comando.

```docker
# executable form preferred way
ENTRYPOINT ["executable", "param1", "param2"]

# shell form
ENTRYPOINT command param1 param2
```

A diferença é `ENTRYPOINT` que `CMD`, ao contrário, o comando e os parâmetros não são ignorados quando o container do Docker é executado com parâmetros de linha de comando.

Prefiro `ENTRYPOINT` ao `CMD` construir a imagem executável do Docker e você precisa de um comando para ser sempre executado, e use `CMD` se precisar fornecer argumentos padrão adicionais que possam ser sobrescritos da linha de comando quando o container do docker for executado.

Use `ENTRYPOINT` se você não quiser que os desenvolvedores alterem o executável que é executado quando o container é iniciado. Você pode pensar no seu container como um "wrapper executável". Uma boa estratégia é definir uma combinação "estável" de parâmetros executáveis ​​+ como o `ENTRYPOINT`. Então você pode (opcionalmente) especificar um padrão `CMD` que os desenvolvedores possam facilmente substituir.

Use somente `CMD`(com não `ENTRYPOINT`) se você deseja que os desenvolvedores substituam facilmente o executável que está sendo executado. Se `ENTRYPOINT` estiver definido, você ainda pode substituir o executável usando `--entrypoint`, mas é muito mais fácil para os desenvolvedores anexarem o comando que desejam no final docker run.

Somente a última `ENTRYPOINT` instrução no `Dockerfile` terá efeito.

##### A forma "exec" é a forma recomendada

Isso ocorre porque os containers são projetados para conter um único processo. Por exemplo, os sinais que são enviados para o container são enviados para o processo em execução dentro do container com o PID 1. Uma maneira interessante de testar isso é executar o container de ping e tentar fazer `CTRL+c` para parar o container.

#### EXPOSE

```
EXPOSE <port> [<port>/<protocol>...]
```

A instrução `EXPOSE` informa ao Docker que o container escuta nas portas de rede especificadas no tempo de execução. Você pode especificar se a porta escuta em TCP ou UDP, e o padrão é TCP se o protocolo não for especificado.

A instrução `EXPOSE` não publica realmente a porta. Funciona como um tipo de documentação entre a pessoa que constrói a imagem e a pessoa que executa o container, sobre quais portas devem ser publicadas. Para realmente publicar a porta ao executar o container, use o sinalizador `-p`  `docker run` para publicar e mapear uma ou mais portas ou o `-P` sinalizador para publicar todas as portas expostas e mapeá-las para portas de alta ordem.

Por padrão, `EXPOSE` assume o *TCP*.

Para expor em *TCP* e *UDP*, inclua duas linhas:

```
EXPOSE 80/tcp
EXPOSE 80/udp
```

#### ADD

A instrução `ADD` copia novos arquivos, diretórios ou URLs de arquivos remotos `<src>` e os adiciona ao sistema de arquivos da imagem no caminho `<dest>`.

O `<dest>` é um caminho absoluto ou um caminho relativo ao `WORKDIR` qual a origem será copiada dentro do container de destino.

```docker
ADD test relativeDir/ # adds "test" to `WORKDIR`/relativeDir/
ADD test /absoluteDir/ # adds "test" to /absoluteDir/
```

Se `<src>` for um diretório, todo o conteúdo do diretório será copiado, incluindo os metadados do sistema de arquivos.
*O diretório em si não é copiado, apenas seu conteúdo.*

Se vários `<src>` recursos forem especificados, diretamente ou devido ao uso de um caractere curinga, `<dest>` deverá ser um diretório e terminar com uma barra `/`. Se `<dest>` não terminar com uma barra final, será considerado um arquivo regular e o conteúdo de `<src>` será gravado em `<dest>`. Se `<dest>` não existir, será criado junto com todos os diretórios ausentes em seu caminho.

#### COPY

A instrução `COPY` copia novos arquivos ou diretórios `<src>` e os adiciona ao sistema de arquivos do container no caminho `<dest>`.

Vários `<src>` recursos podem ser especificados, mas os caminhos dos arquivos e diretórios serão interpretados como relativos à origem do contexto da construção.

O `<dest>` é um caminho absoluto ou um caminho relativo ao `WORKDIR` qual a origem será copiada dentro do container de destino.

```docker
COPY test relativeDir/ # adds "test" to `WORKDIR`/relativeDir/
COPY test /absoluteDir/ # adds "test" to /absoluteDir/
```

Se `<dest>` não terminar com uma barra final, será considerado um arquivo regular e o conteúdo de `<src>` será gravado em `<dest>`.

Se `<dest>` não existir, será criado junto com todos os diretórios ausentes em seu caminho.

#### USER

A instrução `USER` define o nome de usuário (ou UID) e, opcionalmente, o grupo de usuários (ou GID) para usar ao executar a imagem e para qualquer instrução `RUN`, `CMD` ou `ENTRYPOINT`.

#### WORKDIR

A instrução `WORKDIR` define o diretório de trabalho para quaisquer instruções `RUN`, `CMD`, `ENTRYPOINT`, `COPY` e `ADD` que o seguem no Dockerfile. Se o `WORKDIR` não existir, ele será criado mesmo que não seja usado em nenhuma instrução Dockerfile subseqüente.

A instrução `WORKDIR` pode ser usada várias vezes em um Dockerfile. Se um caminho relativo for fornecido, será relativo ao caminho da instrução `WORKDIR` anterior.

```docker
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```

A saída do comando `pwd` final neste Dockerfile seria `/a/b/c`.

A instrução WORKDIR pode resolver variáveis ​​de ambiente previamente definidas usando `ENV`. Você só pode usar variáveis ​​de ambiente explicitamente definidas no arquivo Dockerfile.

```docker
ENV DIRPATH /path
WORKDIR $DIRPATH/$DIRNAME
RUN pwd
```

A saída do comando final `pwd` neste Dockerfile seria `/path/$DIRNAME`

#### ARG

```
ARG <name>[=<default value>]
```

Adicione argumentos de construção, que são variáveis ​​de ambiente acessíveis apenas durante o processo de construção.

A instrução `ARG` define uma variável que os usuários podem transmitir no momento da criação para o build com o comando `docker build`  usando o sinalizador `--build-arg <varname>=<value>`. Se um usuário especificar um argumento de build que não foi definido no Dockerfile, a construção gerará um aviso.

Uma instrução `ARG` pode opcionalmente incluir um valor padrão:

```docker
FROM image
ARG user=someuser
ARG buildno=1
```

Se uma instrução `ARG` tiver um valor padrão e se não houver nenhum valor passado no tempo de construção, o construtor usará o padrão.

Uma definição `ARG` de variável entra em vigor a partir da linha na qual ela é definida no Dockerfile não do uso do argumento na linha de comando ou em outro lugar.

```docker
1 FROM image
2 USER ${user:-some_user}
3 ARG user
4 USER $user
```

Um usuário constrói esse arquivo chamando:

```
$ docker build --build-arg user=what_user .
```

valores de ARG podem ser facilmente inspecionados após a construção de uma imagem, visualizando a `docker history <image>` imagem de uma imagem. Assim, eles são uma má escolha para dados sensíveis.

#### Linha de comando (bash)

- `docker top <container-id-or-name>`(Quais são os processos que estão rodando no container)
- `docker inpect <container-id-or-name>`(Detalhes do container)
- `docker build -t <image-name> <path-to-dockerfile>` (Criar uma imagem com base nas configurações do *Dockerfile*)
- `docker image inspect <image-id-or-name>`(Detalhes da imagem)
- `docker run -it --memory 512m -name <container-name> <image>`(Limitando o uso da memória)
- `docker run -ti --cpu-shares 1024 --name <container-name> <image>`(Limitando o uso da CPU)
- `docker update -m 256m <container-id-or-name>`(Mudar a quantidade de memória de um container já criado e/ou em execução)
	- `docker inpect <container-id-or-name> | grep -i mem`
- `docker update --cpu-shares 1024 <container-id-or-name>`(Atualizando limite da CPU)
	- `docker inspect <container-id-or-name> ... | grep -i cpu`
- `docker container prune`: Remove todos os containers parados.
- `docker container prune --filter "until=24h"`: Remove apenas container paradas com mais de 24 horas.
- `docker volume prune`: Remove todos os volumes não utilizados.
- `docker volume prune --filter "label!=keep"`: Remove apenas os volumes que não estão rotulados com o `keep`.
- `docker image prune`: Remove imagens não utilizadas.
- `docker image prune -a --filter "until=24h"`: Remover imagens que não são utilizadas por containers existentes(`[-a]`) criadas a mais de 24 horas(`[--filter "until=24h"]`).
- `docker info`: Lista o número de containers e imagens, bem como informações sobre o sistema em relação à instalação do Docker.
- `docker container ls -aq`: Lista todos os containers mostrando apenas o seu *ID*.

O `docker system prune` comando é um atalho que remove imagens, containers e redes. No Docker 17.06.0 e anterior, os volumes também são removidos. No Docker 17.06.1 e superior, você deve especificar o `--volumes` sinalizador para `docker system prune` remover volumes.

```bash
$ docker system prune --volumes --force
```

Você pode usar o `docker stats` comando para transmitir ao vivo as métricas de tempo de execução de um container. O comando suporta CPU, uso de memória, limite de memória e métricas de E/S da rede.

Por padrão, um container não possui restrições de recursos e pode usar tanto de um determinado recurso quanto o planejador de kernel do host permite. O Docker fornece maneiras de controlar a quantidade de memória, CPU ou bloco de E/S que um container pode usar, configurando os sinalizadores de configuração de tempo de execução do `docker run` comando.

Se você tiver 1 CPU, cada um dos seguintes comandos garante o container no máximo 50% da CPU a cada segundo.

```bash
docker run -it --cpus=".5" ubuntu /bin/bash
```

*The full command for stopping all docker containers is now*:
`docker container stop $(docker container ls -a -q)`
`docker container stop $(docker container ls -a -q) && docker system prune -a -f --volumes`

*Containers*: `docker container rm $(docker container ls -a -q)`
*Images*: `docker image rm $(docker image ls -a -q)`
*Volumes*: `docker volume rm $(docker volume ls -q)`
*Networks*: `docker network rm $(docker network ls -q)`

https://docs.docker.com/engine/reference/commandline/system_prune/#examples

```bash
alias docker-clean-unused='docker system prune --all --force --volumes'
alias docker-clean-all='docker stop $(docker container ls -a -q) && docker system prune -a -f --volumes'
alias docker-clean-containers='docker container stop $(docker container ls -a -q) && docker container rm $(docker container ls -a -q)'
```

**`Docker run`**
`docker container run <options> <image> <command> <args...>`

`[CTRL-P] + Q`: Sair do `bash` sem encerrar o container.

Os parâmetros mais utilizados na execução do container são:

`[-d]`: Execução do container em background.
`[-i]`: Modo interativo, mantém STDIN aberto, mesmo que não esteja conectado.
`[-t]`: Mantém o STDIN aberto mesmo sem console anexado, aloca um `pseudo-tty`.
`[--rm]`: Automaticamente remove o container após finalização (Não funciona com `[-d]`).
`[-name]`: Nomear o container.
`[-v]`: Mapeamento de volumes. Requer o nome do volume, dois pontos e, em seguida, o caminho absoluto para onde o volume deve aparecer dentro do container.
`[-p]`: Mapeamento de portas.
`[-m], [--memory]`: Limitar o uso de memória RAM.
`[-c], [--cpu-shares]`: Balancear o uso de CPU.

**`Docker tag`**
```
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```

```bash
docker tag app_webserver allysonsilva/nginx:1.0
docker tag app_webserver allysonsilva/nginx:latest

# or
docker tag $USERNAME/$IMAGE:latest $USERNAME/$IMAGE:$version

# Template
docker push $USERNAME/$IMAGE:latest
docker push $USERNAME/$IMAGE:$version

# Sending image to Docker Hub
docker push allysonsilva/nginx:1.0
docker push allysonsilva/nginx:latest
```

###  Cache de instruções

É importante atentar que o arquivo *Dockerfile* é uma sequência de instruções que é lido do topo até sua base e cada linha é executada por vez, ou seja, caso alguma instrução dependa de uma outra instrução, essa dependência deve vir mais acima no documento.

O resultado de cada instrução desse arquivo é armazenado em um cache local, ou seja, caso o *Dockerfile* não seja modificado na próxima criação da imagem (build) o processo não demorará, pois tudo estará no cache, mas caso algo seja modificado apenas a instrução modificada e as posteriores serão executadas novamente.

Uma imagem do Docker é construída a partir de uma série de camadas. Cada camada representa uma instrução no Dockerfile da imagem. Cada camada, exceto a última, é somente leitura. Considere o seguinte Dockerfile:

Sempre que possível, o Docker reutilizará as imagens intermediárias (cache) para acelerar o docker `build` processo de forma significativa.

```docker
FROM ubuntu:16.04
COPY . /app
RUN make /app
CMD python /app/app.py
```

Este Dockerfile contém quatro comandos, cada um dos quais cria uma camada. A `FROM` instrução começa criando uma camada a partir da *ubuntu:16.04* imagem. O `COPY` comando adiciona alguns arquivos do diretório atual do cliente do Docker. O `RUN` comando constrói seu aplicativo usando o `make` comando. Finalmente, a última camada especifica qual comando deve ser executado dentro do container.

Cada camada é apenas um conjunto de diferenças da camada anterior. As camadas estão empilhadas umas sobre as outras. Quando você cria um novo container, adiciona uma nova camada gravável sobre as camadas subjacentes. Essa camada é geralmente chamada de "camada de container". Todas as alterações feitas no container em execução, como gravar novos arquivos, modificar arquivos existentes e excluir arquivos, são gravadas nessa camada de container gravável fina.

A principal diferença entre um container e uma imagem é a camada gravável superior. Todas as gravações no container que adicionam novos dados ou modificam dados existentes são armazenadas nessa camada gravável. Quando o container é excluído, a camada gravável também é excluída. A imagem subjacente permanece inalterada.

Como cada container tem sua própria camada de container gravável fina e todas as alterações são armazenadas nessa camada de container, isso significa que vários containers podem compartilhar o acesso à mesma imagem subjacente e ainda ter seu próprio estado de dados.

Quando você inicia um container, uma camada de container gravável e fina é adicionada na parte superior das outras camadas. Quaisquer alterações feitas no container ao sistema de arquivos são armazenadas aqui. Todos os arquivos que o container não altera não são copiados para essa camada gravável. Isso significa que a camada gravável é a menor possível.

Copy-on-write é uma estratégia de compartilhamento e cópia de arquivos para máxima eficiência. Se um arquivo ou diretório existir em uma camada inferior da imagem, e outra camada (incluindo a camada gravável) precisar de acesso de leitura a ele, ele apenas usará o arquivo existente. A primeira vez que outra camada precisa modificar o arquivo (ao construir a imagem ou ao executar o container), o arquivo é copiado para essa camada e modificado. Isso minimiza a E / S e o tamanho de cada uma das camadas subseqüentes.

A estratégia de copy-on-write do Docker não apenas reduz a quantidade de espaço consumida pelos containers, mas também reduz o tempo necessário para iniciar um container. Na hora de início, o Docker só precisa criar a camada gravável fina para cada container.

O copy-on-write não apenas economiza espaço, mas também reduz o tempo de inicialização. Quando você inicia um container (ou vários containers da mesma imagem), o Docker só precisa criar a camada de container gravável fina.

Docker Engine uses namespaces such as the following on Linux:

The pid namespace: Process isolation (PID: Process ID).
The net namespace: Managing network interfaces (NET: Networking).
The ipc namespace: Managing access to IPC resources (IPC: InterProcess Communication).
The mnt namespace: Managing filesystem mount points (MNT: Mount).
The uts namespace: Isolating kernel and version identifiers. (UTS: Unix Timesharing System).

**Control groups**
Docker Engine on Linux also relies on another technology called control groups (cgroups). A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.

## Containers/Images

> Os containers são isolados a nível de disco, memória, processamento e rede. Essa separação permite grande flexibilidade, onde ambientes distintos podem coexistir no mesmo host, sem causar qualquer problema. Vale salientar que o *overhead* nesse processo é o mínimo necessário, pois cada container normalmente carrega apenas um processo, que é aquele responsável pela entrega do serviço desejado. Em todo caso, esse container também carrega todos os arquivos necessários (configuração, biblioteca e afins) para execução completamente isolada.

> Um container nada mais e do que um, ou poucos processos em execução.

> Docker Container ainda e um serviço e por ser um serviço deve ser tratado como um processo e não como uma máquina.

> Um Container é uma instância em tempo de execução de uma imagem - o que a imagem se torna na memória quando executada (isto é, uma imagem com estado ou um processo do usuário).

- Um Container é executado nativamente no Linux e compartilha o kernel da máquina host com outros containers.
- Um Container é uma instância executável de uma imagem. Você pode criar, iniciar, parar, mover ou excluir um container usando a API do Docker ou o CLI. Você pode conectar um container a uma ou mais redes, anexar armazenamento a ele ou até mesmo criar uma nova imagem com base em seu estado atual.
- O Docker usa uma tecnologia chamada `namespaces` para fornecer o espaço de trabalho isolado chamado container. Quando você executa um container, o Docker cria um conjunto de namespaces para esse container. Esses namespaces fornecem uma camada de isolamento. Cada aspecto de um container é executado em um namespace separado e seu acesso é limitado a esse namespace.
- O Docker Engine no Linux também depende de outra tecnologia chamada de grupos de controle (`cgroups`). Um `cgroup` limita um aplicativo a um conjunto específico de recursos. Grupos de controle permitem que o Docker Engine compartilhe recursos de hardware disponíveis para containers e, opcionalmente, imponha limites e restrições. Por exemplo, você pode limitar a memória disponível para um container específico.
- Se os containers dependerem uns dos outros, você poderá usar as redes de containers do Docker para garantir que esses containers possam se comunicar.
- Uma imagem é um modelo somente leitura com instruções para criar um container do Docker. Muitas vezes, uma imagem é baseada em outra imagem, com alguma personalização adicional.
- Os containers conectados à mesma rede `bridge` definida pelo usuário expõem efetivamente todas as portas entre si. Para uma porta ser acessível a containers ou a hosts não-Docker em redes diferentes, essa porta deve ser publicada usando o sinalizador `-p` ou `--publish`.

Os containers do Docker, em essência, são um agrupamento de várias camadas do sistema de arquivos que são empilhadas umas sobre as outras em uma sequência para criar o layout final que é executado em um ambiente isolado pelo kernel da máquina host. Cada camada descreve quais arquivos foram adicionados, modificados e/ou excluídos em relação à sua camada pai anterior. Por exemplo, você tem uma camada base com um arquivo `/foo/bar`, e a próxima camada adiciona um arquivo `/foo/baz`. Quando o container iniciar, ele combinará as camadas na ordem e o container resultante terá ambos `/foo/bar` e `/foo/baz`. Esse processo é repetido para que qualquer nova camada termine com um sistema de arquivos totalmente composto para executar o serviço ou serviços especificados.

### Commit

> Create a new image from a container’s changes.

É importante saber que toda e qualquer alteração realizada dentro de um container é volátil. Ou seja, se o finalizarmos, ao iniciar qualquer alteração não irá permanecer.

Para tornar as alterações permanentes, é necessário *commitar* o container – sim, o *Docker* possui um recurso de versionamento igual ao *Git*. Sempre que um **commit** for feito, estaremos salvando o estado atual do container na imagem utilizada.

```bash
docker commit <container-id-or-name> ubuntu/nginx
```

```bash
docker login
​docker push HUBUSER/ubuntu_testing
```

Ao executar o commit, estamos criando uma nova imagem baseada na inicial.

**Criando imagens com commit**

```bash
docker container run -it --name <container-name> ubuntu:16.04 bash

apt-get update
apt-get install nginx -y

exit

docker container stop <container-name>
docker container commit <container-name> meuubuntu:nginx
docker commit -m "Installed Nginx" -a "Allyson Silva" <container-name>HUBU HUBUSER/ubuntu_nginx:v1.0.0.2018

docker login
​docker push HUBUSER/ubuntu_nginx:v1.0.0.2018
```

No exemplo acima o nome `HUBUSER/ubuntu_nginx` é a imagem resultante do commit; o estado do `<container-name>` é armazenado em uma imagem chamada `HUBUSER/ubuntu_nginx` que, nesse caso, a única modificação que temos da imagem oficial do ubuntu na versão 16.04 é o pacote `nginx` instalado.

Para testar sua nova imagem, vamos criar um container a partir dela e verificar se o `nginx` está instalado:

```bash
docker container run -it --rm HUBUSER/ubuntu_nginx dpkg -l nginx
```

Vale salientar que o método **commit** não é a melhor opção para criar imagens, pois, como verificamos, o processo de modificação da imagem é completamente manual e apresenta certa dificuldade para rastrear as mudanças efetuadas, uma vez que, o que foi modificado manualmente não é registrado, automaticamente, na estrutura do docker.

Outro exemplo:

```bash
$ docker image pull alpine:latest
$ docker container run -it --name alpine-test alpine /bin/sh
```

```bash
$ apk update
$ apk upgrade
$ apk add --update nginx
$ rm -rf /var/cache/apk/*
$ mkdir -p /tmp/nginx/
$ exit
```

```bash
$ docker container commit <container-name> <REPOSITORY>:<TAG>
$ docker container commit alpine-test local:broken-container
```

### Containers autodestrutivos

> É possível criar containers autodestrutivos com o uso da opção `--rm`.

```bash
docker run -it --rm -p 8080:80 ubuntu/nginx /bin/bash
```

Observe o uso da opção `-p`. Com ela, estamos informando ao *Docker* que a porta *8080* no *host* será aberta e mapeada para a porta *80* no container.

```bash
docker port <container-id-or-name>
```

Finalizando a saída do container "exit" não vemos mais o container com `docker ps -a` o container somente existe enquanto estávamos dentro dele.

### Containers de execução em background

`[-d]` envia toda a execução para background.

```bash
docker run -d -p 8080:80 ubuntu/nginx /usr/sbin/nginx -g "daemon off;"
```

### Comandos

**Inspecionar o mapeamentos dos volumes de um container**
`docker inspect -f {{.Mounts}} <container-id>`

**História da construção da imagem**
`docker history <image>`

**Executar um container**
`docker run -d -p <port_host>:<port_container> –name <container-name> <imagem-base-name> <command>`

**Iniciar uma sessão bash em um container que esteja rodando**
`docker exec -it <container-name> bash`
`docker container exec nginx cat /etc/debian_version`
`docker container exec -i -t nginx /bin/bash`

**Inspecionar alterações em arquivos ou diretórios no sistema de arquivos de um container**
`docker diff <container-name-or-id>`

**Logs de um container**
`docker logs --follow <container-name-or-id>`
`docker container logs --since 2017-06-24T15:00 -t <container-name-or-id>`

**Lista os processos em execução no container**
`docker container top <container-name-or-id>`

**O comando stats fornece informações em tempo real**
`docker container stats <container-name-or-id>`

**Para reduzir pela metade a prioridade da CPU e definir um limite de memória de 128M:**
`docker container run -d --name <container-name> --cpu-shares 512 --memory 128M -p 8080:80 nginx`

**Atualizar um container já em execução**
`docker container update --cpu-shares 512 --memory 128M <container-name-or-id>`
`docker container inspect <container-name-or-id> | grep -i memory`

**Aguardará até 60 segundos antes de enviar um `SIGKILL`, caso precise ser enviado para eliminar o processo**
`docker container stop -t 60 <container-name-or-id>`
`docker container restart -t 60 <container-name-or-id>`

**Enviar um `SIGKILL` imediatamente para o container executando o comando `kill`**
`docker container kill <container-name-or-id>`
`docker container stop <container-name-or-id> && docker container rm <container-name-or-id>`

**O comando `create` é bem parecido com o comando `run`, exceto que ele não inicia o container, mas prepara e configura um**
`docker container create --name <container-name> -p 8080:80 nginx`
`docker container ls -a && docker container run <container-name-or-id>`

**O comando `port` exibe a porta junto com qualquer mapeamento para o container**
`docker container port <container-name-or-id>`

**This will start your application back up, this time in detached mode**
`docker-compose up -d`

**Running the following command will validate docker-compose.yml file**
`docker-compose config`

```bash
$ docker-compose start
$ docker-compose stop
$ docker-compose restart
$ docker-compose pause
$ docker-compose unpause
```

```bash
$ docker-compose pause db
$ docker-compose unpause db
```

```bash
$ docker-compose top
$ docker-compose top db
$ docker-compose logs --follow db
$ docker-compose run --volume data_volume:/app composer install
$ docker-compose up -d --scale worker=3
$ docker-compose kill
$ docker-compose down --rmi all --volumes
```

## Volumes

> Volumes são diretórios configurados dentro de um container que fornecem o recurso de compartilhamento e persistência de dados.

> Por padrão, os volumes não são removidos para evitar que dados importantes sejam excluídos se não houver nenhum container usando o volume no momento.

Ao utilizar volumes, o Docker monta essa camada no nível imediatamente inferior ao do container, o que permite o acesso rápido de todo dado armazenado nessa camada, resolvendo o problema de performance.

O volume também resolve questões de persistência de dados, pois as informações armazenadas na camada do container são perdidas ao remover o container, ou seja, ao utilizar volumes temos maior garantia no armazenamento desses dados.

• Os volumes podem ser compartilhados ou reutilizados entre containers.
• Toda alteração feita em um volume é de forma direta.
• Volumes alterados não são incluídos quando atualizamos uma imagem.

`[-v]` `~/nginxlogs:/var/log/nginx` Configura um volume de **`bindmount`** que vincula o diretório `/var/log/nginx` de dentro do container *Nginx* ao diretório `~/nginxlogs` na máquina host. O Docker usa `:`(dois pontos) para dividir o caminho do host do caminho do container, e o caminho do host sempre vem primeiro.

```bash
$ docker volume create redis_data
$ docker container run -d --name redis -v redis_data:/data --network net_app redis:alpine
$ docker volume inspect redis_data
```

O Docker oferece três maneiras diferentes de montar dados em um container a partir do host do Docker: volumes, bind mounts ou tmpfs volumes. Quando em dúvida, os volumes são quase sempre a escolha certa.

Os **volumes** são armazenados em uma parte do sistema de arquivos do host que é gerenciado pelo Docker (`/var/lib/docker/volumes/` no Linux). Os processos não-Docker não devem modificar essa parte do sistema de arquivos. Os volumes são a melhor maneira de persistir dados no Docker.

**Bind mounts** podem ser armazenadas em qualquer lugar no sistema host. Eles podem até ser arquivos ou diretórios importantes do sistema. Processos não-Docker no host do Docker ou em um container do Docker podem modificá-los a qualquer momento.

**Volumes**: Criado e gerenciado pelo Docker. Você pode criar um volume explicitamente usando o `docker volume create` comando ou o Docker pode criar um volume durante a criação de containers ou serviços.

Quando você cria um volume, ele é armazenado em um diretório no host do Docker. Quando você monta o volume em um container, esse diretório é montado no container. Isso é semelhante à maneira como as bind mounts funcionam, exceto que os volumes são gerenciados pelo Docker e isolados da funcionalidade principal da máquina host.

Um determinado volume pode ser montado em vários containers simultaneamente. Quando nenhum container em execução estiver usando um volume, o volume ainda estará disponível para o Docker e não será removido automaticamente. Você pode remover volumes não utilizados usando `docker volume prune`.

**Bind mounts**: Disponível desde os primeiros dias do Docker. Bind mounts têm funcionalidade limitada em comparação com volumes. Quando você usa uma bind mount, um arquivo ou diretório na máquina host é montado em um container. O arquivo ou diretório é referenciado por seu caminho completo na máquina host. O arquivo ou diretório não precisa existir no host do Docker. Ele é criado sob demanda, se ainda não existir. As bind mounts são muito eficientes, mas dependem do sistema de arquivos da máquina host que possui uma estrutura de diretórios específica disponível. Se você estiver desenvolvendo novos aplicativos Docker, considere o uso de volumes nomeados. Você não pode usar os comandos do Docker CLI para gerenciar diretamente as montagens de ligação.

```bash
$ docker run -d \
  -it \
  --name devtest \
  --mount type=bind,source="$(pwd)"/target,target=/app,readonly \
  nginx:latest
```

```bash
$ docker inspect devtest
```

Bind mounts e volumes podem ser montados em containers usando o sinalizador `-v` ou `--volume`, mas a sintaxe de cada um é um pouco diferente. No entanto, no Docker 17.06 e superior, recomendamos usar o `--mount` sinalizador para containers e serviços, para bind mounts, volumes ou montagens tmpfs, pois a sintaxe é mais clara.

## Docker for macOS

A camada **Hypervisor** dá lugar ao *Docker Engine* que permite que o Kernel seja acessado diretamente pelas aplicações, assim eliminando a necessidade de um sistema operacional para cada aplicação e compartilhando um único Kernel.

Com o *Hypervisor* um sistema operacional completo é virtualizado para poder executar a aplicação. Já o Docker executa a aplicação sem a necessidade de um sistema operacional completo e aproveitando o Kernel do sistema operacional já instalado, sem a necessidade de criar outro sistema operacional de forma virtualizada. Ou seja, com container é possível virtualizar a nível de S.O, iniciando-o como um "*processo*".

**Acessar Docker CLI**
```bash
screen ~/Library/Containers/com.docker.docker/Data/com.docker.driver.amd64-linux/tty
```

## Networks

Em termos de Docker, uma rede bridge usa uma ponte de software que permite que containers conectados à mesma rede bridge se comuniquem, ao mesmo tempo em que fornece isolamento de containers que não estão conectados a essa rede bridge. O driver bridge do Docker instala automaticamente as regras na máquina host para que os containers em diferentes redes bridge não possam se comunicar diretamente entre si.

No Linux, o Docker manipula regras `iptables` para fornecer isolamento de rede. Esse é um detalhe de implementação e você não deve modificar as inserções do Docker de regras em suas políticas `iptables`.

Por padrão, quando você cria um container, ele não publica nenhuma de suas portas para o mundo externo. Para disponibilizar uma porta para serviços fora do Docker ou para containers do Docker que não estejam conectados à rede do container, use o sinalizador `--publish` ou `-p`. Isso cria uma regra de firewall que mapeia uma porta de container para uma porta no host do Docker.

Quando o container é iniciado, ele só pode ser conectado a uma única rede usando `--network`. No entanto, você pode conectar um container em execução a várias redes usando `docker network connect`.

Da mesma forma, o nome do host de um container é padronizado como o nome do container no Docker. Você pode substituir o nome do host usando `--hostname`. Ao se conectar a uma rede existente usando `docker network connect`, você pode usar o `--alias` sinalizador para especificar um alias de rede adicional para o container nessa rede.

Por padrão, um container herda as configurações de `DNS` do daemon do Docker, incluindo o `/etc/hosts` e `/etc/resolv.conf`.

Por padrão, o container recebe um endereço IP para cada rede do Docker à qual ele se conecta. O endereço IP é atribuído a partir do pool atribuído à rede, de modo que o daemon do Docker atua efetivamente como um servidor DHCP para cada container. Cada rede também possui uma máscara de sub-rede e gateway padrão.

**bridge**: O driver de rede padrão. Se você não especificar um driver, esse é o tipo de rede que você está criando. As redes bridges geralmente são usadas quando seus aplicativos são executados em containers autônomos que precisam se comunicar.

Quando você cria um novo container, pode especificar um ou mais sinalizadores `--network`. Este exemplo conecta um container `Nginx` à `my-net` rede. Também publica a porta 80 no container para a porta 8080 no host do Docker, para que os clientes externos possam acessar essa porta. Qualquer outro container conectado à `my-net` rede tem acesso a todas as portas no `my-nginx` container e vice-versa.

```bash
$ docker create --name my-nginx \
  --network my-net \
  --publish 8080:80 \
  nginx:latest
```

Para conectar um container em execução a uma ponte definida pelo usuário existente, use o `docker network connect` comando. O comando a seguir conecta um `my-nginx` container já em execução a uma `my-net` rede já existente :

```bash
$ docker network connect my-net my-nginx
```

Para desconectar um container em execução de uma ponte definida pelo usuário, use o `docker network disconnect` comando. O comando a seguir desconecta o `my-nginx` container da `my-net` rede.

```bash
$ docker network disconnect my-net my-nginx
```

Se você usar o host driver de rede para um container, a pilha de rede desse container não será isolada do host do Docker. Por exemplo, se você executar um container que se liga à porta 80 e usar a host rede, o aplicativo do container estará disponível na porta 80 no endereço IP do host.

Se o seu container ou serviço não publicar portas, a rede do host não terá efeito.

Se você quiser desabilitar completamente a pilha de rede em um container, poderá usar o sinalizador `--network none` ao iniciar o container. Dentro do container, somente o dispositivo de `loopback` é criado.

```bash
$ docker run --rm -dit \
  --network none \
  --name no-net-alpine \
  alpine:latest \
  ash
```

Verifique a pilha de rede do container, executando alguns comandos de rede comuns no container. Observe que não `eth0` foi criado mais retorna vazio porque não há tabela de roteamento.

```bash
$ docker exec no-net-alpine ip link show
$ docker exec no-net-alpine ip route
```

**Inspecione a rede `bridge` rede para ver quais containers estão conectados a ela**.
```bash
$ docker network inspect bridge
```

```bash
$ docker network create --driver bridge alpine-net
```

**Sair do container sem encerrar**
`CTRL+p`  `CTRL+q` (mantenha pressionado `CTRL` e digite `p` seguido por `q`)

### Exemplo rede `host`

O objetivo deste tutorial é iniciar um `nginx` container que se conecta diretamente à porta 80 no host do Docker. Do ponto de vista da rede, esse é o mesmo nível de isolamento que o processo `nginx` estava sendo executado diretamente no host do Docker e não em um container.

```bash
docker run --rm -itd --network host --name my_nginx nginx
ip addr show
sudo netstat -tulpn | grep :80
docker container stop my_nginx
docker container rm my_nginx
```

As redes do Docker não ocupam muito espaço em disco, mas criam iptables regras, conectam dispositivos de rede e entradas da tabela de roteamento. Para limpar essas coisas, você pode usar `docker network prune` para limpar redes que não são usadas por nenhum container.

```bash
$ docker network prune
```

Por padrão, você é solicitado a continuar. Para ignorar o prompt, use o sinalizador `-f` ou `--force`.

Por padrão, todas as redes não utilizadas são removidas. Você pode limitar o escopo usando o `--filter` sinalizador. Por exemplo, o comando a seguir remove apenas redes com mais de 24 horas:

```bash
$ docker network prune --filter "until=24h"
```

## Docker Compose

> Compose é uma ferramenta para definir e executar aplicativos Docker com vários containers. Com Compose, você usa um arquivo YAML para configurar os serviços do seu aplicativo. Então, com um único comando, você cria e inicia todos os serviços da sua configuração.

O comando `docker-compose run` permite que você execute comandos únicos para seus serviços. Por exemplo, para ver quais variáveis ​​de ambiente estão disponíveis para o serviço `web`:

```bash
$ docker-compose run web env
```

Para remover os containers completamente. `--volumes` também para remover o volume de dados usado pelo container:

```bash
$ docker-compose down --volumes
```

### [Variáveis de ambiente](https://docs.docker.com/compose/environment-variables/)

++ https://docs.docker.com/compose/env-file/

> Compose suporta a declaração de variáveis ​​de ambiente padrão em um arquivo de ambiente chamado `.env` colocado na pasta onde o comando `docker-compose` é executado (diretório de trabalho atual).


*Quaisquer valores booleanos; true, false, yes no, precisa ser colocado entre aspas para garantir que elas não sejam convertidas para True ou False pelo analisador YML.*

*Variáveis ​​de ambiente definidas no arquivo `.env` não são automaticamente visíveis dentro de containers.*

#### Regras de sintaxe

Essas regras de sintaxe se aplicam ao arquivo `.env`:

- Compose espera que cada linha em um arquivo `env` esteja em formato `VAR=VAL`.
- Linhas começando com `#` são processadas como comentários e ignoradas.
- Linhas em branco são ignoradas.
- Não há tratamento especial de aspas. Isso significa que eles são parte do `VAL`.

#### Ordem de precedências

Os valores presentes no ambiente em tempo de execução sempre substituem os definidos dentro do arquivo `.env`. Da mesma forma, os valores transmitidos por meio de argumentos da linha de comando têm mais precedência.

Valores no shell têm precedência sobre aqueles especificados no arquivo `.env`.

```
$ export TAG=v2.0
$ docker-compose config
```

```docker
version: '3'
services:
  web:
    image: 'webapp:v2.0'
```

#### Pass environment variables to containers

Quando você define a mesma variável de ambiente em vários arquivos, aqui está a prioridade usada pelo Compose para escolher qual valor usar:

1. Compose arquivo.
2. Arquivo de ambiente.
3. Dockerfile.
4. Variável não está definida.

No exemplo abaixo, definimos a mesma variável de ambiente em um arquivo Environment e no arquivo Compose:

```
$ cat ./Docker/api/api.env
NODE_ENV=test
```

```bash
$ cat docker-compose.yml
version: '3'
services:
  api:
    image: 'node:6-alpine'
    env_file:
     - ./Docker/api/api.env
    environment:
     - NODE_ENV=production
```

Quando você executa o container, a variável de ambiente definida no arquivo Compose tem precedência.

```
$ docker-compose exec api node

> process.env.NODE_ENV
'production'
```

**env_file**

Adicione variáveis ​​de ambiente de um arquivo. Pode ser um valor único ou uma lista.

Se você especificou um arquivo de composição com `docker-compose -f FILE`, os caminhos em `env_file` são relativos ao diretório em que o arquivo está.

As variáveis ​​de ambiente declaradas na seção do ambiente substituem esses valores - isso vale mesmo se esses valores estiverem vazios ou indefinidos.

#### Build com variáveis

> If your service specifies a build option, variables defined in environment are not automatically visible during the build. Use the args sub-option of build to define build-time environment variables.

### [Networking in Compose](https://docs.docker.com/compose/networking/)

### BUILD

> Opções de configuração que são aplicadas no momento da criação.

Se você especificar image, assim como build, então Compose, nomeia a imagem construída com o webapp opcional e tag especificado em image:

```
build: ./dir
image: webapp:tag
```

Isso resulta em uma imagem nomeada webapp e marcada tag, criada a partir de ./dir.

#### CONTEXTO

> Um caminho para um diretório contendo um Dockerfile ou um URL para um repositório git.

Quando o valor fornecido é um caminho relativo, ele é interpretado como relativo ao local do arquivo Compose. Esse diretório também é o contexto de construção que é enviado para o daemon do Docker.

```
build:
  context: ./dir
  dockerfile: Dockerfile-alternate
```
