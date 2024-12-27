# Chain of Responsibility

📍 `Padrão comportamental`

Também conhecido como: 
`CoR, Corrente de responsabilidade, Corrente de comando, Chain of command`

---

## 💡 Propósito
+ O `Chain of Responsibility` é um padrão de projeto comportamental que permite que você passe pedidos por uma `corrente de handlers`. Ao receber um pedido, **cada handler decide se processa o pedido ou o passa adiante para o próximo handler na corrente**.

---

## ❗ Problema

+ Imagine que você está trabalhando em um sistema de encomendas online. Você quer restringir o acesso ao sistema para que apenas usuários autenticados possam criar pedidos. E também somente usuários que tem permissões administrativas devem ter acesso total a todos os pedidos.
<br>

+ Após um pouco de planejamento, você se dá conta que essas checagens devem ser feitas sequencialmente. A aplicação pode tentar autenticar um usuário ao sistema sempre que receber um pedido que contém as credenciais do usuário. Contudo, se essas credenciais não estão corretas e a autenticação falha, não há razão para continuar com outras checagens.

---
## ✅ Solução

+ Como muitos outros padrões de projeto comportamental, o Chain of Responsibility se baseia em transformar certos comportamentos em objetos solitários chamados `handlers`. 
<br>
+ O padrão sugere que você ligue esses `handlers` em uma corrente. Cada handler ligado tem um campo para armazenar uma referência ao próximo handler da corrente. 
<br>
+ Além de processar o pedido, `handlers` o passam adiante na corrente. O pedido viaja através da corrente até que todos os `handlers` tiveram uma chance de processá-lo.
<br>
+ E aqui está a melhor parte: um `handler` pode decidir não passar o pedido adiante na corrente e efetivamente parar qualquer futuro processamento.
<br>
+ É crucial que todas as classes `handler` implementem a mesma interface. Cada `handler` concreto deve se importar apenas se o seguinte tem o método executar. Dessa maneira você pode compor correntes durante a execução, usando vários `handlers` sem acoplar seu código com suas classes concretas.

---

## 👁‍🗨 Analogia com o mundo real

+ Falar com o suporte técnico pode ser difícil
Uma chamada para o suporte técnico pode atravessar diversos operadores.
<br>
+ Você acabou de comprar e instalar um novo hardware em seu computador. Contudo, seu amado Linux se recusa a trabalhar com o novo hardware. Você decide ligar para o número do suporte técnico escrito na caixa.
<br>
+ A primeira coisa que você ouve é uma voz robótica do outro lado. Ela sugere nove soluções populares para vários problemas, nenhum dos quais é relevante para seu caso. Após um tempo, a voz robótica conecta você com um operador de carne e osso.
<br>
+ Infelizmente, o operador não foi capaz de sugerir algo específico também.
<br>
+ Eventualmente o operador passa sua chamada para um dos engenheiros. O engenheiro lhe diz onde baixar os drivers apropriados para seu novo hardware e como instalá-los no Linux. 
<br>
+ Você termina sua chamada.

---

## 💡 Aplicabilidade

+ Utilize o padrão Chain of Responsibility quando é esperado que seu programa processe diferentes tipos de pedidos em várias maneiras, mas os exatos tipos de pedidos e suas sequências são desconhecidos de antemão.
+ O padrão permite que você ligue vários handlers em uma corrente e, ao receber um pedido, perguntar para cada handler se ele pode ou não processá-lo. Dessa forma todos os handlers tem a chance de processar o pedido.
<br>
+ Utilize o padrão quando é essencial executar diversos handlers em uma ordem específica.
+ Já que você pode ligar os handlers em uma corrente em qualquer ordem, todos os pedidos irão atravessar a corrente exatamente como você planejou.
<br>
 + Utilize o padrão CoR quando o conjunto de handlers e suas encomendas devem mudar no momento de execução.
+ Se você providenciar setters para um campo de referência dentro das classes handler, você será capaz de inserir, remover, ou reordenar os handlers de forma dinâmica.

---

## 👣 Como implementar

1. Declare a interface do `handler` e descreva a assinatura de um método para lidar com pedidos.
<br>
2. Decida como o cliente irá passar os dados do pedido para o método. A maneira mais flexível é converter o pedido em um objeto e passá-lo para o método `handler` como um argumento.
<br>
3. Para eliminar código padrão duplicado nos handlers concretos, pode valer a pena criar uma classe handler base **abstrata, derivada da interface do handler**.
<br>
4. **Essa classe deve ter um campo para armazenar uma referência ao próximo handler na corrente**. Considere tornar a classe imutável. Contudo, se você planeja modificar correntes no tempo de execução, você precisa definir um setter para alterar o valor do campo de referência.
<br>
5. Você também pode implementar o comportamento padrão conveniente para o método `handler`, que vai passar adiante o pedido para o próximo objeto a não ser que não haja mais objetos. `Handlers` concretos irão ser capazes de usar esse comportamento ao chamar o método pai.

