# Singleton
📍 `Padrão criacional`

Também conhecido como: `Carta única`

O Singleton nos ajuda a ter uma única instância do objeto ao longo do sistema.

+ *É como um `Flyweight`, porém que cuida somente de 1 objeto.*
<br>
+ *Usar com cuidado*
  + *Padrão de projeto que é bem controversos, e que muitas pessoas, atualmente, criticam, mas eles ainda são importantes porque vários sistemas legados ainda utilizam esses padrões de projeto.*

---
## 💡 Propósito
O `Singleton` é um padrão de projeto criacional que permite a você garantir que uma classe tenha apenas uma instância, enquanto provê um ponto de acesso global para essa instância.

---
## 👁‍🗨 Analogia com o mundo real
O governo é um excelente exemplo de um padrão Singleton. Um país pode ter apenas um governo oficial. 
Independentemente das identidades pessoais dos indivíduos que formam governos, o título, “O Governo de X”, é um ponto de acesso global que identifica o grupo de pessoas no command.

## ❗ Problemas
O `Singleton` só tem uma instância, então essa instância acaba sendo uma **variável global** compartilhada em todo o projeto. E a programação procedural já provou que variáveis globais geram mais problema do que elas resolvem. 
Então, o `Singleton` é muito criticado exatamente porque ele cria essa **variável global**. 
E, se você não sabe, se você não tem certeza de como você pode evitar os problemas das variáveis globais, você não deveria utilizar esse padrão `Singleton`.

---
## 📑 Exemplo

+ Classe Servico como Singleton:
```csharp
public class Servico 
{
    public Servico() { }

      // outros metodos aqui
}

public class ServicoSingleton 
{
    // tem que ser static para ter uma unica instancia na aplicacao
    private static Servico instancia = new Servico();

    public Servico Instancia
    {
        get
        {
            return instancia;
        }
    }
}
```