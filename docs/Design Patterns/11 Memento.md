# Memento
📍 `Padrão comportamental`

O `Memento` é um padrão de projeto que nos ajuda a salvar e restaurar estados de objetos.

Exemplos simples de Memento:
+ `Ctrl + C` e `Ctrl + V`.
+ O Salvar e Desfazer de um editor de documentos.

## 📑 Estrutura
### 📑 Implementação baseada em classes aninhadas
A implementação clássica do padrão dependente do apoio para classes aninhadas, disponível em muitas linguagens de programação populares (tais como C++, C#, e Java).

+ A classe `Originadora` pode produzir retratos de seu próprio estado, bem como restaurar seu estado de retratos quando necessário. `camera`
<br>
+ O `Memento` é um objeto de valor que age como um retrato do estado da originadora. É uma prática comum fazer o memento **imutável** e passar os dados para ele apenas uma vez, através do construtor. `foto`
<br>
+ A `Cuidadora` sabe não só “quando” e “por quê” capturar o estado da originadora, mas também quando o estado deve ser restaurado. `fotógrafo`
  Uma `cuidadora` pode manter registros do **histórico** da `originadora` armazenando os `mementos` em um **pilha**. Quando a `originadora` precisa voltar atrás no histórico, a `cuidadora` busca o memento mais do topo da pilha e o passa para o **método de restauração** da `originadora`.
<br>
+ Nessa implementação, a classe memento está aninhada dentro da originadora. Isso permite que a originadora acesse os campos e métodos do memento, mesmo que eles tenham sido declarados privados. Por outro lado, a cuidadora tem um acesso muito limitado aos campos do memento, que permite ela armazenar os mementos em uma pilha, mas não permite mexer com seu estado.

### 📑 Implementação baseada em uma interface intermediária

Há uma implementação alternativa, adequada para linguagens de programação que não suportam classes aninhadas (sim, PHP, estou falando de você).

+ Na ausência de classes aninhadas, você pode restringir o acesso aos campos do `memento` ao estabelecer uma convenção para que cuidadoras possam trabalhar com um `memento` através apenas de uma interface intermediária explicitamente declarada, que só declararia os métodos relacionados aos metadados do `memento`.

+ Por outro lado, as originadoras podem trabalhar com um objeto `memento` diretamente, acessando campos e métodos declarados na classe `memento`. O lado ruim dessa abordagem é que você precisa declarar todos os membros do `memento` como públicos.

### 📑 Implementação com um encapsulamento ainda mais estrito

Há ainda outra implementação que é útil quando você não quer deixar a mínima chance de outras classes acessarem o estado da originadora através do `memento`.

+ Essa implementação permite ter múltiplos tipos de `originadoras` e `mementos`. Cada originadora trabalha com uma classe `memento` correspondente. Nem as `originadoras` nem os `mementos` expõem seu estado para ninguém.

+ `Cuidadoras` são agora explicitamente restritas de mudar o estado armazenado nos `mementos`. Além disso, a classe `cuidadora` se torna independente da `originadora` porque o método de restauração agora está definido na classe `memento`.

+ Cada `memento` se torna ligado à originadora que o produziu. A originadora passa a si mesmo para o construtor do `memento`, junto com os valores de seu estado. Graças a relação íntima entre essas classes, um `memento` pode restaurar o estado da sua `originadora`, desde que esta última tenha definido os setters apropriados.

## 💡 Aplicabilidade
+ Utilize o padrão Memento quando você quer produzir retratos do estado de um objeto para ser capaz de restaurar um estado anterior do objeto.
<br>
  + O padrão Memento permite que você faça cópias completas do estado de um objeto, incluindo campos privados, e armazená-los separadamente do objeto. Embora a maioria das pessoas vão lembrar desse padrão graças ao caso “desfazer”, ele também é indispensável quando se está lidando com transações (isto é, se você precisa reverter uma operação quando se depara com um erro).
<br>
+ Utilize o padrão quando o acesso direto para os campos/getters/setters de um objeto viola seu encapsulamento.
<br>
  + O Memento faz o próprio objeto ser responsável por criar um retrato de seu estado. Nenhum outro objeto pode ler o retrato, fazendo do estado original do objeto algo seguro e confiável.