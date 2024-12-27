<font face="Calibri">

# ⭐ Versionamento de API

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

+ Criar um novo controlador `Livros2Controller` para evoluir a API na versão 2
+ Instalar o pacote `Microsoft.ApiVersioning`
+ Anotar os controladores com a versão suportada por eles
+ Mudar a rota dos controladores para usar versionamento no caminho da URL

  ```csharp
    // ...
    [ApiController]
    [ApiVersion("2.0")] // <--
    [Route("api/v{version:apiVersion}/livros")] // <--
    // ...
  ```

  + *É possível usar também na Query ou no Header*

+ Configurar opções de versionamento na classe Startup:
  
  ```csharp
  services.AddApiVersioning();
  ```

  + opção para versionamento via Query ou Header:

    ```csharp
    services.AddApiVersioning(options => {
        options.ApiVersionReader = ApiVersionReader.Combine(
            new QueryStringApiVersionReader("api-version"),
            new HeaderApiVersionReader("api-version"));
        });
    ```

---

[`^ topo`](#⭐-versionamento-de-api)
</font>
