<font face="Calibri">

# ⭐ Métodos

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Argumentos opcionais

É quando o argumento do método já tem um valor padrão.

```csharp
public void EnviaMensagem(string mensagem = "Ok") // Argumento opcional
{
    // ...
}

// uso
EnviaMensagem(); // implicitamente o compilador preenche o "Ok"
```

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">

## Argumentos nomeados

+ É quando precisamos informar um argumento opcional no meio da lista de argumentos, sem ter que preencher todos os outros.

```csharp
public void EnviaMensagem(string assunto = "Resposta WS", string mensagem = "Ok")
{
    // ...
}

// uso
EnviaMensagem(mensagem: "Minha mensagem"); // implicitamente o compilador preenche o "Minha mensagem", o assunto continua o padrão
```

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">

## Indexador

+ Os indexadores permitem que instâncias de uma classe ou struct sejam indexados como matrizes.
+ O valor indexado pode ser definido ou recuperado **sem especificar explicitamente um membro de instância ou tipo**.
+ Os indexadores parecem com propriedades, a diferença é que seus acessadores usam parâmetros.
+ É uma propriedade que permite navegar por array, e pode ser a própria class como no exemplo:

```csharp
public class ListaContaCorrente
{
    private ContaCorrente[] _itens;
    private int _proximaPosicao;

    public int Tamanho { get { return _proximaPosicao; } }

    // indexador
    public ContaCorrente this[int indice]
    {
        get
        {
            if (indice < 0 || indice >= _proximaPosicao) throw new ArgumentException(nameof(indice));

            return _itens[indice];
        }
    }
}
```

+ Isso permite acessar a instancia sem precisar indicar nenhuma propriedade para percorrer o array:

```csharp
ListaContaCorrente contas = new ListaContaCorrente();
contas.Adicionar(new ContaCorrente(123, 12344););

for (int i = 0; i < contas.Tamanho; i++)
{
    ContaCorrente itemAtual = contas[i]; // <-- percorrendo como um array
    Console.WriteLine($"({i}) {itemAtual.ToString()}");
}
```

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">

## Argumento com "params"

+ Usando a palavra-chave `params`, você pode especificar um parametro que aceita um número variável de argumentos (ver exemplo abaixo).
+ O tipo de parâmetro deve ser uma matriz unidimensional.
+ Sem o `params` um parâmetro de array só recebe um `array` ja criado.
+ Adicionar `params` como prefixo do argumento de `array` do método faz com que permita inserir multiplos objetos como argumento ou um `array` criado anteriormente:

```csharp
public class ListaContaCorrente
{
    private ContaCorrente[] _itens;
    private int _proximaPosicao;

    public int Tamanho { get { return _proximaPosicao; } }

    public void AdicionarVarios(params ContaCorrente[] itens) // <--
    {
        foreach (ContaCorrente item in itens)
        {
            Adicionar(item);
        }
    }
}

// Exemplo de uso
ListaContaCorrente contas = new ListaContaCorrente();
contas.AdicionarVarios(
    new ContaCorrente(123, 12345),
    new ContaCorrente(123, 12346),
    new ContaCorrente(123, 12347)
    // multiplos argumentos
);
```

Ao chamar um método com um `params` parâmetro, você pode passar:

+ Uma lista separada por vírgulas de argumentos do tipo dos elementos da matriz.
+ Uma matriz de argumentos do tipo especificado.
+ Sem argumentos. Se você não enviar nenhum argumento, o comprimento da lista params será zero.

<hr style="height:1px;border-width:0;color:#27292c;background-color:#27292c">

## Métodos de Extensão

`Extension Methods`

Os `métodos de extensão` permitem que você "adicione" tipos existentes sem criar um novo tipo derivado, recompilar ou, caso contrário, modificar o tipo original... [ver mais](Extensions.md)

---

[`^ topo`](#⭐-métodos)
</font>
