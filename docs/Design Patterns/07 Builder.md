# Builder
📍 `Padrão criacional`

Também conhecido como: `Construtor`

---
## 💡 Propósito
O `Builder` é um padrão de projeto criacional que permite a você construir objetos complexos passo a passo. O padrão permite que você produza diferentes tipos e representações de um objeto usando o mesmo código de construção.

---
## ❗ Problema
Para construir uma casa simples, você precisa construir quatro paredes e um piso, instalar uma porta, encaixar um par de janelas, e construir um teto. Mas e se você quiser uma casa maior e mais iluminada, com um jardim e outras miudezas (como um sistema de aquecimento, encanamento, e fiação elétrica)?

A solução mais simples é estender a classe base `Casa` e **criar um conjunto de subclasses** para cobrir todas as combinações de parâmetros. Mas eventualmente você acabará com um número considerável de subclasses. Qualquer novo parâmetro, tal como o estilo do pórtico, irá forçá-lo a aumentar essa hierarquia cada vez mais.

Há outra abordagem que não envolve a propagação de subclasses. Você pode **criar um construtor gigante** diretamente na classe `Casa` base com todos os possíveis parâmetros que controlam o objeto casa. Embora essa abordagem realmente elimine a necessidade de subclasses, ela cria outro problema.

Na maioria dos casos a maioria dos parâmetros não será usada, tornando as chamadas do construtor em algo feio de se ver. Por exemplo, apenas algumas casas têm piscinas, então os parâmetros relacionados a piscinas serão inúteis nove em cada dez vezes.

---
## ✅ Solução
O padrão `Builder` sugere que você extraia o código de construção do objeto para fora de sua própria classe e mova ele para objetos separados chamados builders. “Builder” significa “construtor”.

>*O padrão Builder permite que você construa objetos complexos passo a passo.* 
*O Builder não permite que outros objetos acessem o produto enquanto ele está sendo construído.*

O padrão organiza a construção de objetos em uma série de etapas (construirParedes, construirPorta, etc.). 

Para criar um objeto você executa uma série de etapas em um objeto builder. 
A parte importante é que você **não precisa chamar todas as etapas**. 
Você chama **apenas aquelas etapas que são necessárias** para a produção de uma configuração específica de um objeto.

>Por exemplo, paredes de uma cabana podem ser construídas com madeira, mas paredes de um castelo devem ser construídas com pedra.
> 
> Nesse caso, você pode criar diferentes classes construturas que implementam as mesmas etapas de construção, mas de maneira diferente. 
> Então você pode usar esses builders no processo de construção (i.e, um pedido ordenado de chamadas para as etapas de construção) para produzir diferentes tipos de objetos.

### ❕ Classe Diretor
Você pode ir além e extrair uma série de chamadas para as etapas do `builder` que você usa para construir um produto em uma classe separada chamada `diretor`. A classe `diretor` define a ordem na qual executar as etapas de construção, enquanto que o builder provê a implementação dessas etapas.

*O diretor sabe quais etapas de construção executar para obter um produto que funciona.*

---
## 📑 Exemplo

+ Builder
```csharp
public class NotaFiscalBuilder
{
    public string RazaoSocial { get; private set; }
    public string Cnpj { get; private set; }
    public string Observacoes { get; private set; }
    public DateTime Data { get; private set; }

    private double valorTotal;
    private double impostos;
    private IList<ItemDaNota> todosItens = new List<ItemDaNota>();

    public NotaFiscalBuilder()
    {
        Data = DateTime.Now; // preenchimento padrão para metodo poder ser opcional
    }

    // classe não apresentada, por ser obvia a sua implementação
    public NotaFiscal Constroi()
    {
        // retorna nova NotaFiscal usando seu construtor passando as props
        // como argumento de seu enorme construtor
        return new NotaFiscal(
            RazaoSocial, 
            Cnpj,
            Data, 
            valorTotal,
            impostos,
            todosItens,
            Observacoes);
    }

    public NotaFiscalBuilder ParaEmpresa(string razaoSocial)
    {
        this.RazaoSocial = razaoSocial;
        return this;
    }

    public NotaFiscalBuilder ComCnpj(string cnpj)
    {
        this.Cnpj = cnpj;
        return this;
    }

    public NotaFiscalBuilder ComItem(ItemDaNota item)
    {
        todosItens.Add(item);
        valorTotal += item.Valor;
        impostos += item.Valor * 0.05;
        return this;
    }

    public NotaFiscalBuilder ComObservacoes(string observacoes)
    {
        this.Observacoes = observacoes;
        return this;
    }

    public NotaFiscalBuilder NaData(DateTime data) // metodo opcional
    {
        this.Data = data;
        return this;
    }
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
        .ComItem(new ItemDaNota("Item 1", 100.0))
        .ComItem(new ItemDaNota("Item 2", 200.0))
        .NaDataAtual()
        .ComObservacoes("Uma obs qualquer");

    NotaFiscal nf = criador.Constroi();

    // ...
}
```