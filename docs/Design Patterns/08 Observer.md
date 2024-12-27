# Observer
📍 `Padrão comportamental`

Também conhecido como: `Observador`, `Assinante do evento`, `Event-Subscriber`, `Escutador`, `Listener`

---
## 💡 Propósito
O `Observer` é um padrão de projeto comportamental que permite que você defina um mecanismo de assinatura para notificar múltiplos objetos sobre quaisquer eventos que aconteçam com o objeto que eles estão observando.

---
## ❗ Problema
Imagine que você tem dois tipos de objetos: um `Cliente` e uma `Loja`. O cliente está muito interessado em uma marca particular de um produto (digamos que seja um novo modelo de iPhone) que logo deverá estar disponível na loja.

O cliente pode visitar a loja todos os dias e checar a disponibilidade do produto. Mas enquanto o produto ainda está a caminho, a maioria desses visitas serão em vão.

Por outro lado, a loja poderia mandar milhares de emails para todos os clientes cada vez que um novo produto se torna disponível. Isso salvaria alguns clientes de incontáveis viagens até a loja. Porém, ao mesmo tempo, irritaria outros clientes que não estão interessados em novos produtos.

Parece que temos um conflito. Ou o cliente gasta tempo verificando a disponibilidade do produto ou a loja gasta recursos notificando os clientes errados.

---
## ✅ Solução
O objeto que tem um estado interessante é quase sempre chamado de `sujeito`, mas já que ele também vai notificar outros objetos sobre as mudanças em seu estado, nós vamos chamá-lo de `publicador`. Todos os outros objetos que querem saber das mudanças do estado do publicador são chamados de `assinantes`.

 O padrão Observer sugere que você adicione um mecanismo de assinatura para a classe publicadora para que objetos individuais possam assinar ou desassinar uma corrente de eventos vindo daquela publicadora. 

Esse mecanismo consiste em:
1. um vetor para armazenar uma lista de referências aos objetos do assinante 
2. alguns métodos públicos que permitem adicionar assinantes e removê-los da lista.

Agora, sempre que um evento importante acontece com a `publicadora`, ele passa para seus `assinantes` e chama um método específico de notificação em seus objetos.

É crucial que todos os `assinantes` implementem a mesma interface e que a publicadora comunique-se com eles apenas através daquela interface. 
Essa interface deve declarar o `método de notificação` junto com um conjunto de parâmetros que a publicadora pode usar para passar alguns dados contextuais junto com a notificação.

---
## 👁‍🗨 Analogia com o mundo real
Se você assinar um jornal ou uma revista, você não vai mais precisar ir até a banca e ver se a próxima edição está disponível. Ao invés disso a publicadora manda novas edições diretamente para sua caixa de correio após a publicação ou até mesmo com antecedência.

A publicadora mantém uma lista de assinantes e sabe em quais revistas eles estão interessados. Os assinantes podem deixar essa lista a qualquer momento quando desejarem que a publicadora pare de enviar novas revistas para eles.

---
## 📑 Exemplo
+ Interface do Assinante do Observer
```csharp
public interface IAcaoAposGerarNota
{
    void Executa(NotaFiscal nf);
}
```

+ Assinantes:
```csharp
public class EnviadorDeEmail : IAcaoAposGerarNota
{
    public void Executa(NotaFiscal nf)
    {
        Console.WriteLine("email");
    }
}

public class Impressora : IAcaoAposGerarNota
{
    public void Executa(NotaFiscal nf)
    {
        Console.WriteLine("imprimindo Nota Fiscal");
    }
}

public class Multiplicador : IAcaoAposGerarNota
{
    public double Factor { get; set; }

    public Multiplicador(double factor)
    {
        Factor = factor;
    }

    public void Executa(NotaFiscal nf)
    {
        Console.WriteLine(nf.ValorBruto * this.Factor);
    }
}
```

+ Publicador que será observado:
```csharp
public class NotaFiscalBuilder
{
    // ...
    private IList<IAcaoAposGerarNota> todasAcoes = new List<IAcaoAposGerarNota>(); // <-- Lista de assinantes

    public NotaFiscal Constroi()
    {
        NotaFiscal nf = new NotaFiscal(
            RazaoSocial, 
            Cnpj,
            Data, 
            valorTotal,
            impostos,
            todosItens,
            Observacoes);
        
        foreach (IAcaoAposGerarNota acao in todasAcoes) // <-- acoes para os assinantes
        {
            acao.Executa(nf);
        }

        return nf;
    }

    public void AdicionaAcao(IAcaoAposGerarNota novaAcao) // <-- Método para adicionar assinante
    {
        this.todasAcoes.Add(novaAcao);
    }

    // ...
}
```

+ Implementação:
```csharp
static void Main(string[] args)
{
    // Builder em Fluent Interface (Method chaining)
    NotaFiscalBuilder criador = new NotaFiscalBuilder();
    criador
        .ParaEmpresa("Alura")
        .ComCnpj("12.123.123/1234-12")
        .Add(new ItemDaNotaBuilder().SetNome("Item 1").SetValor(100.0).Build())
        .Add(new ItemDaNotaBuilder().SetNome("Item 2").SetValor(200.0).Build())
        .ComObservacoes("Uma obs qualquer");

    // Observer - Adicionando assinantes
    criador.AdicionaAcao(new EnviadorDeEmail());
    criador.AdicionaAcao(new NotaFiscalDao());
    criador.AdicionaAcao(new EnviadorDeSms());
    criador.AdicionaAcao(new Impressora());
    criador.AdicionaAcao(new Multiplicador(5));
    criador.AdicionaAcao(new Multiplicador(10));

    NotaFiscal nf = criador.Constroi();

    Console.WriteLine(nf.ValorBruto);
    Console.WriteLine(nf.Impostos);

    Console.ReadKey();
}
```