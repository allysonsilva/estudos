# Definições

- *Instrução é um comando que executa uma ação, como calcular um valor e armazenar o resultado, ou exibir uma mensagem para o usuário.*

- *Um método nada mais é do que um conjunto de instruções.*

- *Sintaxe são regras definidas por uma linguagem de programação que descrevem seu formato e sua construção.*

- *O truque para programar bem em qualquer linguagem é aprender sua sintaxe e semântica e então utilizá-la de maneira natural e idiomática. Essa estratégia facilita a manutenção dos seus programas.*

- *Identificadores são os nomes utilizados para distinguir os elementos nos seus programas, como namespaces, classes, métodos e variáveis.*

- *A linguagem C# reserva, para uso próprio, 77 identificadores, os quais não podem ser reutilizados para outros propósitos. Eles são denominados palavras-chave, e cada um tem um significado específico.*

- *Variável é um local de armazenamento que contém um valor. Você pode considerar uma variável como uma caixa na memória do computador que contém informações temporárias.*

- *Um nome de variável é utilizado para referenciar o valor que ela armazena.*

*O sinal de igual(`=`) é o operador de atribuição, que atribui o valor que está a sua direita à variável que está a sua esquerda.*

- *C# tem vários tipos predefinidos denominados tipos de dados primitivos.*

- *C# não permite utilizar uma variável não atribuída. É necessário atribuir um valor a uma variável antes de usá-la; caso contrário, o programa não compilará. Essa exigência é chamada regra de atribuição definitiva.*

- *Cada tipo de dado no .NET Framework tem um método `ToString`. A finalidade de `ToString` é converter um objeto na sua representação de `string`.*

- *Ao criar seus próprio tipos de dados e classes, você pode definir uma implementação própria do método `ToString` para especificar a maneira como sua classe deve ser representada como uma string*

- *Os símbolos `+`, `–`, `*` e `/` são denominados operadores porque “operam” em valores para criar novos valores*

- *Os valores nos quais um operador efetua sua função chamam-se operandos. Na expressão `750 * 20`, o `*` é o operador e `750` e `20` são os operandos.*

- *O tipo de resultado de uma operação aritmética depende do tipo dos operandos utilizados.*

- *No C#, os números literais com pontos decimais são sempre `double`, não `float`, para manter o máximo de precisão possível*

- *A precedência(ou prioridade) controla a ordem em que os operadores da expressão são avaliados.*

- *Parênteses podem ser utilizados para anular a precedência e forçar os operandos a vincular-se aos operadores de maneira diferente.*

- *Associatividade é a direção(esquerda ou direita) em que os operandos de um operador são avaliados.*

- *O operador de atribuição é associado da direita para a esquerda. A atribuição mais à direita ocorre primeiro, e o valor atribuído se propaga pelas variáveis da direita para a esquerda. Se uma das variáveis já tivesse um valor, esse seria sobrescrito pelo valor que está sendo atribuído.*

- *Os operadores `++` e `--` são unários, ou seja, eles têm um único operando. Eles compartilham a mesma precedência e ambos são associativos à esquerda.*

- *Os operadores de incremento(`++`) e decremento(`--`) fogem do comum, porque você pode colocá-los antes ou depois da variável. Quando o símbolo do operador é colocado antes da variável, chamamos de forma prefixada do operador, e quando colocado depois, chamamos de forma pós-fixada ou sufixada do operador.*

- *O valor retornado por `count++` é o valor de `count` antes do incremento, enquanto o valor retornado por `++count` é o valor de `count` depois que o incremento ocorre.*

- *É possível instruir o compilador C# a deduzir o tipo de uma variável a partir de uma expressão e utilizá-lo ao declarar a variável com a palavra-chave `var` no lugar do tipo. A palavra-chave `var` faz o compilador deduzir o tipo das variáveis a partir dos tipos das expressões utilizadas para inicializá-las.*

- *Só é possível utilizar a palavra-chave `var` quando fornecer uma expressão para inicializar uma variável.*

- *Um método é uma sequência nomeada de instruções. É formado por um nome e um corpo. O nome do método deve ser um identificador significativo que indique sua finalidade geral. podem receber alguns dados para serem processados e retornar informações, que normalmente são o resultado do processamento.*

- *Uma variável só pode ser utilizada depois de ser criada. Quando o método termina, essa variável desaparece e não pode ser usada em outro lugar.*

- *Quando uma variável pode ser acessada em um local específico em um programa, dizemos que ela está no escopo desse local.*

- *Definir uma variável fora de um método, mas dentro de uma classe – essa variável pode ser acessada por qualquer método dentro dessa classe. Diz-se que essa variável tem **escopo de classe**.*

- *O escopo de uma variável é simplesmente a região do programa na qual essa variável é utilizada. O escopo se aplica aos métodos e às variáveis. O escopo de um identificador(de uma variável ou método) está vinculado ao local da declaração que introduz o identificador no programa.*

- *Todas as variáveis que você declara dentro do corpo de um método estão no seu escopo; elas desaparecem quando o método termina e só podem ser acessadas pelo código executado dentro desse método. Essas variáveis são denominadas **variáveis locais** porque são locais para o método em que são declaradas; elas não estão no escopo de nenhum outro método.*

- *As chaves de abertura e fechamento que formam o corpo de uma classe definem o escopo dessa classe. Todas as variáveis que você declara dentro do corpo de uma classe(mas não dentro de um método) estão no escopo dela. O termo apropriado do C# para uma variável definida por uma classe é **field**(campo).*

- *Em um método, você deve declarar uma variável antes de poder utilizá-la. Os campos são um pouco diferentes. Um método pode utilizar um campo antes da instrução que define o campo – o compilador resolve os detalhes para você.*

- *Se dois identificadores têm o mesmo nome e são declarados no mesmo escopo, dizemos que eles estão **sobrecarregados**.*

- *Em tempo de compilação, o compilador examina os tipos de argumentos que você está passando e então providencia para que seu aplicativo chame a versão do método que tem o conjunto de parâmetros correspondente.*

- *A sobrecarga é útil principalmente quando você precisa executar a mesma operação em diferentes tipos de dados ou grupos variados de informações. Você pode sobrecarregar um método quando as diferentes implementações têm diferentes conjuntos de parâmetros – isto é, quando elas têm o mesmo nome, mas um número diferente de parâmetros, ou quando os tipos de parâmetro forem diferentes.*

- *O número e o tipo dos argumentos são utilizados pelo compilador para selecionar um dos métodos sobrecarregados. **Mas lembre-se de que, embora possa sobrecarregar os parâmetros de um método, você não pode sobrecarregar o tipo de retorno de um método**. Ou seja, você não pode declarar dois métodos com o mesmo nome cuja diferença seja apenas o seu tipo de retorno.*

- *Os parâmetros opcionais representam uma solução compacta e simples, quando não é possível utilizar sobrecarga porque os tipos dos parâmetros não variam o bastante para permitir que o compilador possa distinguir entre as implementações.*

- *Ao definir um método, você especifica que um parâmetro é opcional fornecendo um valor padrão para o parâmetro. Para indicar um valor padrão, utilize um operador de atribuição. Deve ser especificado todos os parâmetros obrigatórios antes de qualquer parâmetro opcional.*

- *Por padrão, o C# utiliza a posição de cada argumento na chamada a um método para determinar os parâmetros aos quais eles se aplicam. Os argumentos nomeados permitem que você passe os argumentos em qualquer ordem.*

- *É possível mesclar argumentos posicionais e nomeados. Entretanto, ao utilizar essa técnica, você deve especificar todos os argumentos posicionais antes do primeiro argumento nomeado.*

- *Dois operadores booleanos utilizados com frequência são os operadores de igualdade(`==`) e desigualdade(`!=`). Esses são operadores binários, os quais permitem determinar se um valor é igual a outro valor de mesmo tipo, produzindo um resultado booleano.*

- *Não confunda o operador de igualdade `==` com o operador de atribuição `=`. A expressão `x==y` compara `x` com `y` e tem o valor true se os valores forem idênticos. A expressão `x=y` atribui o valor de `y` a `x` e retorna o valor de `y` como resultado.*

- *O resultado do operador `&&` será `true` se e somente se as duas expressões booleanas que está avaliando forem `true`.*

- *O resultado do operador `||` será true se pelo menos uma das expressões booleanas que avalia for true. O operador `||` é utilizado para determinar se uma expressão de uma combinação de expressões booleanas é `true`.*

- *A expressão em uma instrução `if` deve estar entre parênteses. Além disso, ela deve ser uma expressão booleana.*

- *Um bloco é simplesmente uma sequência de instruções agrupadas entre uma chave de abertura e uma de fechamento.*

- *A instrução `break` é a maneira mais comum de parar um fall-through, mas você também pode usar uma instrução return para sair do método que contém a instrução `switch` ou uma instrução `throw`, para gerar uma exceção e abortar a instrução `switch`.*

- *Você utiliza uma instrução `while` para executar uma instrução repetidamente enquanto alguma condição se mantiver verdadeira.*

- *No C#, a instrução `for` fornece uma versão mais formal desse tipo de construção, combinando a inicialização, a expressão booleana e o código que atualiza a variável de controle. A instrução `for` é útil, pois é muito mais difícil sair acidentalmente do código que inicializa ou atualiza a variável de controle, de modo que é menos provável que você escreva código contendo loops infinitos. A inicialização ocorre apenas uma vez, bem no início do loop. Portanto, se a expressão booleana for avaliada como `true`, a instrução será executada.*

- *As instruções `while` e for testam suas expressões booleanas no início do loop. Isso significa que, se a expressão é avaliada como false no primeiro teste, o corpo do loop não é executado – nem mesmo uma vez. A instrução do é diferente: sua expressão booleana é avaliada após cada iteração e, portanto, o corpo sempre é executado ao menos uma vez.*

- *Uma instrução `break` também pode ser utilizada para sair do corpo de uma instrução de iteração. Quando você sai de um loop, ele encerra imediatamente e a execução continua na primeira instrução após o loop. Nem a atualização nem a condição de continuação do loop são executadas novamente. Por outro lado, a instrução continue faz o programa executar imediatamente a próxima iteração do loop(depois de avaliar de novo a expressão booleana).*

