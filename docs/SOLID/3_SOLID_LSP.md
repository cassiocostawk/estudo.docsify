# 📍 LSP
`Liskov Substitution Principle`<br>
`Princípio da Substituição de Liskov`

É o princípio da substituição da `Bárbara Leskov`, que foi uma pesquisadora dos anos 80 que o `Bob Martin` trouxe para o princípio `SOLID`.

+ Cumpra as promessas das abstrações.
+ Preocupe-se com a coesão e acoplamento das interfaces. (Ver mais em `YAGNI`).

## 🔹 Voce não vai precisar disso `YAGNI`
`You Aint Gonna Need It`

Essa idéia vem de outra metodologia ágil chamada XP (Extreme Programming) e cunhada no livro de `Ron Jeffries` (mais um signatário do manifesto ágil) Extreme Programming Installed. 

 Se a abstração prever incluir, alterar e excluir, precisamos garantir isso. Seria fácil para nós implementar as abstrações, as operações no categoria dao EF core, mas também não vamos criar a funcionalidade se ela não existe no sistema. Não crie confusão para você no futuro.

  Nós aplicamos uma ideia de um padrão chamado `CQRS`, para quebrar o `IDAO` em duas interfaces menores. 
  Uma com operações de leitura, outra com operações de escrita. 
  Criamos o Icommand e o Iquery.

  `CQRS - Command Query Responsibility Segregation`

  

```csharp
public interface IDao<T> // Escrita e Leitura, quebrado nas 2 interfaces abaixo
{
    void Incluir(T obj);
    void Alterar(T obj);
    void Excluir(T obj);
    IEnumerable<T> BuscarTodos();
    T BuscarPorId(int id);
}

public interface ICommand<T> // Escrita
{
    void Incluir(T obj);
    void Alterar(T obj);
    void Excluir(T obj);
}

public interface IQuery<T> // Leitura
{
    IEnumerable<T> BuscarTodos();
    T BuscarPorId(int id);
}
```