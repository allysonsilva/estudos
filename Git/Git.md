# GIT

## Primeiros passos

> O controle de versão é um sistema que registra as mudanças feitas em um arquivo ou um conjunto de arquivos ao longo do tempo de forma que você possa recuperar versões específicas.

> Git considera que os dados são como um conjunto de snapshots (captura de algo em um determinado instante, como em uma foto) de um mini-sistema de arquivos. Cada vez que você salva ou consolida (commit) o estado do seu projeto no Git, é como se ele tirasse uma foto de todos os seus arquivos naquele momento e armazenasse uma referência para essa captura. Para ser eficiente, se nenhum arquivo foi alterado, a informação não é armazenada novamente - apenas um link para o arquivo idêntico anterior que já foi armazenado.

> Tudo no Git tem seu checksum (valor para verificação de integridade) calculado antes que seja armazenado e então passa a ser referenciado pelo checksum. Isso significa que é impossível mudar o conteúdo de qualquer arquivo ou diretório sem que o Git tenha conhecimento. Essa funcionalidade é parte fundamental do Git e é integral à sua filosofia. Você não pode perder informação em trânsito ou ter arquivos corrompidos sem que o Git seja capaz de detectar.

> Tudo que o Git armazena é identificado não por nome do arquivo mas pelo valor do hash do seu conteúdo.

### Os Três Estados

**Git faz com que seus arquivos sempre estejam em um dos três estados fundamentais: consolidado (`committed`), modificado (`modified`) e preparado (`staged`).**

> Dados são ditos consolidados quando estão seguramente armazenados em sua base de dados local.

> Modificado trata de um arquivo que sofreu mudanças mas que ainda não foi consolidado na base de dados.

> Um arquivo é tido como preparado quando você marca um arquivo modificado em sua versão corrente para que ele faça parte do snapshot do próximo commit (consolidação).

Isso nos traz para as três seções principais de um projeto do Git: o diretório do Git (**git directory**, **repository**), o diretório de trabalho (**working directory**), e a área de preparação (**staging area**).