- *Uma rotina de tratamento `catch` é concebida para capturar e tratar um tipo de exceção específico e você pode ter várias rotinas de tratamento `catch` depois de um bloco `try`, cada uma projetada para interceptar e processar uma exceção específica para que você possa fornecer diferentes rotinas de tratamento para os diferentes erros que possam surgir no bloco `try`. Se qualquer uma das instruções dentro do bloco `try` causar um erro, o runtime lançará uma exceção. O runtime examina então as rotinas de tratamento `catch` após o bloco `try` e transfere o controle diretamente para a primeira rotina de tratamento correspondente.*

- *Se, após retornar pela cascata de métodos chamadores, o runtime for incapaz de encontrar uma rotina de tratamento `catch` correspondente, o programa terminará com uma exceção não tratada.*

- *Se quiser capturar a exceção `Exception`, você pode omitir seu nome na rotina de tratamento `catch`, porque ela é a exceção padrão.*

- *Quando ocorre uma exceção, o runtime utiliza a primeira rotina de tratamento correspondente à exceção que encontra e as outras são ignoradas. Isso significa que, se você colocar uma rotina de tratamento para `Exception` antes de uma rotina de tratamento para `FormatException`, a rotina de tratamento de `FormatException` nunca será executada. Portanto, você deve colocar as rotinas de tratamento `catch` mais específicas acima de uma rotina de tratamento `catch` geral, depois de um bloco `try`. Se as rotinas de tratamento `catch` específicas não correspondem à exceção, a rotina de tratamento `catch` geral corresponderá.*

- *É importante lembrar que, quando uma exceção é lançada, ela altera o fluxo da execução no programa. Isso significa que você não pode garantir que uma instrução será sempre executada quando a instrução anterior terminar, porque a instrução anterior poderá lançar uma exceção.*

- *Um bloco `finally` ocorre imediatamente após um bloco `try` ou imediatamente após a última rotina de tratamento `catch`, depois de um bloco `try`. Desde que o programa entre no bloco `try` associado a um bloco `finally`, o bloco `finally` sempre será executado, mesmo que uma exceção ocorra. Se uma exceção for lançada e capturada localmente, a rotina de tratamento de exceção será executada primeiro, seguida pelo bloco `finally`. Em qualquer caso, o bloco `finally` sempre é executado.*

- *Classe é a raiz da palavra classificação. Ao projetar uma classe, você sistematicamente organiza as informações e o comportamento em uma entidade com significado.*

- *O encapsulamento é um princípio importante durante a definição de classes. A ideia é que um programa que utiliza uma classe não precisa se preocupar com o modo como essa classe realmente funciona internamente; o programa simplesmente cria uma instância de uma classe e chama os métodos dessa classe. Desde que esses métodos façam o que se propõem a fazer, o programa não se preocupa com a maneira como eles são implementados.*

- *No C#, você utiliza a palavra-chave `class` para definir uma nova classe. Os dados e os métodos da classe ocorrem no corpo da classe entre um par de chaves.*

- *As variáveis em uma classe são chamadas de `field`.*

- *Não confunda os termos classe e objeto. Uma classe é a definição de um tipo. Um objeto é uma instância desse tipo, criada quando o programa é executado. Vários objetos podem ser instâncias da mesma classe.*

***Nível de Acessibilidade Padrão***

| **Elements**     | **Default Accessibility** | **Allowed declared accessibility of the member** |
| :--------------- | :------------------------ | :----------------------------------------------- |
| `enum`           | `internal`                | `public`                                         |
| `interface`      | `internal`                | `public`                                         |
| `class`          | `internal`                | `private`                                        |
| `struct`         | `internal`                | `private`                                        |
| `property/field` | `private`                 |                                                  |
| `method`         | `private`                 |                                                  |
| `delegate`       | `internal`                | `private`                                        |

***Modificadores de Acesso***

> Todos os tipos e membros de um tipo têm um nível de acessibilidade, que controla se podem ser usados em outro código no seu assembly ou outros assemblies.

| **Modifier**         | **Description**                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| :------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `public`             | *O tipo ou membro pode ser acessado por qualquer outro código no mesmo assembly ou em outro assembly que faz referência a ele. É o nível de acesso mais permissivo. Não há nenhuma restrição quanto ao acesso a membros públicos.*                                                                                                                                                                                                                          |
| `protected`          | *O tipo ou membro pode ser acessado somente pelo código na mesma classe ou `struct` ou em uma classe derivada dessa classe. É acessível dentro de sua classe e por instâncias da classe derivada. O acesso é limitado à classe que os contém ou aos tipos derivados da classe que os contém.*                                                                                                                                                               |
| `internal`           | *O tipo ou membro pode ser acessado por qualquer código no mesmo assembly, mas não de outro assembly. O acesso é limitado ao assembly atual.*                                                                                                                                                                                                                                                                                                               |
| `protected internal` | *O tipo ou membro pode ser acessado por qualquer código no assembly no qual ele é declarado ou de uma classe derivada em outro assembly. O acesso de outro assembly deve ocorrer dentro de uma declaração de classe que deriva da classe na qual o elemento interno protegido é declarado e deve ser executado por meio de uma instância do tipo de classe derivada. O acesso é limitado ao assembly atual ou aos tipos derivados da classe que os contém.* |
| `private`            | *O tipo ou membro pode ser acessado somente pelo código na mesma classe ou `struct`. É o nível de acesso menos permissivo. São acessíveis somente dentro do corpo da classe ou do `struct` em que são declarados. O acesso é limitado ao tipo recipiente.*                                                                                                                                                                                                  |

- *De modo diferente das variáveis declaradas em um método, os campos em uma classe são automaticamente inicializados como **0**, **false** ou **null**, dependendo do seu tipo. Mas ainda é uma boa prática fornecer uma maneira explícita de inicializar os campos.*

- *Os identificadores que são `public` devem iniciar com uma letra maiúscula. Esse sistema é conhecido como esquema de nomes **PascalCase**(porque foi utilizado primeiramente na linguagem Pascal).*

- *Os identificadores que não são `public`(que incluem as variáveis locais) devem começar com uma letra minúscula. Esse sistema é conhecido como camelo(**camelCase**).*

- *Um `construtor` é um método especial executado automaticamente quando você cria uma instância de uma classe. Ele tem o mesmo nome da classe e pode receber parâmetros, mas não pode retornar um valor(nem mesmo `void`).*

- *No C#, o `construtor` padrão é um construtor que não recebe parâmetros. Não importa se é gerado pelo compilador ou escrito por você; ainda assim ele é o construtor padrão.*

- *Se essa palavra-chave do construtor(Modificador de visibilidade) for omitida, o construtor será privado(exatamente como qualquer outro método e campo). Se o construtor for privado, ele não poderá ser utilizado fora da classe, o que lhe impede de criar objetos a partir dos métodos que não fazem parte da classe.*

- *Um construtor é apenas um tipo especial de método e – como todos os métodos pode ser sobrecarregado.*

- *A ordem dos construtores em uma classe é irrelevante; você pode definir construtores na ordem que achar melhor.*

- *Quando você compila o aplicativo, o compilador deduz qual construtor deve chamar com base nos parâmetros especificados para o operador `new`.*

- *Fique atento a um importante recurso da linguagem C#: se você escrever seu próprio construtor para uma classe, o compilador não gerará um construtor padrão. Portanto, se você escreveu um construtor que aceita um ou mais parâmetros e também quiser um construtor padrão, você mesmo terá de escrever o construtor padrão.*

- *Ao dividir uma classe em vários arquivos, você define as partes da classe usando a palavra-chave `partial` em cada arquivo*

- *Ao compilar uma classe que foi dividida em arquivos separados, você deve fornecer todos os arquivos para o compilador.*

- *Dois objetos que são instâncias da mesma classe podem acessar dados privados uns dos outros, mas objetos que são instâncias de outra classe, não podem.*

- *Em C#, todos os métodos devem ser declarados dentro de uma classe. Mas se declarar um método ou um campo como `static`, você pode chamar o método ou acessar o campo utilizando o nome da classe. Não há necessidade de instância*

- *Um método estático não depende de uma instância da classe e não pode acessar um campo ou método de instância definido na classe; ele só utiliza os campos e outros métodos marcados como `static`.*

- *A definição de um campo como estático torna possível criar uma única instância de um campo que é compartilhada entre todos os objetos criados a partir de uma única classe. (Campos não estáticos são locais para cada instância de um objeto.)*

- *Convém lembrar que os métodos `static` também são chamados de métodos de classe. Mas os campos `static` não são em geral chamados de campos de classe; eles são chamados apenas de campos `static`(ou, eventualmente, de variáveis `static`).*

- *Prefixando o campo com a palavra-chave `const`, você pode declarar que um campo é estático, mas que seu valor nunca pode mudar. Um campo `const` não utiliza a palavra-chave `static` na sua declaração, mas mesmo assim é estático. Só pode declarar um campo como `const` quando esse campo é um tipo numérico(como `int` ou `double`), uma string ou uma enumeração.*

- *Outro recurso da linguagem C# é a capacidade de declarar uma classe como `static`. Uma classe `static` só pode conter membros `static`. (Todos os objetos que você cria utilizando essa classe compartilham uma única cópia desses membros.) O objetivo de uma classe `static` é puramente atuar como um contêiner de campos e métodos utilitários. Uma classe `static` não pode conter dados ou métodos de instância e não faz sentido tentar criar um objeto de uma classe `static` usando o operador `new`. De fato, você não pode criar uma instância de um objeto que utiliza uma classe `static` utilizando `new`, mesmo se quiser fazer isso. (O compilador informará um erro se você tentar.) Se você precisar fazer alguma inicialização, uma classe `static` poderá ter um construtor padrão, desde que ele também seja declarado como `static`. Qualquer outro tipo de construtor é ilegal e será reportado como tal pelo compilador.*

- *Uma classe anônima é uma classe que não tem nome. Você cria uma classe anônima simplesmente utilizando a palavra-chave `new` e um par de chaves que definem os campos e valores que você quer que a classe contenha. O compilador infere os tipos dos campos a partir dos tipos de dados que você especifica para inicializá-los.*

- *O compilador do C# utiliza os nomes, os tipos, o número e a ordem dos campos para determinar se duas instâncias de uma classe anônima têm o mesmo tipo.*

