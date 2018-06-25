# Anotações `C#`

- **Os programas C# são verificados de forma estática(em tempo de compilação) e em tempo de execução(pelo CLR)**.

- Classes estáticas tem todos os seus membros estáticos.

- Os membros públicos "encapsulam" os membros privados da classe.

- A diferença fundamental entre tipos de "valor" e tipos de "referência" é como eles são manipulados na memória.
    - *Valor*: Faz uma cópia da instância.
    - *Referência*: Copia a referência, não a instância do objeto. Isso permite que várias variáveis ​​se referem ao mesmo objeto.

- Uma vez que uma matriz foi criada seu comprimento(Length) não pode ser alterado.

- Um parâmetro pode ser passado por referência ou por valor, independentemente de o tipo de parâmetro ser um tipo de referência ou um tipo de valor.

- Um argumento `out` é como um argumento `ref`, exceto:
    - Não precisa ser atribuído antes de entrar na função.
    - Deve ser atribuído antes de sair da função.

- O modificador `out` é mais utilizado para obter vários valores de retorno de um método.

- Os parâmetros obrigatórios devem ocorrer antes dos parâmetros opcionais tanto na declaração do método como na chamada do método(a exceção é com o argumento `params`, que sempre são os últimos).

- Os argumentos nomeados podem ocorrer em qualquer ordem.

- pode misturar argumentos nomeados e posicionais. No entanto, existe uma restrição: os argumentos posicionais devem vir antes dos argumentos nomeados.

- Se a classe não estiver aninhada dentro de outra classe, o modificador de acesso padrão é `internal`. Se a classe estiver aninhada dentro de outra classe, o especificador de acesso padrão é `private`.

- Um tipo pode sobrecarregar métodos(tem vários métodos com o mesmo nome), desde que as assinaturas sejam diferentes.

- Um tipo de retorno de um método não faz parte da assinatura do método para fins de sobrecarga de método. No entanto, ele faz parte da assinatura do método ao determinar a compatibilidade entre um delegate e o método para o qual ele aponta.

- As inicializações das propriedades ocorrem antes que o construtor seja executado e na ordem de suas declarações.

- The `this` reference refers to the instance itself.

- A referência `this` é válido somente dentro membros não-estáticos de uma classe ou struct.

- Embora as propriedades sejam acessadas da mesma forma que os campos, eles diferem em que dão ao implementador um controle completo sobre obter e definir seu valor. Este controle permite o implementador de escolher qualquer representação interna é necessária, sem expor os detalhes internos para o usuário da propriedade.

- Uma constante é declarada com a palavra-chave `const` e deve ser inicializada com um valor.

- Um construtor estático executa uma vez por tipo, em vez de uma vez por instância. Um tipo pode definir apenas um construtor estático, e deve ser sem parâmetros e ter o mesmo nome do tipo.

- Os inicializadores de campos estáticos são executados antes da chamado do método construtor estático.

- Os inicializadores de campos estáticos são executados na ordem em que os campos são declarados.

- Uma classe pode ser marcada como estática, indicando que ela deve ser composta unicamente de membros estáticos e não pode ser subclasses. As classes `System.Console` e `System.Math` são bons exemplos de classes estáticas.

- Os tipos(Classes) parciais permitem que uma definição de tipo seja dividida tipicamente em vários arquivos.

- Um método parcial consiste em duas partes: uma definição e uma implementação.

- Os métodos parciais devem ser `void` e são implicitamente privados.

- Uma classe derivada também é chamada de subclasse. Uma classe base também é chamada de superclasse.

- Uma classe declarada como abstrata nunca pode ser instanciada. Em vez disso, apenas suas subclasses concretas podem ser instanciadas.

- Os construtores da classe base sempre executam primeiro; Isso garante que a inicialização da base ocorre antes da inicialização da classe especializada.

- Quando um métode de sobrecarga é executado, o tipo mais específico tem precedência.

- Uma interface é semelhante a uma classe, mas fornece uma especificação em vez de uma implementação para os seus membros.

- Os membros da interface são todos implicitamente abstratos. Em contraste, uma classe pode fornecer membros abstratos e membros concretos com implementações.

