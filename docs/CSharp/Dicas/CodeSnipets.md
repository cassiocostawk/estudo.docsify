<font face="Calibri">

# 💻 CodeSnipets

[`◀️ voltar`](../Readme.md)
[`⬆️ inicio`](../../README.md)

---

## 📍 Construtor

+ `ctor` + `tab` + `tab`

```csharp
public class MyClass
{
    public MyClassConstructor() // <--
    {

    }          
}
```

---

## 📍 Propriedade

### Pública

+ `prop` + `tab` + `tab`
`public int MyProperty { get; set; }`

#### Pública, Set Privado

+ `propg` + `tab` + `tab`
`public int MyProperty { get; private set; }`

#### Pública com Var Privada

+ `propfull` + `tab` + `tab`

```csharp
private int myVar;

public int MyProperty
{
    get { return myVar; }
    set { myVar = value; }
}
```

#### Somente Leitura

+ Exemplo:
`public string MyProperty { get; }`

#### Privada Somente Leitura

+ So pode ser atribuída pelo construtor ou explicitamente;
+ Exemplo:

```csharp
private readonly string _myVar; // somente no construtor
private readonly string _myVar = "explicitamente";
```

---

[`^ topo`](#💻-codesnipets)
</font>