- *As classes anônimas só podem conter campos públicos, todos esses campos precisam ser inicializados, eles não podem ser estáticos e você não pode definir método algum para elas.*

- *A maioria dos tipos primitivos do C#, como `int`, `float`, `double` e `char`(mas não string, por motivos que serão abordados em breve) é coletivamente chamada de tipos-valor. Esses tipos têm tamanho fixo e, quando você declara uma variável como um tipo-valor, o compilador gera o código que aloca um bloco de memória grande o suficiente para conter um valor correspondente.*

- *Os tipos classe, são tratados de maneira diferente. Quando você declara uma variável de Classe, o compilador não gera um código que aloca um bloco de memória grande o suficiente para armazenar a Classe; tudo o que ele faz é alocar uma pequena parte da memória que possa armazenar o endereço de(ou referência a) outro bloco de memória que contém a Classe. (Um endereço especifica a localização de um item na memória.) A memória para o objeto real só é alocada quando a palavra-chave new é utilizada para criar o objeto. Uma classe é um exemplo de tipo-referência. Tipos-referência contêm referências a blocos de memória.*

- *Em C#, o tipo `string` é na verdade uma classe. Isso porque não há um tamanho padrão para uma `string`(diferentes strings podem conter diferentes números de caracteres) e é bem mais eficiente alocar memória para elas dinamicamente, quando o programa é executado, do que estaticamente, em tempo de compilação. A descrição de tipos-referência, como as classes, também se aplica ao tipo `string`. Na verdade, no C#, a palavra-chave `string` é apenas um alias para a classe `System.String`.*

- *Quando você declara `c` como `Circle`, `c` pode se referir a um objeto `Circle`; o valor real armazenado por `c` é o endereço de um objeto `Circle` na memória. Se você declarar mais uma variável, chamada `refc`(também como `Circle`), e atribuir `S` a `refc`, `refc` terá uma cópia do mesmo endereço que `c`; ou seja, existe apenas um objeto `Circle` e, agora, tanto `refc` como `c` se referem a ele.*

- *`private` significa na verdade “privado para a classe” e não “privado para um objeto”. Mas não confunda `private` com `static`. Se você simplesmente declara um campo como `private`, cada instância da classe recebe seus próprios dados. Se um campo é declarado como `static`, cada instância da classe compartilha os mesmos dados.*

- *Você não deve criar objetos que nunca são utilizados, pois isso desperdiça tempo e recursos.*

- *No C#, você pode atribuir o valor `null` a qualquer variável-referência. O valor `null` simplesmente significa que a variável não referencia objeto algum na memória*

- *O valor `null` é útil para inicializar tipos-referência. Às vezes, você precisa de um valor equivalente para tipos-valor, mas `null` é ele próprio uma referência; assim, não é possível atribuí-lo a um tipo-valor.*

- *Mas o C# define um modificador que pode ser utilizado para declarar se uma variável é um tipo-valor `nullable`. Um tipo-valor `nullable` comporta-se de maneira semelhante ao tipo-valor original, mas você pode atribuir o valor `null` a ele. Use o ponto de interrogação(?) para indicar que um tipo-valor é `nullable`*

- *Um tipo `nullable` expõe um par de propriedades que você pode utilizar para determinar se o tipo realmente tem um valor não `null` e qual é esse valor. A propriedade `HasValue` indica se um tipo `nullable` contém um valor ou é `null` e você pode recuperar o valor de um tipo `nullable` não `null` lendo a propriedade `Value`*

- *A propriedade `Value` de um tipo `nullable` é somente de leitura. Você pode utilizar essa propriedade para ler o valor de uma variável, mas não para modificá-la. Para atualizar uma variável `nullable`, utilize uma instrução de atribuição comum.*

- *Em geral, quando você passa um argumento para um método, o parâmetro correspondente é inicializado com uma cópia do argumento. Isso é verdade independentemente de o parâmetro ser um tipo-valor(como um `int`), um tipo `nullable`(como `int?`) ou um tipo-referência(como um `WrappedInt`).*

- *Se o parâmetro para um método é um tipo-referência, qualquer alteração feita utilizando esse parâmetro modifica os dados referenciados pelo argumento passado por ele. O ponto-chave é este: embora os dados referenciados tenham mudado, o argumento passado como parâmetro não mudou — ele ainda referencia o mesmo objeto. Em outras palavras, embora seja possível modificar o objeto que o argumento referencia, não é possível modificar o argumento propriamente dito*

- *Se você utilizar a palavra-chave `ref` como prefixo de um parâmetro, o compilador do C# gerará código que passa uma referência ao argumento real, em vez de uma cópia do argumento. Ao utilizar um parâmetro `ref`, tudo o que você fizer ao parâmetro também será feito ao argumento original, porque o parâmetro e o argumento referenciam o mesmo dado. Ao passar um argumento como um parâmetro `ref`, você também deve prefixar o argumento com a palavra-chave `ref`. Essa sintaxe fornece uma indicação visual útil para o programador de que o argumento pode mudar.*

- *Lembre-se de que o C# impõe a regra de que você deve atribuir um valor a uma variável, antes que possa lê-la. Essa regra também se aplica aos argumentos de método: você não pode passar um valor não inicializado como argumento para um método, mesmo que o argumento seja definido como `ref`.*

- *A palavra-chave `out` é sintaticamente semelhante à palavra-chave `ref`. Você pode utilizar a palavra-chave `out` como prefixo do parâmetro para que o parâmetro se torne um alias para o argumento. Assim como ao utilizar `ref`, tudo o que você faz no parâmetro também é feito no argumento original. Ao passar um argumento para um parâmetro `out`, você também deve prefixar o argumento com a palavra-chave `out`.*

- *A palavra-chave `out` é uma abreviação de output. Quando você passa um parâmetro `out` para um método, o método deve atribuir um valor a ele antes de terminar ou retornar.*

- *Uma vez que um parâmetro `out` deve receber um valor do método, você pode chamar o método sem inicializar seu argumento.*

- *Você pode usar os modificadores `ref` e `out` nos parâmetros de tipo-referência assim como nos parâmetros de tipo-valor. O efeito é exatamente o mesmo: o parâmetro torna-se um alias para o argumento.*

- *Sistemas operacionais e runtimes(ambientes de execução) de linguagens, como os utilizados pelo C#, em geral, dividem a memória utilizada para armazenar dados em duas áreas separadas, cada uma gerenciada de uma maneira distinta. Essas duas áreas são tradicionalmente chamadas pilha(stack) e heap. Pilha e heap servem para propósitos diferentes.*

- *Quando você chama um método, a memória necessária para seus parâmetros e suas variáveis locais é sempre adquirida da pilha. Quando o método termina(seja porque retornou, seja porque lançou uma exceção), a memória adquirida para os parâmetros e variáveis locais é automaticamente liberada de volta para a pilha e fica disponível para ser reutilizada quando outro método for chamado. Os parâmetros de método e as variáveis locais na pilha têm uma vida útil bem definida: eles nascem quando o método começa e desaparecem assim que o método termina.*

- *Quando você cria um objeto(uma instância de uma classe) utilizando a palavra-chave `new`, a memória necessária para compilar o objeto é sempre adquirida do heap. Você viu que o mesmo objeto pode ser referenciado de vários lugares utilizando variáveis de referência. Quando a última referência a um objeto desaparece, a memória utilizada pelo objeto torna-se disponível para ser reutilizada(embora ela possa não ser utilizada imediatamente). Portanto, os objetos criados no heap têm vida útil mais indeterminada; um objeto é criado com a palavra-chave `new`, mas só desaparece em algum ponto após a última referência a ele ser removida.*

- *Todos os tipos-valor são criados na pilha. Todos os tipos-referência(objetos) são criados no heap(embora a referência em si esteja na pilha). Tipos `nullable` na verdade são tipos-referência e são criados no heap.*

- *A memória de pilha é organizada como uma pilha de caixas sobrepostas umas sobre as outras. Quando um método é chamado, cada parâmetro é colocado em uma caixa que é disposta na parte superior da pilha. Cada variável local é igualmente atribuída a uma caixa, e esta é colocada no topo da pilha de caixas. Quando um método termina, pode-se considerar que todas as caixas são removidas da pilha.*

> - *A memória heap é literalmente um “monte” de caixas espalhadas por uma sala, em vez de empilhadas ordenadamente umas sobre as outras. Cada caixa tem um rótulo indicando se está em uso ou não. Quando um novo objeto é criado, o runtime procura uma caixa vazia e a aloca para o objeto. A referência à caixa é armazenada em uma variável local na pilha. O runtime monitora o número de referências a cada caixa. (Lembre-se de que duas variáveis podem referenciar o mesmo objeto). Quando a última referência desaparece, o runtime marca a caixa como fora de uso e, em algum ponto no futuro, esvaziará a caixa e a disponibilizará para reutilização.*

- *Embora o objeto esteja armazenado no heap, a referência ao objeto(a variável `c`) está armazenada na pilha.*

- *A memória heap não é infinita. Se a memória heap estiver esgotada, o operador `new` lançará uma exceção `OutOfMemoryException` e o objeto não será criado.*

- *Quando o método termina, os parâmetros e variáveis locais saem do escopo. A memória adquirida para `c` e param são automaticamente liberadas na pilha. O runtime nota que o objeto Circle não é mais referenciado e, mais tarde, providenciará para que sua memória seja reivindicada pelo heap.*

- *Todas as classes são tipos especializados da classe `System.Object` e que você pode utilizar `System.Object` para criar uma variável que pode referenciar qualquer tipo-referência. `System.Object` é uma classe tão importante que o C# fornece a palavra-chave `object` como um alias de `System.Object`.*

