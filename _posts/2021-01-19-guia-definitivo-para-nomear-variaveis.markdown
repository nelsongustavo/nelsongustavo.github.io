---
layout: post
title:  "Guia definitivo para nomear variáveis"
date:   2021-01-19 20:30:00
---

Dar nome às coisas não é uma tarefa fácil. Este post vai tentar te ajudar nesse processo.

Essas sugestões podem ser aplicadas pra qualquer linguagem de programação, Vou utilizar JavaScript para ilustrar na prática.

## Inglês

Sempre utilize a língua Inglesa quando estiver nomeando suas variáveis e funções.
```js
/* Ruim */
const primerNome = 'Nelson'
const amigos = ['Mateus', 'Bruno']

/* Bom */
const firstName = 'Nelson'
const friends = ['Mateus', 'Bruno']
```

> Quer você goste ou não, o Inglês é a língua dominante na programação: A sintaxe de todas as linguagens de programação é escrita em Inglês, assim como as inúmeras documentações e materiais educacionais. Escrever seu código em Inglês aumenta muito a coesão.

## Convenção de nome

Escolha **uma** convenção de nome e siga. Ela pode ser `camelCase`, ou `snake_case`, ou qualquer uma outra, isso não importa! O que é realmente importante é que seu código se mantenha consistente com o padrão escolhido.

```js
/* Ruim */
const pages_count = 5
const shouldUpdate = true

/* Bom */
const pageCount = 5
const shouldUpdate = true

/* Bom tambem */
const page_count = 5
const should_update = true
```

## C-I-D

Um bom nome precisa ser _curto_, _intuitivo_ e _descritivo_:

- **Curto**. Não devemos criar um nome que demore muito para digitar, ou até mesmo lembrar;
- **Intuitivo**. Devemos criar um nome que seja lido naturalmente, o mais próximo de uma conversa o possível;
- **Descritivo**. Devemos criar um nome que reflita o que isso faz/possui da forma mais eficiente o possível.

```js
/* Ruim */
const a = 5 // "a" pode significar qualquer coisa!
const isPaginatable = a > 10 // "Paginatable" soa extremamente artificial
const shouldPaginatize = a > 10 // Inventar verbos não é muito legal

/* Bom */
const postCount = 5
const hasPagination = postCount > 10
const shouldDisplayPagination = postCount > 10
```

Não use contrações, Elas não contribuem para nada além de diminuir a legibilidade do código. Encontrar um nome curto e descritivo é difícil, mas utilizar contrações não é o caminho!

```js
/* Ruim */
const onItmClk = () => {}

/* Bom */
const onItemClick = () => {}
```

## Evite a duplicação do contexto

Um nome não pode duplicar o contexto em que ele está definido. Sempre remova o contexto do nome se isso não reduzir a legibilidade.

```js
class MenuItem {
  /* O nome do método duplica o contexto (Que é "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Leitura fluida como: `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

## Reflete o resultado esperado

Um bom nome deve refletir o resultado esperado.

```jsx
/* Ruim */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Bom */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---

# Nomeando Funções

## Padrão A/HC/LC

Existe um ótimo padrão a ser seguido quando estamos nomeando funções.

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Dê uma olhada em como esse padrão deve ser aplicado na tabela abaixo.

| Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getPost`              |          | `get`      | `Post`            |                  |
| `getPostData`          |          | `get`      | `Post`            | `Data`           |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Nota:** A ordem do contexto afeta o significado da variável. Por exemplo, `shouldUpdateComponent` significa _você_ está prestes a atualizar o component, enquanto `shouldComponentUpdate` Te diz que o _component_ vai se atualizar, e você está controlando quando ele deve ser atualizado.
> Em outras palavras, **O high context enfatiza o significado da variável**.

---

## Ações

A parte verbal do nome da função, A parte mais importante responsável por descrever o que a função _faz_.

### `get`

Acesse o dado imediatamente (i.e forma abreviado do getter).

```js
function getFruitCount() {
  return this.fruits.length
}
```

