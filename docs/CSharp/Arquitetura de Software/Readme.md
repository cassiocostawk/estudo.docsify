<font face="Calibri">

# 🏗️ Arquitetura de Software

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

Arquitetura é o estudo que ajuda a definir como cada um dos componentes vão conversar entre si dentro da aplicação.

Visão mais alto nível em relação ao Design.
Separação de camadas, pastas da aplicação.

+ Arquitetura é a organização fundamental de um sistema, incluindo seus componentes, o relacionamento entre eles, o ambiente, e os princípios em que se baseia sua construção.
+ Geralmente representados com a descrição de uma estrutura, se utilizando também de um diagrama ou desenho.

---

## 🏛️ Tipos de Arquiteturas

Divididas em arquiteturas distribuídas e monolíticas.

+ Distribuídas:
  componentes do sistema que estão sendo executados em outros processos, geralmente em outros servidores.
+ Monolíticas:
  componentes do sistema executam no mesmo processo, na mesma máquina.

---

## 🧱 Padrões

### Arquitetura Hexagonal

`Ports & Adapters`

---

### Arquitetura Limpa

`Clean Architecture`

[Ver mais](./Arquitetura%20Limpa.md)

---

### DDD

`Domain-Driven Development`

Design de código orientado ao domínio.

[Ver Mais](./DDD.md)

---

### Mensageria

+ Utilizado com um Message Broker.
+ Um app adiciona no app com `Message Broker` uma mensagem/evento.
+ Um terceiro app pode processar essa mensagem/evento do message broker.
+ Comunicação de forma assíncrona.
+ Message Broker também pode ser chamado de Event Bus.
+ A mensagem pode ser configurada para ser lida, 1 ou n vezes, por um ou mais subscribers.

Ex:
Enviar um evento solicitando que um email seja enviado, se envia a mensagem/evento, e não se aguarda um retorno.

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

---

### Modelagem de eventos

`Event Modeling`

A Modelagem de Eventos é um método de descrição de sistemas usando um exemplo de como as informações mudaram dentro deles ao longo do tempo.

#### Fases

+ Brainstorming
+ The plot (Ordenação lógica, timeline)
+ Storyboard (Interfaces)
+ Identificando entradas
+ Identificando saídas
+ Lei de conway (separaçõa lógica dos eventos)
+ Elaboração de cenários

---

### Arquitetura orientada a eventos

`Event-driven Architecture`

+ Event sourcing
  Exemplo é os commits do Git que usam event sourcing, onde o próximo commit armazena a diferença dele com o commit anterior.

---

### API

`Application Programming Interfaces`

#### Boas práticas

+ Use pronomes e não verbos.
  exemplos errados: `/getAllCars`, `createNewCar`, `deleteAllRedCars`...

+ Nâo misture plural e singular.
  se o endpoint que retorna uma lista está no plural, o que retorna 1 também deve estar no plural.

+ Não ignore os cabeçalhos HTTP.

#### Sub-recursos

`GET /cars/711/drivers/4`
retorna o motorista 4 do carro 711.

#### HATEOAS

HATEOAS é uma restrição que faz parte da arquitetura de aplicações REST, cujo objetivo é ajudar os clientes a consumirem o serviço sem a necessidade de conhecimento prévio profundo da API.

#### API Flexível

+ Permitir Filtro
  `GET /cars?color=red`
  `GET /cars?seats<=2`
+ Ordenação
  `GET /cars?sort=-manufacturer,+model`
+ Paginação
  `GET /cars?offset=10&limit=5`
  forneça a forma de navegar na paginação
+ Seleção de campos
  `GET /cars?fields=manufacturer,model,id,color`

#### Versionamento da API

Nâo quebrar o contrato com o seu usuário da API.

`/blog/api/v1/posts?name=Título do post`
mudança de name para title:
`/blog/api/v2/posts?title=Título do post`

#### HTTP Status Codes

1xx `Informação`
2xx `Sucesso`
3xx `Redirecionamento`
4xx `Erro do Cliente`
5xx `Erro do Servidor`

---

[`^ topo`](#🏗️-arquitetura-de-software)
</font>