- Membros de interface são sempre implicitamente público e não pode declarar um modificador de acesso. Implementar uma interface significa fornecer uma implementação pública para todos os seus membros.

- Os genéricos expressam a reutilização com um "modelo".

- Existem genéricos para escrever um código reutilizável em diferentes tipos.

- Um `delegate` é um objeto que sabe como chamar um método.

- Um tipo de `delegate` define o tipo de método que as instâncias delegadas podem chamar.

- Um upcast sempre é bem-sucedido; Um downcast só é bem sucedido se o objeto for apropriado.

- Uma operação de upcast cria uma referência de classe base a partir de uma referência de subclasse. Upcast referência ainda o mesmo objeto. O objeto a ser referenciado não é alterado ou convertido. O objeto depois de realizado o upcast tem uma visão mais restritiva sobre esse objeto(Original).

- Uma operação de downcast cria uma referência de subclasse a partir de uma referência de classe base. Se um downcast falhar, uma `InvalidCastException` é lançado.

- O operador `as` avalia downcast como `null` (em vez de lançar uma exceção) se o downcast falhar.

- O operador `is` se testar se uma conversão de referência seria bem sucedida; Em outras palavras, se um objeto deriva de uma classe especificada(ou implementa uma interface).

- Boxing é o ato de converter uma instância de tipo de valor em uma instância de tipo de referência.

- Unboxing inverte a operação ao converter o objeto de volta para o tipo de valor original.

- O Boxing copia a instância do tipo de valor no novo objeto e o unboxing copia o conteúdo do objeto de volta para uma instância de tipo de valor.

- Os programas C# são verificados de forma estática(no tempo de compilação) e no tempo de execução(pelo CLR).

- Uma estrutura é um tipo de valor, enquanto uma classe é um tipo de referência.

- Uma estrutura pode ter todos os membros que uma classe pode, exceto o seguinte:
    - Um construtor sem parâmetros.
    - Inicializadores de campo.
    - Um finalizador.
    - Membros virtuais ou protegidos.

- Por convenção, parâmetros, variáveis ​​locais e campos privados devem estar em **camel case**(por exemplo, *myVariable*) e todos os outros identificadores devem estar em **Pascal case**(por exemplo, *MyMethod*).

- **Literals** are primitive pieces of data lexically embedded into the program.

- All values in C# are instances of a *type*. The meaning of a value, and the set of possible values a variable can have, is determined by its type.

- A sintaxe `C#` é inspirada pela sintaxe `C` e `C++`.

- Data is created by **instantiating** a *type*. Predefined types can be instantiated simply by using a literal such as 12 or "Hello world". The new operator creates instances of a custom type.

- Immediately after the **new** operator instantiates an object, the object’s constructor is called to perform initialization. A constructor is defined like a method, except that the method name and return type are reduced to the name of the enclosing type.

- A palavra `this` referência é válido somente dentro de membros não-estáticos de uma classe ou struct.

- Quando uma **overload**(sobrecarga) é chamada, o tipo mais específico tem precedência.

- **GetType** is evaluated at runtime; **typeof** is evaluated statically at compile time(when generic type parameters are involved, it’s resolved by the just-in-time compiler).

- The data members and function members that operate on the **instance** of the type are called instance members. By default, members are instance members.

- Conversões `implícitas` são permitidas quando ambos os seguintes são verdadeiros:
    - O compilador pode garantir que eles sempre terão sucesso.
    - Nenhuma informação é perdida na conversão.
- Conversões `explícitas` são necessárias quando uma das seguintes condições é verdadeira:
    - O compilador não pode garantir que eles sempre terão sucesso.
    - As informações podem ser perdidas durante a conversão.

- All C# types fall into the following categories:
    - **Value types**
        - Os tipos de valor compreendem a maioria dos tipos incorporados(especificamente, todos os tipos `numeric`, o tipo `char` e o tipo `bool`), bem como tipos de struct e enum personalizados .
    - **Reference types**
        - Os tipos de referência compreendem todos os tipos de `class`, `array`, `delegate` e interface.(This includes the predefined string type).
    - Generic type parameters.
    - Pointer types.

