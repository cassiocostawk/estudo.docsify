<font face="Calibri">

# 📨 Mensageria

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Mensageria

+ Utilizado com um Message Broker.
+ Um app adiciona no app com `Message Broker` uma mensagem/evento.
+ Um terceiro app pode processar essa mensagem/evento do message broker.
+ Comunicação de forma assíncrona.
+ Message Broker também pode ser chamado de Event Bus.
+ A mensagem pode ser configurada para ser lida, 1 ou n vezes, por um ou mais subscribers.

> Ex:
> Enviar um evento solicitando que um email seja enviado, se envia a mensagem/evento, e não se aguarda um retorno.

#### Pub/Sub (Google)

Publisher > Message Broker > Subscribers

#### Analogia

*Envio de encomendas.*
Onde o **Publisher** é o **Remetente**,
**Subscriber(s)** seria o **Destinatário(s)**
e o **Message Broker** seria os **Correios**.

#### Sistemas de mensageria

+ RabbitMQ (open source)
+ Amazon SQS
+ Pub/Sub (Google)
+ Apache Kafka
+ Azure Service Bus

---

## Mensageria com RabbitMQ

Devido a comunicação síncrona dificultar a resiliência em arquitetura de microsserviços, a asíncrona é preferida.

Na Mensageria as mensagens, que contém os dados, são enviadas a um Message Broker, que é um intermediário na
comunicação assíncrona.

Um processo, chamado de consumer, então retira a mensagem da fila, processando-a.

O Message Broker, entre outras funções, gerencia filas e tópicos, e garante que as mensagens que estão neles sejam
entregues a consumers ou subscribers.

Entre as tecnologias mais utilizadas estão o RabbitMQ, Apache Kafka e ActiveMQ

### RabbitMQ

O **RabbitMQ** está entre os **Message Brokers** mais populares, e tem uma grande quantidade de bibliotecas oficiais para
diversas linguagens/ Frameworks.

Entre suas principais funcionalidades, estão:

+ Tópicos
+ Deadletter queues
+ Agendamento de entregas
+ Envio em batch (ou lotes)
+ Transações
+ Deduplicação

Fila é o principal conceito em mensagería, e representa uma estrutura onde as mensagens são armazenadas e consumidas.

Entre suas principais características, estão:

+ **Durável**
  + Quando uma fila é marcada como durável, ela permanece existindo mesmo que o message broker seja reiniciado.
    Isso é útil para garantir que as filas persistam mesmo após falhas ou reinicializações.
+ **Auto-Delete**
  + Uma fila marcada como auto-delete é automaticamente excluída quando o último consumidor se desinscreve.
    Isso é útil para filas temporárias que são usadas 1 ou mais consumidores.
    Quando o consumidor se desconecta, a fila é removida.
+ **Exclusiva**
  + Filas exclusivas só podem ser acessadas pela conexão atual e são excluídas quando essa conexão é encerrada.
    Elas são úteis quando você precisa limitar uma fila a apenas um consumidor.

Outros conceitos importantes de se entender são os de **Exchange** e **Routing Key**:

+ **Exchange**
  agentes responsáveis por rotear as mensagens para filas, utilizando atributos de cabeçalho, routing keys, ou bindings
+ **Routing Key**
  funciona como um relacionamento entre um Exchange e uma Fila, descrevendo para qual fila a mensagem deve ser direcionada

---

[`^ topo`](#📨-mensageria)
</font>
