<font face="Calibri">

# ⭐ Estrutura

[`⬆️ inicio`](../../../Readme.md)

[`◀️ voltar`](../../Readme.md)

---

## Tipos de Testes

+ Testes de unidade
  + são testes que tem por objetivo testar as menores unidades de um código, as classes;
+ Testes de integração
  + .
+ Testes de sistema
  + .
+ Testes de interface
  + Selenium.

*xUnit é um dos mais utilizados frameworks de testes em .NET do mercado.*
*Nem sempre precisaremos testar todas as funcionalidades desenvolvidas em uma aplicação.*
*Testar tudo que é possível em um software é tentador, mas o importante é guiar o desenvolvimento e os testes para funcionalidades de maior valor para nosso cliente.*

## Padrão AAA

+ Arrange
  + Preparação do ambiente e cenário, instanciar objetos e inicializar variáveis...
+ Act
  + Execução do método a testar.
+ Assert
  + Verificação do resultado obtido na execução do método.

Exemplo 1:

```csharp
[Fact]
public void TestaVeiculoAcelerar()
{
    // arrange
    var veiculo = new Veiculo();

    // act
    veiculo.Acelerar(10);

    // assert
    Assert.Equal(100, veiculo.VelocidadeAtual);
}
```

Exemplo 2:

```csharp
[Fact]
public void DataTarefaComInfoValidasDeveIncluirNoBD()
{
    /* arrange */
    var comando = new CadastraTarefa("Estudar Xunit", new Core.Models.Categoria("Estudo"), new DateTime(2022, 02, 17));

    var repo = new RepositorioFake();

    var handler = new CadastraTarefaHandler(repo);

    /* act */
    handler.Execute(comando); // SUT >> CadastraTarefaHandlerExecute

    /* assert */
    Assert.True(true);
}
```

## Anotações do XUnit

+ `[Fact]`
  + Trabalha apenas com os dados gerados no arrange.
  + Um método de teste com [Fact], tem como objetivo testar um “fato” único, e como um dos recursos mais utilizados, consegue atender todo um projeto de testes.
  + Exemplo:

    ```csharp
    [Fact]
    public void ValidaFaturamento()
    {
        // arrange
        var estacionamento = new Patio();
        var veiculo = new Veiculo();
        veiculo.Proprietario = "Cássio Costa";
        veiculo.Tipo = TipoVeiculo.Automovel;
        veiculo.Cor = "Verde";
        veiculo.Modelo = "Ford Maveric";
        veiculo.Placa = "asd-9999";

        estacionamento.RegistrarEntradaVeiculo(veiculo);
        estacionamento.RegistrarSaidaVeiculo(veiculo.Placa);

        // act 
        double faturamento = estacionamento.TotalFaturado();

        // assert
        Assert.Equal(2, faturamento);
    }
    ```

+ `[Theory]`
  + É uma anotação que permite trabalhar com um conjunto maior de dados para um método com parâmetros.
  + ela trabalha com uma outra anotação em conjunto, que é o `[InlineData]`
  + `[InlineData]` serve para preencher os parâmetros que serão passados como argumento no método.
  + Exemplo:

    ```csharp
    [Theory] 
    [InlineData("Cássio Costa", "ASD-1111", "Preto", "Gol")]
    [InlineData("Amanda Golçanves", "ASD-2222", "Cinza", "Fusca")]
    [InlineData("Wellington Costa", "ASD-3333", "Azul", "Opala")]
    public void ValidaFaturamentoComVariosVeiculos(string proprietario, string placa, string cor, string modelo)
    {
        // arrange
        Patio estacionamento = new Patio();
        var veiculo = new Veiculo();
        veiculo.Proprietario = proprietario;
        veiculo.Tipo = TipoVeiculo.Automovel;
        veiculo.Cor = cor;
        veiculo.Modelo = modelo;
        veiculo.Placa = placa;

        estacionamento.RegistrarEntradaVeiculo(veiculo);
        estacionamento.RegistrarSaidaVeiculo(veiculo.Placa);

        // act
        double faturamento = estacionamento.TotalFaturado();

        // assert
        Assert.Equal(2, faturamento);
    }
    ```

  + Porém, em alguns casos, o conjunto de `InlineData` pode crescer muito e poluir o código de testes. Ou ainda, temos a necessidade de passar um objeto como parâmetro, mas não temos como gerar uma instância e passar para uma anotação.
  + Para esses casos, temos como opção a anotação `ClassData`. Para utilizar este recurso é importante que a classe que terá os objetos passados como parâmetros implemente a interface `IEnumerable<object[]>`.
  + Exemplo:

    ```csharp
    public class Veiculo:IEnumerable<object[]>
    {
        public IEnumerator<object[]> GetEnumerator()
        {
            yield return new object[]
            {
                new Veiculo
                {
                    Proprietario = "André Silva",
                    Placa = "ASD-9999",
                    Cor="Verde",
                    Modelo="Fusca"
                }
            };
        }

        IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
    }

    // e no teste é feito conforme abaixo
    [Theory]
    [ClassData(typeof(Veiculo))]
    public void TestaVeiculoClass(Veiculo modelo)
    {
        //Arrange
        var veiculo = new Veiculo();

        //Act
        veiculo.Acelerar(10);
        modelo.Acelerar(10);

        //Assert
        Assert.Equal(modelo.VelocidadeAtual, veiculo.VelocidadeAtual);
    }
    ```

    *Apesar de parecer um pouco mais complexo, é sempre preferível passar um objeto para um conjunto grande de parâmetros na assinatura de qualquer método.*

