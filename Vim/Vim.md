# Estudos VIM

## Editando

###  Escrevendo texto

`i` ..... Entra no modo de inserção antes do caractere atual
`I` ..... Entra no modo de inserção no começo da linha
`a` ..... Entra no modo de inserção após o caractere atual
`A` ..... Entra no modo de inserção no final da linha
`o` ..... Entra no modo de inserção uma linha abaixo
`O` ..... Entra em modo de inserção uma linha cima
`<Esc>` . Sai do modo de inserção

###  Copiar, Colar e Deletar

> No modo normal, o ato de **DELETAR** ou eliminar o texto está associado à letra `d`. O comando `d` remove o conteúdo para a memória.

`dd` .... Deleta linha atual
`D` ..... Deleta restante da linha
`d$` .... Deleta do ponto atual até o final da linha (sinônimo: `D`)
`d^` .... Deleta do cursor ao primeiro caractere não nulo da linha
`d0` .... Deleta do cursor até o início da linha
`ddp` ... Troca linhas de lugar
`d5G` ... Apaga até a linha 5
`dw` .... Apaga uma palavra
`5dw` ... Apaga 5 palavras (também pode ser: `d5w`)
`5dl` ... Apaga 5 letras (também pode ser: `d5l` ou `5x`)
`dgg` ... Apaga até o início do arquivo
`dG` .... Apaga até o final do arquivo
`D` ..... Apaga o resto da linha

> No modo normal, o ato de **COPIAR** o texto está associado à letra `y`. Permite copiar uma parte do texto para a memória sem deletar.

`yy` .... Copia a linha atual
`Y` ..... Copia a linha atual
`ye` .... Copia do cursor ao fim da palavra
`yb` .... Copia do começo da palavra ao cursor
`5yy` ... Copia 5 linhas (também pode ser: `y5y` ou `5Y`)
`yw` .... Copia uma palavra
`5yw` ... Copia 5 palavras (também pode ser: `y5w`)
`y^` .... Copia da posição atual até o início da linha (sinônimo: `y0`)
`y$` .... Copia da posição atual até o final da linha
`ygg` ... Copia da posição atual até o início do arquivo
`yG` .... Copia da posição atual até o final do arquivo

> No modo normal, o ato de **COLAR** o texto está associado à letra `p`.

`p` .... Cola o que foi copiado ou deletado abaixo
`P` .... Cola o que foi copiado ou deletado acima
`[p` ... Cola o que foi copiado ou deletado antes do cursor
`]p` ... Cola o que foi copiado ou deletado após o cursor

### Repetindo a digitação de linhas

`Ctrl-y` ........... Repete linha acima
`Ctrl-e` ........... Repete linha abaixo
`Ctrl-x` `Ctrl-l` .. Repete linhas inteiras
`Ctrl-a` ........... Repete a última inserção

### Desfazendo

`u` ............ Desfazer
`U` ............ Desfaz mudanças na última linha editada
`Ctrl-r` ....... Refazer

**Máquina do tempo**

`:earlier 10m`

Com esse comando o documento ficará exatamente como ele estava 10 minutos atrás! Caso após a exclusão perceba-se que foi excluído um minuto a mais, é possível utilizar o mesmo padrão novamente para avançar no tempo:

`:later 60s`

Note que dessa vez foi utilizado _later_ ao invés de _earlier_, e passando segundos como argumento para viajar no tempo. Portanto o comando acima avança 60 segundos no tempo.

Para uma melhor visão de quanto se deve voltar, pode ser usado o comando:

`:undolist`

O comando acima mostra a lista com as informações sobre Desfazer e Refazer. E com essas informações pode-se voltar no tempo seguindo cada modificação:

`:undo 3`

Esse comando fará o documento regredir 3 modificações.

## Movendo-se no documento