+ Um por um crie **subclasses handler concretas** e **implemente seus métodos handler**. Cada `handler` deve fazer duas decisões ao receber um pedido:

  + Se ele vai processar o pedido.
  + Se ele vai passar o pedido adiante na corrente.
<br>
+ O cliente pode tanto montar correntes sozinho ou receber correntes pré construídas de outros objetos. Neste último caso, você deve implementar algumas **classes fábrica** para construir correntes de acordo com a configuração ou definições de ambiente.
<br>
+ O cliente pode ativar qualquer handler da corrente, não apenas o primeiro. O pedido será passado ao longo da corrente até que algum handler se recuse a passá-lo adiante ou até ele chegar ao fim da corrente.
<br>
+ Devido a natureza dinâmica da corrente, o cliente deve estar pronto para lidar com os seguintes cenários:
  + A corrente pode consistir de um único elo.
  + Alguns pedidos podem não chegar ao fim da corrente.
  + Outros podem chegar ao fim da corrente sem terem sido tratados.

---

## 📑 Exemplo

+ Interface para implementar os `Handlers`:

```csharp
public interface IDesconto
{
    double Desconta(Orcamento orcamento);
    IDesconto Proximo { get; set; } // <-- Próximo elo da corrente
}
```

<br>

+ Classe concreta (`Handler`) que implementa a interface do CoR:

```csharp
public class DescontoPorCincoItens : IDesconto
{
    public IDesconto Proximo { get; set; }
    public double Desconta(Orcamento orcamento) // <-- Próximo elo da corrente
    {
        if (orcamento.Itens.Count > 5) // se true processa como elo final
            return orcamento.Valor * 0.1;

        return Proximo.Desconta(orcamento); // se não processado, executa próximo elo
    }
}
```

<br>

+ Último elo da corrente:

```csharp
public class SemDesconto : IDesconto
{
    public IDesconto Proximo { get; set; } // <-- Próximo elo da corrente, neste caso nulo

    public double Desconta(Orcamento orcamento)
    {
        return 0; // não existe Proximo, então retorna um valor padrão
    }
}
```

<br>

+ Classe que monta a corrente na ordem certa:
  *esta classe não é necessária, mas é necessário montar a corrente*

```csharp
public class CorrenteDescontos
{
    public double Calcula(Orcamento orcamento)
    {
        IDesconto d1 = new DescontoPorCincoItens();
        IDesconto d2 = new DescontoPorMaisDeQuinhentosReais();
        IDesconto d3 = new DescontoPorVendaCasada();
        IDesconto d4 = new SemDesconto();

        // Montagem da Corrente
        d1.Proximo = d2; 
        d2.Proximo = d3;
        d3.Proximo = d4;

        // Executar Métodos da Corrente
        return d1.Desconta(orcamento);

        // É possível a Corrente começar de outro Elo
        return d2.Desconta(orcamento);
    }
}
```

<br>

+ Arrange para iniciar a corrente:

```csharp
static void Main(string[] args)
{
    CalculadorDeDescontos calculador = new CalculadorDeDescontos();

    Orcamento orcamento = new Orcamento(500);
    orcamento.AdicionaItem(new Item("Caneta", 250));
    orcamento.AdicionaItem(new Item("Lapis", 250));
    orcamento.AdicionaItem(new Item("Geladeira", 250));
    orcamento.AdicionaItem(new Item("Fogão", 250));
    orcamento.AdicionaItem(new Item("Microondas", 250));
    orcamento.AdicionaItem(new Item("XBOX", 250));

    double desconto = calculador.Calcula(orcamento); // <-- Inicio da Corrente
    Console.WriteLine(desconto);

    Console.ReadKey();
}
```

---

## 💡 Injeção de Dependência do Próximo

+ Ao receber a dependência pelo construtor, garantimos que o cliente dessas classes nunca esquecerá de passar o próximo ítem da sequência, o que pode facilmente acontecer se esquecermos de colocar um valor na propriedade Proxima.
+ Último elo da corrente não deve ter o Proximo no construtor, pois sendo o último não terá um "Proximo" para informar como atributo. *(observação pessoal)*
