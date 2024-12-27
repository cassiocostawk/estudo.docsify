<font face="Calibri">

# ⭐ DDD

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

`Domain-Driven Development`

Design de código orientado ao domínio.

A grande sacada dessa metodologia é a capacidade que ele traz para a modelagem do problema, através do entendimento do mesmo.

Trata-se de uma metodologia de design de software que tem um foco no que está acontecendo no domínio da aplicação.
Em outras palavras, e como o nome sugere, o design é centrado na lógica de negócios (domínio) do software.

+ Modelo de domínio:
Normalmente definido através do padrão Domain Model, essa camada é a base de todo o DDD. É aqui que precisamos definir corretamente nosso modelo de negócios em termos de classes, do contrário teremos problemas posteriores em nosso projeto.

+ Serviços de domínio:
Os serviços de domínio fornecem alguns métodos necessários à aplicação, mas que não estão restritos à uma só entidade – como uma classe do modelo de domínio é chamada. Em linhas gerais, contém regras e comportamentos referentes ao Modelo de domínio.

+ Repositórios de dados:
Os repositórios de dados são definidos através de um padrão: o padrão **Repository**.
A ideia é que o **Modelo** de domínio esteja livre de qualquer definição de infraestrutura de dados, fazendo com que ele siga os princípios **POCO** (Plain Old Common-Runtime Object) e **PI** (Persistence Ignorance) – termos que são utilizados em conjunto para indicar objetos que não possuem comportamentos (métodos) definidos (entidades).

Os três elementos listados acima são essenciais no desenvolvimento utilizando DDD.

## Passo a Passo

### Infrastructure

+ Criar projeto `Biblioteca de classes`.
+ Renomear `Class1` para `NomeProjetoContext`.
+ Instalar o pacote `EntityFramework` nesse projeto.
+ A classe irá extender de `DbContext`, por isso a necessidade do pacote acima.
+ Adicionar referência ao projeto de `Domain` ou `Entity`.

Classe do `DbContext`:

+ Disponibilizar os `DbSets` das Entidades.
+ `Constructor`:
  + Extender a `base` passando o `ConnectionString`.
  + Pode-se definir um `Initializer` com um `Seed`.
  + Definir configurações do entity framework.
+ `OnModelCreating`:
  + Onde é feita a modelagem das tabelas.
  + Registrar os mapeamentos.

---

[`^ topo`](#Dev)
</font>
