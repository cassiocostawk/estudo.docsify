# Bridge
📍 `Padrão estruturais`

Também conhecido como: `Ponte`

---
## 💡 Propósito
O Bridge é um padrão de projeto estrutural que permite que você divida uma classe grande ou um conjunto de classes intimamente ligadas em duas hierarquias separadas—abstração e implementação—que podem ser desenvolvidas independentemente umas das outras.

Exemplos:
+ Classes de TVs e Classes de Controle Remoto.
+ Classes para Formas geométricas (Quadrado, Circulo) e Classe para Pintar essas formas

---
## 💡 Aplicabilidade
+ Utilize o padrão Bridge quando você quer dividir e organizar uma classe monolítica que tem diversas variantes da mesma funcionalidade (por exemplo, se a classe pode trabalhar com diversos servidores de base de dados).
  + Quanto maior a classe se torna, mais difícil é de entender como ela funciona, e mais tempo se leva para fazer mudanças. As mudanças feitas para uma das variações de funcionalidade podem precisar de mudanças feitas em toda a classe, o que quase sempre resulta em erros ou falha em lidar com efeitos colaterais.
<br>
  + O padrão Bridge permite que você divida uma classe monolítica em diversas hierarquias de classe. Após isso, você pode modificar as classes em cada hierarquia independentemente das classes nas outras. Essa abordagem simplifica a manutenção do código e minimiza o risco de quebrar o código existente.
<br>
+ Utilize o padrão quando você precisa estender uma classe em diversas dimensões ortogonais (independentes).
<br>
  + O Bridge sugere que você extraia uma hierarquia de classe separada para cada uma das dimensões. A classe original delega o trabalho relacionado para os objetos pertencentes àquelas hierarquias ao invés de fazer tudo por conta própria.
<br>
+ Utilize o Bridge se você precisar ser capaz de trocar implementações durante o momento de execução.
<br>
 + Embora seja opcional, o padrão Bridge permite que você substitua o objeto de implementação dentro da abstração. É tão fácil quanto designar um novo valor para um campo.
<br>
 *A propósito, este último item é o maior motivo pelo qual muitas pessoas confundem o `Bridge` com o padrão `Strategy`. Lembre-se que um padrão é mais que apenas uma maneira de estruturar suas classes. Ele também pode comunicar intenções e resolver um problema.*

 ---
## 📑 Exemplo

+ Interfaces de 2 abstrações:
```csharp
interface IEnviador
{
    void Envia(IMensagem mensagem);
}

interface IMensagem
{
    IEnviador Enviador { get; set; }

    void Envia();
    string Formata();
}
```

+ Classes Enviadoras:
```csharp
class EnviaPorEmail : IEnviador
{
    public void Envia(IMensagem mensagem)
    {
        Console.WriteLine("Enviando a mensagem por email");
        Console.WriteLine(mensagem.Formata());
    }
}

class EnviaPorSms : IEnviador
{
    public void Envia(IMensagem mensagem)
    {
        Console.WriteLine("Enviando a mensagem por SMS");
        Console.WriteLine(mensagem.Formata());
    }
}
```

+ Classes de Mensagens:
```csharp
class MensagemCliente : IMensagem
{
    private object nome;
    public IEnviador Enviador { get; set; }

    public MensagemCliente(string nome)
    {
        this.nome = nome;
    }        

    public void Envia() // Strategy
    {
        this.Enviador.Envia(this);
    }

    public string Formata()
    {
        return $"Enviando a mensagem para o cliente {nome}";
    }
}

class MensagemAdmin : IMensagem
{
    private string nome;
    public IEnviador Enviador { get; set; }

    public MensagemAdmin(string nome)
    {
        this.nome = nome;
    }

    public void Envia() // Strategy
    {
        this.Enviador.Envia(this);
    }

    public string Formata()
    {
        return $"Enviando a mensagem para o administrador {nome}";
    }
}
```

+ Uso:
```csharp
IMensagem mensagem = new MensagemCliente("Cassio");
IEnviador enviador = new EnviaPorEmail();
mensagem.Enviador = enviador;
mensagem.Envia(); // Strategy

mensagem = new MensagemAdmin("Joao");
enviador = new EnviaPorSms();
mensagem.Enviador = enviador;
mensagem.Envia(); // Strategy
```

#### 👁‍🗨 Observação
Na implementação acima do `bridge`, podemos ver que o `bridge` pode utilizar o `strategy` em sua implementação: a mensagem em seu método `Envia` utiliza o `strategy` para decidir qual é a estratégia de envio que será utilizada pela aplicação.