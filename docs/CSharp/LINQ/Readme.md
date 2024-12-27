<font face="Calibri">

# ⭐ LINQ

[`◀️ voltar`](../Readme.md)
[`⬆️ inicio`](../../README.md)

---

`LINQ` refere-se tanto à **linguagem integrada de consulta** (Language-Integrated Query) quanto à biblioteca de métodos de extensão associada que permite consultas em coleções de dados em C# e outras linguagens .NET.

#### Linguagem Integrada de Consulta (LINQ)

Refere-se à sintaxe de consulta que permite escrever expressões de consulta diretamente no código, tornando a consulta parte integrante da linguagem.

Por exemplo, a sintaxe `from c in pieces select c.ID` é um exemplo de LINQ em sintaxe de consulta.

#### Biblioteca de Métodos de Extensão LINQ

Refere-se à biblioteca de métodos de extensão que fornece operações de consulta padrão em coleções.

Por exemplo, `pieces.Where(c => c.ID == 1)` é um exemplo de LINQ em sintaxe de método, onde **Where é um método de extensão LINQ** e o arqgumento da função é um **lambda**.

### Para distinguir entre eles em uma conversa

#### LINQ `Sintaxe de Consulta`

+ "Estou usando LINQ para consultar os IDs dos objetos em 'pieces'."

#### LINQ `Métodos de Extensão`

+ "Estou usando LINQ para filtrar os objetos em 'pieces' com base em alguma condição."

Ambas as formas são partes integrantes do ecossistema LINQ em C#, e a escolha entre elas muitas vezes se resume à preferência de estilo ou requisitos específicos do problema em questão.

---

+ [ ] [Lista genérica](./LINQ%20Lista%20generica.md)
+ [ ] [Lista numérica](./LINQ%20Lista%20numerica.md)

---

[`^ topo`](#Dev)
</font>