### Anotações adicionais

+ `[Fact(Skip ="Mensagem")]`
  + Serve para ignorar um teste, que por exemplo pode estar em desenvolvimento mas não pode atrapalhar os outros testes.
  + Mensagem serve pra descrever o porque foi ignorado ao executar os testes.
  + Irá exibir no Gerenciador de testes com o ícone ⚠ amarelo de aviso como ignorado.
+ `[Fact(DisplayName = "Testa Frear Veiculo")]`
  + Serve para dar um nome customizado para o teste.
  + Aparece no Gerenciador de testes.
+ `[Trait("Funcionalidade", "Velocidade")]`
  + Agrupar métodos por (chave, valor).

## Propriedades e Construtor (Setup e Cleanup)

+ É possível declarar propriedades e construtor. (Setup)
  + O construtor é invocado 1 vez pra cada teste.
  + É muito útil para reaproveitar código.
+ O Cleanup é para limpar oque foi criado acima.

```csharp
public class VeiculoTestes
{
    private Veiculo veiculo;

    public VeiculoTestes(ITestOutputHelper _saidaConsole)
    {
        this.veiculo = new Veiculo(); // <-- 
    }

    [Fact]
    public void TestaVeiculoAcelerarComParametro10()
    {
        // arrange
        //var veiculo = new Veiculo(); // <-- removida a necessidade de criar em todo método

        // act
        veiculo.Acelerar(10);

        // assert
        Assert.Equal(100, veiculo.VelocidadeAtual);
    }

    // ...
}
```

+ É possível criar uma propriedade e implementar no construtor uma mensagem de retorno parecido com o `Console`.
  + Irá retornar a mensagem no teste.

```csharp
public class VeiculoTestes
{
    private Veiculo veiculo;
    public ITestOutputHelper SaidaConsole; // <--

    public VeiculoTestes(ITestOutputHelper _saidaConsole)
    {
        SaidaConsole = _saidaConsole;
        SaidaConsole.WriteLine("Contrutor invocado."); // <--
        this.veiculo = new Veiculo();
    }

    [Fact]
    public void TestaVeiculoAcelerarComParametro10()
    {
        // arrange
        //var veiculo = new Veiculo();

        // act
        veiculo.Acelerar(10);

        SaidaConsole.WriteLine("Velocidade apos acelerar: " + veiculo.VelocidadeAtual); // <--

        // assert
        Assert.Equal(100, veiculo.VelocidadeAtual);
    }

    public void Dispose() // <-- Cleanup
    {
        SaidaConsole.WriteLine("Dispose invocado.");
    }
    // ...
}
```
  
### Exceções

+ No xUnit podemos testar exceções usando o método `Assert.Throws<Exception>`,
  no qual passamos como parâmetro a função que deve gerar a exceção.
+ Podemos também utilizar uma estrutura `try-catch` para escrever um teste e usar o `Assert.Equal`.
+ No desenvolvimento baseado em testes é altamente recomendável que as exceções geradas pela aplicação também sejam testadas.
+ **O teste só vai passar se a exceção ocorrer.**

Exemplos:

```csharp
[Fact]
public void TestaNomeProprietarioVeiculoComMenosDeTresCaracteres()
{
    // arrange
    string nomeProprietario = "Ab";

    // assert - o teste vai passar se a exceção ocorrer
    Assert.Throws<FormatException>(
        // act
        () => new Veiculo(nomeProprietario) // <-- Delegate
    );
}

[Fact]
public void TestaMensagemExcecaoDoQuartoCaractereDaPlaca()
{
    // arrange
    string placa = "ASDF8888";

    // act assert
    var mensagem = Assert.Throws<FormatException>(
        () => new Veiculo().Placa = placa // <-- delegate
        );

    // assert
    Assert.Equal("O 4° caractere deve ser um hífen", mensagem.Message);
}

[Fact]
public void TestaSeUltimosCaracteresDaPlacaSaoNumeros()
{
    // arrange
    string placa = "ASD-9R90";

    // act
    var mensagem = Assert.Throws<FormatException>(
        () => new Veiculo().Placa = placa // <-- delegate
        );
}
```

