# Command
📍 `Padrão comportamental`

Também conhecido como: `Comando`, `Ação`, `Action`, `Transação`, `Transaction`

Palavra chave: `Fila`

---
## 💡 Propósito
O `Command` é um padrão de projeto comportamental que transforma um pedido em um objeto independente que contém toda a informação sobre o pedido. 
Essa transformação permite que você parametrize métodos com diferentes pedidos, atrase ou coloque a execução do pedido em uma fila, e suporte operações que não podem ser feitas.

---
## 👁‍🗨 Analogia com o mundo real
Após uma longa caminhada pela cidade, você chega a um restaurante bacana e senta numa mesa perto da janela. 
Um garçom amigável se aproxima e rapidamente recebe seu pedido, escrevendo-o em um pedaço de papel. 
O garçom vai até a cozinha e prende o pedido em uma parede. 
Após algum tempo, o pedido chega até o chef, que o lê e cozinha a refeição de acordo. 

O cozinheiro coloca a refeição em uma bandeja junto com o pedido. O garçom acha a bandeja, verifica o pedido para garantir que é aquilo que você queria, e o traz para sua mesa.

O pedido no papel serve como um comando. 
Ele permanece em uma fila até que o chef esteja pronto para serví-lo. 
O pedido contém todas as informações relevantes necessárias para cozinhar a refeição. 
Ele permite ao chef começar a cozinhar imediatamente ao invés de ter que ir até você para clarificar os detalhes do pedido pessoalmente.

---
## ❔ Qual a diferença entre Command e Strategy?
Em termos de implementação, eles são bem parecidos, pois dependem de interfaces.

A ideia do `Command` é abstrair um comando que deve ser executado, pois não é possível executá-lo naquele momento (pois precisamos por em uma **fila** ou coisa do tipo).

Já no `Strategy`, a ideia é que você tenha uma **estratégia** (um algoritmo) para resolver um problema.