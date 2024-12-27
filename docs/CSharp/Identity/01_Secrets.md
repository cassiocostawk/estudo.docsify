<font face="Calibri">

# 🔑 Secrets

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 💻 Comando

+ No caminho do **Projeto** executar o comando:
`dotnet user-secrets init`
<br>
+ Exemplo do caminho do Projeto:
`D:\DotNet\Solução\Projeto`
<br>
+ No `csproj` será adicionada uma `UserSecretsId` conforme abaixo:

```xml
    <Project Sdk="Microsoft.NET.Sdk.Web">

   <PropertyGroup>
       <TargetFramework>net5.0</TargetFramework>
🔹     <UserSecretsId>126c534b-9999-9999-9999-82a98d90bfff</UserSecretsId>
   </PropertyGroup>
...
```

+ Depois pode criar é possível inserir os segredos:
`dotnet user-secrets set "EmailSettings:From" "email@gmail.com"`
`dotnet user-secrets set "EmailSettings:SmtpServer" "smtp.gmail.com"`
`dotnet user-secrets set "EmailSettings:Port" "465"`
`dotnet user-secrets set "EmailSettings:Password" "senha"`

<br>

+ Depois disso somente passar a ler dos Secrets configurando no `Program.cs`:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseStartup<Startup>();
            })
        .ConfigureAppConfiguration((context, builder) => builder.AddUserSecrets<Program>()); 📌
}
```

+ Esse secret é gravado em:
`"%AppData%\Roaming\Microsoft\UserSecrets\126c534b-9999-9999-9999-82a98d90bfff\secrets.json"`
<br>
+ Comando para listar os secrets salvos:
`dotnet user-secrets list`

---

[`^ topo`](#🔑-secrets)
</font>
