<font face="Calibri">

# ⭐ Gerenciador de Pacotes

[`◀️ voltar`](./Readme.md)
[`⬆️ inicio`](../Readme.md)

---

## Entity Framework

+ `PM> Get-Help EntityFramework` 
  + *se retorna oque está abaixo é porque está instalado*

>                     _/\__
>               ---==/    \\
>         ___  ___   |.    \|\
>        | __|| __|  |  )   \\\
>        | _| | _|   \_/ |  //|\\
>        |___||_|       /   \\\/\\

+ TOPIC
    about_EntityFrameworkCore

+ SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

+ LONG DESCRIPTION
    This topic describes the Entity Framework Core Package Manager Console Tools. See https://docs.efproject.net for information on Entity Framework Core.

+ The following Entity Framework Core commands are available.


| Cmdlet__________ | Description | Informação | Passo |
| :--- | :--- | :--- | --- |
| Add-Migration | Adds a new migration. | Gerar versão da  Migrarion no estado atual das entidades. | 1º |
| Drop-Database | Drops the database. | Esse comando é utilizado para dropar o banco de dados apontado pelo contexto. | 1º |
| Remove-Migration | Removes the last migration. | Esse comando é utilizado para remover a última migração não aplicada no banco de dados apontado pelo contexto. | 2º |
| Scaffold-DbContext | Scaffolds a DbContext and entity types for a database. | Esse comando é utilizado para criar uma classe que estende de DbContext, além de classes que representam as tabelas do banco. | |
| Script-Migration | Generates a SQL script from migrations. | Gerar o DDL que vai atualizar o DB, será gerado o script para ser executado externamente. | 2º |
| Update-Database | Updates the database to a specified migration. | Faz a atuaização diretamente no DB. | 2º |

---

## Executar Testes
+ `PM> dotnet test`
```batch
  Determinando os projetos a serem restaurados...
  Todos os projetos estÆo atualizados para restaura‡Æo.
  Alura.Estacionamento -> D:\DotNet\Cursos\alura.estacionamento\Alura.Estacionamento\bin\Debug\net5.0\Alura.Estacionamento.dll
  Alura.Estacionamento.Testes -> D:\DotNet\Cursos\alura.estacionamento\Alura.Estacionamento.Testes\bin\Debug\net5.0\Alura.Estacionamento.Testes.dll
Execucao de teste para D:\DotNet\Cursos\alura.estacionamento\Alura.Estacionamento.Testes\bin\Debug\net5.0\Alura.Estacionamento.Testes.dll (.NETCoreApp,Version=v5.0)
Ferramenta de Linha de Comando de Execucao de Teste da Microsoft (R) Versao 16.11.0
Copyright (c) Microsoft Corporation. Todos os direitos reservados.

Iniciando execu‡Æo de teste, espere...
1 arquivos de teste no total corresponderam ao padrÆo especificado.
[xUnit.net 00:00:00.67]     Alura.Estacionamento.Testes.VeiculoTestes.ValidaNomeProprietario [SKIP]
  Ignorado Alura.Estacionamento.Testes.VeiculoTestes.ValidaNomeProprietario [1 ms]

Aprovado!  - Com falha:     0, Aprovado:     9, Ignorado:     1, Total:    10, Dura‡Æo: 72 ms - Alura.Estacionamento.Testes.dll (net5.0)
```

---

[`^ topo`](#Dev)
</font>
