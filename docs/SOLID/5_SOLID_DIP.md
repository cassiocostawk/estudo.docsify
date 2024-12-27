# 📍 DIP
`Dependency Inversion Principle`<br>
`Princípio da Inversão de Dependência`

*Cunhado por Robert C. Martin (Uncle Bob)*

A inversão de dependência está preocupada com a qualidade da dependência.

Crie abstrações e dependa delas para melhorar a qualidade do acoplamento. Esse hábito é formalizado através do Princípio da Inversão das Dependências (DIP), a letra D na sigla S.O.L.I.D.

---
## 🔹 Acoplamento
Relacionamento representa uma dependência e dependência representa um acoplamento.

Acoplamentos vão sempre existir. 
Precisamos nos preocupar com a qualidade deles. 

Acoplamento diz respeito à dependência entre dois tipos. 
Num sistema orientado a objetos os acoplamentos são inevitáveis. 
O que devemos fazer é cuidar de sua qualidade. 

Acoplamentos bons são aqueles para tipos estáveis (que não vão mudar ou tem alta probabilidade de não mudar). Candidatos a tipos estáveis são aqueles que fazem parte da plataforma .NET e de APIs muito usadas. 

Acoplamentos ruins são aqueles para tipos instáveis. Exemplos dessa categoria são os tipos criados especificamente para nossa aplicação e principalmente implementações para mecanismos específicos.

### ❔ O que é fraco acoplamento?
É quando um "artefato", tem pouco ou nada de dependência em relação aos outros.

Acoplamento forte - é o inverso do fraco obviamente, sendo quando um "artefato" tem uma grande dependência em relação a outro.

Pode-se dizer também que o projeto num todo tem o Acoplamento baixo/alto, mas não para entre duas classes.

---
## 🔹 Injeção de Dependência
A injeção de dependência se preocupa com quem cria.

### ❔ Quem cria? (na linguagem)
No Asp.Net Core, utilizamos o mecanismo de Injeção de Dependêcia `Dependency Injection` deste.

### 👣 Exemplo de uso 
+ #### *para API (que a construção é feita implicitamente pelo Startup):*
    Explicite as dependências de uma classe. 
    Uma das maneiras de fazer isso é usando parâmetros do construtor. 
    Desse jeito aplicamos um conceito chamado Injeção de Dependência (DI). 
    AspNet Core ajuda a injetar as dependências que foram vinculadas no método `ConfigureServices()` da classe `Startup` e assim dizemos que o AspNet Core tem como uma de suas principais funcionalidades ser um container de injeção de dependências.

    ```csharp
    services.AddTransient<ILeilaoDao, LeilaoDaoComEfCore>();
    ```

---
## 🔹 Inversion of Control (IOC)
Está associado à direção.

É quando você olhando para a classe que você está tentando melhorar percebe que ela perde o controle na criação, comunicação da dependência.

Quando a classe dependente deixa de resolver as dependências diretamente e cede esse controle para outrém temos o uso do conceito Inversão de Controle (IoC).