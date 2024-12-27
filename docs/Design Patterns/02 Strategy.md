# Strategy
📍 `Padrão comportamental`

Palavra chave: `Estratégia`

+ O `Strategy` é um `padrão de projeto comportamental` que permite que você defina uma família de algoritmos, coloque-os em classes separadas, e faça os objetos deles intercambiáveis.
<br>

---
## 💡 Aplicabilidade 
+ Utilize o padrão `Strategy` quando você quer usar diferentes variantes de um algoritmo dentro de um objeto e ser capaz de trocar de um algoritmo para outro durante a execução.
<br>
+ Utilize o `Strategy` quando você tem muitas classes parecidas que somente diferem na forma que elas executam algum comportamento.
<br>
+ Utilize o padrão para isolar a lógica do negócio de uma classe dos detalhes de implementação de algoritmos que podem não ser tão importantes no contexto da lógica.
<br>
+ Utilize o padrão quando sua classe tem um operador condicional muito grande que troca entre diferentes variantes do mesmo algoritmo.

## 🔍 Explicação
+ O padrão `Strategy` sugere que você pegue uma classe que faz algo específico em diversas maneiras diferentes e extraia todos esses algoritmos para classes separadas chamadas estratégias.
<br>

+ A classe original, chamada `contexto`, deve ter um campo para armazenar uma referência para um dessas estratégias. O `contexto` delega o trabalho para um `objeto estratégia` ao invés de executá-lo por conta própria.
<br>

+ O `contexto` não é responsável por selecionar um algoritmo apropriado para o trabalho. Ao invés disso, o cliente passa a `estratégia` desejada para o `contexto`. Na verdade, o `contexto` não sabe muito sobre as estratégias. Ele trabalha com todas elas através de uma `interface genérica`, que somente expõe um **único método** para **acionar o algoritmo encapsulado dentro da estratégia selecionada**.

---
## 💡 Idéia
+ Eu tenho uma interface e implemento estratégias embaixo dessa interface, passo essas estratégias para o resto do meu sistema e o meu sistema funciona de maneira genérica independente de imposto específico ou qualquer coisa específica que você esteja criando.

---
## 📑 Exemplo
+  Interface imposto, implementações embaixo, desde que no fim das contas o ICMS e ISS são estratégias de cálculo de imposto, o nome desse padrão de projeto é `Strategy`.
<br>

+ Classe que executa o método em comum das classes que implementam IImposto:
```csharp
public class CalculadorDeImpostos
{
    public void RealizaCalculo(Orcamento orcamento, IImposto imposto) // <-- Polimorfismo em IImposto
    {
        double valor = imposto.Calcula(orcamento); 
        Console.WriteLine(valor);
    }
}    
```

+ Interface IImposto:
```csharp
public interface IImposto
{
    double Calcula(Orcamento orcamento);
}
```

+ Classes que implementam a interface, que seriam as *Classes Estratégicas*:
```csharp
public class ICMS : IImposto
{
    public double Calcula(Orcamento orcamento)
    {
        return orcamento.Valor * 0.1;
    }
}

public class ISS : IImposto
{
    public double Calcula(Orcamento orcamento)
    {
        return orcamento.Valor * 0.06;
    }
}
```