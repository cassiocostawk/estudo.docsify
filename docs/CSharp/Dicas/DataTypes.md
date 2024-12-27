<font face="Calibri">

# ⭐ DataTypes

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

*Inicia em [Variáveis](./Variaveis.md)...*

---

## 🔹 Tipos numéricos integrais

+ O valor padrão de todo tipo numérico integral é `0`.
+ Os tipos numéricos integrais representam números inteiros.
+ Todos os tipos numéricos integrais são tipos de valor.
+ Eles também são tipos simples e podem ser inicializados com literais.
+ Todos os tipos numéricos integrais suportamoperadores aritméticos, lógicos bit a bit, de comparação e de igualdade.

| tipo C#               | Intervalo                                              | Tamanho                                 | Tipo .NET      |
|-----------------------|--------------------------------------------------------|-----------------------------------------|----------------|
| sbyte                 | -128 a 127                                             | Inteiro de 8 bits com sinal             | System.SByte   |
| byte                  | 0 a 255                                                | Inteiro de 8 bits sem sinal             | System.Byte    |
| short                 | -32.768 a 32.767                                       | Inteiro de 16 bits com sinal            | System.Int16   |
| ushort                | 0 a 65.535                                             | Inteiro de 16 bits sem sinal            | System.UInt16  |
| int                   | -2.147.483.648 a 2.147.483.647                         | Inteiro assinado de 32 bits             | System.Int32   |
| uint                  | 0 a 4.294.967.295                                      | Inteiro de 32 bits sem sinal            | System.UInt32  |
| long                  | -9.223.372.036.854.775.808 a 9.223.372.036.854.775.807 | Inteiro assinado de 64 bits             | System.Int64   |
| ulong                 | 0 a 18.446.744.073.709.551.615                         | Inteiro de 64 bits sem sinal            | System.UInt64  |
| *nint*                | Depende da plataforma                                  | Inteiro de 32 bits ou 64 bits assinado  | System.IntPtr  |
| *nuint*               | Depende da plataforma                                  | Inteiro de 32 bits ou 64 bits sem sinal | System.UIntPtr |