- A diferença fundamental entre tipos de valor e tipos de referência é como eles são manipulados na memória.

- The assignment of a value-type instance always copies the instance.

- Um tipo de referência é mais complexo que um tipo de valor, com duas partes: um objeto e a referência a esse objeto. **O conteúdo de uma variável ou constante de tipo de referência é uma referência a um objeto que contém o valor**.

- Atribuir uma **variável de tipo de referência copia a referência, não a instância do objeto**. Isso permite que várias variáveis ​​se referem ao mesmo objeto algo normalmente não possível com tipos de valor.

- Uma referência pode ser atribuída como **null** *literal*, indicando que a referência aponta para nenhum objeto.

- As instâncias do tipo de valor ocupam precisamente a memória necessária para armazenas seus campos.

- Os tipos de referência requerem alocações separadas de memória para referência e objeto.

- Numeric suffixes explicitly define the type of a literal. Suffixes can be either lower or uppercase.

- O *conditional operator*(mais comumente chamado de  *ternary operator*, pois é o único operador que leva três operandos) tem a forma `q ? a : b`, onde se a condição `q` é verdade, `a` é avaliado, senão `b` é avaliado.

- Um literal de *string* é representado dentro de aspas duplas.

- *String* é um tipo de **referência**, em vez de um tipo de valor. Seus operadores de igualdade, no entanto, seguem a semântica do tipo de valor:

    ```csharp
    string a = "test";
    string b = "test";
    Console.Write(a == b); // True
    ```

- C# allows **verbatim string literals**. A verbatim string literal is prefixed with `@` and does not support escape sequences. A verbatim string literal can also span multiple lines.

    ```csharp
    string path = @ "\\server\fileshare\helloworld.cs";

    string escaped = "First Line\r\nSecond Line";
    string verbatim = @"First Line
    Second Line";

    // True if your IDE uses CR-LF line separators:
    Console.WriteLine(escaped == verbatim);

    /* You can include the double-quote character
    in a verbatim literal by writing it twice */
    string xml = @"<customer id=""123""></customer>";
    ```

- Uma seqüência de caracteres precedida com o caractere `$` é chamada de **interpolated string**. As strings interpoladas podem incluir expressões dentro das chaves. Qualquer expressão `C#` válida de qualquer tipo pode aparecer nas chaves que converterá a expressão para uma string chamando seu método `ToString` ou equivalente.

    ```csharp
    int x = 4;
    Console.Write($"A square has {x} sides");

    int y = 2;
    string s = $@"this spans {
    x} lines";
    ```

- Um **array** representa um número fixo de variáveis(chamados elementos) de um tipo específico. Os elementos em uma matriz são sempre armazenados em um bloco contíguo de memória, proporcionando acesso altamente eficiente.
    - Uma matriz é indicada com colchetes após o tipo de elemento.
    - Os colchetes também indexam a matriz, acessando um determinado elemento por posição.
    - Uma vez que uma matriz foi criada seu comprimento não pode ser alterado.

    ```csharp
    char[] vowels = new char[5]; // Declare an array of 5 characters

    vowels[0] = 'a';
    vowels[1] = 'e';
    vowels[2] = 'i';
    vowels[3] = 'o';
    vowels[4] = 'u';

    Console.WriteLine(vowels[1]); // e
    ```

    - Uma **initialization expression** de matriz permite que você declare e preencha uma matriz em uma única etapa. Para que isso funcione, os elementos devem ser completamente convertidos em um único tipo(e pelo menos um dos elementos deve ser desse tipo e deve haver exatamente um melhor tipo).

    ```csharp
    char[] vowels = new char[] {'a', 'e', 'i', 'o', 'u'};

    // or

    char[] vowels = {'a', 'e', 'i', 'o', 'u'}; // Compiler infers char[]
    ```

    - Uma matriz em si é sempre um objeto de tipo de referência, independentemente do tipo de elemento.  Por exemplo, o seguinte é legal:

    ```csharp
    int[] a = null;
    ```

- A **stack**(Pilha) é um bloco de memória para armazenar variáveis e parâmetros locais. A pilha cresce logicamente e diminui quando uma função é inserida e encerrada.

