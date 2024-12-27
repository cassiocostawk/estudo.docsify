# 📍 SRP
`Single Responsibiity Principle`<br>
`Princípio de Responsabilidade Única`

*Cunhado por Robert C. Martin (Uncle Bob)*

Se preocupa com a coesão do código, tanto métodos quanto classes, e em última instância dos módulos. Sempre importante seu código ser coeso. Não repita a si mesmo. Isso está associado a ter uma fonte de informação no código e no projeto como um todo.

## 🔹 Coesão 
 Ligação, Harmonia, União entre as partes

### ❔ O que é baixa coesão?
Uma classe com baixa coesão faz muitas coisas não relacionadas e leva aos seguintes problemas: 
+ Difícil de entender. 
+ Difícil de reusar. 
+ Difícil de manter.

## 🔹 DRY
`Dont Repeat Yourself` foi cunhado por Andrew Hunt e Dave Thomas e escrito em seu livro Pragmatic Programmer.

Citação do livro:
> "Todo pedaço de conhecimento em um sistema deve possuir uma representação única, inequívoca e confiável."


## 🔹 Single Source of Truth
`Única fonte de informação`

Métodos tem que ser coesos, devem ter uma única responsabilidade.
Classes também devem ter uma única responsabilidade.
Responsabilidade de Método != Responsabilidade de Classe