![Local Operations](https://git-scm.com/figures/18333fig0106-tn.png "Local Operations")

**Working Directory** – Toda alteração do repositório fica registrada nessa área antes de ser confirmada (ex.: Untracked files, ou arquivos novos);
**Index (Staging area)** – Aqui ficam todas as alterações já confirmadas, e que entrarão em um próximo commit;
**Repository**: Aqui são os arquivos permanentes do seu diretório (projeto). Todas as alterações já confirmadas e comitadas;

*O diretório do Git é o local onde o Git armazena os metadados e o banco de objetos de seu projeto. Esta é a parte mais importante do Git, e é a parte copiada quando você clona um repositório de outro computador.*

*O diretório de trabalho é um único checkout de uma versão do projeto. Estes arquivos são obtidos a partir da base de dados comprimida no diretório do Git e colocados em disco para que você possa utilizar ou modificar.*

*A área de preparação é um simples arquivo, geralmente contido no seu diretório Git, que armazena informações sobre o que irá em seu próximo commit. É bastante conhecido como índice (index), mas está se tornando padrão chamá-lo de área de preparação.*

O workflow básico do Git pode ser descrito assim:

1. Você modifica arquivos no seu diretório de trabalho.
2. Você seleciona os arquivos, adicionando snapshots deles para sua área de preparação.
3. Você faz um commit, que leva os arquivos como eles estão na sua área de preparação e os armazena permanentemente no seu diretório Git.

**Se uma versão particular de um arquivo está no diretório Git, é considerada _consolidada_. Caso seja modificada mas foi adicionada à área de preparação, está _preparada_. E se foi alterada desde que foi obtida mas não foi preparada, está _modificada_.**

## Essêncial

### Obtendo um Repositório Git

#### Inicializando um Repositório em um Diretório Existente

```bash
$ git init
```

Isso cria um novo subdiretório chamado `.git` que contem todos os arquivos necessários de seu repositório — um esqueleto de repositório Git.

Caso você queira começar a controlar o versionamento dos arquivos existentes (diferente de um diretório vazio), você provavelmente deve começar a monitorar esses arquivos e fazer um commit inicial. Você pode realizar isso com poucos comandos git add que especificam quais arquivos você quer monitorar, seguido de um commit:

```bash
$ git add README.md
$ git commit -m 'Initial commit'
```

#### Clonando um Repositório Existente

Caso você queira copiar um repositório Git já existente — por exemplo, um projeto que você queira contribuir — o comando necessário é `git clone`. Cada versão de cada arquivo no histórico do projeto é obtida quando você roda `git clone`.

Você clona um repositório com `git clone [url]`.

```bash
$ git clone git://github.com/laravel/laravel.git
```

Caso você queira clonar o repositório em um diretório diferente, é possível especificar esse diretório utilizando a opção abaixo:

```bash
$ git clone git://github.com/laravel/laravel.git LaraApp
```

Este comando faz exatamente a mesma coisa que o anterior, mas o diretório alvo será chamado `LaraApp`.

O Git possui diversos protocolos de transferência que você pode utilizar. O exemplo anterior utiliza o protocolo `git://`, mas você também pode ver `http(s)://` ou `user@server:/path.git`, que utilizam o protocolo de transferência _SSH_.

### Gravando Alterações no Repositório

Lembre-se que cada arquivo em seu diretório de trabalho pode estar em um de dois estados: _monitorado_ ou _não monitorado_.

Cada arquivo no repositório pode ter o status monitorado (**tracked**) ou não-monitorado (**untracked**).

> Arquivos monitorados são arquivos que estavam no último snapshot(no último commit); podendo estar inalterados, modificados ou selecionados.

> Arquivos não monitorados são todo o restante — qualquer arquivo em seu diretório de trabalho que não estava no último snapshot e também não estão em sua área de seleção.

> Arquivos monitorados são os que estavam presentes no último *commit*. Portanto, um arquivo novo criado no diretório fica com o status não-monitorado. Quando queremos realizar um *commit*, precisamos antes marcar os arquivos que serão comitados. Isto é feito adicionando-os numa área conhecida como staging. Após todos os arquivos necessários marcados para o *commit*, basta efetivar a operação.

*Quando um repositório é inicialmente clonado, todos os seus arquivos estarão monitorados e inalterados porque você simplesmente os obteve e ainda não os editou.*

Conforme você edita esses arquivos, o Git passa a vê-los como modificados, porque você os alterou desde seu último commit. Você seleciona esses arquivos modificados e então faz o commit de todas as alterações selecionadas e o ciclo se repete.

![File Status Lifecycle](https://git-scm.com/figures/18333fig0201-tn.png "File Status Lifecycle")

#### Verificando o Status de Seus Arquivos

> Um commit é apenas um ponteiro para um snapshot do conteúdo adicionado na área de staging na hora do commit.

> A principal ferramenta utilizada para determinar quais arquivos estão em quais estados é o comando `git status`.

*Não monitorado significa basicamente que o Git está vendo um arquivo que não existia na última captura (commit); o Git não vai incluí-lo nas suas capturas de commit até que você o diga explicitamente que assim o faça.* Ele faz isso para que você não inclua acidentalmente arquivos binários gerados, ou outros arquivos que você não têm a intenção de incluir.

#### Monitorando Novos Arquivos

> Para passar a monitorar um novo arquivo, use o comando `git add`.

*O comando `git add` recebe um caminho de um arquivo ou diretório; se é de um diretório, o comando adiciona todos os arquivos do diretório recursivamente.*

#### Ignorando Arquivos

As regras para os padrões que você pode pôr no arquivo .gitignore são as seguintes:

* Linhas em branco ou iniciando com `#` são ignoradas.
* Padrões glob comuns funcionam.
* Você pode terminar os padrões com uma barra (`/`) para especificar diretórios.
* Você pode negar um padrão ao iniciá-lo com um ponto de exclamação (`!`).

#### Visualizando Suas Mudanças Selecionadas e Não Selecionadas

> Você quer saber exatamente o que você alterou, não apenas quais arquivos foram alterados — você pode utilizar o comando `git diff`.

Você vai utilizá-lo com frequência para responder estas duas perguntas: O que você alterou, mas ainda não selecionou (**stage**)? E o que você selecionou, que está para ser comitado? Apesar do comando `git status` responder essas duas perguntas de maneira geral, o `git diff` mostra as linhas exatas que foram adicionadas e removidas — o patch, por assim dizer.

> Este comando compara o que está no seu diretório de trabalho com o que está na sua área de seleção (**staging**). O resultado te mostra as mudanças que você fez que ainda não foram selecionadas.

Se você quer ver o que selecionou que irá no seu próximo commit, pode utilizar `git diff --cached`. (Nas versões do Git 1.6.1 e superiores, você também pode utilizar `git diff --staged`, que deve ser mais fácil de lembrar.) Este comando compara as mudanças selecionadas com o seu último commit.

É importante notar que o `git diff` por si só não mostra todas as mudanças desde o último commit — apenas as mudanças que ainda não foram selecionadas.

#### Fazendo Commit de Suas Mudanças

Agora que a sua área de seleção está do jeito que você quer, você pode fazer o commit de suas mudanças. Lembre-se que tudo aquilo que ainda não foi selecionado — qualquer arquivo que você criou ou modificou que você não tenha rodado o comando `git add` desde que editou — não fará parte deste commit. Estes arquivos permanecerão como arquivos modificados em seu disco.

O jeito mais simples é digitar `git commit`:

```bash
$ git commit:
```

Ao fazer isso, seu editor de escolha é acionado. (Isto é configurado através da variável de ambiente `$EDITOR` de seu shell - normalmente vim ou emacs, apesar de poder ser configurado o que você quiser utilizando o comando `git config --global core.editor`).

*Para um lembrete ainda mais explícito do que foi modificado, você pode passar a opção `-v` para o `git commit`. Ao fazer isso, aparecerá a diferença (diff) da sua mudança no editor para que possa ver exatamente o que foi feito.*

Quando você sair do editor, o Git criará o seu commit com a mensagem (com os comentários e o diff retirados).

Alternativamente, você pode digitar sua mensagem de commit junto ao comando `commit` ao especificá-la após a flag `-m`.

*Lembre-se que o commit grava a captura da área de seleção. Qualquer coisa que não foi selecionada ainda permanece lá modificada; você pode fazer um outro commit para adicioná-la ao seu histórico. Toda vez que você faz um commit, está gravando a captura do seu projeto o qual poderá reverter ou comparar posteriormente.*

#### Pulando a Área de Seleção

Se você quiser pular a área de seleção, o Git provê um atalho simples. Informar a opção `-a` ao comando `git commit` faz com que o Git selecione automaticamente cada arquivo que está sendo monitorado antes de realizar o `commit`, permitindo que você pule a parte do `git add`.

#### Removendo Arquivos

Para remover um arquivo do Git, você tem que removê-lo dos arquivos que estão sendo monitorados (mais precisamente, removê-lo da sua área de seleção) e então fazer o `commit`. O comando `git rm` faz isso e também remove o arquivo do seu diretório para você não ver ele como arquivo não monitorado (*untracked* file) na próxima vez.

Se você simplesmente remover o arquivo do seu diretório, ele aparecerá em “Changes not staged for commit” (isto é, fora da sua área de seleção ou *unstaged*) na saída do seu `git status`.

Outra coisa útil que você pode querer fazer é manter o arquivo no seu diretório, mas apagá-lo da sua área de seleção. Em outras palavras, você quer manter o arquivo no seu disco rígido mas não quer que o Git o monitore mais. Isso é particularmente útil se você esqueceu de adicionar alguma coisa no seu arquivo `.gitignore` e acidentalmente o adicionou, como um grande arquivo de log ou muitos arquivos `.a` compilados. Para fazer isso, use a opção `--cached`:

```bash
$ git rm --cached README.md
```

Você pode passar arquivos, diretórios, e padrões de nomes de arquivos para o comando `git rm`. Isso significa que você pode fazer coisas como:

```bash
$ git rm log/\*.log
```

Note a barra invertida (`\`) na frente do `*`. Isso é necessário pois o Git faz sua própria expansão no nome do arquivo além da sua expansão no nome do arquivo no shell. Esse comando remove todos os arquivos que tem a extensão `.log` no diretório `log/`.

```bash
$ git rm \*~
```

Esse comando remove todos os arquivos que terminam com `~`.

### Visualizando o Histórico de Commits

*Por padrão, sem argumentos, `git log` lista os commits feitos naquele repositório em ordem cronológica reversa. Isto é, os commits mais recentes primeiro. Este comando lista cada commit com seu checksum SHA-1, o nome e e-mail do autor, a data e a mensagem do commit.*

Uma das opções mais úteis é `-p`, que mostra o diff introduzido em cada commit. Você pode ainda usar `-2`, que limita a saída somente às duas últimas entradas.

```bash
$ git log -p -2
```

Se você quiser ver algumas estatísticas abreviadas para cada commit, você pode usar a opção `--stat`. A opção `--stat` imprime abaixo de cada commit uma lista de arquivos modificados, quantos arquivos foram modificados, e quantas linhas nestes arquivos foram adicionadas e removidas. Ele ainda mostra um resumo destas informações no final.

```bash
$ git log --stat
```

Outra opção realmente útil é `--pretty`. Esta opção muda a saída do log para outro formato que não o padrão. Algumas opções pré-construídas estão disponíveis para você usar. A opção `oneline` mostra cada commit em uma única linha, o que é útil se você está olhando muitos commits. Em adição, as opções `short`, `full` e `fuller` mostram a saída aproximadamente com o mesmo formato, mas com menos ou mais informações, respectivamente.

```bash
$ git log --pretty=oneline
```

A opção mais interessante é `format`, que permite que você especifique seu próprio formato de saída do log.

```bash
$ git log --pretty=format:"%h - %an, %ar : %s"
```

Lista algumas das opções mais importantes para formatação.
| **Opção** |  **Descrição da saída**                         |
|-----------|-------------------------------------------------|
| `%H`      | Hash do commit                                  |
| `%h`      | Hash do commit abreviado                        |
| `%T`      | Árvore hash                                     |
| `%t`      | Árvore hash abreviada                           |
| `%P`      | Hashes pais                                     |
| `%p`      | Hashes pais abreviados                          |
| `%an`     | Nome do autor                                   |
| `%ae`     | Email do autor                                  |
| `%ad`     | Data do autor (formato respeita a opção -date=) |
| `%ar`     | Data do autor, relativa                         |
| `%cn`     | Nome do committer                               |
| `%ce`     | Email do committer                              |
| `%cd`     | Data do committer                               |
| `%cr`     | Data do committer, relativa                     |
| `%s`      | Assunto                                         |

*O autor é a pessoa que originalmente escreveu o trabalho, enquanto o commiter é a pessoa que por último aplicou o trabalho.*

As opções `oneline` e `format` são particularmente úteis com outra opção chamada `--graph`. Esta opção gera um agradável gráfico ASCII mostrando seu branch e histórico de merges.

Lista as opções que nós cobrimos e algumas outras comuns que podem ser úteis, junto com a descrição de como elas mudam a saída do comando log.
| **Opção**         | **Descrição**                                                                                                                                  |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| `-p`              | Mostra o patch introduzido com cada commit.                                                                                                    |
| `--stat`          | Mostra estatísticas de arquivos modificados em cada commit.                                                                                     |
| `--shortstat`     | Mostra somente as linhas modificadas/inseridas/excluídas do comando --stat.                                                                     |
| `--name-only`     | Mostra a lista de arquivos modificados depois das informações do commit.                                                                        |
| `--name-status`   | Mostra a lista de arquivos afetados com informações sobre adição/modificação/exclusão dos mesmos.                                               |
| `--abbrev-commit` | Mostra somente os primeiros caracteres do checksum SHA-1 em vez de todos os 40.                                                                |
| `--relative-date` | Mostra a data em um formato relativo (por exemplo, “2 semanas atrás”) em vez de usar o formato de data completo.                               |
| `--graph`         | Mostra um gráfico ASCII do branch e histórico de merges ao lado da saída de log.                                                                |
| `--pretty`        | Mostra os commits em um formato alternativo. Opções incluem oneline, short, full, fuller, e format (onde você especifica seu próprio formato).  |

#### Limitando a Saída de Log

As opções de limites de tempo como `--since` e `--until` são muito úteis. Por exemplo, este comando pega a lista de commits feitos nas últimas duas semanas:

```bash
$ git log --since=2.weeks
```

Este comando funciona com vários formatos — você pode especificar uma data específica("2008-01-15") ou uma data relativa como "2 years 1 day 3 minutes ago".

Você pode ainda filtrar a lista de commits que casam com alguns critérios de busca. A opção `--author` permite que você filtre por algum autor específico, e a opção `--grep` deixa você buscar por palavras chave nas mensagens dos commits. (Note que se você quiser especificar ambas as opções author e grep simultaneamente, você deve adicionar `--all-match`, ou o comando considerará commits que casam com qualquer um.)

A última opção realmente útil para passar para `git log` como um filtro, é o caminho. Se você especificar um diretório ou um nome de arquivo, você pode limitar a saída a commits que modificaram aqueles arquivos. Essa é sempre a última opção, e geralmente é precedida por dois traços (`--`) para separar caminhos das opções.

Opções comuns para referência.
| **Opção**           | **Descrição**                                                                          |
|---------------------|----------------------------------------------------------------------------------------|
| `-(n)`              | Mostra somente os últimos n commits.                                                   |
| `--since, --after`  | Limita aos commits feitos depois da data especificada.                                  |
| `--until, --before` | Limita aos commits feitos antes da data especificada.                                   |
| `--author`          | Somente mostra commits que o autor casa com a string especificada.                      |
| `--committer`       | Somente mostra os commits em que a entrada do commiter bate com a string especificada.  |

### Trabalhando com Remotos

> Repositórios remotos são versões do seu projeto que estão hospedados na Internet ou em uma rede em algum lugar.

#### Exibindo Seus Remotos

> Para ver quais servidores remotos você configurou, você pode executar o comando `git remote`. Ele lista o nome de cada remoto que você especificou.

Se você tiver clonado seu repositório, você deve pelo menos ver um chamado `origin` — esse é o nome padrão que o Git dá ao servidor de onde você fez o clone.

```bash
$ git remote
```

Você também pode especificar `-v`, que mostra a URL que o Git armazenou para o nome do remoto:

```bash
$ git remote -v
```

#### Adicionando Repositórios Remotos

> Para adicionar um novo repositório remoto no Git com um nome curto, para que você possa fazer referência facilmente, execute `git remote add [nomecurto] [url]`.

```bash
$ git remote add xyz git://github.com/user/repo.git
$ git remote -v
```

#### Fazendo o Fetch e Pull de Seus Remotos

Para pegar dados dos seus projetos remotos, você pode executar:

```bash
$ git fetch [nome-remoto] [--all]
```

Esse comando vai até o projeto remoto e pega todos os dados que você ainda não tem. Depois de fazer isso, você deve ter referências para todos os branches desse remoto.

Se você clonar um repositório, o comando automaticamente adiciona o remoto com o nome origin. Então, `git fetch origin` busca qualquer novo trabalho que foi enviado para esse servidor desde que você o clonou (ou fez a última busca). É importante notar que o comando `fetch` traz os dados para o seu repositório local — ele não faz o merge automaticamente com o seus dados ou modifica o que você está trabalhando atualmente. Você terá que fazer o merge manualmente no seu trabalho quando estiver pronto.

Se você tem um branch configurado para acompanhar um branch remoto, você pode usar o comando `git pull` para automaticamente fazer o fetch e o merge de um branch remoto no seu branch atual. Essa pode ser uma maneira mais fácil ou confortável pra você; e por padrão, o comando `git clone` automaticamente configura seu branch local master para acompanhar o branch remoto master do servidor de onde você clonou (desde que o remoto tenha um branch master). Executar `git pull` geralmente busca os dados do servidor de onde você fez o clone originalmente e automaticamente tenta fazer o merge dele no código que você está trabalhando atualmente.

O `pull` nada mais é do que um `fetch` (baixa objetos e referências do remoto) mais `merge` (junta duas ou mais histórias).

#### Pushing Para Seus Remotos

Quando o seu projeto estiver pronto para ser compartilhado, você tem que enviá-lo para a fonte. O comando para isso é simples: `git push [nome-remoto] [branch]`. Se você quer enviar o seu branch master para o servidor origin (novamente, clonando normalmente define estes dois nomes para você automaticamente), então você pode rodar o comando abaixo para enviar o seu trabalho para o servidor:

```bash
$ git push origin master
```

Este comando funciona apenas se você clonou de um servidor que você têm permissão para escrita, e se mais ninguém enviou dados no meio tempo. Se você e mais alguém clonarem ao mesmo tempo, e você enviar suas modificações após a pessoa ter enviado as dela, o seu push será rejeitado. Antes, você terá que fazer um pull das modificações deste outro alguém, e incorporá-las às suas para que você tenha permissão para enviá-las.

#### Inspecionando um Remoto

Se você quer ver mais informação sobre algum remoto em particular, você pode usar o comando `git remote show [nome-remoto]`.

```bash
$ git remote show origin
```

*Ele lista a URL do repositório remoto assim como as branches sendo rastreadas.*

Este comando mostra qual branch é automaticamente enviado (pushed) quando você roda `git push` em determinados branches. Ele também mostra quais branches remotos que estão no servidor e você não tem, quais branches remotos você tem e que foram removidos do servidor, e múltiplos branches que são automaticamente mesclados (merged) quando você roda `git pull`.

#### Removendo e Renomeando Remotos

Se você quiser renomear uma referência, em versões novas do Git você pode rodar `git remote rename` para modificar um apelido de um remoto.

```bash
$ git remote rename foo bar
$ git remote
origin
bar
```

É válido mencionar que isso modifica também os nomes dos branches no servidor remoto. O que costumava ser referenciado como `foo/master` agora é `bar/master`.

Se você quiser remover uma referência por qualquer razão — você moveu o servidor ou não está mais usando um mirror específico, ou talvez um contribuidor não está mais contribuindo — você usa `git remote rm`:

```bash
$ git remote rm bar
$ git remote
origin
```

### Tagging

O Git suporta dois tipos diferentes de tags, tags annotated e lightweight. Tags lightweight e tags annotated diferem na quantidade de metadados acompanhantes que armazenam. Uma prática recomendada é considerar as tags Annotated como públicas e as tags Lightweight como particulares. As tags annotated armazenam metadados extras, como: o nome da tagger, o e-mail e a data. Estes são dados importantes para um lançamento público. Tags lightweight são essencialmente 'bookmarks' para um commit, são apenas um nome e um apontador para um commit, útil para criar links rápidos para commits relevantes.

*Uma tag é apenas um ponteiro para um determinado commit. Ao fazer o checkout para uma tag, na prática se está fazendo o checkout para as versões de arquivo do commit específico, o que significa que você não estará apontando seu diretório de trabalho para nenhum branch, algo conhecido como detached.*

#### Listando Suas Tags

Listar as tags disponíveis em Git é fácil. Apenas execute o comando `git tag`:

```bash
$ git tag
v0.1
v1.3
```

Este comando lista as tags em ordem alfabética; a ordem que elas aparecem não tem importância.

Você também pode procurar por tags com uma nomenclatura particular.

```bash
$ git tag -l 'v1.4.2.*'
v1.4.2.1
v1.4.2.2
v1.4.2.3
v1.4.2.4
```

#### Criando Tags

Git têm dois tipos principais de tags: leve e anotada. Um tag leve é muito similar a uma branch que não muda — é um ponteiro para um commit específico. Tags anotadas, entretanto, são armazenadas como objetos inteiros no banco de dados do Git. Eles possuem uma chave de verificação; o nome da pessoa que criou a tag, email e data; uma mensagem relativa à tag; e podem ser assinadas e verificadas com o GNU Privacy Guard (GPG). É geralmente recomendado que você crie tags anotadas para que você tenha toda essa informação; mas se você quiser uma tag temporária ou por algum motivo você não queira armazenar toda essa informação, tags leves também estão disponíveis.

##### Tags Anotadas

> As tags anotadas são armazenadas como objetos completos no banco de dados do Git.

> As práticas recomendadas sugeridas para a criação de tags do GIT são as tags anotadas em vez de leves, para que você possa ter todos os metadados associados.

Criando uma tag anotada em Git é simples. O jeito mais fácil é especificar `-a` quando você executar o comando `tag`:

```bash
$ git tag -a v1.4 -m 'My version 1.4'
$ git tag
v0.1
v1.3
v1.4
```

O parâmetro `-m` define uma mensagem, que é armazenada com a tag. Se você não especificar uma mensagem para uma tag anotada, o Git vai rodar seu editor de texto para você digitar alguma coisa.

Você pode ver os dados da tag junto com o commit que foi tagueado usando o comando `git show`:

```bash
$ git show v1.4
```

O comando mostra a informação da pessoa que criou a tag, a data de quando o commit foi tagueado, e a mensagem antes de mostrar a informação do commit.

##### Tags Leves

> Tags leves são criados com a ausência das opções `-a`, `-s` ou `-m`.

Outro jeito para taguear commits é com a tag leve. Esta é basicamente a chave de verificação armazenada num arquivo — nenhuma outra informação é armazenada. Para criar uma tag leve, não informe os parâmetros `-a`, `-s`, ou `-m`:

```bash
$ git tag v1.5
$ git tag
v0.1
v1.3
v1.4
v1.5
```

Desta vez, se você executar `git show` na tag, você não verá nenhuma informação extra. O comando apenas mostra o commit:

```bash
$ git show v1.5
```

#### Compartilhando Tags

Por padrão, o comando `git push` não transfere tags para os servidores remotos. Você deve enviar as tags explicitamente para um servidor compartilhado após tê-las criado. Este processo é igual ao compartilhamento de branches remotos – você executa `git push origin [nome-tag]`.

```bash
$ git push origin v1.5
```

Se você tem muitas tags que você deseja enviar ao mesmo tempo, você pode usar a opção `--tags` no comando `git push`. Ele irá transferir todas as suas tags que ainda não estão no servidor remoto.

```bash
$ git push origin --tags
```

#### Checking out Tags

Você pode criar um novo `branch` a partir de uma `tag` específica `git checkout -b [branchname] [tagname]`:

```bash
$ git checkout -b version2 v2.0.0
```

#### Tagging Old Commits

```bash
git tag -a v1.2 15027957951b64cf874c3557a0f3547bd83b3ff6
```

A execução do comando acima criará uma nova tag anotada que terá como sua referência o commit específicado.

#### ReTagging/Replacing Old Tags

Para atualizar uma tag existente apontando para um novo commit utilize a opção `[-f]` forçando a ação do comando.

```bash
git tag -a -f v1.4 15027957951b64cf874c3557a0f3547bd83b3ff6
```

## Ramificação (Branching)

> Criar um branch significa dizer que você vai divergir da linha principal de desenvolvimento e continuar a trabalhar sem bagunçar essa linha principal.

### Branch em poucas palavras

Um branch no Git é simplesmente um leve ponteiro móvel que aponta para um `commit`. Um branch em Git é na verdade um arquivo simples que contém os 40 caracteres do checksum SHA-1 do commit para o qual ele aponta, os branches são baratos para criar e destruir.

> Branches são ramificações do desenvolvimento do projeto, em suma, são compostas pelos commits realizados, sendo que o último commit da branch é para onde ela aponta.

> Um *branch* é apenas um ponteiro para um commit específico. Para indicar qual o branch atual de trabalho, o Git guarda um ponteiro especial conhecido como `HEAD`.

> Branches são apenas ponteiros para commits. Quando você cria um *branch*, tudo que o Git precisa fazer é criar um novo ponteiro - ele não cria um novo conjunto de arquivos ou pastas.

> `HEAD` no Git, este é um ponteiro para o branch local em que você está no momento. `HEAD` apontando para o branch em que você está.

Mostrar onde os ponteiros de ramificação estão apontando:

```bash
$ git log --oneline --decorate
```

Imprimirá o histórico de seus commits, mostrando onde estão seus ponteiros de ramificação e como seu histórico divergiu:

```bash
$ git log --oneline --decorate --graph --all
```

### Gerenciamento de Branches

O comando `git branch` faz mais do que criar e apagar branches. Se você executá-lo sem argumentos, você verá uma lista simples dos seus branches atuais.

Para ver o último commit em cada branch, você pode executar o comando `git branch -vv`.

Outra opção útil para saber em que estado estão seus branches é filtrar na lista somente branches que você já fez ou não o merge no branch que você está atualmente. As opções `--merged` e `--no-merged` estão disponíveis no Git desde a versão 1.5.6 para esse propósito. Para ver quais branches já foram mesclados no branch que você está, você pode executar `git branch --merged`.

Para ver todos os branches que contém trabalho que você ainda não fez o merge, você pode executar `git branch --no-merged`.

Para listar todos os branches remotos:
```bash
git branch -r
```

Lista apenas os branches já mergeados:
```bash
git branch --merged
```

Lista apenas os branches não mergeados:
```bash
git branch --no-merged
```

### Branches Remotos

> Branches remotos são referências ao estado de seus branches no seu repositório remoto. São branches locais que você não pode mover, eles se movem automaticamente sempre que você faz alguma comunicação via rede. Branches remotos agem como marcadores para lembrá-lo onde estavam seus branches no seu repositório remoto na última vez que você se conectou a eles. São referências de branches locais que você não pode modificá-los.

**Eles seguem o padrão `(remote)/(branch)`.**

Um comando clone do Git dá a você seu próprio branch *master* e *origin/master* faz referência ao branch *master* original.

Para sincronizar suas coisas, você executa o comando `git fetch origin`. Esse comando verifica qual servidor "origin" representa, obtém todos os dados que você ainda não tem e atualiza o seu banco de dados local, movendo o seu `origin/master` para a posição mais recente e atualizada.

`git fetch` busca todas as alterações no servidor que você ainda não possui.

![git fetch](https://git-scm.com/book/en/v2/images/remote-branches-3.png "git fetch")
**O comando `git fetch` atualiza suas referências remotas. Obtem tudo que o servidor remote tem e você ainda não tem.**

#### Enviando (Pushing)

> Quando você quer compartilhar um branch com o mundo, você precisa enviá-lo a um servidor remoto que você tem acesso. Seus branches locais não são automaticamente sincronizados com os remotos — você tem que explicitamente enviar (push) os branches que quer compartilhar. Desta maneira, você pode usar branches privados para o trabalho que não quer compartilhar, e enviar somente os branches tópicos em que quer colaborar.

Se você tem um branch e quer enviá-lo Execute o comando `git push (remote) (branch)`.

Você pode executar também `git push origin local-branch:remote-branch`, que faz a mesma coisa — é como, "pegue meu branch local e o transforme no remoto". Você pode usar esse formato para enviar (push) um branch local para o branch remoto que tem nome diferente. Se você não quer chamá-lo de xyz no remoto, você pode executar `git push origin foo:bar` para enviar seu branch local `foo` para o branch `bar` no projeto remoto.

```bash
$ git checkout -b baz foo/bar
```

Isso dá a você um branch local para trabalhar que começa onde `foo/bar` está.

#### Branches Rastreados (Tracking branches)

Fazer `checkout` de um branch local a partir de um branch remoto cria automaticamente o chamado **tracking branch** (branches rastreados) e o branch que ele rastreia é chamada de (**upstream branch**).

> Tracking branches são branches locais que tem uma relação direta com um branch remoto. Se você está em um tracking branch e digita `git push`, Git automaticamente sabe para que servidor e branch deve fazer o envio (push). Além disso, ao executar o comando `git pull` em um desses branches, é obtido todos os dados remotos e é automaticamente feito o merge do branch remoto correspondente.

Quando você faz o clone de um repositório, é automaticamente criado um branch `master` que segue `origin/master`. Esse é o motivo pelo qual `git push` e `git pull` funcionam sem argumentos. Entretanto, você pode criar outros tracking branches se quiser — outros que não seguem branches em `origin` e não seguem o branch `master`. Um caso simples é o exemplo que você acabou de ver, executando o comando `git checkout -b [branch] [nomeremoto]/[branch]`. Você pode usar também o atalho `--track`.

```bash
$ git checkout --track foo/bar
```

Na verdade, isso é tão comum que existe até um atalho para esse atalho. Se o nome do branch que você está tentando fazer checkout não existir e corresponder exatamente a um nome em apenas um controle remoto, o Git criará um branch de rastreamento para você:

```bash
$ git checkout bar
```

Para criar um branch local com um nome diferente do branch remoto, você pode facilmente usar a primeira versão com um nome diferente para o branch local:

```bash
$ git checkout -b foo origin/bar
```

Agora, seu branch local `foo` irá automaticamente enviar e obter dados de `origin/bar`.

#### Apagando Branches Remotos

> Você pode apagar um branch remoto usando a sintaxe `git push [nomeremoto]:[branch]` ou usando a opção `--delete`.

```bash
$ git push origin :bar
```

```bash
$ git push origin --delete bar
```

Basicamente tudo isso faz é remover o ponteiro do servidor. O servidor Git geralmente manterá os dados lá por um tempo até que uma coleta de lixo seja executada, por isso, se ele foi excluído acidentalmente, geralmente é fácil recuperá-lo.

### Rebasing

No Git, existem duas maneiras principais de integrar mudanças de um branch em outro: o `merge` e o `rebase`. Nessa seção você aprenderá o que é rebase, como fazê-lo, por que é uma ferramenta sensacional, e em quais casos você não deve usá-la.

#### O Rebase Básico

> Com o comando `rebase`, você pode pegar todas as mudanças que foram comitadas em um branch e replicá-las em outro.

Ele vai ao ancestral comum dos dois branches (no que você está e no qual será feito o rebase), pega a diferença (diff) de cada commit do branch que você está, salva elas em um arquivo temporário, restaura o brach atual para o mesmo commit do branch que está sendo feito o rebase e, finalmente, aplica uma mudança de cada vez.

O rebase monta um histórico mais limpo. Se você examinar um log de um branch com rebase, ele parece um histórico linear: como se todo o trabalho tivesse sido feito em série, mesmo que originalmente tenha sido feito em paralelo.

Rebasing permite que você reescreva o histórico de maneira automatizada, em vez de ter que relaxar e repetir as mudanças manualmente. É muito utilizado com `git rebase -i HEAD~2` (ou algum outro número pequeno) para corrigir alterações no seu histórico local antes de fazer merge ou push para um repositório central.

O snapshot apontado pelo o commit final, o último commit dos que vieram no rebase ou o último commit depois do merge, são o mesmo snapshot — somente o histórico é diferente. Fazer o rebase reproduz mudanças de uma linha de trabalho para outra na ordem em que foram feitas, já que o merge pega os pontos e os une.

Digamos que você decidiu obter o branch do seu servidor também. Você pode fazer o rebase do branch do servidor no seu branch master sem ter que fazer o checkout primeiro com o comando `git rebase [branchbase] [branchtopico]` — que faz o checkout do branch tópico pra você e aplica-o no branch base (master).

Então, se você quiser reescrever o histórico, `git-rebase` é o que você quer. Observe que você nunca deve reescrever o histórico que foi enviado e estava disponível para outra pessoa, já que a rebase reescreve os objetos tornando-os incompatíveis com os objetos antigos, resultando em uma confusão para qualquer outra pessoa envolvida.

## Ferramentas do Git

### Fazendo Stash

> Fazer Stash é tirar o estado sujo do seu diretório de trabalho — isto é, seus arquivos modificados que estão sendo rastreados e mudanças na área de seleção — e o salva em uma pilha de modificações inacabadas que você pode voltar a qualquer momento.

O comando `stash` é normalmente usado quando se deseja mudar de uma branch para outra, porém os arquivos que estão na branch atual ainda não estão em um estado adequado para serem comitados. O comando `stash` irá basicamente pegar as alterações no repositório atual e irá colocá-las em uma pilha.

Se nenhuma número for passado como parâmetro para o comando stash, o git irá assumir que o usuário deseja recuperar o último stash salvo na pilha. O `git stash apply` aplica a ultima alteração da pilha no topo da branch atual, mas mantém o histórico do stash.

*Caso queiramos criar uma branch, a partir do nosso último stash feito, basta utilizar o seguinte comando:*
```bash
git stash branch new_branch
```

### Git Cherry Pick

> Talvez o comando cherry-pick seja um dos mais precisos para manipulação de commits. Ele é muito usado quando necessita-se incorporar cirurgicamente um commit de uma outra branch no topo da branch corrente (apontado por HEAD).

**Por meio do cherry-pick podemos selecionar exatamente quais commits queremos recuperar de uma branch A e aplicar em nossa branch atual.**

*O cherry-pick sempre irá ocorrer entre duas branches diferentes, mas que pertencem ao mesmo repositório.*

É muito possível que em algum momento que estivermos utilizando o cherry-pick, conflitos terão de ser resolvidos. Para isso o cherry-pick possui sub-comandos iguais aos do rebase, que permitem arrumar esses conflitos antes de enviar o commit para o repositório local.

```bash
git cherry-pick --continue
git cherry-pick --abort
git cherry-pick --quit
```

### Git blame

Se você rastrear um bug em seu código e quiser saber quando ele foi introduzido e por quê, a anotação de arquivo geralmente é sua melhor ferramenta. Ele mostra a você qual commit foi o último a modificar cada linha de qualquer arquivo. Então, se você ver que um método no seu código é defeituoso, você pode anotar o arquivo com `git blame` para ver quando cada linha do método foi editada pela última vez e por quem. Este exemplo usa a opção `-L` para limitar a saída às linhas 12 a 22:

```bash
git blame -L 12,22 README.md
```

- `[-L]`: Restringirá a saída ao intervalo de linhas solicitado.
- `[-e]`: Mostra o endereço de e-mail dos autores em vez do nome de usuário.
- `[-w]`: Ignora as alterações de espaço em branco.
- `[-M]`: Detecta linhas movidas ou copiadas dentro do mesmo arquivo.
- `[-C]`: Detecta linhas que foram movidas ou copiadas de outros arquivos.


## Git Internamente

### Encanamento (Plumbing) e Porcelana (Porcelain)

> Isso deixa quatro entradas importantes: os arquivos `HEAD` e `index` e diretórios `objects` e `refs`. Estas são as peças centrais do Git. O diretório `objects` armazena todo o conteúdo do seu banco de dados, o diretório `refs` armazena os ponteiros para objetos de commit (branches), o arquivo `HEAD` aponta para o branch atual, e o arquivo `index` é onde Git armazena suas informações da área de preparação (staging area).

### O HEAD

O arquivo `HEAD` é uma referência simbólica para o branch em que você está no momento. Por referência simbólica, quer dizer que, ao contrário de uma referência normal, ele geralmente não contêm um valor SHA-1 mas sim um apontador para uma outra referência.

`HEAD` é o ponteiro para a referência de ramificação atual, que por sua vez é um ponteiro para o último commit feito nesse branch. Isso significa que o `HEAD` será o pai do próximo commit criado. Geralmente é mais simples pensar no `HEAD` como o snapshot de seu último commit.

### Tags

Você acabou de ver os três tipos de objetos principais do Git, mas há um quarto. O objeto tag é muito parecido com um objeto commit — contém um tagger (pessoa que cria a tag), uma data, uma mensagem e um ponteiro. A principal diferença é que um objeto tag aponta para um commit em vez de uma árvore. É como uma referência de branch, mas nunca se move — ele sempre aponta para o mesmo commit, mas te dá um nome mais amigável para ele.

Isso é tudo que uma tag leve é — um branch que nunca se move. Uma tag anotada é mais complexa. Se você criar uma tag anotada, Git cria um objeto tag e depois escreve uma referência para apontar para ela em vez de diretamente para um commit.

### Manutenção e Recuperação de Dados

Muitas vezes, a maneira mais rápida é usar uma ferramenta chamada git reflog. Quando você está trabalhando, Git silenciosamente registra onde está o HEAD cada vez que você mudá-lo. Cada vez que você fizer um commit ou alterar branches, o reflog é atualizado. O reflog também é atualizado pelo comando git update-ref, o que é mais um motivo para usá-lo em vez de apenas escrever o valor SHA em seus arquivos ref, como abordado anteriormente na seção "Referências Git" deste capítulo. Você pode ver onde você está, a qualquer momento, executando git reflog:

`git log -g`, que vai lhe dar uma saída de log normal do seu reflog.

Parece que o commit de baixo é o que você perdeu, então você pode recuperá-lo através da criação de um novo branch neste commit. Por exemplo, você pode criar um branch chamado recover-branch naquele commit (ab1afef):

```
$ git branch recover-branch ab1afef
```
