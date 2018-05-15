# Tests double

Em **Testes Unitários**, a classe que você está testando é chamada de `System Under Test` (`SUT`). Existem poucas classes que operam inteiramente isoladas. Geralmente, eles precisam de outras classes ou objetos para funcionar, sejam injetados por meio do construtor ou passados ​​como parâmetros do método. Estes são conhecidos como **Collaborators**. Quando falamos de *Collaborators* no contexto de testes unitários, estamos falando especificamente sobre *Collaborators* para o *System Under Test*.

Quando a maioria das pessoas fala sobre o **Mocks**, o que eles estão realmente se referindo são **Test Doubles**. Um *Test Doubles* é simplesmente outro objeto que está em conformidade com a interface do *Collaborator* e pode ser passado em seu lugar. Existem vários tipos diferentes de *Test Doubles*, dos quais um **Mock** é apenas um. Grande parte da confusão surge quando "**Mocking Frameworks**" pode gerar numerosos tipos diferentes de *Test Double*, assim como *Mocks*.

## Dummy

> Um **Dummy** é utilizado para substituir um objeto quando você não precisa se importar como ele será utilizado. Geralmente ele é utilizado para melhorar a legibilidade, já que *não possuem nenhum comportamento*. Um **Dummy** é um objeto que simplesmente implementa uma interface e não faz mais nada. Não se destina a ser usado em seus testes e *não terá efeito sobre o comportamento*.

Um exemplo seria passar um objeto para um construtor que não é usado na trajetória do tempo de vida do objeto `SUT`, ou um objeto simples para adicionar a uma coleção. Por exemplo:

```csharp
private class FooDummy : Foo
{
    public string bar()
    {
        return null;
    }
}

public class FooCollectionTest
{
    [Test]
    public void it_should_maintain_a_count()
    {
        FooCollection sut = new FooCollection();

        sut.add(newFooDummy);
        sut.add(newFooDummy);

        assertEquals(2, sut.count());
    }
}
```

##  Stubs

> Os **Stubs** são simples e diretos - eles são essencialmente a implementação mais simples possível de um método e retornam os mesmos dados em todas as vezes.

> A função do **Stub** de teste é retornar valores controlados para o objeto que está sendo testado. Só retornam o que lhes pede para retornar durante o teste.

> Utilizamos um **Stub** quando não precisamos nos preocupar com a execução de um método, apenas com o resultado que ele retorna. fornecem respostas prontas para as chamadas feitas durante o teste.

> **Stubs** são programas de computador que atuam como substitutos temporários para um módulo chamado e fornecem a mesma saída que o produto ou software real.

Pense em um **Stub** como um passo acima de um **Dummy**. Ele implementa uma interface necessária, mas em vez de não ter lógica ou retornar `null/void` no corpo do método, geralmente retorna respostas prontas para que você faça afirmações na saída do `SUT` (**System Under Test**).

```csharp
private class FooStub : Foo
{
    public string bar()
    {
        return "baz";
    }
}

public class FooCollectionTest
{
    [Test]
    public void it_should_return_joined_bars()
    {
        FooCollection sut = new FooCollection();

        sut.add(newFooStub);
        sut.add(newFooStub);

        assertEquals("bazbaz", sut.joined());
    }
}
```

### `CQRS` (Command Query Responsibility Segregation)

Métodos que retornam algum resultado e não alteram o estado do sistema, são chamados de **Query**. Retorna um valor e é livre de efeitos colaterais. Para testar esse tipo de método, usamos **Stubs**. Estamos substituindo a funcionalidade real para fornecer valores necessários para o método executar seu trabalho. Em seguida, os valores retornados pelo método podem ser usados ​​para asserções.

Há também outra categoria de métodos chamada **Command**. É quando um método executa algumas ações, que alteram o estado do sistema, mas não esperamos nenhum valor de retorno dele.

