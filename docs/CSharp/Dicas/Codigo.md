<font face="Calibri">

# ⭐ Dicas de Código

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Diretivas

### 💡 Region

Recurso que visa diminuir visualmente o código e deixá-lo agrupado por tipo de execução, por exemplo: Eventos, Funções e Métodos, Construtores, Enumeradores, Variáveis, etc.

[Leia mais](http://www.linhadecodigo.com.br/artigo/213/%E2%80%9Cregion%E2%80%9D-organizando-o-seu-codigo.aspx#ixzz7RI8EbcFP)

---

## Keywords

### partial

É possível **dividir** uma classe ou struct, uma interface ou um método em dois ou mais arquivos de origem.

Cada arquivo de origem contém uma seção da definição de tipo ou método e **todas as partes são combinadas** quando o aplicativo é compilado.

```csharp
//Arquivo 1
namespace ByteBankImportacaoExportacao
{
    partial class Program // <--
    {
        static void Metodo1()
        {
             // ...
        }

        // ...
    }
}

// Arquivo 2
namespace ByteBankImportacaoExportacao
{
    partial class Program // <--
    {
        static void Metodo2()
        {
             // ...
        }

        // ...
    }
}
```

### using

+ O using só funciona com objetos que implementam a interface IDisposable.

```csharp
using (var fluxoArquivo = new FileStream(enderecoArquivo, FileMode.Open))
{
    // ...

    // no final ele executa o método do IDisposable que nesse aso fecha o arquivo, pra ele nao ficar em uso
}
```

+ É importante lembrar que quando utilizamos a construção `using` no C#, será criado o try e o finaly, que verificará se o fluxo é nulo, e caso não seja, chamaremos o método `Dispose()`, que internamente chamará o método `Close()`.
 
---

[`^ topo`](#⭐-dicas-de-código)
</font>
