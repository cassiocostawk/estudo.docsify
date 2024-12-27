# Entidades e Enums

[`🔼 início`](../Readme.md)

## Entidade.cs

```csharp
using Core.Enums;
using Core.Enums.Modulo;
using System;

namespace Core.Entities.Modulo
{
    public class Entidade : BaseEntity
    {
        // TODO: Campos
    }
}
```

## EnumEntidadeStatus.cs
**se necessário*

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Text;

namespace Core.Enums.Modulo
{
    public enum EnumEntidadeStatus
    {
        Interno,
        Externo,
        Apto
    }
}
```

---

[`próximo ▶️`](./02%20ModelConfiguration.md)