Para testar os métodos de tipo de consulta, devemos preferir o uso de **Stubs**, pois podemos verificar o valor de retorno do método. Mas e o tipo de comando de métodos, como o método de envio de um e-mail? Como testá-los quando eles não retornam nenhum valor? A resposta é **Mock** - o último tipo de *test double* que vamos cobrir.

##  Spy

> Um **Spy** é utilizado quando queremos validar que métodos foram chamados, quantas vezes, com quais argumentos e quaisquer outras informações que nos interesse. Em outras palavras, ele rastreia como o código foi executado em vez do que ele fez.

> **Spies** são usados ​​exclusivamente para verificação de comportamento.

Na prática utilizamos na maioria das vezes *Stubs*, para apenas substituir o comportamento de um método por seu valor fixo, ou um *Spy*, para avaliar informação relacionadas a chamadas de método. Como o próprio nome já diz, eles gravam informações sobre o comportamento do que está sendo "*espionado*".

Você usa um **Spy** quando quer ter certeza de que um método foi chamado pelo seu sistema. Ele também pode registrar todos os tipos de coisas, como contar o número de invocações ou manter um registro dos argumentos transmitidos a cada vez.

Um *Stub* também pode ser usado para manter o estado e fazer asserções, quando um Stub faz isso também é conhecido como um **Spy de Teste**. Por exemplo, verificando o conteúdo de um parâmetro ou o número total de vezes que um método é chamado:

```csharp
private class ThirdPartyApiSpy : ThirdPartyApi
{
    public int callCount = 0;

    public bool hasMore(Response previousResponse)
    {
        if ((this.callCount == 0)) {
            return true;
        }

        return false;
    }

    public Response get(int page)
    {
        this.callCount++;

        return new DummyResponse();
    }
}

public class ApiConsumerTest
{

    [Test]
    public void it_should_get_all_pages()
    {
        ThirdPartyApiSpy spy = new ThirdPartyApiSpy();
        ApiConsumer sut = new ApiConsumer(spy);

        sut.fetchAll();

        assertEquals(2, spy.callCount);
    }
}
```

##  Fake

> Semelhante ao *Dummy*, o **Fake** é utilizado para substituir objetos no entanto ele possui uma determinada lógica que é será utilizada em diferentes cenários de teste. É uma implementação leve de uma API que se comporta como a implementação real.

Semelhante a um *Stub* sendo um passo acima de um *Dummy*, a maneira mais simples de pensar em um **Fake** é como um passo acima de um *Stub*. Isso significa não apenas retornar valores, mas também *funciona exatamente como um verdadeiro colaborador*. Normalmente, um **Fake** tem algum tipo de atalho, o que significa que eles são inadequados para produção. Um bom exemplo disso seria um objeto *InMemoryRepository*, com uma contraparte "real" que persiste os dados:

```csharp
private class InMemoryUserRepository : UserRepository
{
    private UserCollection users = new UserCollection();

    public User load(UserIdentifier identifier)
    {
        if (!this.users.exists(identifier)) {
            throw;
        }

        return this.users.get(identifier);
    }

    public User find(UserIdentifier identifier)
    {
        if (!this.users.exists(identifier)) {
            return null;
        }

        return this.users.get(identifier);
    }

    public UserCollection fetchAll()
    {
        return this.users;
    }

    public bool add(User user)
    {
        return this.users.add(user);
    }

    public bool delete(User user)
    {
        return this.delete(user.getIdentifier());
    }

    public bool delete(UserIdentifier identifier)
    {
        return this.users.remove(identifier);
    }
}

public class CreateUserServiceTest
{

    [Test]
    public void it_should_save_a_new_user()
    {
        UserRepository userRepository = new InMemoryUserRepository();
        CreateUserService sut = new CreateUserService(userRepository);

        sut.createUser(new UserRequestStub());

        assertEquals(new UserCollectionStub(), userRepository.fetchAll());
    }
}
```

##  Mocks

