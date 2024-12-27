# Adapter
📍 `Padrão estrutural`

Também conhecido como: `Adaptador`, `Wrapper`

Palavra chave: `Adaptar`

---
## 💡 Propósito
O Adapter é um padrão de projeto estrutural que permite objetos com interfaces incompatíveis colaborarem entre si.

---
## 👁‍🗨 Analogia com o mundo real
Quando você viaja do Brasil para a Europa pela primeira vez, você pode ter uma pequena surpresa quando tenta carregar seu laptop. 
O plugue e os padrões de tomadas são diferentes em diferentes países. 
É por isso que seu plugue do Brasil não vai caber em uma tomada da Alemanha. 
O problema pode ser resolvido usando um `adaptador` de tomada que tenha o estilo de tomada Brasileira e o plugue no estilo Europeu.

---
## 👁‍🗨 Observação
A implementação do Adapter é muito parecida com a implementação do `Command` e do `Strategy`, a diferença está realmente no propósito do padrão, o problema que está sendo resolvido.

`Command` é utilizado quando queremos guardar um bloco de código que será executado posteriormente, como feito no exemplo da fila de trabalho implementada anteriormente.

No `Strategy` estamos tentando implementar uma estratégia diferente para resolver um determinado problema que temos no código. Por exemplo, imagine que a aplicação precisa enviar buscas para diversos bancos de dados diferentes, nesse caso, o código que será executado em cada banco de dados será levemente diferente, mas o propósito é o mesmo: Fazer o acesso aos dados.

Já no `Adapter`, nós temos uma biblioteca ou um sistema antigo cuja interface não se encaixa perfeitamente no sistema atual, nesse caso, podemos adaptar essa interface definida pela biblioteca ou sistema legado utilizando uma nova classe dentro de nosso sistema. Esse é um propósito bem diferente do que o dos outros padrões.