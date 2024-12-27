# Interpreter
📍 `Padrão específico`

O padrão Interpreter é geralmente útil para interpretar DSLs. É comum que, ao ler a string (como por exemplo 2+3/4), o programa transforme-a em uma estrutura de dados melhor (como as nossas classes Expressao) e aí interprete essa árvore.

É realmente um padrão de projeto bem peculiar, e com utilização bem específica.

---
## 📑 Exemplo 
### 🔹 Calculadora

+ Interface das Expressões:
```csharp
interface IExpressao
{
    int Avalia();
}
```

+ Classes que representam as operações aritiméticas que implementa, a IExpressao:
```csharp
class Soma : IExpressao
{
    public IExpressao Esquerda { get; set; }
    public IExpressao Direita { get; set; }

    public Soma(IExpressao esquerda, IExpressao direita)
    {
        Esquerda = esquerda;
        Direita = direita;
    }

    public int Avalia()
    {
        int valorEsquerda = Esquerda.Avalia();
        int valorDireita = Direita.Avalia();
        return valorEsquerda + valorDireita;
    }
}

class Subtracao : IExpressao
{
    public IExpressao Esquerda { get; set; }
    public IExpressao Direita { get; set; }

    public Subtracao(IExpressao esquerda, IExpressao direita)
    {
        Esquerda = esquerda;
        Direita = direita;
    }

    public int Avalia()
    {
        int valorEsquerda = Esquerda.Avalia();
        int valorDireita = Direita.Avalia();
        return valorEsquerda - valorDireita;
    }
}

class Multiplicacao : IExpressao
{
    public IExpressao Esquerda { get; set; }
    public IExpressao Direita { get; set; }

    public Multiplicacao(IExpressao esquerda, IExpressao direita)
    {
        Esquerda = esquerda;
        Direita = direita;
    }

    public int Avalia()
    {
        int valorEsquerda = Esquerda.Avalia();
        int valorDireita = Direita.Avalia();
        return valorEsquerda * valorDireita;
    }
}

class Divisao : IExpressao
{
    public IExpressao Esquerda { get; set; }
    public IExpressao Direita { get; set; }

    public Divisao(IExpressao esquerda, IExpressao direita)
    {
        Esquerda = esquerda;
        Direita = direita;
    }

    public int Avalia()
    {
        int valorEsquerda = Esquerda.Avalia();
        int valorDireita = Direita.Avalia();
        return valorEsquerda / valorDireita;
    }
}

class RaizQuadrada : IExpressao
{
    public IExpressao Expressao { get; set; }

    public RaizQuadrada(IExpressao expressao)
    {
        Expressao = expressao;
    }

    public int Avalia()
    {
        return (int)(Math.Sqrt(Expressao.Avalia()));
    }
}
```


+ Classe que representa o Numero que implementa a IExpressao também:
```csharp
class Numero : IExpressao
{
    public int numero { get; set; }

    public Numero(int numero)
    {
        this.numero = numero;
    }

    public int Avalia()
    {
        return this.numero;
    }
}
```

+ Exemplos de uso:
```csharp
// Expressao Matemática ((1 + 100) + 10) + (20 - 10) = 121
IExpressao esquerda = new Soma(new Soma(new Numero(1), new Numero(100)), new Numero(10));
IExpressao direita = new Subtracao(new Numero(20), new Numero(10));
IExpressao resultado = new Soma(esquerda, direita);
Console.WriteLine(resultado.Avalia());

// Expressao Matemática (10 * 5 + 50 / 2) = 75
esquerda = new Multiplicacao(new Numero(10), new Numero(5));
direita = new Divisao(new Numero(50), new Numero(2));
resultado = new Soma(esquerda, direita);
Console.WriteLine(resultado.Avalia());

// Expressao raiz quadrada de 144 = 12
IExpressao expressao = new RaizQuadrada(new Numero(144));
Console.WriteLine(expressao.Avalia());
```