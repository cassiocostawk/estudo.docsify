<font face="Calibri">

# Design Patterns

[`⬆️ inicio`](../Readme.md)

---

Visão mais baixo nível em relação a Arquitetura.
Como escrever cada classe, quais padrões aplicar.

+ Padrões de projeto (design patterns) são soluções típicas para problemas comuns em projeto de software.
+ Um padrão de projeto é uma solução elegante para um problema que é recorrente no dia-a-dia do desenvolvedor.
+ Cada padrão é como uma planta de construção que você pode customizar para resolver um problema de projeto particular em seu código.

Os três principais padrões de projeto definidos pelo livro “Design Patterns: Elements of Reusable Object-Oriented Software” de 1994, escrito por **GOF** (Gang of Four: Rich Gamma, Richard Helm, Ralph Johnson e John Vlissides) são os padrões criacionais, estruturais e comportamentais. Esses padrões foram divididos e agrupados de acordo com a natureza do problema que eles solucionam.

**Lembre-se. Aprenda a motivação de cada padrão, e aí use-os sempre que precisar.**

---

## 📍 Tipos

### ◽ Padrões criacionais

Estes padrões oferecem diversas alternativas de criação de objetos, o que aumenta a flexibilidade e a reutilização de código. Alguns dos principais padrões desse tipo são:

+ Factory Method
+ Abstract Factory
+ Builder

### ◽ Padrões estruturais

Nos mostram como montar objetos e classes em estruturas maiores, sem perder a eficiência e flexibilidade. Alguns dos principais padrões desse tipo são:

+ Adapter
+ Bridge
+ Composite

### ◽ Padrões comportamentais

Nos ajudam a trabalhar melhor com os algoritmos e com a delegação de responsabilidades entre os objetos. Os padrões que se destacam nesse tipo são:

+ Chain of Responsibility
+ Command
+ Interpreter

---

## Padrões

+ [Strategy](./02%20Strategy.md)
+ [Chain of Responsibility](./03%20Chain%20of%20Responsibility.md)
+ [Template Method](./04%20Template%20Method.md)
+ [Decorator](./05%20Decorator.md)
+ [State](./06%20State.md)
+ [Builder](./07%20Builder.md)
+ [Observer](./08%20Observer.md)
+ [Factory](./09%20Factory.md)
+ [Flyweight](./10%20Flyweight.md)
+ [Memento](./11%20Memento.md)
+ [Interpreter](./12%20Interpreter.md)
+ [Visitor](./13%20Visitor.md)
+ [Bridge](./14%20Bridge.md)
+ [Command](./15%20Command.md)
+ [Adapter](./16%20Adapter.md)
+ [Façade](./17%20Façade.md)
+ [Singleton](./18%20Singleton.md)

---

## 🔠 Classificação dos padrões

+ Padrões de projeto diferem por sua complexidade, nível de detalhes, e escala de aplicabilidade ao sistema inteiro sendo desenvolvido.
<br>
+ Os padrões mais básicos e de baixo nível são comumente chamados `idiomáticos`. Eles geralmente se aplicam apenas à uma única linguagem de programação.
<br>
+ Os padrões mais universais e de alto nível são os `padrões arquitetônicos`; desenvolvedores podem implementar esses padrões em praticamente qualquer linguagem. Ao contrário de outros padrões, eles podem ser usados para fazer o projeto da arquitetura de toda uma aplicação.
<br>
+ Além disso, todos os padrões podem ser categorizados por seu propósito, ou intenção.
+ **Esses são os três grupos principais de padrões:**
<br>
  + `Padrões Criacionais`
  fornecem mecanismos de criação de objetos que aumentam a flexibilidade e a reutilização de código.
  <br>
  + `Padrões Estruturais`
  explicam como montar objetos e classes em estruturas maiores, enquanto ainda mantém as estruturas flexíveis e eficientes.
  <br>
  + `Padrões Comportamentais`
  cuidam da comunicação eficiente e da assinalação de responsabilidades entre objetos.

## 🧔🏻‍♂️ Termos

+ `Coesão` é o nome que se dá para quando uma classe tem responsabilidade específica.

### Site Refactoring Guru

+ Site intuitivo sobre Padrões de Projeto.

<https://refactoring.guru/pt-br/design-patterns>

---

## Benefícios

### Quais os benefícios que padrões de projeto trazem a um sistema OO

Padrões de projeto são alternativas para que o desenvolvedor consiga escrever código com responsabilidades mais bem definidas, com um baixo acoplamento, e que evolua de maneira natural. Características essas que não são encontrados em sistemas procedurais, que tipicamente apresentam código complexo, cheio de ifs, e que fazem muita coisa, tornando a manutenção custosa.

Códigos que fazem bom uso de OO evoluem geralmente não pela adição de mais um if, mas sim pela criação de mais uma estratégia, mais um observador, mais um estado, e assim por diante.

Padrões de projeto no fim apenas fazem bom uso da orientação a objetos, e seus conceitos e mecanismos, como encapsulamento, abstrações, interfaces, polimorfismo, e etc.

---

### Usar padrões 100% do tempo é o ideal?

Não. Padrões de projeto trazem grande flexibilidade ao seu código, mas isso tem um preço: complexidade. Naturalmente um código que faz uso de padrões de projeto é, do ponto de vista de implementação, mais complexo do que um código que não os utiliza. Afinal, como tudo está desacoplado, o desenvolvedor precisa entender melhor a solução para entender o que o código faz como um todo.

Um bom desenvolvedor sabe fazer essa avaliação e entender quais os ganhos e perdas da utilização de um padrão. Em um problema simples de resolver, talvez o uso de um padrão de projeto não faça sentido; o código vai apenas ficar mais complicado. Agora, em problemas que são por natureza complexos, ou que demandam flexibilidade, pois mudam o tempo todo, talvez a utilização de um padrão de projeto traga benefícios, já que todos os padrões sempre agregam flexibilidade ao código gerado.

Nunca se esqueça: a sua experiência é fundamental! Padrões de projeto são mais uma ferramenta. Eles devem ser um meio para se chegar à solução final, e não o "fim", ou seja, o seu objetivo principal.

---

## 💻 Links Úteis

🖱 [Design patterns: Breve introdução aos padrões de projeto](https://www.alura.com.br/artigos/design-patterns-introducao-padroes-projeto)

🖱 [Refactoring Guru](https://refactoring.guru/pt-br/design-patterns)

---

[`^ topo`](#design-patterns)
</font>