- *Utilize a palavra-chave `object` em vez de `System.Object`. Ela é mais direta e coerente com outras palavras-chave que são sinônimos para classes(como string para `System.String`*

- *As variáveis do tipo `object` podem referenciar qualquer item de qualquer tipo-referência. Mas as variáveis do tipo `object` também podem referenciar um tipo-valor.*

- *Todas as referências devem referenciar objetos no heap; criar referências a itens na pilha pode comprometer seriamente a robustez do runtime e criar uma potencial brecha de segurança; logo, isso não é permitido*

- *Cópia automática de um item da pilha para o heap é chamada de boxing*

- ***casting**. Essa é uma operação que verifica se é seguro converter um item de um tipo em outro, antes de realmente fazer a cópia. Você coloca o nome do tipo como prefixo da variável object entre parênteses*

- *Utilizando um casting, você pode especificar que, em sua opinião, os dados referenciados por um objeto têm um tipo específico e que é seguro referenciar o objeto utilizando esse tipo. A expressão-chave aqui é “em sua opinião”. O compilador do C# não verificará se esse é o caso, mas o runtime sim. Se o tipo de objeto na memória não corresponder ao casting, o runtime lançará uma InvalidCastException*

- *Mas capturar uma exceção e tentar se recuperar dela, caso o tipo de um objeto não seja aquele que você esperava, é uma estratégia bastante inepta. O C# fornece dois operadores bem mais úteis que podem ajudar a fazer um casting de uma maneira muito mais elegante, os operadores `is` e `as`.*

- *Utilize o operador `is` para verificar se o tipo de um objeto é aquele que você espera*

- *O operador `is` aceita dois operandos: uma referência a um objeto à esquerda e o nome de um tipo à direita. Se o tipo do objeto referenciado no heap tiver o tipo especificado, `is` será avaliado como true; caso contrário, será avaliado como `false`.*

- *O operador `as` desempenha um papel semelhante a `is`, mas de uma maneira ligeiramente mais abreviada.*

- *Como ocorre com o operador `is`, o operador `as` recebe um objeto e um tipo como seus operandos. O runtime tenta fazer o casting do objeto para o tipo especificado. Se o casting for bem-sucedido, o resultado será retornado. Se o casting for malsucedido, o operador as será avaliado como o valor `null`.*

- *Um ponteiro é uma variável que armazena o endereço ou uma referência a um item na memória(no heap ou na pilha).*

- *Aprendeu algumas diferenças importantes entre tipos-valor, que armazenam seus valores diretamente na pilha, e tipos-referência, que referenciam indiretamente seus objetos no heap. Também aprendeu a utilizar as palavras-chave `ref` e `out` nos parâmetros de método para obter acesso aos argumentos.*

- *Variável de tipo-valor armazena seu valor diretamente na pilha, ao passo que uma variável de tipo-referência armazena uma referência a um objeto no heap.*

- *Defina uma enumeração utilizando a palavra-chave `enum`, seguida por um conjunto de símbolos que identificam os valores válidos que o tipo pode ter, incluídos entre chaves.*

- *Todos os nomes literais de enumerações têm escopo definido pelo seu tipo enumerado. Isso é muito útil, porque torna possível que diferentes tipos enumerados coincidentemente contenham literais com o mesmo nome.*

- *Para que o valor de uma variável de tipo enumerado possa ser lido, é necessário atribuir-lhe um valor. Você só pode atribuir um valor definido pela enumeração a uma variável do tipo enumerado.*

- *Como ocorre com todos os tipos-valor, você pode criar uma versão `nullable` de uma variável de tipo enumerado utilizando o modificador `?`. Você então pode atribuir à variável o valor `null`, bem como os valores definidos pela enumeração.*

- *Todos os nomes literais de enumerações têm escopo definido pelo seu tipo enumerado. Isso é muito útil, porque torna possível que diferentes tipos enumerados coincidentemente contenham literais com o mesmo nome.*

- *Internamente, uma enumeração associa um valor inteiro a cada elemento da enumeração. Por padrão, a numeração inicia em `0` para o primeiro elemento e sobe em incrementos de `1`. É possível recuperar o valor inteiro subjacente de uma variável de tipo enumerado. Para isso, você deve fazer um casting para seu tipo subjacente.*

- *Quando você declara uma enumeração, os literais de enumeração recebem valores do tipo `int`. Você também pode optar por basear sua enumeração em um tipo inteiro subjacente diferente.*

- *As classes definem tipos-referência, que são sempre criados no heap.*

- *Uma estrutura é um tipo-valor. Como as estruturas são armazenadas na pilha, desde que a estrutura seja razoavelmente pequena, a sobrecarga de gerenciamento da memória, em geral, é reduzida. Como uma classe, uma estrutura pode ter campos, métodos e construtores próprios.*

- *Para declarar seu tipo-estrutura, você utiliza a palavra-chave `struct` seguida pelo nome do tipo, seguido pelo corpo da estrutura entre chaves de abertura e fechamento. Sintaticamente, o processo é semelhante a declarar uma classe*

- *Ao copiar uma variável do tipo-valor, você obtém duas cópias do valor. Por outro lado, ao copiar uma variável do tipo-referência, você obtém duas referências ao mesmo objeto. Em resumo, use estruturas para valores pequenos de dados para os quais elas sejam tão eficientes, ou quase tão eficientes, para copiar o valor quanto seriam para copiar um endereço. Utilize classes para dados mais complexos a fim de copiar com eficiência.*

- *Utilize estruturas para implementar conceitos simples cujas características principais são seus valores, em vez da funcionalidade que fornecem.*

- *Você não pode declarar um construtor padrão(um construtor sem parâmetros) para uma estrutura. A razão pela qual você não pode declarar seu próprio construtor padrão em uma estrutura é que o compilador sempre gera um. Em uma classe, o compilador só gerará o construtor padrão se você não escrever seu próprio construtor. O construtor padrão gerado pelo compilador para uma estrutura sempre define os campos como `0`, `false` ou `null` – assim como para uma classe.*

- *Em uma classe, você pode inicializar os campos de instância no seu ponto de declaração. Em uma estrutura, isso não é possível.*

- *Assim como nas enumerações, você pode criar uma versão `nullable` de uma variável de estrutura utilizando o modificador `?`. Você pode então atribuir o valor `null` à variável.*

- *Você só pode inicializar ou atribuir uma variável de estrutura a outra variável de estrutura se a do lado direito estiver totalmente inicializada(ou seja, se todos os seus campos estiverem preenchidos com valores válidos e não com valores indefinidos).*

- *Quando você copia uma variável de estrutura, cada campo posicionado no lado esquerdo é definido diretamente a partir do campo correspondente no lado direito.*

- *Um `array` é uma sequência não ordenada de itens. Todos os itens em um array têm o mesmo tipo, ao contrário dos campos em uma estrutura ou classe, que têm tipos diferentes. Os itens em um array residem em um bloco contíguo da memória e são acessados por meio de um índice; ao contrário dos campos em uma estrutura ou classe, que são acessados pelo nome.*

- *Você declara uma variável de `array` especificando o nome do tipo de elemento, seguido por um par de colchetes, seguido pelo nome da variável. Os colchetes significam que a variável é um `array`.*

- *Os elementos do array não estão restritos aos tipos primitivos. Também é possível criar arrays de estruturas, enumerações e classes*

- *Os arrays são tipos-referência, independentemente do tipo dos seus elementos. Isso significa que uma variável de array referencia um bloco contíguo de memória armazenando elementos de array no heap, assim como uma variável de classe referencia um objeto no heap. Essa regra se aplica independentemente do tipo dos itens de dados do array. Mesmo que o array contenha um tipo-valor, como int, a memória ainda será alocada no heap; esse é o único caso em que os tipos-valor não alocam memória na pilha.*

- *Lembre-se de que, ao declarar uma variável de classe, a memória não é alocada para o objeto até que você crie a instância utilizando `new`. Os arrays seguem o mesmo padrão: quando você declara uma variável de array, não declara seu tamanho e as memórias não são alocadas(somente a utilizada para armazenar a referência na pilha). O array aloca memória apenas quando a instância é criada, sendo esse também o ponto no qual você especifica o tamanho do array.*

- *Para criar uma instância de array, você utiliza a palavra-chave `new`, seguida pelo tipo do elemento, seguido pelo tamanho(entre colchetes) do array que está sendo criado. Criar um array também inicializa seus elementos com os valores padrão agora familiares(`0`, `null` ou `false`, dependendo se o tipo é numérico, uma referência ou um booleano, respectivamente).*

- *Como a memória para a instância do `array` é alocada dinamicamente, o tamanho do array não precisa ser uma constante; ele pode ser calculado em tempo de execução.*

- *Quando você cria uma instância de array, todos os elementos do array são inicializados com um valor padrão que depende do seu tipo. Por exemplo, o padrão para todos os valores numéricos é `0`, os objetos são inicializados com `null`, valores `DateTime` são definidos com a data e hora “`01/01/0001 00:00:00`” e strings são inicializadas com `null`.*

- *Ao inicializar uma variável de `array` dessa maneira, você pode omitir a expressão `new` e o tamanho do array. Nesse caso, o compilador calcula o tamanho a partir do número de inicializadores e gera o código para criar o array.*

- *Se você criar um array de estruturas ou objetos, poderá inicializar cada estrutura do array chamando o construtor da estrutura ou da classe.*

- *Quando você declara um array, o tipo do elemento precisa corresponder ao tipo dos elementos que você quer armazenar no array. Por exemplo, se declarar pins como um array de int, como mostrado nos exemplos anteriores, você não poderá armazenar um `double`, uma `string`, uma `struct` ou qualquer coisa que não seja um int nesse array. Se especificar uma lista de inicializadores ao declarar um array, você poderá deixar o compilador C# inferir o tipo real dos elementos do array*

- *Arrays implicitamente tipados são mais úteis quando você está trabalhando com tipos anônimos.*

- *Os campos dos tipos anônimos devem ser os mesmos para cada elemento do array.*

- *Para acessar um elemento individual de um array, você deve fornecer um índice indicando o elemento desejado. Os índices dos arrays começam em zero; assim, o elemento inicial de um array reside no índice `0` e não no índice `1`. Um valor de índice de `1` acessa o segundo elemento.*

- *Todo acesso aos elementos do array é verificado quanto aos limites. Se você especificar um índice menor que `0` ou maior ou igual ao tamanho do array, o compilador lançará uma exceção IndexOutOfRangeException.*

- *É comum que novos programadores se esqueçam de que arrays iniciam no elemento `0` e que o último elemento está na posição `Length – 1`. O C# fornece a instrução `foreach` com a qual é possível iterar pelos elementos de um array sem se preocupar com essas questões.*

- *É importante lembrar que os arrays são objetos de referência; portanto, se você modificar o conteúdo de um array passado como parâmetro dentro de um método, a modificação será visível por todas as referências ao array, incluindo o argumento original passado como parâmetro.*

- *Para retornar um array de um método, especifique o tipo do array como o tipo de retorno. No método, você cria e preenche o array.*

- *Os arrays são tipos-referência. (Lembre-se de que um array é uma instância da classe `System.Array`.) Uma variável de array contém uma referência a uma instância do array. Isso significa que, ao copiar uma variável de array, você realmente acaba ficando com duas referências à mesma instância do array,*

- *Você pode considerar um array bidimensional como uma tabela, com a primeira dimensão especificando um número de linhas e a segunda especificando um número de colunas.*

- *No C#, os arrays multidimensionais normais às vezes são chamados de arrays retangulares. Cada dimensão tem um formato regular.*

- *Sobrecarregar é o termo técnico utilizado para declarar dois ou mais métodos com o mesmo nome no mesmo escopo. A capacidade de sobrecarregar um método é muito útil para os casos nos quais você quer realizar a mesma ação sobre argumentos de tipos diferentes.*

- *Com um array `params` é possível passar um número variável de argumentos para um método. Você indica um array `params` utilizando a palavra-chave `params` como modificador de parâmetro do array, ao definir os parâmetros do método.*

- *Você não pode sobrecarregar um método baseado apenas na palavra-chave `params`. A palavra-chave `params` não faz parte da assinatura do método*

- *Um método não `params` sempre tem prioridade sobre um método `params`. Isso significa que, se quiser, você ainda pode criar uma versão sobrecarregada de um método para os casos comuns*

- *Você pode utilizar um array de parâmetros do tipo `object` para declarar um método que aceita qualquer número de argumentos `object`, permitindo que os argumentos passados sejam de qualquer tipo.*

- *Em geral, os arrays de parâmetros são utilizados nos métodos que aceitam qualquer número de parâmetros(inclusive nenhum), enquanto os parâmetros opcionais são utilizados somente quando não é conveniente instruir um chamador a fornecer um argumento para cada parâmetro.*

- *O compilador ainda gerou um código que chamou o método que aceita parâmetros opcionais, embora a assinatura do método não correspondesse exatamente à chamada. Diante da escolha entre utilizar um método que aceita parâmetros opcionais e um método que aceita uma lista de parâmetros, o compilador usará o método que aceita parâmetros opcionais.*

- *Herança em programação é, essencialmente, classificação – é uma relação entre.*

- *A classe derivada herda da classe base e os métodos na classe base se tornam parte da classe derivada. No C#, uma classe pode ser derivada de, no máximo, uma classe base; uma classe não pode ser derivada de duas ou mais classes. Mas, a menos que `DerivedClass` seja declarada como `sealed`, você pode criar outras classes derivadas que herdam de `DerivedClass` utilizando a mesma sintaxe.*

- *Se você é programador de C++, deve notar que não pode explicitar se a herança é pública, privada ou protegida. A herança no C# é sempre implicitamente pública. Se você conhece Java, note o uso do sinal de dois pontos e que não há palavra-chave `extends`.*

- *A herança só se aplica às classes, não às estruturas. Não é possível definir uma hierarquia de herança própria com estruturas e não é possível definir uma estrutura derivada de uma classe ou de outra estrutura. Na verdade, todas as estruturas herdam de uma classe abstrata chamada `System.ValueType`.*

- *A classe `System.Object` é a classe raiz de todas as classes. Todas as classes derivam implicitamente de `System.Object`. Qualquer método da classe `System.Object` é automaticamente repassado para baixo na cadeia de herança das classes. todas as classes que você define herdam automaticamente as características da classe `System.Object`*

- *Além dos métodos por ela herdados, uma classe derivada contém automaticamente todos os campos da classe base. Esses campos vão precisar de inicialização quando um objeto for criado. Esse tipo de inicialização costuma ser realizado em um construtor. Lembre-se de que todas as classes têm pelo menos um construtor. (Se você não fornecer um, o compilador gerará um construtor padrão.)*

- *Uma boa prática é o construtor em uma classe derivada chamar o construtor de sua classe base como parte da inicialização, o que permite ao construtor da classe base realizar qualquer inicialização adicional exigida. Você pode especificar a palavra-chave `base` para chamar um construtor de classe base ao definir um construtor para uma classe herdeira.*

- *Lembre-se de que o compilador gera apenas um construtor padrão, caso você não escreva construtores não padrão.*

- *A assinatura do método é o nome do método e o número e tipos dos seus parâmetros, mas não seus tipos de retorno. Dois métodos que possuem o mesmo nome e a mesma lista de parâmetros têm a mesma assinatura, mesmo que retornem tipos diferentes.*

- *o uso da palavra-chave `new` não altera o fato de que os dois métodos não têm relação alguma e de que a ocultação continua ocorrendo. Ela simplesmente desativa o aviso.*

- *Um método em uma classe derivada mascara(ou oculta) um método em uma classe base que tem a mesma assinatura.*

- *Um método escrito para ser redefinido é chamado método `virtual`.*

- *Você precisa saber a diferença entre redefinir um método e ocultar um método. Redefinir um método é um mecanismo para fornecer diferentes implementações do mesmo método – os métodos estão todos relacionados porque se destinam a executar a mesma tarefa, mas de uma maneira específica à classe. Ocultar um método é um meio de substituir um método por outro – os métodos em geral não estão relacionados e podem executar tarefas totalmente diferentes. Sobrescrever um método é um conceito útil de programação; ocultar um método é, muitas vezes, um erro.*

- *Se uma classe base declara que um método é `virtual`, uma classe derivada pode utilizar a palavra-chave `override` para declarar outra implementação desse método*

- *Os métodos virtuais tornam possível chamar diferentes versões do mesmo método com base no tipo de objeto determinado dinamicamente em tempo de execução.*

- *A herança é uma maneira muito poderosa de conectar as classes, e há claramente uma relação especial e próxima entre uma classe derivada e sua classe base. Em geral, é útil para uma classe base permitir que as classes derivadas acessem alguns de seus membros, enquanto oculta esses mesmos membros das classes que não fazem parte da hierarquia de herança.*

- *Os campos públicos violam o encapsulamento porque todos os usuários da classe têm acesso direto e irrestrito aos campos. Os campos protegidos mantêm o encapsulamento para os usuários de uma classe, para quem são inacessíveis. Contudo, os campos protegidos ainda permitem que o encapsulamento seja violado por outras classes que herdam da classe base.*

- *Você pode acessar um membro protegido de uma classe base não só em uma classe derivada, mas também em classes derivadas da classe derivada. Um membro protegido de uma classe base mantém sua acessibilidade protegida em uma classe derivada e é acessível às outras classes derivadas.*

- *A herança é um recurso poderoso que torna possível ampliar a funcionalidade de uma classe criando uma nova classe derivada dela.*

- *Utilizando um método de extensão é possível estender um tipo existente(uma classe ou uma estrutura) com métodos estáticos adicionais. Esses métodos estáticos tornam-se imediatamente disponíveis para seu código em qualquer instrução que referencie dados do tipo estendido.*

- *Você define um método de extensão em uma classe estática e especifica o tipo que o método aplica a ela como o primeiro parâmetro para o método, juntamente com a palavra-chave `this`.*

- *O compilador C# detecta automaticamente todos os métodos de extensão para determinado tipo a partir de todas as classes estáticas que estão em escopo.*

- *Uma interface não contém qualquer código ou dado; ela especifica os métodos e as propriedades que devem ser fornecidos por uma classe que herda da interface. Utilizar uma interface possibilita a separação completa dos nomes e das assinaturas dos métodos de uma classe de um lado e a implementação do método de outro.*

- *Classes abstratas e interfaces se parecem, exceto pelo fato de que podem conter código e dados. É possível, entretanto, especificar que determinados métodos de uma classe abstrata sejam virtuais, de maneira que uma classe que herde da classe abstrata disponha de sua própria implementação desses métodos. Muitas vezes, você faz uso de classes abstratas com interfaces, e elas fornecem conjuntamente uma técnica fundamental que possibilita construir estruturas de programação extensíveis*

- *Uma interface é, assim, semelhante a um contrato. Se uma classe implementar uma interface, esta garante que a classe conterá todos os métodos especificados na interface.*

- *Utilizando interfaces é possível realmente separar “o que” do “como”. A interface fornece apenas o nome, o tipo de retorno e os parâmetros do método. A forma como o método é implementado não é uma preocupação da interface. A interface descreve a funcionalidade que uma classe deve fornecer, mas não como essa funcionalidade é implementada.*

- *A definição de uma interface é sintaticamente semelhante à definição de uma classe, exceto que é utilizada a palavra-chave `interface` em lugar de class. Dentro da interface, os métodos são declarados exatamente como em uma classe ou em uma estrutura, exceto pelo fato de você nunca especificar um modificador de acesso(`public`, `private` ou `protected`). Além disso, em uma interface, os métodos não têm implementação; eles são simplesmente declarações, e todos os tipos que implementam a interface devem fornecer suas próprias implementações.*

- *Uma interface não pode conter dados; você não pode adicionar campos(nem mesmo privados) a uma interface.*

- *C# emprega uma notação posicional. A classe base é sempre nomeada primeiramente, seguida por uma vírgula, seguida pela interface.*

- *Uma interface, `InterfaceA`, pode herdar de outra interface, `InterfaceB`. Tecnicamente, isso é conhecido como extensão de interface, em vez de herança. Nesse caso, qualquer classe ou estrutura que implemente `InterfaceA` deve fornecer implementações de todos os métodos de `InterfaceB` e de `InterfaceA`.*

- *Da mesma maneira que você pode referenciar um objeto utilizando uma variável definida como uma classe mais elevada na hierarquia, também pode referenciar um objeto utilizando uma variável definida como uma interface que sua classe implementa.*

- *Você pode verificar se um objeto é uma instância de uma classe que implementa uma interface específica utilizando o operador `is`. O operador `is` é utilizado para determinar se um objeto tem.*

- *Uma classe pode ter no máximo uma classe base, mas pode implementar um número ilimitado de interfaces. Uma classe deve implementar todos os métodos declarados por essas interfaces. Se uma estrutura ou classe implementa mais de uma interface, você especifica as interfaces como uma lista separada por vírgulas. Se uma classe também tem uma classe base, as interfaces são listadas após a classe base.*

- *C# não distingue qual interface o método está implementando, de modo que o mesmo método implementa as duas interfaces.*

- *Você pode implementar as interfaces explicitamente. Para isso, especifique a qual interface um método pertence, quando você a implementar.*

- *Além de prefixar o nome do método com o nome da interface, existe outra diferença sutil nessa sintaxe: os métodos não são marcados como public. Não é possível especificar a proteção para os métodos que fazem parte de uma implementação explícita de interface. Ambos são privados.*

- *É recomendável implementar interfaces explicitamente, sempre que possível.*

- *Você não tem permissão para definir campos em uma interface, nem mesmo campos estáticos. Um campo é um detalhe de implementação de uma classe ou estrutura.*

- *Você não pode especificar um modificador de acesso para qualquer método. Todos os métodos de uma interface são implicitamente públicos.*

- *Uma interface não pode ser herdada de uma estrutura nem de uma classe, embora uma interface possa herdar de outra interface. As estruturas e classes contêm implementações; se uma interface tivesse permissão para herdar de qualquer uma das duas, estaria herdando alguma implementação.*

- *Para declarar que a criação de instâncias de uma classe não é permitida, você pode explicitar que a classe é abstrata, utilizando a palavra-chave `abstract`*

- *Uma classe abstrata pode conter métodos abstratos. Um método abstrato é semelhante em princípio a um método `virtual`, exceto por não conter um corpo de método. Uma classe derivada precisa redefinir esse método.*

- *Um método abstrato é útil se não fizer sentido fornecer uma implementação padrão na classe abstrata, mas se você quiser assegurar que uma classe que herda forneça uma implementação própria desse método.*

- *No C#, se quiser, você pode utilizar a palavra-chave `sealed` para impedir que uma classe seja utilizada como uma classe base. Uma classe selada não pode declarar método `virtual` algum e que uma classe abstrata não pode ser selada.*

- *Você também pode utilizar a palavra-chave `sealed` para declarar que um método individual em uma classe não selada está selado. Isso significa que uma classe derivada não poderá redefinir esse método. Você só pode selar um método `override`, e esse método é declarado como `sealed override`.*

- *Os tipos-valor são criados na pilha e os tipos-referência são memória alocada a partir do heap.) Como os computadores não possuem memória infinita, é preciso que ela seja recuperada quando uma variável ou objeto não necessitar mais dela. Os tipos-valor são destruídos e sua memória é reivindicada quando eles saem de escopo.*