- O **Heap** é um bloco de memória em que os objetos(ou seja, exemplos de tipo de referência) residem. Sempre que um novo objeto é criado, ele é alocado no heap e uma referência a esse objeto é retornada. Durante a execução de um programa, o heap começa a se preencher à medida que novos objetos são criados. O tempo de execução tem um coletor de lixo que periodicamente localiza objetos do heap, então seu programa não fica sem memória. Um objeto é elegível para desalocação, logo que não seja referenciado por qualquer coisa que esteja "alive".

- O Heap também armazena campos estáticos. Ao contrário dos objetos alocados no heap(que podem ser coletados), estes vivem até que o domínio do aplicativo seja derrubado.

- **Definite Assignment**.
    - As variáveis locais devem ter um valor antes de serem lidas.
    - Os argumentos de função devem ser fornecidos quando um método é chamado(a menos que seja marcado como opcional).
    - Todas as outras variáveis(como campos e elementos de matriz) são automaticamente inicializadas em tempo de execução.

- Todas as instâncias de tipo têm um valor padrão. O valor padrão para os tipos predefinidos é o resultado de uma redução zero na hora da memória:
    - You can obtain the default value for any type with the `default` keyword.

    | **Type**                       | **Default value** |
    | ------------------------------ | :---------------: |
    | All *reference* types          | _null_            |
    | All *numeric* and *enum* types | 0                 |
    | *char* type                    | '\0'              |
    | *bool* type                    | false             |

- Você pode controlar como os parâmetros são passados com os modificadores de `ref` e `out`:

    | Parameter modifier | Passed by | Variable must be definitely assigned |
    | ------------------ | --------- | ------------------------------------ |
    | (None)             | Value     | Going in                             |
    | **ref**            | Reference | Going in                             |
    | **out**            | Reference | Going out                            |

- Por padrão, os argumentos em `C#` são passados ​​por valor. Isso significa que uma **cópia do valor é criada** quando passou para o método.
    - Residem em diferentes locais da memória.
    - Passar um argumento de tipo de referência por valor copia a referência, mas não o objeto.

    ```csharp
    class Test
    {
        static void Foo(StringBuilder fooSB)
        {
            fooSB.Append("test");
            fooSB = null;
        }

        static void Main()
        {
            StringBuilder sb = new StringBuilder();
            Foo(sb);
            Console.WriteLine(sb.ToString()); // test
        }
    }
    ```
    > Como `fooSB` é uma *cópia* de uma referência, configurá-la como **null** não faz `sb` null. (No entanto, se `fooSB` foi declarado e chamado com o modificador de referência, o `sb` se tornaria null.)

- Para passar o valor como referência `C#` fornece o modificador de parâmetro `ref`, que deve ser usado tanto na assinatura quando na implementação.

- Um modificador de parâmetro `out` é semelhante ao `ref` exceto:
    - Não precisa ser atribuído antes de entrar na função.
    -  Deve ser atribuído antes que ele `out`(saia) da função.

- O modificador de `out` é mais utilizado para obter vários valores de retorno de um método. Como um parâmetro `ref`, um parâmetro `out` é passado por referência.

- Um parâmetro pode ser passado por referência ou por valor, independentemente de o tipo de parâmetro ser um tipo de referência ou um tipo de valor.

- O modificador do parâmetro `params` pode ser especificado no último parâmetro de um método para que o método **aceite qualquer número de argumentos de um tipo específico**. O tipo de parâmetro deve ser declarado como uma matriz.

- A partir do `C#` 4.0, métodos, construtores e indexadores podem declarar **optional parameters**.
    - Um parâmetro é opcional se especifica um valor padrão em sua declaração.
    - Podem ser omitidos ao chamar o método.
    - Não podem ser marcados por argumentos de referência(`ref` ou `out`).
    - Os *parâmetros obrigatórios devem ocorrer antes dos parâmetros opcionais*, tanto na declaração do método como na chamada do método(a exceção é com os argumentos `params`, que sempre são os últimos).

- Em vez de identificar um argumento por posição, você pode identificar um argumento por nome(Named arguments).
    - Argumentos nomeados podem ocorrer em qualquer ordem.
    - Pode misturar argumentos nomeados e posicionais com uma restrição: Os argumentos posicionais devem vir antes dos argumentos nomeados.

