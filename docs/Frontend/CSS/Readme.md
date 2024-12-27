# 🎨 CSS

`Cascading Style Sheets`

[⬆️ inicio](../../Readme.md)
[◀️ voltar](../Readme.md)  

---

O **CSS** (Cascading Style Sheets) é um mecanismo para adicionar estilo (cores, fontes, espaçamento, etc.) a um documento web. [(fonte: Wikipedia)](https://pt.wikipedia.org/wiki/CSS)  

[Base de consulta de CSS - W3 School](https://www.w3schools.com/css/default.asp)

---

## Incluindo CSS no HTML

Existem três formas principais de usar CSS em um documento HTML:

### 1. CSS Inline
Estilos aplicados diretamente nos elementos HTML, dentro do atributo `style`.

```html
<p style="color: red; font-size: 14px;">Texto em vermelho.</p>
```

### 2. CSS Interno
Estilos definidos dentro da tag `<style>` no documento HTML.

```html
<!DOCTYPE html>
<html>
<head>
  <style>
    p {
      color: blue;
      font-size: 16px;
    }
  </style>
</head>
<body>
  <p>Texto em azul.</p>
</body>
</html>
```

### 3. CSS Externo
Estilos definidos em um arquivo separado, referenciado com a tag `<link>`.

```html
<!DOCTYPE html>
<html>
<head>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <p>Texto estilizado pelo arquivo externo.</p>
</body>
</html>
```

> Atalho VSCode: `link:css`

---

## Resetando o CSS Padrão do Navegador

Os navegadores aplicam estilos padrão aos elementos HTML. Para remover esses estilos, utilize um reset CSS, como o exemplo abaixo:

```css
/* Reset CSS - Eric Meyer v2.0 | Public Domain */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd, ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td,
article, aside, canvas, details, embed,
figure, figcaption, footer, header, hgroup,
menu, nav, output, ruby, section, summary,
time, mark, audio, video {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}
article, aside, details, figcaption, figure,
footer, header, hgroup, menu, nav, section {
  display: block;
}
body {
  line-height: 1;
}
ol, ul {
  list-style: none;
}
blockquote, q {
  quotes: none;
}
table {
  border-collapse: collapse;
  border-spacing: 0;
}
```

---

## Padrões CSS

### Padrão ITCSS 

`Inverted Triangle CSS`

**ITCSS** é uma metodologia para organizar CSS de forma hierárquica, dividindo-o em camadas. Essas camadas formam um triângulo invertido, onde:

1. Camadas mais genéricas (ex.: resets e configurações).
2. Camadas mais específicas (ex.: componentes e utilitários).

**Estrutura de pastas:**

```plaintext
styles/
  settings/   → Variáveis, configurações globais
  generic/    → Resets e normalizações
  base/       → Estilos base, como tags HTML
  objects/    → Estruturas reutilizáveis, como grids
  components/ → Componentes de interface
  trumps/     → Estilos utilitários com alta prioridade
```

- [Exemplo](./Exemplos/Exemplo5/index.html)  
- [Material de estudo](https://willianjusten.com.br/organizando-seu-css-com-itcss)

### Padrão RSCSS

`Reasonable System for CSS Stylesheet Structure`

O **RSCSS** é um conjunto de ideias para organizar o CSS de forma sustentável, focado em componentes reutilizáveis.

**Regras principais:**

1. Use hífens `-` para separar nomes de classes.
2. Estruture tudo como componentes ou objetos.
3. Use a menor especificidade possível nas classes.
4. Adicione modificadores com hífens `-`.
5. Utilize subelementos com underline `_`.

**Exemplo de organização:**

```html
<div class="menu">
  <div class="menu_item">Item 1</div>
  <div class="menu_item">Item 2</div>
</div>
```

- [Exemplo](./Exemplos/Exemplo6/index.html)  
- [Exemplo CSS puro](./Exemplos/Exemplo6/style.css)  
- [Exemplo SCSS - Pre-processador](./Exemplos/Exemplo6/style.scss)

---

## Tags CSS e Seletores

No CSS, utilizamos **seletores** para aplicar estilos. Aqui estão os principais:

- `element` → Seleciona todas as tags de um tipo específico.  
  Ex.: `p { color: red; }` (Seleciona todas as tags `<p>`).

- `.class` → Seleciona elementos com uma classe específica.  
  Ex.: `.highlight { font-weight: bold; }`

- `#id` → Seleciona elementos com um ID específico.  
  Ex.: `#header { background-color: blue; }`

- `*` → Seleciona todos os elementos.  
  Ex.: `* { margin: 0; }`

- `[attribute]` → Seleciona elementos com um atributo específico.  
  Ex.: `input[type="text"] { border: 1px solid gray; }`

- `::pseudo-element` → Seleciona partes de elementos.  
  Ex.: `p::first-line { font-weight: bold; }`

- `:pseudo-class` → Seleciona elementos com estados específicos.  
  Ex.: `a:hover { color: green; }`

---

[^ topo](#🎨-CSS)