- *A operação `new` aloca uma parte da memória bruta a partir do heap. Você não tem controle algum sobre essa fase da criação de um objeto.*

- *A operação `new` converte a parte da memória bruta em um objeto; ela tem de inicializar o objeto. Você pode controlar essa fase utilizando um construtor.*

- *Quando a variável sai de escopo, o objeto Square não está mais sendo referenciado ativamente; o objeto pode ser destruído e a memória que ele está utilizando pode ser reivindicada(contudo, talvez isso não aconteça mediatamente, conforme você vai ver mais adiante). Assim como a criação de objetos, sua destruição é um processo de duas fases. As duas fases da destruição espelham exatamente as duas fases de criação:*

1. *O Common Language Runtime(CLR) precisa organizar as coisas. Você pode controlar isso escrevendo um destrutor.*

2. *O CLR precisa retornar para o heap a memória que anteriormente pertencia ao objeto; a memória em que o objeto residia precisa ser desalocada. Você não tem controle algum sobre essa fase.*

- *O processo de destruição de um objeto e devolução da memória para o heap é conhecido como coleta de lixo.*

- *Você pode utilizar um destrutor para executar qualquer limpeza necessária quando um objeto vai para a coleta de lixo. O CLR limpará automaticamente os recursos gerenciados utilizados por um objeto e, em muitos desses casos, é desnecessário escrever um destrutor. Contudo, se um recurso gerenciado é grande(como um array multidimensional), talvez faça sentido torná-lo disponível para descarte imediato, definindo como `null` qualquer referência que o objeto tenha a ele. Além disso, se um objeto faz referência, direta ou indiretamente, a um recurso não gerenciado, um destrutor pode se mostrar útil.*