+ Literais inteiros podem ser:
  + decimal: sem nenhum prefixo
  + hexadecimal: com o prefixo ou 0X
  + binary: com o prefixo ou 0B (disponível no C# 7.0 e posterior)

```csharp
var decimalLiteral = 42;
var hexLiteral = 0x2A;
var binaryLiteral = 0b_0010_1010;
```

## 🔹 Tipos numéricos de ponto flutuante

| palavra-chave/tipo C# | Intervalo aproximado             | Precisão         | Tamanho  | Tipo .NET      |
|-----------------------|----------------------------------|------------------|----------|----------------|
| float                 | ±1,5 x 10−45 para ±3,4 x 1038    | ~6 a 9 dígitos   | 4 bytes  | System.Single  |
| double                | ±5.0 × 10−324 to ±1.7 × 10308    | ~15 a 17 dígitos | 8 bytes  | System.Double  |
| decimal               | ±1,0 x 10-28 para ±7,9228 x 1028 | 28 a 29 dígitos  | 16 bytes | System.Decimal |

## 🔹 Descrevendo de forma literal

L = long
D = double
F = float
M = decimal
U = uint
UL = ulong

---
---

## 🔹 null

+ Setar uma variável como nula, está na verdade setando uma referencia nula a um objeto.

---

## 🔹 object

+ Classe base de qualquer classe sem herança (implicitamente):

```csharp
public class Teste // : object (implicitamente, object model)
{
    //...
}
```

Exceções:

+ `Struct` não herda de `Object` mas de `ValueType`.
+ `Interfaces` são convertíveis para `object`.
+ `Ponteiros`.

---

### 📍 ToString()

*Método `virtual` da classe `object`, ou seja, pode ser sobrescrito.*

+ Método padrão usado ao obter uma string de um objeto é o `ToString()`:

```csharp
object teste = new object(); 
// ToString() implicito
Console.WriteLine(teste); // System.Object (namespace e nome da classe)
```

+ Porém pode ser sobrescrito o método `ToString()` e mudar o resultado:

```csharp
public class ContaCorrente
{
    public int Numero { get; } // = 12345
    public int Agencia { get; } // = 012
    public double Saldo { get; } // = 200.00

    public override string ToString()
    {
        return $"Numero {Numero}, Agencia {Agencia}, Saldo {Saldo}";
    }
}

// usando
object conta = new ContaCorrente(); 
conta.Numero = 12345;
conta.Agencia = 012;
conta.Saldo = 200.00;
Console.WriteLine(conta); // Numero 12345, Agencia 012, Saldo 200
```

---

### 📍 Equals()

+ Por padrão compara a referencia do objeto, como se usasse `==`, como abaixo.
+ Erro de comparação por sinal de `==`:

```csharp
Cliente cliente1 = new Cliente()
{
    CPF = "123.456.789.09",
    Nome = "Cassio",
    Profissao = "Programador"
};

Cliente cliente2 = new Cliente()
{
    CPF = "123.456.789.09",
    Nome = "Cassio",
    Profissao = "Programador"
};

Cliente cliente3 = cliente1; // cliente3 recebe a referencia de cliente1

Console.WriteLine(cliente1 == cliente2); // false
Console.WriteLine(cliente1 == cliente3); // true pois tem a mesma referência

Console.WriteLine(cliente1.Equals(cliente2)); // false antes de sobrescrever Equals(), true depois
```

+ Isso acontece porque estão sendo comparadas as referências dos objetos (que estão em locais de memória diferentes) e não o conteúdo deles entre si.

+ Para resolvermos isso podemos sobrescrever o método `Equals()` assim como é feito para o `ToString`:

```csharp
// sobrescrevendo o método Equals() na classe Cliente
public class Cliente
{
    public string Nome { get; set; }
    public string CPF { get; set; }
    public string Profissao { get; set; }

    public override bool Equals(object obj)
    {
        Cliente outroCliente = obj as Cliente; // conversão explicita de outra forma

        if (outroCliente == null) return false;

        return Nome == outroCliente.Nome &&
            CPF == outroCliente.CPF &&
            Profissao == outroCliente.Profissao;
    }

    // outra forma de fazer
    public override bool Equals(object obj)
    {
        if (!(obj is Cliente))
            return false;

        Cliente outroCliente = (Cliente)obj; // conversão explicita

        return Nome == outroCliente.Nome &&
            CPF == outroCliente.CPF &&
            Profissao == outroCliente.Profissao;
    }
}

// assim o resultado acima será true
Console.WriteLine(cliente1.Equals(cliente2)); // true
```

---

## 🔹 ValueType

+ Herda de `object`.
+ Classe base de `Structs`.

---
---

## 🔹 string

+ Palavra reservada, por isso é minúscula.
+ É interpretada no compilador como a classe `String`.
+ A `string` é imútável.
  Ou seja, uma vez criada em uma variavel terá sempre o mesmo valor, ao setar um novo valor pra a variavel é criada uma nova string e a variável passa a referenciar a nova string, a referencia da string anterior é perdida.

+ Exemplo de como acontece internamente:

```csharp
string url = "pagina?argumentos";

url += "sufixo";

// exemplo de como é feito internamente a instrução acima
string temporaria = url + "sufixo";
url = temporaria;
```

---

### 📍 Substring()

*Método da classe `String`*

+ Recupera um trecho de uma string.
  + a partir da posição
  + por quantos caracteres (opcional)

+ Exemplo:

```csharp
string url = "pagina?argumentos";
           // 012345678

Console.WriteLine(url);   // pagina?argumentos
var s = url.Substring(6); // a partir do caractere 6   <--
Console.WriteLine(s);     // ?argumentos
s = url.Substring(6,1);   // 1 caractere a partir do caractere 6  
Console.WriteLine(s);     // ?
```

---

### 📍 IndexOf()

*Método da classe `String`*

+ Retorna o indice da primeira correspondencia do caractere informado.
  Exemplo:

```csharp
// Indices....012345678
string url = "pagina?argumentos";

int indice = url.IndexOf('?'); // <--

Console.WriteLine(indice);                    // 6
Console.WriteLine(url.Substring(indice + 1)); // argumentos
```

+ Retorna o indice do primeiro caractere da correspondencia da string informada
  Exemplo:

```csharp
// Indices....012345678
string url = "pagina?argumentos";

int indice = url.IndexOf("argumentos"); // <--

Console.WriteLine(indice);                // 7
Console.WriteLine(url.Substring(indice)); // argumentos
```

---

### 📍 IsNullOrEmpty()

*Método estático da classe `String`*

+ Retorna `True` se string nula ou vazia.

```csharp
// Exemplo de uso
bool VazioNulo = string.IsNullOrEmpty(MyVar);

// Exemplo de teste logico
string textoVazio = "";
string textoNulo = null;
string textoAbc = "abc123";

Console.WriteLine(string.IsNullOrEmpty(textoVazio)); // True
Console.WriteLine(string.IsNullOrEmpty(textoNulo));  // True
Console.WriteLine(string.IsNullOrEmpty(textoAbc));   // False
```

---

### 📍 Remove()

*Método da classe `String`*

+ Remove caracteres da string a partir do `StartIndex`:

```csharp
//                01234567890123
string exemplo = "argumento1=abc&argumento2=123";
int indice = exemplo.IndexOf('&'); 
Console.WriteLine(exemplo.Remove(indice)); // argumento2=123
```

+ Remove caracteres da string a partir do `StartIndex` por `count` caracteres;

```csharp
//                01234567890123
string exemplo = "argumento1=abc&argumento2=123";
int indiceInicio = exemplo.IndexOf('&');
string valor = exemplo.Remove(indiceInicio); 
Console.WriteLine(valor); // argumento1=abc

int indiceFim = valor.IndexOf('=');
Console.WriteLine(valor.Remove(indiceFim)); // argumento1  
```

---

### 📍 Replace()

*Método da classe `String`*

+ Substitui todas as ocorrencias de `oldValue` pelo `newValue?`:

```csharp
string exemplo1 = "argumento1=abc&argumento2=123";
string exemplo2 = exemplo1.Replace("argumento","valor");
Console.WriteLine(exemplo2); // valor1=abc&valor2=123
```

+ `newValue` pode ser opcional, assim removendo o conteúdo do `oldValue`.
+ Existem outras sobrecargas que usam `ignoreCase`, `CultureInfo` e `StringComparison`.

---

### 📍 ToUpper()

*Método da classe `String`*

+ Normalizar toda `string` em MAIÚSCULO.

```csharp
string exemplo = "Cassio Costa";

string exemploUpper = exemplo.ToUpper();
Console.WriteLine(exemploUpper); // CASSIO COSTA
```

---

### 📍 ToLower()

*Método da classe `String`*

+ Normalizar toda `string` em minúsculo.

```csharp
string exemplo = "Cassio Costa";

string exemploLower = exemplo.ToLower(); 
Console.WriteLine(exemploLower); // cassio costa
```

---

### 📍 Split()

*Método da classe `String`*

+ Passado por parâmetro o **Separador** do tipo `char`;
+ Retorna um `string[]`;

```csharp
string texto = "1,Joao,Rua A,123";

string[] colecao = texto.Split(','); // [1][Joao][Rua A][123]
```

---

### 📍 StartsWith()

*Método da classe `String`*

+ `string` começa com `value`:

```csharp
string exemplo = "Cassio Costa";
Console.WriteLine(exemplo.StartsWith("Cassio")); // true
```

---

### 📍 EndsWith()

*Método da classe `String`*

+ `string` termina com `value`:

```csharp
string exemplo = "Cassio Costa";
Console.WriteLine(exemplo.EndsWith("Costa")); // true
```

---

### 📍 Contains()

*Método da classe `String`*

+ `string` contém `value`:

```csharp
string exemplo = "Cassio Costa";
Console.WriteLine(exemplo.Contains("Co")); // true
```

---
---

## 🔹 int

+ Palavra reservada, por isso é minúscula.
+ É interpretada no compilador como a classe `Int32`.
+ Valor Default `0`.

---
---

## 🔹 double

+ Palavra reservada, por isso é minúscula.
+ É interpretada no compilador como a classe `Double`.
+ Valor Default `0`.

---
---

## 🔹 bool

+ Palavra reservada, por isso é minúscula.
+ É interpretada no compilador como a classe `Boolean`.
+ Valor Default `false`.

---
---

## 🔹 char

+ Palavra reservada, por isso é minúscula.
+ É interpretada no compilador como a classe `Char`.
+ Valor Default `'\0'` (U+0000).

---
---

## 🔹 enum

`Enumerator`

+ Um `tipo de enumeração` é um `tipo de valor` definido por um conjunto de constantes nomeadas do tipo numérico integral subjacente. 
+ Por padrão, os valores constantes associados de membros de enumeração são do tipo int ; eles começam com zero e aumentam em um seguindo a ordem de texto de definição.
+ O valor padrão de um tipo `E` de enumeração é o valor produzido por expressão `(E)0` , mesmo se zero não tiver o membro enum correspondente.

---
---

## 🔹 Arrays

+ O `array` tem **tamanho fixo**, definiu não pode mudar mais.
  + Se precisa mudar o tamanho usar o `List<T>`.
    + `List<T>` usa array, mas redimensiona internamente.
  + Para redimensionar o array deve-se criar um novo array e copiar o array atual para ele, normalmente é dobrado o tamanho atual do array.
+ "Na maior parte do tempo deve preferir uma lista."
+ "O `array` só deve ser usado quando há um bom motivo para ele."
+ Não use `ArrayList` como sugerido em outra resposta, porque ele não é genérico, e usa tudo como `object` (boxing, unboxing toda hora na CIL).

Como curiosidade o `array` é **usado internamente** dentro do tipo `List` como implementação concreta.
Mas ele gerencia as manipulações necessárias quando o tamanho não é suficiente.

Exemplos de arrays fixos:

```csharp
ContaCorrente[] contas = new ContaCorrente[3]; // tamanho fixo

contas[0] = new ContaCorrente(874, 123456);
contas[1] = new ContaCorrente(874, 123457);
contas[2] = new ContaCorrente(874, 123458);

for (int i = 0; i < contas.Length; i++)
{
    Console.WriteLine($"Conta {i} {contas[i].Numero}");
}
```

Exemplo de array *dinamicamente fixado*:

```csharp
ContaCorrente[] contas = new ContaCorrente[] // inicializacao de arrays - é um syntax sugar
{
    new ContaCorrente(874, 123456),
    new ContaCorrente(874, 123457),
    new ContaCorrente(874, 123458)
}; // tem que ser criado na inicialização de array, não da pra adicionar mais depois disso

for (int i = 0; i < contas.Length; i++)
{
    Console.WriteLine($"Conta {i} {contas[i].Numero}");
}
```

### "Redimensionar" o Array

+ Criando novo array e copiando os dados do antigo para o novo:

```csharp
ContaCorrente[] novoArray = new ContaCorrente[novoTamanho];

for(int indice = 0; indice < _itens.Length; indice++)
{
    novoArray[indice] = _itens[indice];
}

_itens = novoArray;
```

+ Alternativa Array.Copy(), exemplo de uma das sobrecargas:

```csharp
Array.Copy(_itens, novoArray, _itens.Length);
```

### Inicializador de Arrays

Exemplo:

```csharp
int[] idades = new int[] { 12, 15, 27 };
```

---
---

## 🔹 Tipos Genericos

+ `<>` indica ser genérico.
+ Não necessáriamente preicsa ser `T`, mas por convenção usa-se `<T>`.
  + `Lista<T>` funciona a msm coisa se eu escrevesse `Lista<MeuTipoGenerico>`.
  + Como acontece com `Dictionary<TKey, TSource>` onde `TKey` e `TSource` fazem o mesmo trabalho de `T`.

+ Exemplo de **método** utilizando `Tipos Genéricos`:

```csharp
// método
T GetDefault<T>()
{
    return default(T);
}

// uso
GetDefault<int>(); // 0
GetDefault<string>(); // null
GetDefault<DateTime>(); // 01/01/0001 00:00:00
GetDefault<TimeSpan>(); // 00:00:00
```

+ Exemplo desenvolvendo a **classe** `List<T>` na mão completa utilizando `Tipos Genéricos`:

```csharp
public class Lista<T> 
{
    private T[] _itens;
    private int _nextPosition;

    public int Lenght { get { return _nextPosition; } }

    // indexador de _itens
    public T this[int indice]
    {
        get
        {
            return GetItemNoIndice(indice);
        }
    }

    // construtor nao inclui <T>
    public Lista(int capacidadeInicial = 5)
    {
        _itens = new T[capacidadeInicial];
        _nextPosition = 0;
    }

    public void Add(T item)
    {
        VerificarCapacidade(_nextPosition + 1);

        _itens[_nextPosition] = item;
        _nextPosition++;
    }

    public void AddList(params T[] itens)
    {
        foreach (T item in itens)
        {
            Adicionar(item);
        }
    }

    public void Remove(T item)
    {
        int indice = -1;
        for (int i = 0; i < _nextPosition; i++)
        {
            T itemAtual = _itens[i];

            if (itemAtual.Equals(item))
            {
                indice = i;
                break;
            }
        }

        for (int i = indice; i < _nextPosition - 1; i++)
        {
            _itens[i] = _itens[i + 1];
        }

        _nextPosition--;
    }

    private void VerificarCapacidade(int tamanhoNecessario)
    {
        if (_itens.Length >= tamanhoNecessario) return;

        int novoTamanho = _itens.Length * 2;

        if (novoTamanho < tamanhoNecessario)
            novoTamanho = tamanhoNecessario;

        T[] novoArray = new T[novoTamanho];

        Array.Copy(_itens, novoArray, _itens.Length);

        _itens = novoArray;
    }

    public void Imprimir()
    {
        for (int i = 0; i < _nextPosition; i++)
        {
            Console.WriteLine($"({i}) " + _itens[i].ToString());
        }
    }

    public T GetItemNoIndice(int indice)
    {
        if (indice < 0 || indice >= _nextPosition) throw new ArgumentException(nameof(indice));

        return _itens[indice];
    }
```

---

[`^ topo`](#⭐-datatypes)
</font>