> **Martin Fowler** describes mocks as “objects pre-programmed with expectations which form a specification of the calls they are expected to receive”. Where as **Uncle Bob** says that a mock spies on the behavior of the module being tested and knows what behavior to expect.

> São objetos que **simulam** o comportamento de **objetos** reais de *forma*  **controlada**. São normalmente criados para testar o comportamento de outros objetos. Em outras palavras, os objetos **mock** são objetos **falsos** que **simulam** o comportamento de uma classe ou objeto *real* para que possamos focar o teste na unidade a ser testada.

> Assim como o *Spy*, o **Mock** também valida métodos que foram chamados. A diferença é bem sútil, um **Mock** sabe o que deve ser testado, ao contrário do *Spy* que apenas guarda informação da execução.

> **Mocks** são usados ​​para verificar o comportamento do objeto durante um teste. Por comportamento de objetos, quer dizer que verificamos se os métodos e caminhos corretos são exercidos no objeto quando o teste é executado.

*Mock objects have the same interface as the real objects they mimic, allowing a client object to remain unaware of whether it is using a real object or a mock object. Many available mock object frameworks allow the programmer to specify which, and in what order, methods will be invoked on a mock object and what parameters will be passed to them, as well as what values will be returned. Thus, the behavior of a complex object such as a network socket can be mimicked by a mock object, allowing the programmer to discover whether the object being tested responds appropriately to the wide variety of states such mock objects may be in.*

Whilst all of the other types of Test Double build on each other, gradually adding more functionality, Mocks are something totally different. Until now, all of our test doubles have made assertions on state. Mocks are test doubles that make assertions on behaviour. Mocks say "I expect you to call foo() with bar, and if you don't there's an error".

Typically, using a Mock will consist of three phases:

```
// Arrange
// Act
// Assert
```

1. Creating the instance
2. Defining the behaviour
3. Asserting the calls

Taking our *Stub* example from above, we could turn that into a **Mock** like so:

```csharp
public class FooCollectionTest
{
    [Test()]
    public void it_should_return_joined_bars()
    {
        Foo fooMock = mock(Foo.class); // Instance
        when(fooMock.bar()).thenReturn("baz", "qux"); // Behaviour

        FooCollection sut = new FooCollection();
        sut.add(fooMock);
        sut.add(fooMock);

        assertEquals("bazqux", sut.joined());
        verify(fooMock, times(2)).bar(); // Verify
    }
}
```

If we left out the call to `verify()`, the above example is still a *Stub*, even though it's using a mocking framework. The difference here is so subtle it's no wonder it leads to confusion. `fooMock` doesn't become a Mock until we call verify at the end, as that's the key differentiator between a Mock and the other types of Test Doubles.

With the *Stub* we only assert on the state of the object, but with the Mock we assert that the correct methods were called the correct number of times (and sometimes in the correct order and with the correct parameters).

--------------------------

Referências:
http://xunitpatterns.com/Test%20Double.html
http://xunitpatterns.com/Mock%20Object.html
http://xunitpatterns.com/Test%20Stub.html
http://xunitpatterns.com/Dummy%20Object.html
http://xunitpatterns.com/Fake%20Object.html
http://xunitpatterns.com/Test%20Spy.html
http://xunitpatterns.com/Mocks,%20Fakes,%20Stubs%20and%20Dummies.html
https://martinfowler.com/bliki/TestDouble.html
https://martinfowler.com/articles/mocksArentStubs.html
https://www.infoq.com/br/articles/mocks-Arent-Stubs
https://medium.com/trainingcenter/testes-unit%C3%A1rios-mocks-stubs-spies-e-todas-essas-palavras-dif%C3%ADceis-f2765ac87cc8
https://brizeno.wordpress.com/2016/09/28/aprendendo-tdd-mais-sobre-dubles-de-teste/
https://8thlight.com/blog/uncle-bob/2014/05/14/TheLittleMocker.html
https://pt.stackoverflow.com/questions/36745/qual-a-diferen%C3%A7a-entre-mock-stub