- *Um destrutor é um método especial, parecido com um construtor, exceto pelo fato de o CLR o chamar depois da referência a um objeto desaparecer. A sintaxe para escrever um destrutor é um til(`~`), seguido pelo nome da classe.*

1. *Os destrutores se aplicam somente a tipos-referência; você não pode declarar um destrutor em um tipo-valor, como um `struct`.*
2. *Você não pode especificar um modificador de acesso(como `public`) para um destrutor. Você nunca chama o destrutor no seu próprio código; uma parte do CLR, chamada coletor de lixo, faz isso para você.*
3. *Um destrutor não pode aceitar qualquer parâmetro. Novamente, isso ocorre porque você nunca chama o destrutor por conta própria.*

- *Internamente, o compilador C# converte automaticamente um destrutor em uma redefinição do método `Object.Finalize`.*

- *Um destrutor sempre chamará o destrutor da sua classe base, mesmo que ocorra uma exceção durante seu código de destrutor.*

- *É importante entender que apenas o compilador pode fazer essa conversão. Você não pode escrever seu próprio método para substituir `Finalize`, nem pode chamar `Finalize` por conta própria.*

- *O tempo de vida de um objeto não pode estar vinculado a determinada variável de referência. Um objeto só poderá ser destruído e sua memória se tornar disponível para reutilização quando todas as referências a ele desaparecerem.*

1. *Todo objeto será destruído e seu destrutor será executado. Quando um programa terminar, todos os objetos existentes serão destruídos.*
2. *Cada objeto será destruído apenas uma vez.*
3. *Cada objeto será destruído somente quando se tornar inacessível – isto é, quando não houver qualquer referência ao objeto no processo de execução de seu aplicativo.*

- *Quando ocorre a coleta de lixo? Essa pergunta pode parecer estranha. Afinal de contas, a coleta de lixo ocorre quando um objeto não é mais necessário. É isso mesmo, mas não necessariamente de imediato. A coleta de lixo pode ser um processo caro; portanto, o CLR só coleta o lixo quando há necessidade(quando a memória disponível está diminuindo ou o tamanho do heap ultrapassa o limite definido pelo sistema, por exemplo) e, então, coleta o máximo possível. É melhor fazer algumas limpezas grandes do que muitas limpezas pequenas.*

- *Outra característica do coletor de lixo é que você não sabe a ordem em que os objetos serão destruídos. A questão final a discutir talvez seja a mais importante: os destrutores não são executados até que os objetos sofram coleta de lixo.*

- *O coletor de lixo executa em sua própria thread e só pode executar em certas ocasiões – em geral, quando o aplicativo chega ao final de um método. Enquanto ele executa, outras threads em execução no seu aplicativo são temporariamente suspensas. Isso acontece porque o coletor de lixo talvez precise mover os objetos e atualizar as referências de objeto, e ele não pode fazer isso enquanto os objetos estão em uso.*

- *Escrever classes que contenham destrutores aumenta a complexidade do seu código e do processo de coleta de lixo, e faz seu programa executar com mais lentidão. Se o programa não contiver destrutor algum, o coletor de lixo não precisará posicionar objetos inacessíveis na fila freachable e finalizá-los. utilize-os apenas para reivindicar recursos não gerenciados.*

- *Você precisa ter bastante cuidado ao escrever um destrutor. Em particular, esteja ciente de que, se seu destrutor chamar outros objetos, é possível que o coletor de lixo já tenha chamado os destrutores desses outros objetos. Lembre-se de que a ordem da finalização não é garantida. Portanto, certifique-se de que os destrutores não dependam um do outro nem se sobreponham – evite que dois destrutores tentem liberar o mesmo recurso, por exemplo.*

- *A única opção é você mesmo liberar o recurso. Você pode fazer isso criando um método de descarte. Um método de descarte descarta um recurso. Se uma classe tem um método de descarte, você pode chamá-lo e controlar quando o recurso será liberado.*

- *O termo método de descarte refere-se ao propósito do método e não ao seu nome. Um método de descarte pode ser nomeado utilizando qualquer identificador C# válido.*

- *A instrução `using` fornece um mecanismo limpo para controlar os tempos de vida dos recursos. Você pode criar um objeto e esse ser destruído quando o bloco da instrução `using` terminar.*

- *A instrução `using` introduz um bloco próprio para propósitos de definição de escopo. Esse arranjo significa que a variável declarada em uma instrução `using` sai automaticamente de escopo no final da instrução embutida e não há como você acidentalmente tentar acessar um recurso removido*.

- *A variável declarada em uma instrução `using` deve ser do tipo que implementa a interface `IDisposable`. A interface `IDisposable` reside no namespace `System` e contém apenas um método, chamado `Dispose`*

- *O objetivo do método `Dispose` é liberar os recursos utilizados por um objeto.*

- *Você sabe exatamente quando uma chamada para o método `Dispose` acontece, mas só não pode ter certeza de que ela realmente acontecerá, pois ela precisa que o programador que está utilizando suas classes se lembre de escrever uma instrução `using`.*

- *O destrutor só é chamado pelo coletor de lixo quando seu objeto estiver sendo finalizado.*

- *O método `Dispose` público chama o método estático `GC.SuppressFinalize`. Esse método faz o coletor de lixo parar de chamar o destrutor nesse objeto, pois o objeto já foi finalizado.*

- *O CLR garante que todos os objetos criados por seus aplicativos se sujeitarão à coleta de lixo, mas nem sempre você pode garantir quando isso acontecerá. você quer liberar os recursos assim que acabar de utilizá-los, em vez de esperar que o aplicativo termine.*

- *A classe `GC` dá acesso ao coletor de lixo e implementa vários métodos estáticos com os quais é possível controlar algumas das ações que ele executa. Com o método `SuppressFinalize`, você pode indicar se o coletor de lixo não deve realizar a finalização no objeto especificado, e isso impede a execução do destrutor.*

- *O objetivo da instrução `using` é garantir que um objeto sempre seja descartado, mesmo que ocorra uma exceção enquanto ele está sendo utilizado.*

- *Herança deve ser usada em um e um só caso: quando existe uma relação "é um tipo de" entre a classe concreta e a abstracta. Por exemplo, "um gato é um tipo de animal" ou "um apartamento é um tipo de casa".*

