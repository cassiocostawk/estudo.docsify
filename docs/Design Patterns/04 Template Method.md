# Template Method
📍 `Padrão comportamental`

Também conhecido como: `Método padrão`

---
## 💡 Propósito
O `Template Method` é um padrão de projeto comportamental que define o esqueleto de um algoritmo na superclasse mas deixa as subclasses sobrescreverem etapas específicas do algoritmo sem modificar sua estrutura.

---
## ❗ Problema
Imagine que você está criando uma aplicação de mineração de dados que analisa documentos corporativos. Os usuários alimentam a aplicação com documentos em vários formatos (PDF, DOC, CSV), e ela tenta extrair dados significativos desses documentos para um formato uniforme.

A primeira versão da aplicação podia funcionar somente com arquivos DOC. Na versão seguinte, ela era capaz de suportar arquivos CSV. Um mês depois, você a “ensinou” a extrair dados de arquivos PDF.

Em algum momento você percebeu que todas as três classes tem muito código parecido. Embora o código para lidar com vários formatos seja inteiramente diferente em todas as classes, o código para processamento de dados e análise é quase idêntico. 

---
## ✅ Solução
O padrão do Template Method sugere que você quebre um algoritmo em uma série de etapas, transforme essas etapas em métodos, e coloque uma série de chamadas para esses métodos dentro de um único método padrão. As etapas podem ser tanto abstratas, ou ter alguma implementação padrão. Para usar o algoritmo, o cliente deve fornecer sua própria subclasse, implementar todas as etapas abstratas, e sobrescrever algumas das opcionais se necessário (mas não o próprio método padrão).

---
## 👣 Como implementar
1. Analise o algoritmo alvo para ver se você quer quebrá-lo em etapas. Considere quais etapas são comuns a todas as subclasses e quais permanecerão únicas.
<br>
2. Crie a classe abstrata base e declare o método padrão e o conjunto de métodos abstratos representando as etapas do algoritmo. Contorne a estrutura do algoritmo no método padrão ao executar as etapas correspondentes. Considere tornar o método padrão como final para prevenir subclasses de sobrescrevê-lo.
<br>
3. Tudo bem se todas as etapas terminarem sendo abstratas. Contudo, alguns passos podem se beneficiar de ter uma implementação padrão. Subclasses não tem que implementar esses métodos.
<br>
4. Pense em adicionar ganchos entre as etapas cruciais do algoritmo.

5. Para cada variação do algoritmo, crie uma nova subclasse concreta. Ela deve implementar todas as etapas abstratas, mas pode também sobrescrever algumas das opcionais.

---
## 📑 Exemplo

+ Classe base abstrata:
```csharp
abstract class Relatorio
{
    protected abstract void Cabecalho();
    protected abstract void Rodape();
    protected abstract void Corpo(IList<Conta> contas);

    public void Imprime(IList<Conta> contas)
    {
        Cabecalho();
        Corpo(contas);
        Rodape();
    }
}
```

+ Implementações da Interface:
```csharp
class RelatorioSimples : Relatorio
{
    protected override void Cabecalho()
    {
        Console.WriteLine("Banco XYZ");
    }

    protected override void Rodape()
    {
        Console.WriteLine("(11) 1234-5678");
    }

    protected override void Corpo(IList<Conta> contas)
    {
        foreach (Conta c in contas)
        {
            Console.WriteLine(c.Nome + " - " + c.Saldo);
        }
    }
}

class RelatorioComplexo : Relatorio
{
    protected override void Cabecalho()
    {
        Console.WriteLine("Banco XYZ");
        Console.WriteLine("Avenida Paulista, 1234");
        Console.WriteLine("(11) 1234-5678");
    }

    protected override void Rodape()
    {
        Console.WriteLine("banco@xyz.com.br");
        Console.WriteLine(DateTime.Now);
    }
    protected override void Corpo(IList<Conta> contas)
    {
        foreach (Conta c in contas)
        {
            Console.WriteLine(c.Nome + " - " + c.Numero + " - " + c.Agencia + " - " + c.Saldo);
        }
    }
}
```

---
## 💡 Aplicabilidade

+ Utilize o padrão Template Method quando você quer deixar os clientes estender apenas etapas particulares de um algoritmo, mas não todo o algoritmo e sua estrutura.
<br>
  + Template Method permite que você transforme um algoritmo monolítico em uma série de etapas individuais que podem facilmente ser estendidas por subclasses enquanto ainda mantém intacta a estrutura definida em uma superclasse.
<br>
 + Utilize o padrão quando você tem várias classes que contém algoritmos quase idênticos com algumas diferenças menores. Como resultado, você pode querer modificar todas as classes quando o algoritmo muda.
 <br>
  + Quando você transforma tal algoritmo em um Template Method, você também pode erguer as etapas com implementações similares para dentro de uma superclasse, eliminando duplicação de código. Códigos que variam entre subclasses podem permanecer dentro das subclasses.