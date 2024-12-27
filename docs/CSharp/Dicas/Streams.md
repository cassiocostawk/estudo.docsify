<font face="Calibri">

# ⭐ Streams

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Stream

+ FileStream
+ CryptoStream
+ NetworkStream
+ MemoryStream
+ BufferedStream

## MarshalByRefObject

+ Text(Reader/Writer) (classe base)
  + String(Reader/Writer)
  + Stream(Reader/Writer)

## Exemplos

### `FileStream` e `StreamReader`

`FileStream`

+ Fornece um `Stream` para um arquivo, dando suporte a operações de leitura e gravação síncronas e assíncronas.

`StreamReader`

+ Implementa um `TextReader` que lê caracteres de um fluxo de bytes em uma codificação específica.

Exemplo do par funcionando:
```csharp
var enderecoArquivo = "contas.txt"; // na raiz do sistema

using (var fluxoArquivo = new FileStream(enderecoArquivo, FileMode.Open))
using (var leitor = new StreamReader(fluxoArquivo)) 
{ 
  var linha = "";
  while (!leitor.EndOfStream)
  {
    linha = leitor.ReadLine();  // le a linha
    Console.WriteLine(linha);
  }

  // ou
  var texto = leitor.ReadToEnd(); // carrega todo arquivo, usar com cautela devido ao limite de memória
  // ou
  var abyte = leitor.Read();      // byte                
}
```

Exemplo simplificado, somente com o `StreamReader`:

```csharp
string line;

using(var sr = new StreamReader("contas.txt"))
{
    while ((line = sr.ReadLine()) != null)
    {
        Console.WriteLine(line);
    }
}
```

### `File`

Fornece **métodos estáticos** para a criação, cópia, exclusão, deslocamento e abertura de um arquivo, além de ajudar na criação de objetos `FileStream`.

Alguns dos principais métodos:

+ `ReadAllText(String)`
  + **Abre** um arquivo de texto, **lê** todo o texto no arquivo, **retorna** uma `string` com o conteúdo e o **fecha**.
+ `ReadAllLines(String)`
  + **Abre** um arquivo de texto, **lê** todas as linhas dele, **retorna** um array de `string[]` com o conteúdo e o **fecha**.
+ `ReadAllBytes`
  + **Abre** um arquivo binário, **lê** o conteúdo do arquivo em uma **matriz de bytes** e, em seguida, **fecha** o arquivo.

Exemplo:
```csharp
// usar quando nao precisa de controlar o buffer pois le o arquivo todo de uma vez
var linhas = File.ReadAllLines("contas.txt");
Console.WriteLine($"Linhas lidas: {linhas.Length }");

foreach (var linha in linhas)
{
    Console.WriteLine(linha);
}
```

---

## Métodos

+ `ReadLine()`
  + Le linha.
+ `ReadToEnd()`
  + Le todo arquivo, usar com cautela devido ao limite de memória.
+ `Read()`
  + Le o byte.

---

## FileMode

Especifica como o sistema operacional deve abrir um arquivo.

### `Append`

Abre o arquivo, caso ele exista, e busca o final do arquivo ou cria um novo arquivo.

Isso requer a permissão Append.

`FileMode.Append` pode ser usado apenas em conjunto com `FileAccess.Write`. 

*Tentar buscar uma posição antes do final do arquivo gera uma exceção `IOException` e qualquer tentativa de leitura falha e gera uma exceção `NotSupportedException`.*

### `Create`

+ Especifica que o SO deve **criar um novo arquivo**.
+ Se o arquivo já existir, ele será **substituído**.
+ Isso requer a permissão `Write`.
+ `FileMode.Create` é equivalente a solicitar que, se o arquivo não existir, `CreateNew` seja usado; caso contrário, `Truncate` deverá ser usado.
+ Se o arquivo já existir, mas for um arquivo oculto, será gerada uma exceção `UnauthorizedAccessException`.

### `CreateNew`

+ Especifica que o SO deve **criar um novo arquivo**.
+ Isso requer a permissão `Write`.
+ Se o arquivo já existir, será gerada uma exceção `IOException`.

### `Open`

+ Especifica que o SO deve **abrir um arquivo existente**.
+ A capacidade de abrir o arquivo depende do valor especificado pela enumeração `FileAccess`.
+ Uma exceção `FileNotFoundException` será gerada se o arquivo não existir.

### `OpenOrCreate`

+ Especifica que o SO deverá **abrir um arquivo**, se ele existir; caso **contrário**, um **novo arquivo deverá ser criado**.
+ Se o arquivo for aberto com `FileAccess.Read`, a permissão `Read` será necessária.
+ Se o acesso ao arquivo for `FileAccess.Write`, a permissão `Write` será necessária.
+ Se o arquivo for aberto com `FileAccess.ReadWrite`, as permissões `Read` e `Write` serão necessárias.

### `Truncate`

+ Especifica que o SO deve **abrir um arquivo existente**.
+ Quando o arquivo é aberto, ele deve ser truncado para que seu tamanho seja de zero byte. + Isso requer a permissão `Write`.
+ As tentativas de ler de um arquivo aberto com `FileMode.Truncate` causam uma exceção `ArgumentException`.

---

## FileAccess

Define constantes para acesso de leitura, gravação ou leitura/gravação para um arquivo.

### `Read`

+ Acesso de **leitura** ao arquivo.
+ Combine com `Write` para acesso de **leitura/gravação**.

### `ReadWrite`

+ Acesso de **leitura e gravação** ao arquivo.

### `Write`

+ Acesso de **gravação** ao arquivo.
+ Combine com `Read` para acesso de **leitura/gravação**.

---

[`^ topo`](#⭐-streams)
</font>