### Testes de Regressão

+ Nos testes de regressão adicionamos novas funcionalidades à aplicação “quebrando” testes que já funcionavam;
+ Para a evolução da aplicação não regredir,conforme implementamos novas funções, precisamos sempre fazer testes.

### Testar métodos privados

+ Testar métodos privados pode ser uma tarefa complicada, para não quebrar o encapsulamento. 
+ O ideal é testar um método público que execute o método privado dentro dele.

### Usando Banco de Dados InMemoryDatabase como Dublê

+ Para isso é necessário que o contexto seja criado e passado por injeção de dependência.
+ Passos obrigatórios para usar o banco de dados em memória do Entity Framework Core:
  + Instalar o pacote Microsoft.EntityFrameworkCore.InMemory
  + Garantir que o contexto EF Core está sendo injetado e não criado "na mão" dentro do repositório
  + No método OnConfiguring() verificar se as opções do contexto já foram configuradas através da propriedade IsConfigured
  + No método de teste configurar as opções do contexto para utilizar o InMemoryDatabase

```csharp
public class RepositorioTarefa : IRepositorioTarefas
{
    DbTarefasContext _ctx;

    public RepositorioTarefa(DbTarefasContext context) // <-- Injetação de dependência do contexto
    {
        _ctx = context;
    }

    //...
}
```

+ Permitindo que o contexto consiga conectar não só com o DB padrão, mas com o `InMemoryDatabase` também:

```csharp
public class DbTarefasContext : DbContext
{
    // construtores...

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        if (!optionsBuilder.IsConfigured) // <-- não foi configurado externamente, configura com o connection string abaixo
            optionsBuilder.UseSqlServer("Data Source=(localdb)\\MSSQLLocalDB;Initial Catalog=DbTarefas;Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;ApplicationIntent=ReadWrite;MultiSubnetFailover=False");
    }

    // DbSets ...
}
```

+ Método de teste usando o contexto de `InMemoryDatabase`:

```csharp
[Fact]
public void DataTarefaComInfoValidasDeveIncluirNoBD()
{
    /* arrange */
    var comando = new CadastraTarefa("Estudar Xunit", new Core.Models.Categoria("Estudo"), new DateTime(2022, 02, 17));

    // Configurando o contexto para usar o InMemoryDatabase
    var options = new DbContextOptionsBuilder<DbTarefasContext>()
        .UseInMemoryDatabase("DbTarefasContext")
        .Options;
    var context = new DbTarefasContext(options);
    var repo = new RepositorioTarefa(context);

    var handler = new CadastraTarefaHandler(repo);

    /* act */
    handler.Execute(comando); // SUT >> CadastraTarefaHandlerExecute

    /* assert */
    var tarefa = repo.ObtemTarefas(t => t.Titulo == "Estudar Xunit").FirstOrDefault();
    Assert.NotNull(tarefa);
}
```

---

### Dublês para Testes

#### Dummy Object

+ Esse objeto é justamente um objeto que nós precisamos criar, mas nós não vamos utilizar ele nos nossos testes.

```csharp
[Fact]
public void DataTarefaComInfoValidasDeveIncluirNoBD()
{
    /* arrange */
    var comando = new CadastraTarefa(
        "Estudar Xunit", 
        new Categoria("Estudo"), //  <-- preciso criar mas não é usado nos testes
        new DateTime(2022, 02, 17));

    // ...

    /* act */
    handler.Execute(comando); 

    /* assert */ // <-- Não é usado nos testes (assert)
    var tarefa = repo.ObtemTarefas(t => t.Titulo == "Estudar Xunit").FirstOrDefault();
    Assert.NotNull(tarefa);
}
```

#### Fake Object

+ É uma classe que nós criamos ou usamos para simular algum recurso de forma mais rápida e leve.
+ Um fake object famoso é o `InMemoryDatabase`.

#### Stubs

+ É aquele onde você precisa fornecer alguma informação de entrada, algum dado de entrada para o seu teste. 
+ Como exemplo é justamente o lançamento de uma exceção.

### Informações adicionais

+ Commando para executar no Gerenciador de pacotes:
  + `dotnet tests`

---

[`^ topo`](#Dev)
</font>

