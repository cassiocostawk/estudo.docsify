# SOLID

`S` - Single Responsibiity Principle (SRP) [[+]](1_SOLID_SRP.md)
`Princípio de Responsabilidade Única`

+ uma classe deve ter uma e apenas uma razão para mudar.
  
`O` - Open/Closed Principle (OCP) [[+]](2_SOLID_OCP.md)
`Princípio do Aberto/Fechado`

+ objetos devem estar disponíveis para extensão, mas fechados para modificação.

`L` - Liskov Substitution Principle (LSP) [[+]](3_SOLID_LSP.md)
`Princípio da substituição de Liskov`

+ uma subclasse deve ser substituível por sua superclasse.

`I` - Interface Segregation Principle (ISP) [[+]](4_SOLID_ISP.md)
`Princípio da segregação de Interface` 

+ uma classe não deve ser obrigada a implementar métodos e interfaces que não serão utilizadas.
  
`D` - Dependency Inversion Principle (DIP) [[+]](5_SOLID_DIP.md)
`Princípio da Inversão de Dependência`

+ dependa de abstrações e não de implementações.

*Cunhado por Robert C. Martin (Uncle Bob)*

---

## 💸 Divida Técnica

Conforme o programa aumenta, a complexidade também, e a estrutura começa a se deteriorar. 
Você precisa trabalhar para manter e reduzir essa complexidade. 
Se tenho um código mais complexo, tenho mais dificuldade para fazer essa mudança.

**Ward Cunningham** [[+]](../Dicas/Personalidades.md) (Um dos pioneiros do Design Patterns)
foi o primeiro cara que falou sobre complexidade técnica e dívida usando a metáfora:

*`Colocar o código em produção é como entrar numa dívida.`*
*`Quanto maior, mais difícil de pagar.`*

### E como se faz para pagar essa "dívida técnica"?

+ Identificar os problemas através de `Code Smells`. [[+]](7_Code_Smells.md)
+ Usar experiência de Devs para corrigir:
  + `Design Patterns`
  + Princípios do `S.O.L.I.D.`

### 👁‍🗨 Causas da dívida técnica

◽ Pressão comercial
◽ Falta de compreensão das consequências da dívida técnica
◽ Falha ao combater a coerência estrita dos componentes
◽ Falta de testes
◽ Falta de documentação
◽ Falta de interação entre os membros da equipe
◽ Desenvolvimento simultâneo de longo prazo em vários ramos
◽ Refatoração atrasada
◽ Falta de monitoramento de conformidade
◽ Incompetência

---

## 🧹 Código limpo

O principal objetivo da refatoração é combater a dívida técnica. Ele transforma uma bagunça em código limpo e design simples.

Agradável! Mas o que é código limpo, afinal? Aqui estão algumas de suas características:

### ◽ Código limpo é óbvio para outros programadores

E não estou falando de algoritmos super sofisticados. Má nomenclatura de variáveis, classes e métodos inchados, números mágicos - você escolhe - tudo isso torna o código desleixado e difícil de entender.

### ◽ O código limpo não contém duplicação

Cada vez que você precisar fazer uma alteração em um código duplicado, lembre-se de fazer a mesma alteração em todas as instâncias. Isso aumenta a carga cognitiva e retarda o progresso.

### ◽ O código limpo contém um número mínimo de classes e outras partes móveis

Menos código é menos coisas para manter em sua cabeça. Menos código é menos manutenção. Menos código é menos bugs. Código é responsabilidade, mantenha-o curto e simples.

### ◽ Código limpo passa em todos os testes

Você sabe que seu código está sujo quando apenas 95% dos seus testes foram aprovados. Você sabe que está ferrado quando a cobertura do teste é 0%.

### ◽ Código limpo é mais fácil e barato de manter

---

## 🔠 Conceitos

+ Código repetido é um risco para o negócio.
+ Separe as funcionalidades da aplicação em uma camada de serviço
+ Crie o hábito de minimizar a alteração de código pronto
  + Manter essa classe fechada para alteração, dentro do possível
  + Criando novas classes e substituir através da injeção de dependência

---
