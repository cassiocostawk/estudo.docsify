# State
📍 `Padrão comportamental`

Também conhecido como: `Estado`

---
## 💡 Propósito
O `State` é um padrão de projeto comportamental que permite que um objeto altere seu comportamento quando seu estado interno muda. Parece como se o objeto mudasse de classe.

---
## ❗ Problema
O padrão State é intimamente relacionado com o conceito de uma Máquina de Estado Finito.

Imagine que nós temos uma classe `Documento`. Um documento pode estar em um de três estados: `Rascunho`, `Moderação` e `Publicado`. O método `publicar` do documento funciona um pouco diferente em cada estado:

+ No `Rascunho`, ele move o documento para a moderação.
+ Na `Moderação` ele torna o documento público, mas apenas se o usuário atual é um administrador.
+ No `Publicado` ele não faz nada.

É muito difícil prever todos os possíveis estados e transições no estágio inicial de projeto. 
Portanto, uma máquina de estados enxuta, construída com um número limitado de condicionais pode se tornar uma massa inchada e disforme com o tempo.

---
## ✅ Solução

O padrão `State` sugere que você crie novas `classes` para todos os `estados` possíveis de um objeto e extraia todos os comportamentos específicos de estados para dentro dessas classes.

Ao invés de implementar todos os comportamentos por conta própria, o objeto original, chamado `contexto`, armazena uma referência para um dos objetos de estado que representa seu estado atual, e delega todo o trabalho relacionado aos estados para aquele objeto.

Essa estrutura pode ser parecida com o padrão `Strategy`, mas há uma diferença chave. No padrão `State`, os estados em particular podem estar cientes de cada um e iniciar transições de um estado para outro, enquanto que estratégias quase nunca sabem sobre as outras estratégias.

---
## 📑 Exemplo

+ State Interface:
```csharp
public interface IEstadoDaConta
{
    void Saca(Conta conta, double valor);
    void Deposita(Conta conta, double valor);
}
```

+ State Classes:
```csharp
public class Positivo : IEstadoDaConta
{
    public void Deposita(Conta conta, double valor)
    {
        conta.Saldo -= valor;
    }

    public void Saca(Conta conta, double valor)
    {
        conta.Saldo += valor * 0.98;
    }
}

public class Negativo : IEstadoDaConta
{
    public void Deposita(Conta conta, double valor)
    {
        conta.Saldo += valor * 0.95;

        if (conta.Saldo > 0) conta.Estado = new Positivo();
    }

    public void Saca(Conta conta, double valor)
    {
        throw new Exception("Saldo insuficiente.");
    }
}
```

+ Classe em uso dos estados:
```csharp
public class Conta
{
    // ...
    public IEstadoDaConta Estado { get; set; } 

    // ...

    public void Saca(double valor) 
    {
        Estado.Saca(this, valor);
    }

    public void Deposita(double valor) 
    {
        Estado.Deposita(this, valor);
    }
}
```

---
## 👁‍🗨 Analogia com o mundo real
Os botões e interruptores de seu smartphone comportam-se de forma diferente dependendo do estado atual do dispositivo:

+ Quando o telefone está desbloqueado, apertar os botões leva eles a executar várias funções.
+ Quando o telefone está bloqueado, apertar qualquer botão leva a desbloquear a tela.
+ Quando a carga da bateria está baixa, apertar qualquer botão mostra a tela de carregamento.