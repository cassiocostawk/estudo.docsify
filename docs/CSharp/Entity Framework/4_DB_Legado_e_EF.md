<font face="Calibri">

# ⭐ DB Legado e Entity Framework

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## Como mapear

## Convenção do Entity com o DB

+ Nome da Classe para mapear com a Tabela (que são diferentes) é usado o nome da property `DbSet` no contexto, para alinhar os nomes será necessário usar `DataAnnotations` na classe.
+ O nome da tabela é determinado pelo nome da propriedade `DbSet` para o tipo da entidade. Por exemplo, se o nome da propriedade `DbSet<Cliente>` for `TblClientes`, o nome da tabela será `TblClientes`.

> As regras implícitas que o Entity está utilizando, chamadas convenções, permitem que o desenvolvedor não precise ficar mapeando cada classe e cada propriedade que será gerida pelo EF, trazendo simplicidade de código sem perder flexibilidade.
>
>Esta abordagem é bastante difundida para utilização de bibliotecas e tecnologias e é chamada de Convenção sobre Configuração.
>
>Com essa abordagem, o desenvolvedor só precisará configurar (ou explicitar) aquilo que se desvie da convenção adotada (implícita).
>
> Como benefícios do uso da Convenção sobre Configuração podemos citar:
>
> + curva de aprendizado menor
> + promove uniformidade entre sistemas
> + facilita a mudança das classes, uma vez que não é necessário mudar arquivos de configuração
>
> Já como desvantagens, podemos elencar:
>
> + exige familiaridade com as tais regras implícitas
> + o código do framework fica maior para embutir as regras implícitas

+ Para quebrar a regra implícita, será necessário explicitar na classe através de `DataAnnotations`.

+ Exemplo de classe onde estão as regras implicitas para SQLServer `SqlServerTypeMapper`.

---

[`^ topo`](#⭐-db-legado-e-entity-framework)
</font>
