<font face="Calibri">

# ⭐ Roles

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

**Roles** funcionam como **cargos** dentro de um sistema.

---

## 👮🏼‍♂️ Criando Role

*na API*

+ São criadas a partir de uma instancia de `RoleManager`:

```csharp
_roleManager.CreateAsync(new IdentityRole<int>("regular")).Result;
```

---

## 👨🏼‍💼 Aplicando Role a Usuário

*na API*

+ São vinculadas a usuários através do `UserManager`:

```csharp
_userManager.AddToRoleAsync(usuarioIdentity, "regular").Result;
```

---

## 👮🏼‍♂️ Criando Usuário Admin e Role Admin no DbContext

+ No `DbContext` iremos **sobrepor** o método `OnModelCreating()`:

```csharp
public class UserDBContext : IdentityDbContext<IdentityUser<int>, IdentityRole<int>, int>
{
    private IConfiguration _configuration; // <-- incluído para acessar as Secrets

    public UserDBContext(DbContextOptions<UserDBContext> opt, IConfiguration configuration) : base(opt)
    {
        _configuration = configuration; // <-- incluído para acessar as Secrets
    }

    protected override void OnModelCreating(ModelBuilder builder) // <--
    {
        base.OnModelCreating(builder);

        IdentityUser<int> admin = new IdentityUser<int>
        {
            UserName = "admin",
            NormalizedUserName = "ADMIN",
            Email = "admin@admin.com",
            NormalizedEmail = "ADMIN@ADMIN.COM",
            EmailConfirmed = true,
            SecurityStamp = Guid.NewGuid().ToString(),
            Id = 99999
        };

        // criptografar a nova senha
        PasswordHasher<IdentityUser<int>> hasher = new PasswordHasher<IdentityUser<int>>();

        // usar string que é menos seguro ou secrets
        // admin.PasswordHash = hasher.HashPassword(admin, "Admin123!"); 
        admin.PasswordHash = hasher.HashPassword(admin, _configuration.GetValue<string>("admininfo:password"));

        // buildar o dado
        builder.Entity<IdentityUser<int>>().HasData(admin); 

        // Role Admin
        builder.Entity<IdentityRole<int>>().HasData(
            new IdentityRole<int> { Id = 99999, Name = "admin", NormalizedName = "ADMIN" }
        );

        // Role Regular
        builder.Entity<IdentityRole<int>>().HasData(
            new IdentityRole<int> { Id = 99998, Name = "regular", NormalizedName = "REGULAR" }
        );

        // Vincular Role Admin ao Usuario Admin
        builder.Entity<IdentityUserRole<int>>().HasData(
            new IdentityUserRole<int> { RoleId = 99999, UserId = 99999 }
        );
    }
}
```

+ Adicionar a **Secret** pelo **console**:

```powershell
dotnet user-secrets set "admininfo:password" "Admin123!"
```

+ Adicionar o `IConfiguration` ao `DbContext` para usar pra buscar a **Secret**:

```csharp
public class UserDBContext : IdentityDbContext<IdentityUser<int>, IdentityRole<int>, int>
{
    private IConfiguration _configuration; // <--

    public UserDBContext(DbContextOptions<UserDBContext> opt, IConfiguration configuration /* <-- */) : base(opt)
    {
        _configuration = configuration; // <--
    }

    //...
}
```

+ Para Concluir a criação dos dados acima é necessário através do **Console do Gerenciador de Pacotes**:
  + **Adicionar** uma `Migration`
  + dar **Update** no Banco de Dados

```powershell
Add-Migration "Criando roles admin e regular, e usuario admin"
Update-Database
```

---

## 🔑 Passando Role no Token

*Nessa parte não é usado Identity, os dados de usuário poderiam vir de qualquer classe.*

+ Adicionando ao `TokenService` um novo `Claim` com novo parâmetro `string role` adicionado ao construtor:

```csharp
public class TokenService
{
    public Token CreateToken(IdentityUser<int> usuario, 
        string role) // <--
    {
        Claim[] direitoUsuario = new Claim[]
        {
            new Claim("username", usuario.UserName),
            new Claim("id", usuario.Id.ToString()),
            new Claim(ClaimTypes.Role, role) // <--
        };

        var chave = new SymmetricSecurityKey(Encoding.UTF8.GetBytes("0asdjas09djsa09djasdjsadajsd09asjd09sajcnzxn"));
        var credenciais = new SigningCredentials(chave, SecurityAlgorithms.HmacSha256);
        var token = new JwtSecurityToken(
            claims: direitoUsuario,
            signingCredentials: credenciais,
            expires: DateTime.UtcNow.AddHours(1)
            );

        var tokenString = new JwtSecurityTokenHandler().WriteToken(token);
        return new Token(tokenString);
    }
}
```

---

## 🔐 Autenticando por Role

*em outra aplicação*
*a outra aplicação não usa Identity, usa somente `Autorization` e `JwtBearer` do NetCore.*

+ Para autenticar por um token é preciso interpretar um token gerado por outra aplicação através dos métodos `AddAuthentication()` e `AddJwtBearer()` no `Startup`.

+ Para utilizar autenticação por Role nas `Actions`, é inserida a anotação:

```csharp
[HttpPost]
[Authorize(Roles = "admin")] // <--
[Authorize(Roles = "admin, regular")] // ou multiplos roles
public IActionResult Insert([FromBody] CreateFilmeDto filmeDto)
{
    // ...
}
```

+ É necessário instalar os pacotes no projeto que irá autenticar:
  + `Microsoft.AspNetCore.Authentication.JwtBearer`
  + `Microsoft.AspNetCore.Authorization`
  
*Note que não é usado Identity nesse projeto para essa autenticação/autorização*
<br>

+ Em seguida é necessário **Configurar o Serviço** da aplicação (`Startup.ConfigureServices`)
  **Adicionando** um serviço de Autenticação:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...

    services.AddAuthentication(auth =>
    {
        auth.DefaultAuthenticateScheme = JwtBearerDefaults.AuthenticationScheme; // autenticacao padrao
        auth.DefaultChallengeScheme = JwtBearerDefaults.AuthenticationScheme; // desafio de autenticidade do token
    }).AddJwtBearer(token => //add JWT Bearer
    {
        token.RequireHttpsMetadata = false; // não requerer HTTPS
        token.SaveToken = true; // Salvar o token nas propriedades Authentication.AuthenticationProperties após autenticação de sucesso
        token.TokenValidationParameters = new TokenValidationParameters // definir os parametros de validação
        {
            ValidateIssuerSigningKey = true, // validar quem assinou o token
            IssuerSigningKey = new SymmetricSecurityKey( // simetria do token, igual feita na API de Usuarios
                Encoding.UTF8.GetBytes("0asdjas09djsa09djasdjsadajsd09asjd09sajcnzxn")),
            ValidateIssuer = false, // hierarquia de permissão -não se preocupar com o Issuer desse token
            ValidateAudience = false, // não validar a audiencia
            ClockSkew = TimeSpan.Zero // sincronizar o tempo de expcom uma margem (inclinação do relógio), contagem de tempo para expiração será a partir de 0
        };
    });

    // ...
}
```

+ **Configurar** a aplicação para **Usar Autenticação** e **Autorização** (`Startup.Configure`):

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ...

    app.UseAuthentication(); // <--
    app.UseAuthorization(); 

    // ...
}
```

---

[`^ topo`](#⭐-roles)
</font>
