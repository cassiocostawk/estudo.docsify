<font face="Calibri">

# ⭐ Props

[`◀️ voltar`](../Readme.md)
[`⬆️ inicio`](../../README.md)

---

## Forma 1

Component:

```ts
class Botao extends React.Component<{ texto: string }> { // <--
  render() {
    return (
        <button className={style.botao}>
            {this.props.texto} // <--
        </button>
    );
  }
}
```

Uso:

```html
<Botao texto="Adicionar"/>
```

---

## Forma 2

---

[`^ topo`](#Dev)
</font>
