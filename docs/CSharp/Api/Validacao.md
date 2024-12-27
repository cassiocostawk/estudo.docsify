<font face="Calibri">

# ✅ Validação de APIs

[`⬆️ inicio`](../../Readme.md)

[`◀️ voltar`](../Readme.md)

---

## ✅ Validação de Dados de Entrada

+ Uma das maiores dores de cabeça de um desenvolvedor é tratar de dados inconsistentes no banco de dados.
+ É confiar que vai vir o CEP o nome da cidade, e o CPF, em formatos corretos.
+ Prevenir é muito mais barato do que passar as sprints apagando incêndio por conta de falta de validação na API.

---

## ✅ Estrutura ideal

A estrutura ideal de validação em um sistema:

+ Validação no client (front-end)
+ Validação na entrada de API (back-end)
+ Validação a nível de domínio

---

## ✅ Fluent Validation

+ Biblioteca utilizada para validação de objetos, portanto permitindo ser utilizada para verificar nossos modelos de entrada.
+ Instalada na aplicação pelo pacote `FluentValidation.AspNetCore`.
+ São criadas classes de validação para cada modelo de entrada a ser validado;

### Implementação

Para utilizar essa biblioteca, o fluxo simples é:

+ Configurar o **Fluent Validation** (global, feito 1 vez)
+ Criar uma classe que herde de `AbstractValidator<T>`
+ Implementar as regras no `construtor`
+ Checar o `ModelState` na `Action`, e retornar o erro

### Funcionalidades do FluentValidation

+ Validações de propriedades simples
+ Validações de objetos complexos
+ Validação de listas
+ Múltiplos validadores para a mesma classe

---

[`^ topo`](#✅-validação-de-apis)
</font>