`gg` .... Vai para o início do arquivo
`G` ..... Vai para o final do arquivo
`0` ..... Vai para o início da linha
`^` ..... vai para o primeiro caractere da linha (ignora espaços)
`$` ..... Vai para o final da linha
`’’` .... Salta para a linha da ´ultima posição em que o cursor estava
`*` ..... Próxima ocorrência de palavra sob o cursor
`‘’` .... Salta exatamente para a posição em que o cursor estava
`w` ..... Move para o início da próxima palavra
`W` ..... Pula para próxima palavra (desconsidera hífens)
`E` ..... Pula para o final da próxima palavra (desconsidera hífens)
`e` ..... Move o cursor para o final da próxima palavra
`zt` .... Move o cursor para o topo da página
`zm` .... Move o cursor para o meio da página
`zz` .... Move a página de modo com que o cursor fique no centro
`n` ..... move o cursor para a próxima ocorrência da busca
`N` ..... move o cursor para a ocorrência anterior da busca
`yG` .... Copia da linha atual até o final do arquivo
`d$` .... Deleta do ponto atual até o final da linha
`gi` .... Entra em modo de inserção no ponto da última edição
`gf` .... Abre o arquivo sob o cursor
`)` ..... Pula uma sentença para frente
`(` ..... Pula uma sentença para trás
`}` ..... Pula um parágrafo para frente
`{` ..... Pula um parágrafo para trás
`y)` .... Copia uma sentença para frente
`d}` .... Deleta um parágrafo para frente

A maioria dos comandos do _Vim_ pode ser precedida por um quantificador:

`5j` ..... Desce 5 linhas
`d5j` .... Deleta as próximas 5 linhas
`k` ...... Em modo normal sobe uma linha
`5k` ..... Sobe 5 linhas
`y5k` .... Copia 5 linhas (para cima)
`w` ...... Pula uma palavra para frente
`5w` ..... Pula 5 palavras
`d5w` .... Deleta 5 palavras
`b` ...... Retrocede uma palavra
`5b` ..... Retrocede 5 palavras
`dgg` .... Deleta da linha atual até o começo do arquivo
`dG` ..... Deleta até o final do arquivo
`yG` ..... Copia até o final do arquivo
`yfx` .... Copia até o próximo ‘‘x’’
`y5j` .... Copia 5 linhas

## Paginando

Para rolar uma página de cada vez (em modo normal)

`Ctrl-f`
`Ctrl-b`

`Ctrl-u`
`Ctrl-d`

`:h jumps` . Ajuda sobre a lista de saltos
`:jumps` ... Exibe a lista de saltos
`Ctrl-i` ... Salta para a posição mais recente
`Ctrl-o` ... Salta para a posição mais antiga
`’0` ....... Abre o último arquivo editado
`’1` ....... Abre o penúltimo arquivo editado
`gd` ....... Pula para a definição de uma variável
`[i` ....... Pula para definição de variável sob o cursor
`:bn` ...... Vai para o próximo da lista
`:bp` ...... Volta para o arquivo anterior
`:ls` ...... lista todos os arquivos abertos
`:wn` ...... salva e vai para o próximo
`:wp` ...... salva e vai para o prévio

## Folders

Folders são como dobras nas quais o Vim esconde partes do texto:

### Manipulando dobras

`zo` ..... Abre a dobra
`zO` ..... Abre a dobra, recursivamente
`za` ..... Abre/fecha (alterna) a dobra
`zA` ..... Abre/fecha (alterna) a dobra, recursivamente
`zR` ..... Abre todas as dobras do arquivo atual
`zM` ..... Fecha todas as dobras do arquivo atual
`zc` ..... Fecha uma dobra
`zC` ..... Fecha a dobra abaixo do cursor, recursivamente
`zd` ..... Apaga a dobra (não o seu conteúdo)
`zj` ..... Move para o início da próxima dobra
`zk` ..... Move para o final da dobra anterior
`[z` ..... Move o cursor para início da dobra aberta
`]z` ..... Move o cursor para o fim da dobra aberta
`zi` ..... Desabilita ou habilita as dobras

## Trabalhando com Janelas

