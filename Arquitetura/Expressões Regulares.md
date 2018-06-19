# Expressões Regulares

## Apresentando as Expressões Regulares

Bem resumido, uma expressão regular é um método formal de se especificar um padrão de texto.

Mais detalhadamente, é uma composição de símbolos, caracteres com funções especiais, que, agrupados entre si e com caracteres literais, formam uma sequência, uma expressão. Essa expressão é interpretada como uma regra que indicará sucesso se uma entrada de dados qualquer casar com essa regra, ou seja, obedecer exatamente a todas as suas condições. Esses símbolos são chamados de metacaracteres, pois possuem funções especiais, que veremos detalhadamente adiante.

## Os metacaracteres

Então, para já matar sua curiosidade, aqui estão os tão falados metacaracteres-padrão que serão nossos personagens dos próximos estudos:
`. ? * + ^ $ | [ ] { } ( ) \`

E aí, sentiu um frio na barriga? Cada simbolozinho desses tem sua função específica, que pode mudar dependendo do contexto no qual está inserido, e podemos agregá-los uns com os outros, combinando suas funções e fazendo construções mais complexas.

Então deixe eu te assustar mais um pouquinho. Além destes, temos outros metacaracteres estendidos que foram criados posteriormente, pois tarefas mais complexas requisitavam funções mais específicas ainda. E para terminar de complicar, sua sintaxe de utilização não é a mesma para todos os programas que suportam expressões regulares.

| **Metacaractere** | **Nome**       |
| ----------------- | -------------- |
| `.`               | *Ponto*        |
| `^`               | *Circunflexo*  |
| `[]`              | *Lista*        |
| `$`               | *Cifrão*       |
| `[^]`             | *Lista negada* |
| `\b`              | *Borda*        |
| `?`               | *Opcional*     |
| `\`               | *Escape*       |
| `*`               | *Asterisco*    |
| `|`               | *Ou*           |
| `+`               | *Mais*         |
| `()`              | *Grupo*        |
| `{}`              | *Chaves*       |
| `\1`              | *Retrovisor*   |

Agora que sabemos como chamar nossos amigos novos, veremos uma prévia, um apanhado geral de todos os metacaracteres e suas funções. Eles estão divididos em quatro grupos distintos, de acordo com características comuns entre eles.

### Representantes

| **Metacaractere** | **Nome**     | **Função**                     |
| ----------------- | ------------ | ------------------------------ |
| `.`               | Ponto        | Um caractere qualquer          |
| `[...]`           | Lista        | Lista de caracteres permitidos |
| `[^...]`          | Lista negada | Lista de caracteres proibidos  |

#### Metacaracteres tipo Representante

O primeiro grupo de metacaracteres que veremos são os do tipo representante, ou seja, são metacaracteres cuja função é representar um ou mais caracteres.

Também podem ser encarados como apelidos, links ou qualquer outra coisa que lhe lembre essa associação entre elementos. Todos os metacaracteres desse tipo casam a posição de um único caractere, e não mais que um.

#### Ponto: o necessitado `.`

O ponto é nosso curinga solitário, que está sempre à procura de um casamento, não importa com quem seja. Pode ser um número, uma letra, um Tab, um @, o que vier ele traça, pois o ponto casa qualquer coisa.

- O ponto casa com qualquer coisa.
- O ponto casa com o ponto.
- O ponto é um curinga para casar um caractere.

Como exemplos de uso do ponto, em um texto normal em português, você pode procurar palavras que você não se lembra se acentuou ou não, que podem começar com maiúsculas ou não ou que foram escritas errado:

| **Expressão** | **Casa com**                         |
| ------------- | ------------------------------------ |
| `n.o`         | não, nao, ...                        |
| `.eclado`     | teclado, Teclado, ...                |
| `e.tendido`   | estendido, extendido, eztendido, ... |

#### Lista: a exigente `[...]`

Bem mais exigente que o ponto, a lista não casa com qualquer um. Ela sabe exatamente o que quer, e não aceita nada diferente daquilo, a lista casa com quem ela conhece.

Ela guarda dentro de si os caracteres permitidos para casar, então algo como `[aeiou]` limita nosso casamento a letras vogais.

No exemplo anterior do ponto, sobre acentuação, tínhamos a ER `n.o`. Além dos casamentos desejados, ela é muito abrangente, e também casa coisas indesejáveis como `neo`, `n-o`, `n5o` e `n o`.

Para que nossa ER fique mais específica, trocamos o ponto pela lista, para casar apenas “não” e “nao”, como da seguinte forma: `n[ãa]`. E, assim como o `n.o`, todos os outros exemplos anteriores do ponto casam muito mais que o desejado, justo pela sua natureza promíscua.

Exatamente, eles indicam que havia mais possibilidades de casamento. Como o ponto casa com qualquer coisa, ele é nada específico. Então vamos impor limites às ERs:

| **Expressão**  | **Casa com**         |
| -------------- | -------------------- |
| `n[ãa]o`       | não, nao             |
| `[Tt]eclado`   | Teclado, teclado     |
| `e[sx]tendido` | estendido, extendido |
| `12[:. ]45`    | 12:45, 12.45, 12 45  |
| `<[BIP]>`      | `<B>, <I>, <P>`      |

Pegadinha! Não. Registre em algum canto de seu cérebro: dentro da lista, todo mundo é normal. Repetindo: dentro da lista, todo mundo é normal. Então aquele ponto é simplesmente um ponto normal, e não um metacaractere.

##### Intervalo de lista

Por enquanto, vimos que a lista abriga todos os caracteres permitidos em uma posição. Como seria uma lista que dissesse que em uma determinada posição poderia haver apenas números?

*Lembra que eu disse para você memorizar que dentro da lista, todo mundo é normal? Pois é, aqui temos a primeira exceção à regra. Todo mundo, fora o traço. Se tivermos um traço (`-`) entre dois caracteres, isso representa todo o intervalo entre eles.*

Então isso:

`[012345678]`

É melhor assim:

`[0-9]`

Simples assim. Aquele tracinho indica um intervalo entre os números de zero a nove.

Voltando a nossa ER da hora, poderíamos fazer:

`[0-9][0-9]:[0-9][0-9]`

*Qualquer intervalo é válido, como `a-z`, `A-Z`, `5-9`, `a-f`, `:-@` etc.*

Sim. Por exemplo, se eu quiser uma lista que case apenas letras maiúsculas, minúsculas e números: `[A-Za-z0-9]`.

*Não se pode ter uma lista dentro de outra.*

Como o traço é especial dentro da lista, como fazer quando você quiser colocar na lista um traço literal?

##### Dominando caracteres acentuados (POSIX)

Como para nós brasileiros se `a-z` não casar letras acentuadas não serve para muita coisa, temos uns curingas para usar dentro de listas que são uma mão na roda. Duas até.

Eles são chamados de classes de caracteres *POSIX*. São grupos definidos por tipo, e *POSIX* é um padrão internacional que define esse tipo de regra, como será sua sintaxe etc. Falando em sintaxe, aqui estão as classes:

| **Classe POSIX** | **Similar**      | **Significa**          |
| ---------------- | ---------------- | ---------------------- |
| `[:upper:]`      | `[A-Z]`          | Letras maiúsculas      |
| `[:lower:]`      | `[a-z]`          | Letras minúsculas      |
| `[:alpha:]`      | `[A-Za-z]`       | Maiúsculas/minúsculas  |
| `[:alnum:]`      | `[A-Za-z0-9]`    | Letras e números       |
| `[:digit:]`      | `[0-9]`          | Números                |
| `[:xdigit:]`     | `[0-9A-Fa-f]`    | Números hexadecimais   |
| `[:punct:]`      | `[.,!?:...]`     | Sinais de pontuação    |
| `[:blank:]`      | `[ \t]`          | Espaço e Tab           |
| `[:space:]`      | `[ \t\n\r\f\v]`  | Caracteres brancos     |
| `[:cntrl:]`      |                  | Caracteres de controle |
| `[:graph:]`      | `[^ \t\n\r\f\v]` | Caracteres imprimíveis |
| `[:print:]`      | `[^\t\n\r\f\v]`  | Imprimíveis e o espaço |


Note que os colchetes fazem parte da classe e não são os mesmos colchetes da lista. Para dizer maiúsculas, fica `[[:upper:]]`, ou seja, um `[:upper:]` dentro de uma lista `[]`.

Então, em uma primeira olhada, `[:upper:]` é o mesmo que `A-Z`, letras maiúsculas. Mas a diferença é que essas classes POSIX levam em conta a localidade do sistema.

Atenção para essa diferença, pois a literatura na língua inglesa sempre fala sobre esse assunto muito superficialmente, pois eles não utilizam acentuação e deve ser às vezes até difícil para quem está escrevendo o documento entender isso.

Como nossa situação é inversa, e nossa língua é rica em caracteres acentuados, entender essa diferença é de suma importância. Como estamos no Brasil, geralmente nossas máquinas estão configuradas para usar os números no formato *nnn.nnn,nn*, a data é no formato *dd/mm/aaaa*, medidas de distância são em centímetros e há outras coisinhas que são diferentes nos demais países.

Entre outros, também está definido que *áéíóú* são caracteres válidos em nosso alfabeto, bem como *ÁÉÍÓÚ*.

Então, toda essa volta foi para dizer que o `[:upper:]` leva isso em conta e inclui as letras acentuadas também na lista. O mesmo para o `[:lower:]`, o `[:alpha:]` e o `[:alnum:]`.

*Nos Estados Unidos `[[:upper:]]` é igual a `[A-Z]`. No Brasil, `[[:upper:]]` é igual a `[A-ZÁÃÂÀÉÊÍÓÕÔÚÇ...]`.*

Por isso, para nós, essas classes *POSIX* são importantíssimas, e sempre que você tiver de fazer ERs que procurarão em textos em português, prefira `[:alpha:]` em vez de `A-Za-z`, sempre.

Então, refazendo a ER que casava maiúsculas, minúsculas e números, temos:

`[[:upper:][:lower:][:digit:]]`

ou melhor:

`[[:alpha:][:digit:]]`

ou melhor ainda:

`[[:alnum:]]`

Todas são equivalentes.

-------------

**Resumo**

- A lista casa com quem ela conhece e tem suas próprias regras.
- Dentro da lista, todo mundo é normal.
- Dentro da lista, traço indica intervalo.
- Um `-` literal deve ser o último item da lista.
- Um `]` literal deve ser o primeiro item da lista.
- Os intervalos respeitam a tabela ASCII (não use `A-z`).
- `[:classes POSIX:]` incluem acentuação, `A-Z` não.

#### Lista negada: a exigente `[^...]`

- Uma lista negada segue todas as regras de uma lista normal.
- Um `^` literal não deve ser o primeiro item da lista.
- [:classes POSIX:] podem ser negadas.
- A lista negada sempre deve casar algo.

Nem tão exigente quanto a lista nem tão necessitada quanto o ponto, temos a lista negada, que pelas suas más experiências passadas, sabe o que não serve para ela casar.

A lista negada é exatamente igual à lista, podendo ter caracteres literais, intervalos e classes POSIX. Tudo o que se aplica a lista normal se aplica à negada também.

A única diferença é que ela possui lógica inversa, ou seja, ela casará com qualquer coisa, exceto com os caracteres listados.

Observe que a diferença em sua notação é que o primeiro caractere da lista é um circunflexo, ele indica que essa é uma lista negada. Então, se `[0-9]` são números, `[^0-9]` é qualquer coisa fora números. Pode ser letras, símbolos, espaço em branco, qualquer coisa, menos números.

Explicando em outras palavras, se você diz “qualquer coisa fora números”, deve haver outra coisa no lugar dos números e não simplesmente “se não houver números”. Então essa ER não casaria uma linha vazia, por exemplo.

*“qualquer coisa fora alguns caracteres” não inclui “nenhum caractere”.*

Como o traço e o colchete que fecha, o circunflexo é especial, então, para colocarmos um ^ literal em uma lista, precisamos pô-lo em qualquer posição que não seja a primeira. Assim `[A-Z^]` casa maiúsculas e o circunflexo, e `[^A-Z^]` é o inverso: qualquer coisa fora maiúsculas e o circunflexo.

As classes POSIX também podem ser negadas, então `[^[:digit:]]` caso com "qualquer coisa fora, diferente de números".

A lista negada é muito útil quando você sabe exatamente o que não pode ter em uma posição, como um erro ortográfico ou de escrita. Por exemplo, como mandam as regras da boa escrita, sempre após caracteres de pontuação, como a vírgula ou o ponto, devemos ter um espaço em branco os separando do resto do texto. Então vamos procurar por qualquer coisa que não seja o espaço após a pontuação:

`[:;,.!?][^ ]`

Ou, ainda, explicitando melhor nosso objetivo:

`[[:punct:]][^ ]`

### Quantificadores

| **Metacaractere** | **Nome**  | **Função**       |
| ----------------- | --------- | ---------------- |
| `?`               | Opcional  | Zero ou um       |
| `*`               | Asterisco | Zero, um ou mais |
| `+`               | Mais      | Um ou mais       |
| `{n, m}`          | Chaves    | De n até m       |

#### Metacaracteres tipo Quantificador

Aqui chegamos ao segundo tipo de metacaracteres, os quantificadores, que servem para indicar o número de repetições permitidas para a entidade imediatamente anterior. Essa entidade pode ser um caractere ou metacaractere.

Em outras palavras, eles dizem a quantidade de repetições que o átomo anterior pode ter, ou seja, quantas vezes ele pode aparecer.

Os quantificadores não são quantificáveis, então dois deles seguidos em uma ER é um erro, salvo quantificadores não-gulosos, que veremos depois.

E memorize, por enquanto sem entender o porquê: todos os quantificadores são gulosos.

#### Opcional: o opcional `?`

O opcional é um quantificador que não esquenta a cabeça, para ele pode ter ou não a ocorrência da entidade anterior, pois ele a repete 0 ou 1 vez. Por exemplo, a ER `7?` significa zero ou uma ocorrência do número 7. Se tiver um 7, beleza, casamento efetuado. Se não tiver, beleza também. Isso torna o 7 opcional (daí o nome), ou seja, tendo ou não, a ER casa. Veja mais um exemplo, o plural. A ER `ondas?` tem a letra s marcada como opcional, então ela casa onda e ondas.

*Cada letra normal é um átomo da ER, então o opcional é aplicado somente ao `s` e não à palavra toda.*

Claro! Agora vamos começar a ver o poder de uma expressão regular. Já vimos o metacaractere lista e agora vimos o opcional, então, que tal fazermos uma lista opcional? Voltando um pouco àquele exemplo da palavra fala, vamos fazer a ER `fala[r!]?.` Mmmmmm... As ERs estão começando a ficar interessantes, não? Mas, antes de analisar essa ER, uma dica que vale ouro, memorize já: *leia a ER átomo por átomo, da esquerda para a direita. Repetindo: leia a ER átomo por átomo, da esquerda para a direita.*

##### Como ler um ER

É bem simples, uma ER se lê exatamente como o robozinho (lembra quem ele é?) leria. Primeiro lê-se átomo por átomo, depois entende-se o todo e então se analisa as possibilidades. Na nossa ER `fala[r!]?` em questão, sua leitura fica: um f seguido de um a, seguido de um l, seguido de um a, seguido de: ou r, ou !, ambos opcionais.

Essa leitura é essencial para o entendimento da ER. Ela pode em um primeiro momento ser feita em voz alta, de maneira repetitiva, até esse processo se tornar natural. Depois ela pode ser feita mentalmente mesmo, e de maneira automática. É como você está fazendo agora, repetindo mentalmente estas palavras escritas aqui enquanto as lê. Você não percebe, faz normalmente.

Feita a leitura, agora temos de entender o todo, ou seja, temos um trecho literal `fala`, seguido de uma lista opcional de caracteres. Para descobrirmos as possibilidades, é o fala seguido de cada um dos itens da lista e por fim seguido por nenhum deles, pois a lista é opcional. Então fica:

| **Expressão** | **Casa com**     |
| ------------- | ---------------- |
| `fala[r!]?`   | falar,fala!,fala |

Pronto! Desvendamos os segredos da ER. É claro, esta é pequena e fácil, mas o que são ER grandes senão várias partes pequenas agrupadas? O principal é dominar essa leitura por átomos. O resto é ler devagar até chegar ao final. Não há mistério.

Então voltemos ao nosso exemplo de marcações HTML, podemos facilmente incluir agora as marcações que fecham o trecho, em que a única diferença é que vem uma barra `/` antes da letra:

| **Expressão**  | **Casa com**                                                       |
| -------------- | ------------------------------------------------------------------ |
| `</?[BIPbip]>` | `</B>, </I>, </P>, </b>, </i>, </p>, <B>, <I>, <P>, <b>, <i>, <p>` |

- O opcional é opcional.
- O opcional é útil para procurar palavras no singular e plural.
- Podemos tornar opcionais caracteres e metacaracteres.
- Leia a ER átomo por átomo, da esquerda para a direita.
- Leia a ER, entenda o todo e analise as possibilidades.

#### Asterisco: o tanto-faz `*`

- O asterisco repete em qualquer quantidade.
- Quantificadores são gulosos.
- O curinga .* é o tudo e o nada, qualquer coisa.

Se o opcional já não esquenta a cabeça, podendo ter ou não a entidade anterior, o asterisco é mais tranquilo ainda, pois para ele pode ter, não ter ou ter vários, infinitos. Em outras palavras, a entidade anterior pode aparecer em qualquer quantidade.

| **Expressão** | **Casa com**                                   |
| ------------- | ---------------------------------------------- |
| `7*0`         | 0, 70, 770, 7770, ..., 777777777770, ...       |
| `bi*p`        | bp, bip, biip, biiip, biiiip...                |
| `b[ip]*`      | b, bi, bip, biipp, bpipipi, biiiiip, bppp, ... |

##### Apresentando a gulodice

Pergunta: o que casará `[ar]*a` na palavra arara? Alternativas:

1) `a[ar]` zero vezes, seguido de a
2) `ara[ar]` duas vezes (a, r), seguido de a
3) `arara[ar]` quatro vezes (a, r, a, r), seguido de a
4) n.d.a.

Acertou se você escolheu a número 3. O asterisco repete em qualquer quantidade, mas ele sempre tentará repetir o máximo que conseguir. As três alternativas são válidas, mas entre casar a lista `[ar]` zero, duas ou quatro vezes, ele escolherá o maior número possível. *Por isso se diz que o asterisco é guloso*.

Essa gulodice às vezes é boa, às vezes é ruim. Os próximos quantificadores, mais e chaves, bem como o opcional já visto, são igualmente gulosos. Mais detalhes sobre o assunto, confira mais adiante.

##### Apresentando o coringa `.*`

Vimos até agora que temos dois metacaracteres extremamente abrangentes, como o ponto (qualquer caractere) e o asterisco (em qualquer quantidade). E se juntarmos os dois? Teremos qualquer caractere, em qualquer quantidade. Pare um instante para pensar nisso. O que isso significa? Tudo? Nada? A resposta é: ambos.

O nada, pois “qualquer quantidade” também é igual a “nenhuma quantidade”. Então é opcional termos qualquer caractere, não importa. Assim, uma ER que seja simplesmente `.*` sempre será válida e casará mesmo uma linha vazia.

O tudo, pois “qualquer quantidade” também é igual a “tudo o que tiver”. E é exatamente isso o que o asterisco faz, ele é guloso, ganancioso, e sempre tentará casar o máximo que conseguir. Repita comigo: o MÁXIMO que conseguir.

*O curinga `.*` é qualquer coisa!*

#### Mais: o tem-que-ter `+`

- O mais repete em qualquer quantidade, pelo menos uma vez.
- O mais é igual ao asterisco, só mais exigente.

O mais tem funcionamento idêntico ao do asterisco, tudo o que vale para um se aplica ao outro.

A única diferença é que o mais não é opcional, então a entidade anterior deve casar pelo menos uma vez, e pode haver várias.

Sua utilidade é quando queremos no mínimo uma repetição. Não há muito que acrescentar, é um asterisco mais exigente...

| **Expressão** | **Casa com**                                |
| ------------- | ------------------------------------------- |
| `7+0`         | 70, 770, 7770, ..., 777777777770, ...       |
| `bi+p`        | bip, biip, biiip, biiiip...                 |
| `b[ip]+`      | bi, bip, biipp, bpipipi, biiiiip, bppp, ... |

#### Chaves: o controle `{n, m}`

- Chaves são precisas.
- Você pode especificar um número exato, um mínimo, um máximo, ou uma faixa numérica.
- As chaves simulam os seguintes metacaracteres: `*` `+` `?`.
- As chaves não são o Chaves.

As chaves são a solução para uma quantificação mais controlada, onde se pode especificar exatamente quantas repetições se quer da entidade anterior. Basicamente, `{n, m}` significa de `n` até `m` vezes, assim algo como `7{1,4}` casa 7, 77, 777 e 7777. Só, nada mais que isso.

Temos também a sintaxe relaxada das chaves, em que podemos omitir a quantidade final ou ainda, especificar exatamente um número:

| **Metacaractere** | **Repetições**                    |
| ----------------- | --------------------------------- |
| `{1,3}`           | De 1 a 3                          |
| `{3,}`            | Pelo menos 3 (3 ou mais)          |
| `{0,3}`           | Até 3                             |
| `{3}`             | Exatamente 3                      |
| `{1}`             | Exatamente 1                      |
| `{0,1}`           | Zero ou 1 (igual ao opcional)     |
| `{0,}`            | Zero ou mais (igual ao asterisco) |
| `{1,}`            | Um ou mais (igual ao mais)        |

*Com as chaves, conseguimos simular o funcionamento de outros três metacaracteres, o opcional, o asterisco e o mais.*

Se temos as chaves que já fazem o serviço, então pra que ter os outros três? Você pode escolher a resposta que achar melhor. Eu tenho algumas:

- `*` é menor e mais fácil que `{0,}`.
- As chaves foram criadas só depois dos outros.
- Precisavam de mais metacaracteres para complicar o assunto.
- `*`, `+` e `?` são links para as chaves.
- Alguns teclados antigos vinham sem a tecla `{`
- O asterisco é tão bonitinho...

### Âncoras

| **Metacaractere** | **Nome**    | **Função**               |
| ----------------- | ----------- | ------------------------ |
| `^`               | Circunflexo | Início da linha          |
| `$`               | Cifrão      | Fim da linha             |
| `\b`              | Borda       | Início ou fim de palavra |

#### Metacaracteres tipo Âncora

Por que âncora? Porque eles não casam caracteres ou definem quantidades, em vez disso, eles marcam uma posição específica na linha. Assim, eles não podem ser quantificados, então o mais, o asterisco e as chaves não têm influência sobre âncoras.

#### Circunflexo: o ínicio `^`

- Circunflexo é um nome chato, porém chapeuzinho é legal.
- Serve para procurar palavras no começo da linha.
- Só é especial no começo da ER (e de uma lista).

> Marca o começo de uma linha. Nada mais.

`^[0-9]` Isso quer dizer: a partir do começo da linha, case um número, ou seja, procuramos linhas que começam com números. O contrário seria: `^[^0-9]`.

Ou seja, procuramos linhas que não começam com números. O primeiro circunflexo é a âncora e o segundo é o “negador” da lista. E como não poderia deixar de ser, é claro que o circunflexo como marcador de começo de linha só é especial se estiver no começo da ER. Não faz sentido procurarmos uma palavra seguida de um começo de linha, pois se tiver uma palavra antes do começo de uma linha, ali não é o começo da linha! Desse modo, a ER: `[0-9]^` Casa um número seguido de um circunflexo literal, em qualquer posição da linha.

#### Cifrão: o fim `$`

- Serve para procurar palavras no fim da linha.
- Só é especial no final da ER.
- É cifrão, e não dólar.

> Similar e complementar ao circunflexo, o cifrão marca o fim de uma linha e só é válido no final de uma ER. Assim, `[0-9]$` casa com linhas que terminam com um número.

#### Borda: a limítrofe `\b`

- A borda marca os limites de uma palavra.
- O conceito “palavra” engloba letras, números e o sublinhado.
- A borda é útil para casar palavras exatas e não parciais.

> A outra âncora que temos é a borda, que como o próprio nome já diz, marca uma borda, mais especificamente, uma borda de palavra.

Ela marca os limites de uma palavra, ou seja, onde ela começa e/ou termina. Muito útil para casar palavras exatas, e não partes de palavras. Veja como se comportam as ERs nas palavras dia, diafragma, melodia, radial e bom-dia!:

| **Expressão** | **Casa com**                              |
| ------------- | ----------------------------------------- |
| `dia`         | dia, diafragma, melodia, radial, bom-dia! |
| `\bdia`       | dia, diafragma, bom-dia!                  |
| `dia\b`       | dia, melodia, bom-dia!                    |
| `\bdia\b`,    | dia, bom-dia!                             |

Assim vimos que a borda força um começo *ou* a terminação de uma palavra. Entenda que “palavra” aqui é um conceito que engloba `[A-Za-z0-9_]` apenas, ou seja, letras, números e o sublinhado. Por isso `\bdia\b` também casa bom-dia!, pois o traço e a exclamação não são parte de uma palavra.

### Outros

| **Metacaractere** | **Nome**   | **Função**                    |
| ----------------- | ---------- | ----------------------------- |
| `\c`              | Escape     | Torna literal o caractere `c` |
| `|`               | Ou         | Ou um ou outro                |
| `(...)`           | Grupo      | Delimita um grupo             |
| `\1...\9`         | Retrovisor | Texto casado nos grupos 1...9 |

#### Escape: a criptonita `\`

- O escape escapa um metacaractere, tirando seu poder.
- `\*` = `[*]` = asterisco literal
- O escape escapa o escape, escapando-se a si próprio simultaneamente.

Escapando, `\*` é igual a `[*]` que é igual a um asterisco literal. Similarmente podemos escapar todos os metacaracteres já vistos: `\. \[ \] \? \+ \{ \} \^ \$`

E para você ver como são as coisas, o escape é tão poderoso que pode escapar a si próprio! O `\\` casa uma barra invertida `\` literal.

#### Ou: o alternativo `|`

- O ou indica alternativas.
- Lista para um caractere, ou para vários.
- O grupo multiplica o poder do ou.

É muito comum em uma posição específica de nossa ER termos mais de uma alternativa possível, por exemplo, ao casar um cumprimento amistoso, podemos ter uma terminação diferente para cada parte do dia:

`boa-tarde|boa-noite`

O *ou*, representado pela barra vertical `|`, serve para esses casos em que precisamos dessas alternativas. Essa ER se lê: “ou boa-tarde, ou boa-noite”, ou seja, “ou isso ou aquilo”. *Lembre-se de que a lista também é uma espécie de ou, mas apenas para uma letra*, então:

`[gpr]ato` e `gato|pato|rato`

Em outro exemplo, o *ou* é útil também para casarmos um endereço de Internet, que pode ser uma página ou um servidor FTP. `http://|ftp://` Ou isso ou aquilo, ou aquele outro... E assim vai. Podem-se ter tantas opções quantas for preciso. Não deixe de conhecer o parente de 1º grau do ou, o grupo, que multiplica seu poder. A seguir, neste mesmo canal.

#### Grupo: o pop `(...)`

- Grupos servem para agrupar.
- Grupos são muito poderosos.
- Grupos podem conter grupos.

Assim como artistas famosos e personalidades que conseguem arrastar multidões, o grupo tem o dom de juntar vários tipos de sujeitos em um mesmo local. Dentro de um grupo, podemos ter um ou mais caracteres, metacaracteres e inclusive outros grupos! Como em uma expressão matemática, os parênteses definem um grupo, e seu conteúdo pode ser visto como um bloco na expressão.

Todos os metacaracteres quantificadores que vimos anteriormente podem ter seu poder ampliado pelo grupo, pois ele lhes dá mais abrangência. E o ou, pelo contrário, tem sua abrangência limitada pelo grupo: pode parecer estranho, mas é essa limitação que lhe dá mais poder.

| **Expressão**     | **Casa com**                |
| ----------------- | --------------------------- |
| `(ha!)+`          | ha!, ha!ha!, ha!ha!ha!, ... |
| `(\.[0-9]){3}`    | .0.6.2, .2.8.9, .7.7.7, ... |
| `(www\.)?zz\.com` | www.zz.com, zz.com          |

E em especial, nosso amigo `"ou"` ganha limites e seu poder cresce:

| **Expressão**       | **Casa com**             |
| ------------------- | ------------------------ |
| `boa-(tarde|noite)` | boa-tarde, boa-noite     |
| `(#|n\.|núm) 7`     | # 7, n. 7, núm 7         |
| `(in|con)?certo`    | incerto, concerto, certo |

Note que o grupo não altera o sentido da ER, apenas serve como marcador. Podemos criar subgrupos também, então imagine que você esteja procurando o nome de um supermercado em uma listagem e não sabe se este é um mercado, supermercado ou um hipermercado.

`(super|hiper)mercado`

Consegue casar as duas últimas possibilidades, mas note que nas alternativas super e hiper temos um trecho `per` comum aos dois, então podíamos “alternativizar” apenas as diferenças su e hi:

`(su|hi)permercado`

Precisamos também casar apenas o mercado sem os aumentativos, então temos de agrupá-los e torná-los opcionais:

`((su|hi)per)?mercado`

Pronto! Temos a ER que buscávamos e ainda esbanjamos habilidade utilizando um grupo dentro do outro.

#### Retrovisor: o saudosista `\1...\9`

Ao usar um (grupo) qualquer, você ganha um brinde, e muitas vezes nem sabe. O brinde é o trecho de texto casado pela ER que está no grupo, que fica guardado em um cantinho especial, e pode ser usado em outras partes da mesma ER!

Então vamos tentar de novo. Como o nome diz, é retrovisor porque ele “olha pra trás”, para buscar um trecho já casado. Isso é muito útil para casar trechos repetidos em uma mesma linha. Veja bem, é o trecho de texto, e não a ER.

Como exemplo, em um texto sobre passarinhos, procuramos o quero-quero. Podemos procurar literalmente por quero-quero, mas assim não tem graça, pois somos mestres em ERs e vamos usar o grupo e o retrovisor para fazer isso:

`(quero)-\1`

Então o retrovisor `\1` é uma referência ao texto casado do primeiro grupo, nesse caso, quero, ficando, no fim das contas, a expressão que queríamos. O retrovisor pode ser lembrado também como um link ou um ladrão, pois copia o texto do grupo.

lembra que o escape `\` servia para tirar os poderes do metacaractere seguinte. Então, a essa definição agora incluímos: a não ser que este próximo caractere seja um número de 1 a 9, então estamos lidando com um retrovisor.

Podemos ter no máximo nove retrovisores por ER, então `\10` é o retrovisor número 1 seguido de um zero. Alguns aplicativos novos permitem mais de nove.

O verdadeiro poder do retrovisor é quando não sabemos exatamente qual texto o grupo casará. Vamos estender nosso quero para “qualquer palavra”:

`([A-Za-z] +)-\1`

Percebeu o poder dessa ER? Ela casa palavras repetidas, separadas por um traço, como o próprio quero-quero, e mais: bate-bate, come-come etc. Mas e se tornássemos o traço opcional?

`([A-Za-z] +)-?\1`

Agora, além das anteriores, nossa ER também casa bombom, lili, dudu, bibi e outros apelidos e nomes de cachorro.

Com uma modificação pequena, fazemos um minicorretor ortográfico para procurar por palavras repetidas como como estas em um texto:

`([A-Za-z]+) \1`

Mas como procuramos por palavras inteiras e não apenas trechos delas, então precisamos usar as bordas para completar nossa ER:

`\b([A-Za-z]+) \1\b`

Legal, né? Note como vamos construindo as ERs aos poucos, melhorando, testando e não simplesmente escrevendo tudo de uma vez. Esta é a arte ninja de se escrever ERs.

**Mais detalhes**

Como já dito, podemos usar no máximo nove retrovisores. Vamos ver uns exemplos com mais de um de nossos amigos novos:

| **Expressão**                  | **Casa com**               |
| ------------------------------ | -------------------------- |
| `(lenta) (mente) é \2\1`       | lentamente é mente lenta   |
| `((band)eira)nte \1 \2a`       | bandeirante bandeira banda |
| `in(d)ol(or) é sem \1\2`       | indolor é sem dor          |
| `((((a)b)c)d)-1 = \1,\2,\3,\4` | abcd-1 = abcd, abc, ab, a  |

Para não se perder nas contagens, há uma dica valiosa: conte somente os parênteses que abrem, da esquerda para a direita. Este vai ser o número do retrovisor. E o conteúdo é o texto casado pela ER do parêntese que abre até seu correspondente que fecha.

*O retrovisor referencia o texto casado e não a ER do grupo.*

Nos nossos exemplos ocorre a mesma coisa porque a ER dentro do grupo já é o próprio texto, sem metacaracteres. Veja, entretanto, que `([0-9])\1` casa 66 mas não 69.

Apenas como lembrete, algumas linguagens e programas, além da função de busca, têm a função de substituição. O retrovisor é muito útil nesse caso, para substituir “alguma coisa” por “apenas uma parte dessa coisa”, ou seja, extrair trechos de uma linha. Mais detalhes sobre isso adiante.

- O retrovisor só funciona se usado com o grupo.
- O retrovisor serve para procurar palavras repetidas.
- Numeram-se retrovisores contando os grupos da esquerda para a direita.
- Temos no máximo 9 retrovisores por ER.
