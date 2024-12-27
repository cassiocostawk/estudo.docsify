# Decorator
📍 `Padrão estrutural`

Também conhecido como: `Decorador`, `Envoltório`, `Wrapper`
Criado pela Gangue dos Quatro (Erich Gamma, Richard Helm, Ralph Johnson, John Vlissides)
Muito usado para aplicar o `OCP` (SOLID).

---
## 💡 Propósito
O `Decorator` é um padrão de projeto estrutural que permite que você acople novos comportamentos para objetos ao colocá-los dentro de invólucros de objetos que contém os comportamentos.

---
## ❗ Problema
Imagine que você está trabalhando em um biblioteca de notificação que permite que outros programas notifiquem seus usuários sobre eventos importantes.

A versão inicial da biblioteca foi baseada na classe `Notificador` que tinha apenas alguns poucos campos, um construtor, e um único método `enviar`. 

O método podia aceitar um argumento de mensagem de um cliente e enviar a mensagem para uma lista de emails que eram passadas para o notificador através de seu construtor.

Pensando em não só notificar por email, gostariamos de receber um SMS, Facebook ou Slack acerca de problemas críticos.

Mas então alguém, com razão, pergunta a você, “Por que você não usa diversos tipos de notificação de uma só vez? Se a sua casa pegar fogo, você provavelmente vai querer ser notificado por todos os canais.”

Você tenta resolver esse problema criando subclasses especiais que combinam diversos tipos de métodos de notificação dentro de uma classe. Contudo, rapidamente você nota que isso irá inflar o código imensamente, e não só da biblioteca, o código cliente também.

---
## ✅ Solução

No nosso exemplo de notificações vamos deixar o simples comportamento de notificação por email dentro da classe Notificador base, mas transformar todos os métodos de notificação em decoradores.

O código cliente vai precisar envolver um objeto notificador básico em um conjunto de decoradores que coincidem com as preferências do cliente. Os objetos resultantes serão estruturados como uma pilha.

Podemos utilizar a mesma abordagem para vários comportamentos tais como formatação de mensagens ou compor uma lista de recipientes. O cliente pode decorar o objeto com quaisquer decoradores customizados, desde que sigam a mesma interface que os demais.

---
## 👁‍🗨 Analogia com o mundo real
+ Vestir roupas é um exemplo de usar decoradores. 
+ Quando você está com frio, você se envolve com um suéter. 
+ Se você ainda sente frio com um suéter, você pode vestir um casaco por cima. 
+ Se está chovendo, você pode colocar uma capa de chuva. 

Todas essas vestimentas “estendem” seu comportamento básico mas não são parte de você, e você pode facilmente remover uma peça de roupa sempre que não precisar mais dela.

---
## 👣 Como implementar
1. Certifique-se que seu domínio de negócio pode ser representado como um componente primário com múltiplas camadas opcionais sobre ele.
<br>
2. Descubra quais métodos são comuns tanto para o componente primário e para as camadas opcionais. Crie uma interface componente e declare aqueles métodos ali.
<br>
3. Crie uma classe componente concreta e defina o comportamento base nela.
<br>
4. Crie uma classe decorador base. Ela deve ter um campo para armazenar uma referência ao objeto envolvido. 
   O campo deve ser declarado com o tipo da interface componente para permitir uma ligação entre os componentes concretos e decoradores. 
   O decorador base deve delegar todo o trabalho para o objeto envolvido.
<br>
1. Certifique-se que todas as classes implementam a interface componente.
<br>
6. Crie decoradores concretos estendendo-os a partir do decorador base. Um decorador concreto deve executar seu comportamento antes ou depois da chamada para o método pai (que sempre delega para o objeto envolvido).
<br>
7. O código cliente deve ser responsável por criar decoradores e compô-los do jeito que o cliente precisa.

---
## 📑 Exemplo

+ Classe base abstrata:
```csharp
public abstract class Imposto
{
    public Imposto OutroImposto { get; set; }

    public Imposto(Imposto outroImposto)
    {
        OutroImposto = outroImposto;
    }

    public Imposto()
    {
        OutroImposto = null;
    }

    public abstract double Calcula(Orcamento orcamento);

    protected double CalculoOutroImposto(Orcamento orcamento)
    {
        if (OutroImposto == null) return 0; // <-- ultimo da pilha será null

        return OutroImposto.Calcula(orcamento);
    }
}
```

+ Implementações da classe abstrata:
```csharp
public class ISS : Imposto
{
    public ISS(Imposto outroImposto) : base(outroImposto) { }
    public ISS() { }

    public override double Calcula(Orcamento orcamento)
    {
        return orcamento.Valor * 0.06 + CalculoOutroImposto(orcamento);
    }
}

public class ICMS : Imposto
{
    public ICMS(Imposto outroImposto) : base(outroImposto) { }
    public ICMS() : base() { }
    
    public override double Calcula(Orcamento orcamento)
    {
        return orcamento.Valor * 0.1 + CalculoOutroImposto(orcamento);
    }
}

public class ImpostoMuitoAlto : Imposto
{
    public ImpostoMuitoAlto(Imposto outroImposto) : base(outroImposto) { }
    public ImpostoMuitoAlto() { }

    public override double Calcula(Orcamento orcamento)
    {
        return orcamento.Valor * 0.2 + CalculoOutroImposto(orcamento);
    }
}
```

+ Execução:
```csharp
static void Main(string[] args)
{
    Imposto iss = new ISS(new ICMS(new ImpostoMuitoAlto())); // <--

    Orcamento orcamento = new Orcamento(500);

    double valor = iss.Calcula(orcamento);

    Console.WriteLine(valor);

    Console.ReadKey();
}
```