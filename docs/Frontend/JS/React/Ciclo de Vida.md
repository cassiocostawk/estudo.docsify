# Ciclo de vida de um componente

Um componente, assim como o ser humano, tem um ciclo de vida! No caso do componente, ele nasce, compartilha informação e morre!

Mas como assim ele morre???

Nossa aplicação não tem múltiplas telas ou renderização condicional de componentes, por isso não conseguimos ver um componente morrendo em si, mas o termo "morrer" é mais conhecido como unmount, ou "desmontar" em português, assim como "nascer" seria mount, que é "montar" em português, o que faz muito mais sentido para um componente né?

Em geral um componente tem um ciclo de vida assim:

+ `componentWillMount` (antes de ser montado)
+ `componentDidMount` (acabou de ser montado)
+ `componentWillUpdate` (componente acabou de atualizar)
+ `componentWillUnmount` (componente vai ser desmontado)

Esses palavrões que eu coloquei acima e não foram à toa, são métodos que podem ser utilizados em class components assim como o método render que utilizamos para renderizar o JSX.

Claro que esses não são os únicos métodos, mas são os mais importantes para que consigamos entender o ciclo de vida de forma didática, caso queira saber mais, acesse a documentação sobre React.Component.

Você está me falando sobre class components, mas não estamos trabalhando com hooks??? cadê o ciclo de vida com function components???

---

## Function Components

Vamos ver então essas funções acima escritos com function components

### 🔹 `componentWillMount`

```js
 useLayoutEffect(() => {
    …
  },[])
```

Começamos com um bem pouco utilizado, o hook `useLayoutEffect`, ele com o array vazio atua como o `componentWillMount`. É usado quando você precisa mudar algo visualmente antes do componente aparecer, para que não haja aquele problema da tela piscar assim que a tela carrega, um bom exemplo disso atualmente é a mudança de temas para light/dark.

### 🔹 `componentDidMount`

```js
 useEffect(() => {
    …
  }, [])
```

O `useEffect` com o array de dependências vazio atua como o `componentDidMount`, diferente do `useLayoutEffect`, ele executa assim que o componente é renderizado, normalmente é utilizado para fazer chamadas para o servidor ou fazer algum cálculo com props passados.

### 🔹 `componentWillUpdate`

```js
 useEffect(() => {
    …
  }, [variavel])
```

O `componentWillUpdate` pode ser feito tanto pelo `useLayoutEffect` quanto pelo `useEffect`, desde que tenha uma variável no array de dependências e, à partir da primeira execução, os 2 atuarão como `componentWillUpdate`, sempre executando quando essa variável mudar.

### 🔹 `componentWillUnmount`

```js
useEffect(() => {
  return () => {
    …
  }
},[])
```

Diferente do que muitas pessoas pensam, também existe a representação do `componentWillUnmount` em hooks, que é retornar uma função dentro do `useEffect`! dessa forma essa função dentro do `return` só será executada quando o componente estiver desmontando. É bastante usado para `clearTimeout`, `clearInterval` ou para enviar informações de acesso daquele componente para outro lugar.
