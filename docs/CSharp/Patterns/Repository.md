<font face="Calibri">

# 🧱 Repository

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## 🔗 O que é Padrão Repository

+ Padrão utilizado para encapsular o acesso a dados, e permitir o desacoplamento dos detalhes de implementação ao se utilizar junto com interface.
+ Componente de domínio do Domain-Driven Design responsável pelo acesso aos objetos armazenados (mas sem especificações a respeito de sua fonte)

### Implementação

+ Geralmente, existirá um repositório por **AGREGADO**.
  Em nosso projeto, por exemplo, existirá um repositório para `Project`, que é o Agregado, mas não para `ProjectComment`.
+ Por utilizar interfaces no acesso a dados, permite a escrita de **Testes Unitários** na camada **Application**, seja a implementação utilizada a de **CQRS** ou de **serviços de
camada de Application**.

Cada repositório, na **Arquitetura Limpa**, está dividido em:

+ **Interface**:
  contém o contrato dos métodos de acesso a dados, e devem ficar localizados na camada **Core**.
+ **Implementação**:
  contém a implementação dos métodos de acesso a dados, seja utilizando **EF Core** ou **Dapper**, ou outras ORMs/ferramentas.

---

[`^ topo`](#🧱-repository)
</font>