O Vim trabalha com o conceito de m´ultiplos buffers de arquivo. Cada buffer é um arquivo carregado para edição. Um buffer pode estar visível ou não, e é possível dividir a tela em janelas, de forma a visualizar mais de um buffer simultaneamente.

### Alternando entre Buffers de arquivo

Para saber quantos documentos estão abertos no momento utiliza-se o comando `:ls` ou `:buffers`. Esses comandos listam todos os arquivos que estão referenciados no buffer com suas respectivas "chaves" de referencia.

Para trocar a visualização do Buffer atual pode-se usar:

:buffer# ...... Altera para o buffer anterior
:b2 ........... Altera para o buffer cujo a chave é 2

### Modos de divisão da janela

#### Utilizando abas TAB

A partir do Vim 7 foi disponibilizada a função de abrir arquivos em abas, portanto é possível ter vários buffers abertos em abas distintas e alternar entre elas facilmente. Os comandos para utilização das abas são:

:tabnew ........... Abre uma nova tab
:tabprevious ...... Vai para a tab anterior
:tabnext .......... Vai para a pr´oxima tab

### Abrindo e fechando janelas

```
Ctrl-w-s ...... Divide a janela horizontalmente (:split)
Ctrl-w-v ...... Divide a janela verticalmente (:vsplit)
Ctrl-w-o ...... Faz a janela atual ser a ´unica (:only)
Ctrl-w-n ...... Abre nova janela (:new)
Ctrl-w-q ...... Fecha a janela atual (:quit)
Ctrl-w-c ...... Fecha a janela atual (:close)
```

_Lembrando que o Vim considera todos os arquivos como "buffers" portanto quando um arquivo é fechado, o que está sendo fechado é a visualização do mesmo, pois ele continua aberto no "buffer"._

### Manipulando janelas

```
Ctrl-w-w ......... Alterna entre janelas
Ctrl-w-j ......... desce uma janela ‘j’
Ctrl-w-k ......... sobe uma janela ‘k’
Ctrl-w-l ......... move para a janela da direta ‘l’
Ctrl-w-h ......... move para a janela da direta ‘h’
Ctrl-w-r ......... Rotaciona janelas na tela
Ctrl-w-+ ......... Aumenta o espa¸co da janela atual
Ctrl-w-- ......... Diminui o espa¸co da janela atual
```

Pode-se mapear um atalho para redimensionar janelas com as teclas ‘+’ e ‘-’.

```
:map + <c-w>+
:map - <c-w>-
```

### File Explorer

Para abrir o gerenciador de arquivos do Vim use:

```
:Vex ............ Abre o file explorer verticalmente
:Sex ............ Abre o file explorer em nova janela
:Tex ............ Abre o file explorer em nova aba
:e .............. Abre o file explorer na janela atual
```

## Salvando Sessões de Trabalho

Suponha a situação em que um usuário está trabalhando em um projeto no qual vários arquivos são editados simultaneamente; quatro arquivos estão abertos, algumas macros foram criadas e variáveis que não constam no vimrc foram definidas. Em uma situação normal, se o Vim for fechado a quase totalidade dessas informações se perde; para evitar isto uma sessão pode ser criada, gerando-se um "retrato do estado atual", e então restaurada futuramente pelo usuário-na prática é como se o usuário não tivesse saído do editor.

Uma sessão do Vim guarda, portanto, uma série de informações sobre a edição corrente, de modo a permitir que o usuário possa restaurá-la quando desejar. Sessões são bastante úteis, por exemplo, para se alternar entre diferentes projetos, carregando-se rapidamente os arquivos e definições relativas a cada projeto.

### Criando sessões

```
:mks[ession] sessao.vim .... Cria a sessão e armazena-a em sessao.vim
:mks[ession]! sessao.vim ... Salva a sessão e sobrescreve-a em sessao.vim
```

### Restaurando sessões

Após gravar sessões, elas podem ser carregadas ao iniciar o Vim:

```
vim -S sessao.vim
```

ou então de dentro do próprio Vim (no modo de comando):

```
:so sessao.vim
```