> Veja também [compose](#compose).

### `set`

_Seta_ a variável de uma forma declarativa, com o valor `A` para o valor `B`.

```js
let fruits = 0

function setFruits(nextFruits) {
  fruits = nextFruits
}

setFruits(5)
console.log(fruits) // 5
```

### `reset`

_Seta_ a variável para o seu estado inicial.

```js
const initialFruits = 5
let fruits = initialFruits
setFruits(10)
console.log(fruits) // 10

function resetFruits() {
  fruits = initialFruits
}

resetFruits()
console.log(fruits) // 5
```

### `fetch`

Requisição para algum dado, que leva algum tempo indeterminado (i.e Requisição assíncrona)

```js
function fetchPosts(postCount) {
  return fetch('https://api.dev/posts', {...})
}
```

### `remove`

Remove alguma coisa _de_ algum lugar.

Por exemplo, se você tem uma coleção de filtros selecionados em uma página de pesquisa, remover um deles da coleção seria `removeFilter`, e **não** `deleteFilter` (E essa também seria a forma natural de dizer isso em Inglês):

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName)
}

const selectedFilters = ['price', 'availability', 'size']
removeFilter('price', selectedFilters)
```

> Veja também [delete](#delete).

### `delete`

Apaga completamente alguma coisa da face da terra.

Imagine que você seja um editor de conteúdo, e existe um post bem famoso que você queira se livrar. Assim que você clicar no lindo e bem desenhado botão "Delete post", o CMS vai executar uma ação `deletePost`, e **não** uma `removePost`.

```js
function deletePost(id) {
  return database.find({ id }).delete()
}
```

> Veja também [remove](#remove).

### `compose`

Cria um novo dado a partir de um já existente. Aplicado a strings, objetos ou funções na maioria das vezes.

```js
function composePageUrl(pageName, pageId) {
  return `${pageName.toLowerCase()}-${pageId}`
}
```

> Veja também [get](#get).

### `handle`

Lida com uma ação. Comumente utilizado para nomear um método de callback.

```js
function handleLinkClick() {
  console.log('Clicked a link!')
}

link.addEventListener('click', handleLinkClick)
```

---

## Context

O domínio que a função está operando.

Uma função é geralmente uma função em _alguma coisa_. É importante declarar qual é o seu domínio, ou ao menos o tipo de dado esperado.

```js
/* Uma função pura operando com primitivos */
function filter(predicate, list) {
  return list.filter(predicate)
}

/* Função operando exatamente nos posts */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```


> Algumas especificidades da linguagem permite que o contexto seja omitido. Por exemplo, no JavaScript, é comum que `filter` opere em arrays, adicionar explicitamente `filterArray` pode ser desnecessário.

--

## Prefixos

O prefixo melhora o significado da variável. E raramente é utilizado em nomes de função.

### `is`

Descreve a característica ou estado do contexto atual (geralmente `boolean`).

```js
const color = 'blue'
const isBlue = color === 'blue' // Característica
const isPresent = true // Estado

if (isBlue && isPresent) {
  console.log('Blue is present!')
}
```

### `has`

Descreve se o contexto atual possui um certo valor ou estado (geralmente `boolean`).

```js
/* Ruim */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Bom */
const hasProducts = productsCount > 0
```

### `should`

Reflete um estado condicional positivo (geralmente `boolean`) associado a alguma ação.

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

### `min`/`max`

Representa um valor mínimo ou máximo. Utilizado quando estiver descrevendo fronteiras ou limites. 

```js
/**
 * Renderiza uma quantidade aleatória de posts
 * dada a fronteira min/max
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts))
}
```

### `prev`/`next`

Indica o estado anterior ou seguinte de uma variável no contexto atual. Utilizada quando estamos descrevendo uma transição de estado.

```jsx
function fetchPosts() {
  const prevPosts = this.state.posts

  const fetchedPosts = fetch('...')
  const nextPosts = concat(prevPosts, fetchedPosts)

  this.setState({ posts: nextPosts })
}
```

## Singular e Plural

Assim como um prefixo, o nome das variáveis podem estar no singular ou plural dependendo se elas armazenam um único valor ou múltiplos valores.

```js
/* Ruim */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Bom */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```

Sempre me frustrei muito ao perceber uma falta de padrão nos projetos em que trabalhava, muitas vezes gastava tempo demais por não saber exatamente qual caminho seguir ao nomear uma simples variável. Já faz muito tempo que venho batendo nessa tecla de adotar um padrão para nomes de variáveis e funções e segui-lo, quando atuei como líder do time de desenvolvimento, fiz o máximo para manter as coisas mais padronizadas o possível, para facilitar a manutenção e qualidade do código.


## Referência
[naming-cheatsheet](https://github.com/kettanaito/naming-cheatsheet)
