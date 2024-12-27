<font face="Calibri">

# 🗝️ Token

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 🔑 JWT

No site jwl.io é possível decodificar o token.

Exemplo:

```jwt
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VybmFtZSI6ImNhc3NpbzEiLCJpZCI6IjIwIiwiaHR0cDovL3NjaGVtYXMubWljcm9zb2Z0LmNvbS93cy8yMDA4LzA2L2lkZW50aXR5L2NsYWltcy9yb2xlIjoiYWRtaW4iLCJleHAiOjE2NDk1MTQyMTd9.AFLGGMcJrJHXfhgOgEvFeKbjFuFziyXw5b56nBydOrM
```

+ Header:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

+ Payload:

```json
{
  "username": "cassio1",
  "id": "20",
  "http://schemas.microsoft.com/ws/2008/06/identity/claims/role": "admin",
  "exp": 1649514217
}
```

+ Assinatura verificada:

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  
your-256-bit-secret

)
```

+ Estrutura de uma classe que gera o `Token`:

```csharp
public Token CreateToken(CustomIdentityUser usuario, string role)
{
    Claim[] direitoUsuario = new Claim[]
    {
        new Claim("username", usuario.UserName),
        new Claim("id", usuario.Id.ToString()),
        new Claim(ClaimTypes.Role, role)
        new Claim(ClaimTypes.Role, role),
        new Claim(ClaimTypes.DateOfBirth, usuario.DataNascimento.ToString()) // <-- adicionar a data de nascimento ao Token
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
```

---

[`^ topo`](#🗝️-token)
</font>
