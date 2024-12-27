<font face="Calibri">

# ⭐ Variáveis

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 🔹 Tipo Valor ou Referência

+ Os Tipos são armazenados em espaços diferentes na memória, stack e heap.
+ A `stack` é usada para alocação de **memória estática** e `heap` para alocação de **memória dinâmica**, ambas armazenadas na RAM do computador.

### Stack

As variáveis ​​alocadas na `stack` são armazenadas diretamente na memória e o acesso a essa memória **é muito rápido**, e sua alocação é tratada **quando o programa é compilado**.

A `stack` é sempre reservada em uma ordem LIFO (last-in-first-out), o bloco reservado mais recente é sempre o próximo bloco a ser liberado. Isso torna muito simples acompanhar a `stack`, liberar um bloco da `stack` nada mais é do que **ajustar um ponteiro**.

### Heap

As variáveis ​​alocadas no heap têm sua memória alocada **em tempo de execução** e o acesso a essa memória **é um pouco mais lento**, mas o tamanho do `heap` é **limitado apenas pelo tamanho da memória virtual**.

O elemento da heap não tem dependências entre si e sempre pode ser acessado aleatoriamente a qualquer momento.

Você pode alocar um bloco a qualquer momento e liberá-lo a qualquer momento. Isso torna muito mais complexo acompanhar quais partes do heap estão alocadas ou livres a qualquer momento.

### Tipo Valor

`Value Types`

+ O valor padrão dos tipos valores são especificados por cada tipo (abaixo mais informações).
+ Um tipo de valor mantém os dados dentro de sua própria alocação de memória.
+ Um tipo valor armazena seu conteúdo na memória alocada na **Stack**.
+ Não aceita ser `null`.

| Palavra-chave do tipo C# | Tipo .NET      |
|--------------------------|----------------|
| bool                     | System.Boolean |
| byte                     | System.Byte    |
| sbyte                    | System.SByte   |
| char                     | System.Char    |
| decimal                  | System.Decimal |
| double                   | System.Double  |
| float                    | System.Single  |
| int                      | System.Int32   |
| uint                     | System.UInt32  |
| nint                     | System.IntPtr  |
| nuint                    | System.UIntPtr |
| long                     | System.Int64   |
| ulong                    | System.UInt64  |
| short                    | System.Int16   |
| ushort                   | System.UInt16  |
| Struct types             |                |
| Enumerações              |                |

+ Exemplo de tipo Valor:

```csharp
int inteiro1 = 10;
int inteiro2 = inteiro1; // copiado de inteiro1 para inteiro2 (= 10)

inteiro1++; // = 11
inteiro2;   // continua = 10
```

+ Nos `Tipos Valores`, as variáveis copiam os valores entre si.

---

### Tipo Referência

`Reference Types`

+ O valor padrão de todo tipo de `referência` é `null`.
+ Os `Tipos Referência` mantém uma **referência (endereço)** ao objeto, mas não para o próprio objeto.
+ Composição:
  + Variável na `stack` armazenando a referência do objeto.
  + Variável na `heap` armazenando o objeto.
+ Quando você cria um tipo por `referência`, como o objeto está em outro lugar e a variável tem só uma **referência** para ele, sendo assim duas variáveis podem apontar para o mesmo objeto.
+ Aceita ser `null`.

| Palavra-chave do tipo C# | Tipo .NET     |
|--------------------------|---------------|
| object                   | System.Object |
| string                   | System.String |
| Arrays                   | System.Object |
| Classes                  | System.Object |
| Delegates                | System.Object |

+ A `string` é um tipo imutável e por isso é um tipo referência.
  + Exemplo do Tipo Referência com `string` imutável:

   ```csharp
   string str1 = "Joao";
   string str2 = str1; // str2 recebe a referência de str1 
   str1 = "Maria"; // criado novo valor na heap e apontada nova referencia
   str2;           // str2 continua apontando pra referencia anterior (= João)
   ```

+ Exemplo do `Tipo Referência`:
  
  ```csharp
  int[] int1 = new int[2];
  int1[0] = 10;
  int1[1] = 20;
  int[] int2 = int1; // int2 recebe a referência de int1 
  
  Console.WriteLine(int1[0]); // 10
  Console.WriteLine(int2[0]); // 10
  
  int1[0] = 30; // alterando o valor do objeto na heap com int1
  Console.WriteLine(int1[0]); // 30
  Console.WriteLine(int2[0]); // 30
  
  int2[1] = 40; // alterando o valor do objeto na heap com int2
  Console.WriteLine(int1[1]); // 40
  Console.WriteLine(int2[1]); // 40
  ```

+ Como visto no exemplo acima:
  + As variáveis copiam a `referência` ao objeto, ou seja, apenas o **endereço de memória** desse objeto.
  + Percebemos que alterando os valores em qualquer das 2 variáveis o valor é alterado nas duas, isso se deve porque as 2 variáveis **apontam** (ponteiro) para o **mesmo objeto**.

---

## 🔹 Inferência de Tipo de Variável

O uso de var é denominado **Inferência de Tipo de Variável**, e é muito comum.

```csharp
var resultadoMetodo = SomarVarios(1, 5, 9);
var conta = new ContaCorrente(344,56456556);
```

---

*... continuar em [DataTypes](DataTypes.md)*

---

[`^ topo`](#Dev)
</font>