- *Quando pensar em usar classes abstractas e herança, lembre-se do LSP(Liskov Substitution Principle). Este princípio diz que se uma classe Derivada deriva da classe Base, então qualquer código que use Base pode também usar uma instancia de Derivada sem efeitos surpreendentes.*

- *Interfaces também desempenham um papel semelhante à Classes Abstratas. Quando olhamos Interfaces pelo ponto de vista de Orientação a objetos, concluimos que em termos genéricos fazem exatamente a mesma coisa: Abstrair conceitos para futura implementação.*

**Prover coesão, baixo acoplamento, independência de implementações para diferentes situações, abstração de conceitos.**

- *Uma propriedade é um cruzamento entre um campo e um método – ela parece um campo, mas atua como um método. Você acessa uma propriedade utilizando exatamente a mesma sintaxe empregada para acessar um campo. O compilador, porém, converte automaticamente essa sintaxe do tipo campo em chamadas a métodos de acesso(às vezes referidos como métodos de obtenção de propriedade e de definição de propriedade).*

- *Uma propriedade pode conter dois blocos de código, começando com as palavras- chave `get` e `set`. O bloco `get` contém instruções que são executadas quando a propriedade é lida e o bloco `set` engloba instruções que são executadas quando a propriedade é gravada.*

- *Ao utilizar uma propriedade em uma expressão, você pode empregá-la em um contexto de leitura(quando estiver recuperando seu valor) ou em um contexto de gravação(quando estiver modificando seu valor).*

- *As propriedades e os campos são acessados usando uma sintaxe idêntica. Quando você utiliza uma propriedade em um contexto de leitura, o compilador automaticamente traduz seu código do tipo campo em uma chamada ao método de acesso `get` dessa propriedade. Da mesma maneira, se você utilizar uma propriedade em um contexto de gravação, o compilador automaticamente traduzirá seu código do tipo campo em uma chamada para o método de acesso `set` dessa propriedade.*

- *Você pode declarar propriedades `static` assim como campos e métodos `static`. As propriedades estáticas são acessadas por meio do nome da classe ou estrutura, em vez de uma instância da classe ou estrutura.*

- *As propriedades somente-gravação são úteis para dados que devem ter segurança absoluta, como senhas. Teoricamente, um aplicativo que implementa segurança permitirá que você defina sua senha, mas nunca que você a leia. Ao tentar se conectar, o usuário poderá informar a senha. O método de logon pode comparar essa senha com a senha armazenada e retornar apenas uma indicação de uma possível combinação.*

- *Você pode especificar a acessibilidade de uma propriedade(`public`, `private` ou `protected`) ao declará-la. Mas também é possível, dentro da declaração da propriedade, redefinir sua acessibilidade para os métodos de acesso `get` e `set`.(Os métodos de acesso `get` são `public`, porque as propriedades são `public`.)*

- *Por isso, você deve definir estruturas e classes utilizando propriedades desde o início, em vez de usar campos que mais tarde migrarão para propriedades. O código que emprega suas classes e estruturas poderá não funcionar mais, se você transformar os campos em propriedades.*

- *Uma implementação explícita de uma propriedade é não pública e não `virtual`(e não pode ser redefinida).*

- *O principal propósito das propriedades é ocultar a implementação dos campos. Isso é bom se suas propriedades realizam algum trabalho útil, mas se os métodos de acesso `get` e `set` simplesmente envolvem operações que apenas leem ou atribuem um valor a um campo, você poderia questionar o valor dessa estratégia.*

- *Portanto, com o mínimo de esforço, você pode implementar uma propriedade simples utilizando um código gerado automaticamente; se precisar incluir uma lógica adicional posteriormente, você poderá fazer isso sem estragar aplicativos existentes. Você deve observar, porém, que com uma propriedade automaticamente gerada é necessário especificar um método de acesso `get` e um método de acesso `set` – uma propriedade automática não pode ser somente-leitura ou somente-gravação.*

```csharp
class Circle
{
    public int Radius{ get; set; }
}
```

```csharp
class Circle
{
    private int _radius;

    public int Radius {
        get { return this._radius; }
        set { this._radius = value; }
    }
}
```

- *Inicializador de objeto. Quando você chama um inicializador de objeto utilizando essa sintaxe, o compilador C# gera o código que chama o construtor padrão e então chama o método de acesso `set` de cada propriedade identificada para inicializá-la com o valor especificado. Você também pode especificar inicializadores de objeto em combinação com construtores não padrão.*

- *O construtor executa primeiro, e as propriedades são configuradas subsequentemente. Entender essa sequência é importante se o construtor configurar os campos em um objeto com valores específicos e se as propriedades que você especifica mudarem esses valores.*

- *Você também pode utilizar inicializadores de objeto com propriedades automáticas*

- *As propriedades são úteis para espelhar campos com um valor único. Já os indexadores são inestimáveis caso você queira fornecer acesso aos itens com múltiplos valores utilizando uma sintaxe natural e familiar.*

- *Considere um indexador um array inteligente quase como você considera uma propriedade um campo inteligente. Enquanto uma propriedade encapsula um único valor em uma classe, um indexador encapsula um conjunto de valores. A sintaxe utilizada para um indexador é a mesma utilizada para um array.*

- *Para definir o indexador, você utiliza uma notação que é um cruzamento entre uma propriedade e um array. Introduza o indexador com a palavra-chave `this`, especifique o tipo do valor retornado pelo indexador e o tipo do valor a ser utilizado como o índice para o indexador, entre colchetes.*

- *Quando você lê um indexador, o compilador traduz automaticamente seu código com lógica de arrays em uma chamada para o método de acesso `get` desse indexador. se você gravar em um indexador, o compilador traduzirá automaticamente seu código com lógica de array para uma chamada ao método de acesso `set` desse indexador, definindo o argumento index com o valor especificado entre colchetes.*

- *Você pode declarar um indexador que contém apenas um método de acesso `get`(um indexador somente-leitura) ou apenas um método de acesso `set`(um método de acesso somente-gravação).*

- *Os indexadores podem ser sobrecarregados(assim como os métodos), enquanto os arrays não podem. Os indexadores não podem ser utilizados como parâmetros `ref` ou `out`, enquanto os elementos do array podem.*

```csharp
struct Wrapper
{
    private int[] data;

    public int this [int i]
    {
        get { return this.data[i]; }
        set { this.data[i] = value; }
    }
}

Wrapper wrap = new Wrapper();

int[] myData = new int[2];

myData[0] = wrap[0];
myData[1] = wrap[1];
myData[0]++;
myData[1]++;
```

- *Você pode declarar indexadores em uma interface. Para fazer isso, especifique a palavra-chave `get`, a palavra-chave `set`, ou ambas, mas substitua o corpo do método `get` ou `set` por um ponto e vírgula. Toda classe ou estrutura que implementa a interface deve implementar os métodos de acesso indexadores declarados na interface.*

- *Se você implementar o indexador da interface em uma classe, poderá declarar as implementações do indexador como virtuais. Isso permite que outras classes derivadas redefinam os métodos de acesso `get` e `set`*

- *Você também pode optar por implementar um indexador utilizando a sintaxe de implementação explícita. Uma implementação explícita de um indexador é não pública e não `virtual`(e, portanto, não pode ser redefinida)*

- *Utilizar indexadores para fornecer acesso do tipo array aos dados em uma classe. Você aprendeu a criar indexadores que podem aceitar um índice e retornar o respectivo valor utilizando uma lógica definida pelo método de acesso `get`, e viu como utiliza o método de acesso `set` com um índice para preencher um valor em um indexador.*

- *Um método também pode retornar valores de qualquer tipo, especificando object como o tipo de retorno. Mesmo que essa prática seja muito flexível, deixa o programador responsável por lembrar quais tipos de dados estão sendo de fato utilizados. Isso pode levar a erros de tempo de execução, caso o programador cometa um engano.*

