<font face="Calibri">

# 🔑 Customizando o Identity

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 🆕 Criando novos campos no `IdentityUser`

+ Precisamos criar uma classe própria que contenha o campo desejado. Ela deverá estender a classe `IdentityUser`.
+ Exemplo de criação de nova classe que estende o `IdentityUser`:

```csharp
public class CustomIdentityUser : IdentityUser<int>
{
    // esse campo seria um adicional aos campos que já existem no IdentityUser padrão
    public DateTime DataNascimento { get; set; } 
}
```

+ Devido a isso devemos alterar as classes que mapeam e modelam um Usuário:
  + `Model` Usuario:

    ```csharp
    public class Usuario
    {
        [Key]
        [Required]
        public int Id { get; set; }
        public string Username { get; set; }
        public string Email { get; set; }
        public DateTime DataNascimento { get; set; } // <--
    }
    ```

  + `DTO` CreateUsuarioDTO:

    ```csharp
    public class CreateUsuarioDto
    {
        [Required]
        public string Username { get; set; }
        
        [Required]
        public string Email { get; set; }

        [Required]
        [DataType(DataType.Password)]
        public string Password { get; set; }

        [Required]
        [Compare("Password")]
        public string RePassword { get; set; }

        [Required]
        public DateTime DataNascimento { get; set; } // <--

    }    
    ```

  + `Profile` UsuarioProfile, criar um mapeamento de `Usuario` para `CustomIdentityUser`:

    ```csharp
    public class UsuarioProfile : Profile
    {
        public UsuarioProfile()
        {
            CreateMap<CreateUsuarioDto, Usuario>();
            CreateMap<Usuario, IdentityUser<int>>();
            CreateMap<Usuario, CustomIdentityUser>(); // <--
        }            
    }    
    ```

  + E onde estiver referenciando o `IdentityUser` deverá referenciar o `CustomIdentityUser`:

    ```csharp
    UserManager<CustomIdentityUser>
    SignInManager<CustomIdentityUser>
    CustomIdentityUser usuarioIdentity = _mapper.Map<CustomIdentityUser>(usuario);
    //Mapeia de um "usuario" para um CustomIdentityUser

    //Mapper mapeia de uma classe de origem a uma classe de destino:
    CustomIdentityUser exemploUsuario = _mapper.Map<ClasseDestino>(ClasseOrigem);    
    ```

  + `Startup`:

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddDbContext<UserDBContext>(options => options.UseMySQL(Configuration.GetConnectionString("UsuarioConnection")));

        // service.AddIdentity passa a usar o CustomIdentityUser
        services.AddIdentity<CustomIdentityUser, IdentityRole<int>>(opt => // <--
        {
            opt.SignIn.RequireConfirmedEmail = true;
        })            
        .AddEntityFrameworkStores<UserDBContext>()
        .AddDefaultTokenProviders();

        // ...
    }  
    ```

---

[`^ topo`](#🔑-customizando-o-identity)
</font>