- `var` - Implicitly Typed Local Variables - Se o compilador for capaz de inferir o tipo da expressão de inicialização, você pode usar a palavra-chave `var`(introduzida em `C#` 3.0) no lugar da declaração de tipo.

- Uma `expression` essencialmente denota um valor. Os tipos mais simples de expressões são constantes e variáveis. As expressões podem ser transformadas e combinadas usando operadores. Um `operator` leva um ou mais operandos de entrada para produzir uma nova expressão.

- Quando uma expressão contém vários operadores, a `precedence` e a `associativity` determinam a ordem de sua avaliação. Os operadores com maior precedência são executados antes dos operadores de menor precedência. Se os operadores tiverem a mesma precedência, a associatividade do operador determina a ordem de avaliação.

- Os *assignment operators*(Operadores de atribuição), lambda, null-coalescing e operador condicional são *right-associative*(Associativos direito); Em outras palavras, são avaliadas da direita para a esquerda. A associatividade direta permite atribuições múltiplas.

- O `??` operador é o *null-coalescing operator*. Diz "Se o operando não é null, dê-me; Caso contrário, me dê um valor padrão. ". Se a expressão esquerda não for nula, a expressão à direita nunca será avaliada.

    ```csharp
    string x = null;
    string y = x ?? "nothing"; // y evaluates to "nothing"
    ```

- O `?.` O operador é o operador null-conditional e é novo no `C#` 6. Ele permite chamar métodos e acessar membros exatamente como o operador de ponto padrão, exceto que, se o operando à esquerda for null, a expressão é avaliada para `null` em vez de jogar uma `NullReferenceException`:

    ```csharp
    System.Text.StringBuilder sb = null;
    string s = sb?.ToString(); // No error; s instead evaluates to null
    ```

    The last line is equivalent to:

    ```csharp
    string s = (sb == null ? null : sb.ToString());
    ```

    Ao encontrar um null, o operador interrompe o restante da expressão:

    ```csharp
    System.Text.StringBuilder sb = null;
    string s = sb?.ToString().ToUpper(); // s evaluates to null without error
    ```

    O uso repetido do operador é necessário somente se o operando imediatamente a sua esquerda pode ser null. A seguinte expressão é robusta, sendo x null e x.y sendo null:

    ```csharp
    x?.y?.z

    // and is equivalent to the following
    // (except that x.y is evaluated only once)

    x == null ? null : (x.y == null ? null : x.y.z)
    ```

    Com tipos *nullable*(anuláveis) e com null-coalescing operator:

    ```csharp
    System.Text.StringBuilder sb = null;
    int length = sb?.ToString().Length; // Illegal : int cannot be null

    int? length = sb?.ToString().Length; // OK : int? can be null
    ```

    ```csharp
    System.Text.StringBuilder sb = null;
    string s = sb?.ToString() ?? "nothing"; // s evaluates to "nothing"

    // The last line is equivalent to:

    string s = (sb == null ? "nothing" : sb.ToString());
    ```

- O escopo de uma variável local ou constante local se estende por todo o bloco atual. Você não pode declarar outra variável local com o mesmo nome no bloco atual ou em qualquer bloco aninhado.

- A instrução `foreach` itera sobre cada elemento em um objeto `enumerable`. A maioria dos tipos em C# e o *.NET Framework* que representam um conjunto ou lista de elementos são enumeráveis. Por exemplo, tanto uma matriz como uma string são enumeráveis.

- The `using` directive is different from the `using` statement.

- From C# 6, you can import not just a namespace, but a specific type, with the using static directive. All static members of that type can then be used without being qualified with the type name.

    ```csharp
    using PropertyInfo2 = System.Reflection.PropertyInfo;
    class Program { PropertyInfo2 p; }

    // An entire namespace can be aliased, as follows:

    using R = System.Reflection;
    class Program { R.PropertyInfo p; }
    ```

- Namespace Extern + Namespace alias qualifiers.

- Use classes e subclasses para tipos que naturalmente compartilham uma implementação.

- Use interfaces para tipos que tenham implementações independentes.