- *Lembre-se de que é possível utilizar o tipo `object` para referenciar um valor ou uma variável de qualquer tipo. Todos os tipos-referência herdam automaticamente(direta ou indiretamente) da classe `System.Object` no Microsoft .NET Framework(no C#, `object` é um alias para `System.Object`).*

- *Outra desvantagem de utilizar a estratégia object para criar classes e métodos generalizados é que ela pode consumir memória e tempo adicionais do processador, se o runtime precisar converter um object em um tipo-valor e vice-versa.*

- *O C# fornece genéricos para eliminar a necessidade de casting, melhorar a segurança, reduzir a quantidade de boxing necessária e facilitar a criação de classes e métodos generalizados. Classes e métodos genéricos aceitam parâmetros de tipo, que especificam os tipos dos objetos em que operam. No C#, você indica que uma classe é genérica fornecendo um parâmetro de tipo entre sinais de maior e menor.*

- *Você também pode definir estruturas e interfaces genéricas utilizando a mesma sintaxe de parâmetro de tipo das classes genéricas.*

- *Você pode pensar numa classe genérica como uma classe que define um template que é, então, utilizado pelo compilador para gerar novas classes de tipo específico sob demanda.*

- *Com um método genérico, é possível especificar os tipos dos parâmetros e o tipo de retorno utilizando um parâmetro de tipo de uma forma semelhante àquela empregada para definir uma classe genérica. Dessa maneira, você pode definir métodos generalizados que são seguros quanto ao tipo e evitar a sobrecarga do casting(e boxing, em alguns casos). Os métodos genéricos são frequentemente utilizados junto com as classes genéricas; você precisa delas para os métodos que recebem tipos genéricos como parâmetros ou que têm um tipo de retorno genérico.*

- *Os métodos genéricos são definidos utilizando-se a mesma sintaxe de parâmetro de tipo utilizada para criar classes genéricas. (Também é possível especificar restrições.).*

- *Lembre-se de que, segundo os termos da herança, a classe String é derivada da classe Object, de modo que todas as strings são objetos.*

- *Diz-se que a interface `IWrapper<T>` é invariável. Não é possível atribuir um objeto `IWrapper<A>` a uma referência de tipo `IWrapper<B>`, mesmo que o tipo `A` seja derivado do tipo `B`. Por padrão, o C# implementa essa restrição para garantir a segurançade tipos em seu código.*

- *Em situações dessa natureza, em que o parâmetro de tipo ocorre somente como valor de retorno dos métodos em uma interface genérica, você pode informar ao compilador que algumas conversões implícitas são válidas e que não é necessário impor uma segurança de tipos rigorosa. Para isso, especifique a palavra-chave `out` ao declarar o parâmetro de tipo*

- *Esse recurso é chamado de covariância. Você pode atribuir um objeto `IRetrieveWrapper<A>` a uma referência a `IRetrieveWrapper<B>`, desde que exista uma conversão válida do tipo `A` para o tipo `B`, ou o tipo `A` derive do tipo `B`.*

- *Só é possível especificar o qualificador out com um parâmetro de tipo se o parâmetro de tipo ocorrer como o tipo de retorno de métodos. Se você utilizar o parâmetro de tipo para especificar o tipo de qualquer parâmetro de método, o qualificador out será inválido e seu código não será compilado. Além disso, a covariância funciona apenas com tipos-referência. É por isso que os tipos-valor não podem formar hierarquias de herança.*

- *Somente tipos de interfaces e delegates podem ser declarados como covariantes. Você não especifica o modificador out com classes genéricas*

- *A contravariância segue um princípio semelhante ao da covariância, exceto pelo fato de que funciona na direção oposta; ela permite utilizar uma interface genérica para fazer referência a um objeto do tipo `B` por meio de uma referência ao tipo `A`, desde que o tipo `B` seja derivado do tipo `A`.*

- *Exemplo de covariância: Se os métodos de uma interface genérica puderem retornar strings, também poderão retornar objetos. (Todas as strings são objetos.)*

- *Exemplo de contravariância: Se os métodos de uma interface genérica puderem aceitar parâmetros de objeto, também poderão aceitar parâmetros de string. (Se você pode efetuar uma operação por meio de um objeto, poderá executar essa mesma operação por meio de uma string, porque todas as strings são objetos.)*

- *Como na covariância, somente interfaces e delegates podem ser declarados contravariantes. Você não especifica o modificador in com classes genéricas.*

- *O Microsoft .NET Framework fornece diversas classes que colecionam elementos para que um aplicativo possa acessá-los de maneiras especializadas. São as classes de coleção, e elas residem no namespace `System.Collections.Generic`. Conforme o namespace indica, essas coleções são tipos genéricos; todas elas esperam que você forneça um parâmetro de tipo indicando o tipo dos dados que seu aplicativo vai armazenar nelas. Cada classe de coleção é otimizada para uma forma específica de armazenamento e acesso aos dados, e cada uma fornece métodos especializados que suportam essa funcionalidade.*

- *Como com arrays, se utilizar foreach para iterar por uma coleção `List<T>`, você não poderá utilizar a variável de iteração para modificar o conteúdo da coleção. Além disso, você não pode chamar os métodos `Remove`, `Add` ou `Insert` em um loop foreach que itera por uma coleção `List<T>`; qualquer tentativa de fazer isso resultará em uma exceção `InvalidOperationException`.*

- *A maneira de determinar o número de elementos para uma coleção `List<T>` é diferente de consultar o número de itens em um array. Ao utilizar uma coleção `List<T>`, você examina a propriedade `Count` e, ao utilizar um array, examina a propriedade `Length`.*

- *A classe de coleção `LinkedList<T>` implementa uma lista duplamente encadeada. Cada item da lista armazena o valor do item, junto com uma referência para o próximo item da lista(a propriedade `Next`) e para o item anterior(a propriedade `Previous`). O item do início da lista tem a propriedade `Previous` configurada como `null` e o item do final da lista tem a propriedade `Next` configurada como `null`.*

- *A classe `SortedList<TKey, TValue>` é muito parecida com a classe `Dictionary<TKey, TValue>` no sentido de que é possível utilizá-la para associar chaves a valores. A principal diferença é que o array de chaves está sempre ordenado. (Afinal, ela se chama `SortedList`, ou seja, lista ordenada.) Na maioria dos casos, demora mais para inserir dados em um objeto `SortedList<TKey, TValue>` do que em um objeto `SortedDictionary<TKey,TValue>`, mas a recuperação de dados frequentemente é mais rápida(ou pelo menostão rápida quanto) e a classe `SortedList<TKey, TValue>` utiliza menos memória.*

- *Quando você insere um par chave/valor em uma coleção `SortedList<TKey, TValue>`, a chave é inserida no array de chaves no índice correto para manter as chaves ordenadas. O valor é então inserido no array de valores no mesmo índice. A classe `SortedList<TKey, TValue>` garante automaticamente que as chaves e valores mantenham o sincronismo, mesmo quando você adiciona e remove elementos. Isso significa que você pode inserir pares chave/valor em uma `SortedList<TKey, TValue>` em qualquer sequência; eles sempre são ordenados com base no valor das chaves.*

- *A classe `HashSet<T>` é otimizada para efetuar operações de conjunto, como por exemplo para determinar a participação como membro do conjunto e gerar a união e a interseção de conjuntos.*

- *Alguns tipos de coleção também podem ser inicializados ao serem declarados, utilizando uma sintaxe parecida com aquela suportada por arrays.*

- *Para coleções mais complexas que recebem pares chave/valor, como a classe `Dictionary<TKey, TValue>`, você pode especificar cada par chave/valor como um tipo anônimo na lista inicializadora.*

- *A maneira mais fácil de especificar o predicado é usando uma expressão lambda. Expressão lambda é uma expressão que retorna um método.*

- *Uma expressão lambda contém dois desses elementos: uma lista de parâmetros e um corpo de método. As expressões lambda não definem um nome de método e o tipo de retorno(se houver algum) é inferido do contexto em que a expressão lambda é utilizada.*

- *O construtor `foreach` fornece um mecanismo elegante que simplifica bastante o código que você precisa escrever, mas ele só pode ser executado sob certas circunstâncias – você só pode utilizar `foreach` para percorrer uma coleção enumerável. Então, o que é exatamente uma coleção enumerável? A resposta rápida é que é uma coleção que implementa a interface `System.Collections.IEnumerable`.*

- *Lembre-se de que todos os arrays no C# são, na verdade, instâncias da classe `System.Array`. A classe `System.Array` é uma classe de coleção que implementa a interface `IEnumerable`.*

- *Se você é observador, deve ter notado que a propriedade `Current` da interface `IEnumerator` exibe um comportamento não seguro(non-type-safe) tipado, porque retorna um object em vez de um tipo específico. Mas você deverá ficar contente de saber que a biblioteca de classes Microsoft .NET Framework também fornece a interface genérica `IEnumerator<T>`, que tem uma propriedade `Current` que retorna um `T`. Da mesma forma, também há uma interface `IEnumerable<T>` contendo um método Get-Enumerator que retorna um objeto `Enumerator<T>.` Essas duas interfaces são definidas no namespace `System.Collections.Generic`, e se estiver compilando aplicativos para o .NET Framework versão 2.0 ou superior, você deve fazer uso de interfaces genéricas ao definir coleções enumeráveis, em vez de utilizar a versão não genérica.*

- *Iterador é um bloco de código que produz uma sequência ordenada de valores. Um iterador não é realmente membro de uma classe enumerável; em vez disso, ele especifica a sequência que um enumerador utilizará para retornar os seus valores. Em outras palavras, um iterador é apenas uma descrição da sequência de enumeração que o compilador do C# pode usar para criar o seu próprio enumerador.*

- *A palavra-chave `yield` indica o valor que deve ser retornado por cada iteração. Se isso ajudar, você pode imaginar a instrução `yield` como a execução de uma parada temporária para o método, passando um valor de volta para o chamador. Quando o chamador precisar do valor seguinte, o método `GetEnumerator` continuará do ponto em que parou, fazendo um loop e produzindo o próximo valor. Por fim, os dados são esgotados, o loop termina e o método `GetEnumerator` se encerra. Nesse ponto a iteração estará completa.*

- *As interfaces `IEnumerable<T>` e `IEnumerator<T>` com uma classe de coleção, para permitir que os aplicativos interagissem por meio dos itens contidos na mesma coleção.*

- *Um tipo delegate cria uma abstração de uma assinatura de método.*

- *Os delegates fornecem a solução ideal, tornando possível desacoplar completamente a lógica do aplicativo em seus métodos dos aplicativos que os chamam.*

- *Em conjunto com os delegates, os eventos fornecem a infraestrutura com a qual é possível implementar sistemas que seguem essa estratégia.*

- *Delegate é uma referência para um método. Trata-se de um conceito muito simples com implicações extraordinariamente poderosas. Os delegates recebem esse nome porque, quando são chamados, “delegam” o processamento para o método referenciado.*

- *Um delegate é um objeto que faz referência a um método. Você pode atribuir uma referência a um método para um delegate da mesma maneira como pode atribuir um valor `int` a uma variável `int`.*

- *É importante entender que a instrução que atribui a referência de método ao delegate não executa o método nesse ponto; não existem parênteses após o nome do método, e você não especifica nenhum parâmetro(se o método os recebe). Trata-se apenas de uma instrução de atribuição.*

- *Um delegate pode referenciar mais de um método por vez(pense nisso como uma coleção de referências de método) e, quando você chamá-lo, todos os métodos aos quais ele faz referência serão executados.*

- *Você somente pode fazer um delegate referenciar um método que corresponda à assinatura do delegate e não pode chamar um delegate que não referencie um método válido.*

- *Um predicado é apenas um delegate que retorna um valor booleano.*

- *O parâmetro recebido pelos métodos `Average`, `Max`, `Count` e outros da classe `List<T>` são delegates `Func<T, TResult>` genéricos; os parâmetros de tipo fazem referência ao tipo do parâmetro passado para o delegate e ao tipo do valor de retorno.*

- *Um delegate `Func` referencia um método que retorna um valor(uma função).*

- *Um delegate `Action` é utilizado para fazer referência a um método que executa uma ação, em vez de retornar um valor(um método `void`).*

- *Para chamar um delegate, utilize a mesma sintaxe utilizada para fazer uma chamada de método. Se o método que o delegate referencia receber parâmetros, você deve especificá-los nesse momento, entre os parênteses.*

- *Se tentar chamar um delegate que não foi inicializado e que não referencia método algum, você receberá uma exceção `NullReferenceException`.*
