# Divisio JavaScript Style Guide = () => (

*Essa é a abordagem da Divisio com JavaScript*

Mantenha em mente que algumas regras serão automaticamente arrumadas com o [Prettier](https://prettier.io/), e outras não. Este guia também está disponível em outras linguagens. Veja em [Tradução](#tradução)

Outros Style Guides

  - [React](react/pt-br.md)

## Lista de conteúdos

  1. [Regras básicas](#regras-básicas)
  1. [Referências](#referências)
  1. [Objetos](#objetos)  
  1. [Arrays](#arrays) 
  1. [Desestruturação](#desestruturação) 
  1. [Strings](#strings) 
  1. [Funções](#funções) 
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Construtores](#classes--construtores)
  1. [Módulos](#módulos)
  1. [Iteradores e Geradores](#iteradores)
  1. [Propriedades](#propriedades)
  1. [Variáveis](#variáveis)
  1. [Hoisting](#hoisting)
  1. [Operadores de comparação & igualdade](#operadores-de-comparação--igualdade)
  1. [Blocos](#blocos)
  1. [Declarações de controle](#declarações-de-controle)
  1. [Comentários](#comentários)
  1. [Espaçamento](#espaçamento)
  1. [Vírgulas](#vírgulas)
  1. [Ponto e vírgula](#ponto-e-vírgula)
  1. [Fundição e coerção de tipos](#fundição-e-coerção-de-tipos)
  1. [Nomeação](#nomeação)
  1. [Accessors](#accessors)
  1. [Testes](#testes)
  1. [Tradução](#tradução)

## Regras básicas

  - Não use `;` no fim de cada linha.
  - Sempre nomeie **tudo** em inglês por convenção.

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Referências

  - Use `const` para todas as suas referências, e evite usar `var`. eslint: [`prefer-const`](https://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](https://eslint.org/docs/rules/no-const-assign.html)

    > Por que? This ensures that you can’t reassign your references, which can lead to bugs and difficult to comprehend code.

    ```javascript
    // ruim
    var a = 1
    var b = 2

    // bom
    const a = 1
    const b = 2
    ```

  - Se você precisa , use `let` ao invés de `var`. eslint: [`no-var`](https://eslint.org/docs/rules/no-var.html)

    > Por que? `let` é tem escopo por bloco, diferente de `var` que tem escopo por função.

    ```javascript
    // ruim
    var count = 1
    if (true) {
      count += 1
    }

    // bom, use let.
    let count = 1
    if (true) {
      count += 1
    }
    ```

    > **Nota**: Note que ambas `let` e `const` são escopos por bloco.

    ```javascript
    // const e let existem apenas aonde foram definidas.
    {
      let a = 1
      const b = 1
    }
    console.log(a) // ReferenceError
    console.log(b) // ReferenceError
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Objetos

  - Use a sintaxe literal para criação de objetos. eslint: [`no-new-object`](https://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // ruim
    const item = new Object()

    // bom
    const item = {}
    ```

  - Use nomes de propriedades usando notação de colchetes ao criar objetos com nomes de propriedades dinâmicos.

    > Por que? Elas permitem que você defina todas as propriedades de um objeto em apenas um lugar.

    ```javascript

    const getKey = k => `a key named ${k}`

    // ruim
    const obj = {
      id: 5,
      name: 'San Francisco',
    }
  
    obj[getKey('enabled')] = true

    // bom
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    }
    ```

  - Declare métodos de objetos de forma abreviada. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    ```javascript
    // ruim
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value
      },
    }

    // bom
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value
      },
    }
    ```

    > **Nota**: Essa regra não aponta para arrow functions dentro de objetos literais. O exemplo a seguir não indica erro!

    ```javascript
    /*eslint object-shorthand: "error"*/
    /*eslint-env es6*/

    var foo = {
        x: y => y
    }
    ```

  - Declare propriedades de forma abreviada. eslint: [`object-shorthand`](https://eslint.org/docs/rules/object-shorthand.html)

    > Por que? Assim fica menor e mais discreto.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker'

    // ruim
    const obj = {
      lukeSkywalker: lukeSkywalker,
    }

    // bom
    const obj = {
      lukeSkywalker,
    }
    ```

  - Agrupe suas propriedades abreviadas no início da declaração do objeto.

    > Por que? Assim é mais fácil de ver qual propriedade está sendo abreviada.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker'
    const lukeSkywalker = 'Luke Skywalker'

    // ruim
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    }

    // bom
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    }
    ```

  - Apenas abarace em aspas simples as propriedades que são identificadores inválidos. eslint: [`quote-props`](https://eslint.org/docs/rules/quote-props.html)

    > Por que? Em geral nós consideramos mais fácil de ler. Ainda promove destaque, e também é mais otimizado para vários engines de JS. 

    ```javascript
    // ruim
    const bad = {
      'foo': 3,
      'bar': 4,
      'data-blah': 5,
    }

    // bom
    const good = {
      foo: 3,
      bar: 4,
      'data-blah': 5,
    }
    ```

   - Não use os métodos de `Object.prototype` de forma direta, como `hasOwnProperty`, `propertyIsEnumerable`, e `isPrototypeOf`. eslint: [`no-prototype-builtins`](https://eslint.org/docs/rules/no-prototype-builtins)

      > Por que? Esses métodos podem ser sombreados por propriedades no objeto em   questão - considere `{ hasOwnProperty: false }` - ou, o objeto podendo ser nulo   (`Object.create(null)`).
  
      ```javascript
      // ruim
      console.log(object.hasOwnProperty(key))
  
      // bom
      console.log(Object.prototype.hasOwnProperty.call(object, key))
  
      // melhor
      const has = Object.prototype.hasOwnProperty // armazena a pesquisa em cache uma   vez, no escopo do módulo.
      console.log(has.call(object, key))
      /* ou */
      import has from 'has' // https://www.npmjs.com/package/has
      console.log(has(object, key))
      ```

  - Prefira usar spread operator no objeto ao invés de [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) ao copiar objetos. Use o rest operator do objeto para pegar um novo objeto com propriedades omitidas

    ```javascript
    // muito ruim
    const original = { a: 1, b: 2 }
    const copy = Object.assign(original, { c: 3 }) // Isso muda o `original`
    delete copy.a // este também

    // ruim
    const original = { a: 1, b: 2 }
    const copy = Object.assign({}, original, { c: 3 }) // copy => { a: 1, b: 2, c: 3 }

    // bom
    const original = { a: 1, b: 2 }
    const copy = { ...original, c: 3 } // copy => { a: 1, b: 2, c: 3 }

    const { a, ...rest } = copy // rest => { b: 2, c: 3 }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Arrays

  - Use a sintaxe literal para a criação de Arrays. eslint: [`no-array-constructor`](https://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // ruim
    const items = new Array()

    // bom
    const items = []
    ```

  - Use [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) ao inves de atribuir um item diretamente ao array.

    ```javascript
    const someStack = []

    // ruim
    someStack[someStack.length] = 'abracadabra'

    // bom
    someStack.push('abracadabra')
    ```

  - Use array spreads `...` para copiar arrays.

    ```javascript
    // ruim
    const len = items.length
    const itemsCopy = []
    let i

    for (i = 0 i < len i += 1) {
      itemsCopy[i] = items[i]
    }

    // bom
    const itemsCopy = [...items]
    ```

  - Para converter um objeto para array, use spreads `...` ao inves de [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo')

    // bom
    const nodes = Array.from(foo)

    // melhor
    const nodes = [...foo]
    ```

  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) para converter um objeto similar a um array para array.

    ```javascript
    const arrLike = { 0: 'foo', 1: 'bar', 2: 'baz', length: 3 }

    // ruim
    const arr = Array.prototype.slice.call(arrLike)

    // bom
    const arr = Array.from(arrLike)
    ```

  - Use [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from) ao invés de spread `...` para mapear em cima de iteráveis, porque isso evita criar um array intermediário.

    ```javascript
    // ruim
    const baz = [...foo].map(bar)

    // bom
    const baz = Array.from(foo, bar)
    ```

  - Use return em métodos callbacks de array. Está ok omitir o return se a função consistir em um único retorno de expressão sem efeitos colaterais. eslint: [`array-callback-return`](https://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // bom
    [1, 2, 3].map(x => {
      const y = x + 1
      return x * y
    })

    // bom
    [1, 2, 3].map(x => x + 1)

    // ruim - não retornar um valor significa que 'acc' será undefined depois da primeira iteração
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item)
    })

    // bom
    [[0, 1], [2, 3], [4, 5]].reduce((acc, item, index) => {
      const flatten = acc.concat(item)
      return flatten
    })

    // ruim
    inbox.filter(msg => {
      const { subject, author } = msg
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee'
      } else {
        return false
      }
    })

    // bom
    inbox.filter(msg => {
      const { subject, author } = msg
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee'
      }

      return false
    })
    ```

  - Use quebras de linhas depois de abrir e fechar colchetes de arrays se o array tiver múltiplas linhas

    ```javascript
    // ruim
    const arr = [
      [0, 1], [2, 3], [4, 5],
    ]

    const objectInArray = [{
      id: 1,
    }, {
      id: 2,
    }]

    const numberInArray = [
      1, 2,
    ]

    // bom
    const arr = [[0, 1], [2, 3], [4, 5]]

    const objectInArray = [
      {
        id: 1,
      },
      {
        id: 2,
      },
    ]

    const numberInArray = [
      1,
      2,
    ]
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Desestruturação

  - Use a desestruturação de objeto quando estiver acessando e usando múltiplas propriedades de um objeto. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    > Por que? Desestruturação te salva de criar referências temporárias para estas propriedades.

    ```javascript
    // ruim
    const getFullName = user => {
      const firstName = user.firstName
      const lastName = user.lastName

      return `${firstName} ${lastName}`
    }

    // bom
    const getFullName = user => {
      const { firstName, lastName } = user
      return `${firstName} ${lastName}`
    }

    // melhor
    const getFullName = ({ firstName, lastName }) => `${firstName} ${lastName}`
    ```

  - Use desestruturação de array. eslint: [`prefer-destructuring`](https://eslint.org/docs/rules/prefer-destructuring)

    ```javascript
    const arr = [1, 2, 3, 4]

    // ruim
    const first = arr[0]
    const second = arr[1]

    // bom
    const [first, second] = arr
    ```

  - Use desestruturação de objeto para valores múltiplos em um return, não desestruturação de array.

    > Por que? Você pode adicionar novas propriedades mais tarde, ou mudar a ordem das coisas sem quebrar outras chamadas da função.

    ```javascript
    // ruim
    const processInput = input => {
      // ...
      return [left, right, top, bottom]
    }

    // O caller precisa pensar sobre a ordem do conteúdo do return 
    const [left, __, top] = processInput(input)

    // bom
    const processInput = input => {
      // ...
      return { left, right, top, bottom }
    }

    // O caller seleciona apenas o conteúdo que precisa
    const { left, top } = processInput(input)
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Strings

  - Use aspas simples `''` para strings. eslint: [`quotes`](https://eslint.org/docs/rules/quotes.html)

    ```javascript
    // ruim
    const name = "Capt. Janeway"

    // ruim - template literals deve conter interpolação ou novas linhas
    const name = `Capt. Janeway`

    // bom
    const name = 'Capt. Janeway'
    ```

  - Strings maiores que 100 caractéres **não** devem ser escritas em várias linhas usando concatenação de string.

    > Por que? Strings quebradas são dolorodas de trabalhar e ainda faz o código ser menos pesquisável.

    ```javascript
    // ruim
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.'

    // ruim
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.'

    // bom
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.'
    ```

  - Quando precisar de variáveis dentro da string, utilize template string ao inves de concatenação. eslint: [`prefer-template`](https://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](https://eslint.org/docs/rules/template-curly-spacing)

    > Por que? Template strings te dá uma sintaxe legível e concisa com boa forma de quebrar novas linhas, e ainda funcionalidades de interpolação.

    ```javascript
    // ruim
    const sayHi = name => 'How are you, ' + name + '?'

    // ruim
    const sayHi = name => ['How are you, ', name, '?'].join()

    // ruim
    const sayHi = name => `How are you, ${ name }?`

    // bom  - Never use `eval()` on a string, it opens too many vulnerabilities. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

    const sayHi = name => `How are you, ${name}?`
    ```

  - Nunca use `eval()` em uma string, pois deixa aberto para muitas vulnerabilidades. eslint: [`no-eval`](https://eslint.org/docs/rules/no-eval)

  - Não escape caractéres em string de forma desnecessária. eslint: [`no-useless-escape`](https://eslint.org/docs/rules/no-useless-escape)

    > Por que? Barra invertida prejudica a leitura, portanto é bom que estejam presentes apenas quando necessário.

    ```javascript
    // ruim
    const foo = '\'this\' \i\s \"quoted\"'

    // bom
    const foo = '\'this\' is "quoted"'
    const foo = `my name is '${name}'`
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Funções

  - Prefira usar arrow functions sempre que possível. Veja em [Arrow Functions](#arrow-functions)

  - Use funçoes nomeadas ao inves de declarações de funções. eslint: [`func-style`](https://eslint.org/docs/rules/func-style)

    > Por que? Declarações de funções são hoisted(içadas), o que significa que é fácil - muito fácil - para referenciar a função antes que ela seja definida no arquivo. Isso prejudica a legibilidade e a capacidade de manutenção. Se você achar que a definição de uma função é grande ou complexa o suficiente para interferir na compreensão do restante do arquivo, talvez seja hora de extraí-la para seu próprio módulo! Não se esqueça de nomear explicitamente a expressão, independentemente de o nome ser inferido ou não da variável que contém (o que geralmente acontece nos navegadores modernos ou quando se usa compiladores como o Babel). Isso elimina quaisquer suposições feitas sobre a pilha de chamadas do erro.

    ```javascript
    // ruim
    function foo() {
      // ...
    }

    // ruim
    const foo = function () {
      // ...
    }

    // bom
    // nome distinto da(s) chamada(s) de variável-referenciada
    const short = function longUniqueMoreDescriptiveLexicalFoo() {
      // ...
    }
    ```

  - Abraçar entre parênteses as funções que são imediatamente invocadas. eslint: [`wrap-iife`](https://eslint.org/docs/rules/wrap-iife.html)

    > Por que? Uma expressão de função invocada imediatamente é uma única unidade - agrupando-a em sua invocação, ela será expressada de maneira limpa. Observe que em um arquivo com módulos em todos os lugares, você quase nunca precisa de um IIFE.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.')
    }())
    ```

  - Nunca declare uma função em um bloco não funcional (`if`,` while`, etc.). Atribua a função a uma variável. Os navegadores permitirão que você faça isso, mas todos interpretam de maneira diferente, o que é bem ruim. eslint: [`no-loop-func`](https://eslint.org/docs/rules/no-loop-func.html)

    > **Nota**: O ECMA-262 define um bloco como uma lista de instruções. Uma declaração de  função não é uma instrução.

    ```javascript
    // ruim
    if (currentUser) {
      function test() {
        console.log('Nope.')
      }
    }

    // bom
    let test
    if (currentUser) {
      test = () => {
        console.log('Yup.')
      }
    }
    ```

  - Nunca nomeie um parâmetro como `arguments`. Isto terá precedência sobre o objeto `arguments` que é dado a todo escopo de função.

    ```javascript
    // ruim
    function foo(name, options, arguments) {
      // ...
    }

    // bom
    function foo(name, options, args) {
      // ...
    }
    ```

  - Nunca use `arguments`, opte por usar spread `...`. eslint: [`prefer-rest-params`](https://eslint.org/docs/rules/prefer-rest-params)

    > Por que? `...` é explícito sobre quais argumentos você deseja extrair. Além disso, os argumentos vindo de `...rest` são um array real, e não apenas um Array-like como em `arguments`.

    ```javascript
    // ruim
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments)
      return args.join('')
    }

    // bom
    function concatenateAll(...args) {
      return args.join('')
    }
    ```

  - Use a sintaxe para definir um padrão para parâmetros em vez de alterar os argumentos da função.

    ```javascript
    // really bad
    function handleThings(opts) {
      // We shouldn’t mutate function arguments.
      opts = opts || {}
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {}
      }
      // ...
    }

    // bom
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - Evita efeitos colaterais em paramêtros com valor padrão.

    > Por que? Eles são bastante confusos de entender.

    ```javascript
    var b = 1
  
    // ruim
    function count(a = b++) {
      console.log(a)
    }
  
    count()  // 1
    count()  // 2
    count(3) // 3
    count()  // 3
    ```

  - Sempre coloque parâmetros com valor padrão por último.

    ```javascript
    // ruim
    function handleThings(opts = {}, name) {
      // ...
    }

    // bom
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  - Nunca use o construtor de Função para create uma nova função. eslint: [`no-new-func`](https://eslint.org/docs/rules/no-new-func)

    > Por que? A criação de uma função dessa maneira avalia uma string de forma semelhante a `eval()`, abrindo mais espaço para vulnerabilidades.

    ```javascript
    // ruim
    var add = new Function('a', 'b', 'return a + b')

    // still bad
    var subtract = Function('a', 'b', 'return a - b')
    ```

  - Espace uma assinatura de função. eslint: [`space-before-function-paren`](https://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks)

    > Por que? Consistência é bom, e você não deve precisar adicionar ou remover um espaço quando adicionar ou remover um nome.

    ```javascript
    // ruim
    const f = function(){}
    const g = function (){}
    const h = function() {}

    // bom
    const x = function () {}
    const y = function a() {}
    ```

  - Nunca mude parâmetros. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Por que? Manipular objetos passados ​​como parâmetros pode causar efeitos colaterais variáveis ​​indesejados no caller original.

    ```javascript
    // ruim
    function f1(obj) {
      obj.key = 1
    }

    // bom
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1
    }
    ```

  - Nunca retribua parâmetros. eslint: [`no-param-reassign`](https://eslint.org/docs/rules/no-param-reassign.html)

    > Por que? A reatribuição de parâmetros pode levar a um comportamento inesperado, especialmente ao acessar o objeto `arguments`. Também pode causar problemas de otimização, especialmente na V8.

    ```javascript
    // ruim
    function f1(a) {
      a = 1
      // ...
    }

    function f2(a) {
      if (!a) { a = 1 }
      // ...
    }

    // bom
    function f3(a) {
      const b = a || 1
      // ...
    }

    function f4(a = 1) {
      // ...
    }
    ```

  - Prefira o uso do spread operator `...` ao chamas funções variadas. eslint: [`prefer-spread`](https://eslint.org/docs/rules/prefer-spread)

    > Por que? É mais limpo, você não precisa fornecer um contexto e não há como compor facilmente `new` com `apply`.

    ```javascript
    // ruim
    const x = [1, 2, 3, 4, 5]
    console.log.apply(console, x)

    // bom
    const x = [1, 2, 3, 4, 5]
    console.log(...x)

    // ruim
    new (Function.prototype.bind.apply(Date, [null, 2016, 8, 5]))

    // bom
    new Date(...[2016, 8, 5])
    ```

  - As funções com assinaturas ou chamadas de várias linhas devem ser indentadas como qualquer outra lista de várias linhas neste guia: com cada item em uma linha por si só. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

    ```javascript
    // ruim
    function foo(bar,
                 baz,
                 quux) {
      // ...
    }

    // bom
    function foo(
      bar,
      baz,
      quux,
    ) {
      // ...
    }

    // ruim
    console.log(foo,
      bar,
      baz)

    // bom
    console.log(
      foo,
      bar,
      baz,
    )
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Arrow Functions

  - Sempre prefira usar arrow functions em vez de funções normais se puder.
    > Por que? A consistência é sempre uma coisa boa, e a sintaxe se torna mais concisa.

  - Quando você deve usar uma função anônima (como ao passar um retorno de chamada em linha), use a notação de arrow function. eslint: [`prefer-arrow-callback`](https://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](https://eslint.org/docs/rules/arrow-spacing.html)

    > Por que? Ele cria uma versão da função que é executada no contexto de `this`, que geralmente é o que você deseja.

    > Por que não? Se você tiver uma função bastante complicada, poderá mover essa lógica para sua própria expressão de função nomeada

    ```javascript
    // ruim
    [1, 2, 3].map(function (x) {
      const y = x + 1
      return x * y
    })

    // bom
    [1, 2, 3].map(x => {
      const y = x + 1
      return x * y
    })
    ```

  - Se o corpo da função consistir em uma única instrução retornando uma [expressão](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions) sem efeitos colaterais, omita as chaves e utilize a sintaxe do return implícito. Se não for o caso, mantenha as chaves e utilize a instrução `return`. eslint: [`arrow-parens`](https://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](https://eslint.org/docs/rules/arrow-body-style.html)

    > Por que? Syntactic sugar. Fica de fácil leitura quando há várias funções encadeadas.
    ```javascript
    // ruim
    [1, 2, 3].map(number => {
      const nextNumber = number + 1
      `A string containing the ${nextNumber}.`
    })

    // bom
    [1, 2, 3].map(number => `A string containing the ${number + 1}.`)

    // bom
    [1, 2, 3].map(number => {
      const nextNumber = number + 1
      return `A string containing the ${nextNumber}.`
    })

    // bom
    [1, 2, 3].map((number, index) => ({
      [index]: number,
    }))
    ```

  - Se houver apenas um argumento, não inclua parênteses em torno para maior clareza e consistência

    ```javascript
    // ruim
    [1, 2, 3].map(x => x * x)

    // bom
    [1, 2, 3].map(x => x * x)


    // ruim
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ))

    // bom
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we don’t want it to take up space on the .map line!`
    ))
    ```

  - Evite uma confusão de sintaxe de arrow function (`=>`) com operadores de comparação (`<=`, `>=`). eslint: [`no-confusing-arrow`](https://eslint.org/docs/rules/no-confusing-arrow)

    ```javascript
    // ruim
    const itemHeight = item => item.height <= 256 ? item.largeSize : item.smallSize

    // ruim
    const itemHeight = item => item.height >= 256 ? item.largeSize : item.smallSize

    // bom
    const itemHeight = item => (item.height <= 256 ? item.largeSize : item.smallSize)

    // bom
    const itemHeight = item => {
      const { height, largeSize, smallSize } = item
      return height <= 256 ? largeSize : smallSize
    }
    ```

  - Reforce a localização do corpo das arrow functions com retornos implícitos. eslint: [`implicit-arrow-linebreak`](https://eslint.org/docs/rules/implicit-arrow-linebreak)

    ```javascript
    // ruim
    foo =>
      bar

    foo =>
      (bar)

    // bom
    foo => bar
    foo => (bar)
    foo => (
       bar
    )
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Classes & Construtores

  - Classes tornam seu código mais conciso e auto-documentado, e é um ótimo recurso do ES6, mas não é só porque você tem um martelo que significa que tudo agora é um prego. Tente descobrir o que funciona melhor e esteja ciente das advertências antes de decidir se precisa de uma Classe ou não. Você pode ver os próximos tópicos como sugestões, e não como regras.

    ```javascript
    // função
    function Queue(contents = []) {
      this.queue = [...contents]
    }

    Queue.prototype.pop = function () {
      const value = this.queue[0]
      this.queue.splice(0, 1)
      return value
    }

    // classe
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents]
      }

      pop() {
        const value = this.queue[0]
        this.queue.splice(0, 1)
        return value
      }
    }
    ```

  - Use `extends` para inheritance.

    > Por que? É uma maneira integrada de herdar a funcionalidade do protótipo sem interromper `instanceof`.

    ```javascript
    // função
    const inherits = require('inherits')

    function PeekableQueue(contents) {
      Queue.apply(this, contents)
    }

    inherits(PeekableQueue, Queue)

    PeekableQueue.prototype.peek = function () {
      return this.queue[0]
    }

    // classe
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0]
      }
    }
    ```

  - Métodos podem retornar `this` para ajudar em uma cadeia de métodos.

    ```javascript
    // função
    Jedi.prototype.jump = function () {
      this.jumping = true
      return true
    }

    Jedi.prototype.setHeight = function (height) {
      this.height = height
    }

    const luke = new Jedi()
    luke.jump() // => true
    luke.setHeight(20) // => undefined

    // classe
    class Jedi {
      jump() {
        this.jumping = true
        return this
      }

      setHeight(height) {
        this.height = height
        return this
      }
    }

    const luke = new Jedi()

    luke.jump()
      .setHeight(20)
    ```

  - Esa tudo bem escrever um método `toString()` costumizado, apenas tenha certeza de que funciona bem e não tenha efeitos colaterais

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name'
      }

      getName() {
        return this.name
      }

      toString() {
        return `Jedi - ${this.getName()}`
      }
    }
    ```

  - As classes têm um construtor padrão se um não for especificado. Uma função de construtor vazia ou que apenas delega para uma classe pai é desnecessária. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // ruim
    class Jedi {
      constructor() {}

      getName() {
        return this.name
      }
    }

    // ruim
    class Rey extends Jedi {
      constructor(...args) {
        super(...args)
      }
    }

    // bom
    class Rey extends Jedi {
      constructor(...args) {
        super(...args)
        this.name = 'Rey'
      }
    }
    ```

  - Evite membros de classe duplicados. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Por que? As declarações duplicadas dos membros da classe irão preferir silenciosamente a última delas - ter duplicatas é quase certamente um bug.

    ```javascript
    // ruim
    class Foo {
      bar() { return 1 }
      bar() { return 2 }
    }

    // bom
    class Foo {
      bar() { return 1 }
    }

    // bom
    class Foo {
      bar() { return 2 }
    }
    ```

  - Os métodos de classe devem usar `this` ou ser transformados em um método estático, a menos que uma biblioteca ou estrutura externa exija o uso de métodos não estáticos específicos. Ser um método de instância deve indicar que ele se comporta de maneira diferente com base nas propriedades do receptor. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // ruim
    class Foo {
      bar() {
        console.log('bar')
      }
    }

    // bom - this é usado
    class Foo {
      bar() {
        console.log(this.bar)
      }
    }

    // bom - constructor está isento
    class Foo {
      constructor() {
        // ...
      }
    }

    // bom - static methods aren't expected to use this
    class Foo {
      static bar() {
        console.log('bar')
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Módulos

  - Sempre use módulos (`import` /` export`) em vez de um sistema de módulos não padronizado. Você sempre pode transpilar para o seu sistema de módulo preferido.

    > Por que? Módulos são o futuro, então vamos começar a usar o futuro agora.

    ```javascript
    // ruim
    const AirbnbStyleGuide = require('./AirbnbStyleGuide')
    module.exports = AirbnbStyleGuide.es6

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide'
    export default AirbnbStyleGuide.es6

    // melhor
    import { es6 } from './AirbnbStyleGuide'
    export default es6
    ```

  - Não use wildcard imports.

    > Por que? Isso garante que você tenha um único default export.

    ```javascript
    // ruim
    import * as AirbnbStyleGuide from './AirbnbStyleGuide'

    // bom
    import AirbnbStyleGuide from './AirbnbStyleGuide'
    ```

  - E não use default export diretamente de um import.

    > Por que? Although the one-liner is concise, having one clear way to import and one clear way to export makes things consistent.

    ```javascript
    // ruim
    // es6.js
    export { es6 as default } from './AirbnbStyleGuide'

    // bom
    // es6.js
    import { es6 } from './AirbnbStyleGuide'
    export default es6
    ```

  - Somente importe um caminho em uma única importação
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    
    > Por que? Ter várias linhas importadas do mesmo caminho pode dificultar a manutenção do código.

    ```javascript
    // ruim
    import foo from 'foo'
    // … outros imports … //
    import { named1, named2 } from 'foo'

    // bom
    import foo, { named1, named2 } from 'foo'

    // bom
    import foo, {
      named1,
      named2,
    } from 'foo'
    ```

  - Não exporte mutable bindings.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Por que? A mutação deve ser evitada em geral, mas em particular ao exportar mutable bindings. Embora essa técnica possa ser necessária para alguns casos especiais, em geral, apenas referências constantes devem ser exportadas.

    ```javascript
    // ruim
    let foo = 3
    export { foo }

    // bom
    const foo = 3
    export { foo }
    ```

  - Nos módulos com uma única exportação, prefira a exportação padrão à exportação nomeada. Sempre exporte após o nome.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    
    > Por que? Incentivar mais arquivos que exportam apenas uma coisa, o que é melhor para legibilidade e manutenção.

    ```javascript
    // ruim
    export function foo() {}

    // bom, mas não o melhor
    export default function foo() {}

    // melhor
    const foo = function foo() {}

    export default foo
    ```

  - Coloque todos os `import`s acima de instruções que não são importo.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Por que? Como os `import`s são hoisted, mantê-los todos por cima de tudo previne que não haverá comportamentos inesperados.

    ```javascript
    // ruim
    import foo from 'foo'
    foo.init()

    import bar from 'bar'

    // bom
    import foo from 'foo'
    import bar from 'bar'

    foo.init()
    ```

  - As importações de múltiplas linhas devem ser indentadas assim como arrays múltiplas linhas e objetos.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > Por que? Blocos de chaves de importação seguem as mesmas regras de indentação que todos os outros blocos de chaves no guia de estilo.

    ```javascript
    // ruim
    import {longNameA, longNameB, longNameC, longNameD, longNameE} from 'path'

    // bom
    import {
      longNameA,
      longNameB,
      longNameC,
      longNameD,
      longNameE,
    } from 'path'
    ```

  - Não permita a syntax do Webpack loader nas instruções de import.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Por que? Como o uso da sintaxe do Webpack nas importações, o código é associado a um empacotador de módulo. Prefira usar a sintaxe do loader syntax em `webpack.config.js`.

    ```javascript
    // ruim
    import fooSass from 'css!sass!foo.scss'
    import barCss from 'style!css!bar.css'

    // bom
    import fooSass from 'foo.scss'
    import barCss from 'bar.css'
    ```

  - Não defina as extensões nos arquivos de JS e JSX
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > Por que? A inclusão de extensões inibe a refatoração e codifica inadequadamente os detalhes da implementação do módulo que você está importando para todos os consumidores.
  
    ```javascript
    // ruim
    import foo from './foo.js'
    import bar from './bar.jsx'
    import baz from './baz/index.jsx'

    // bom
    import foo from './foo'
    import bar from './bar'
    import baz from './baz'
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Iteradores

  - Prefira as funções high-order de JavaScript em vez de loops como `for-in` ou `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > Por que? Isso reforça nossa regra imutável. Lidar com funções puras que retornam valores é mais fácil de entender do que efeitos colaterais.

    > Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... para iterar sob arrays, e `Object.keys()` / `Object.values()` / `Object.entries()` para produzir arrays e então iterar sob objectos.

    ```javascript
    const numbers = [1, 2, 3, 4, 5]

    // ruim
    let sum = 0
    for (let num of numbers) {
      sum += num
    }
    sum === 15

    // bom
    let sum = 0
    numbers.forEach(num => {
      sum += num
    })
    sum === 15

    // melhor
    const sum = numbers.reduce((total, num) => total + num, 0)
    sum === 15

    // ruim
    const increasedByOne = []
    for (let i = 0 i < numbers.length i++) {
      increasedByOne.push(numbers[i] + 1)
    }

    // bom
    const increasedByOne = []
    numbers.forEach(num => {
      increasedByOne.push(num + 1)
    })

    // melhor (keeping it functional)
    const increasedByOne = numbers.map(num => num + 1)
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Propriedades

  - Use notação de ponto ao acessar propriedades. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    }

    // ruim
    const isJedi = luke['jedi']

    // bom
    const isJedi = luke.jedi
    ```

  - Use notação de colchetes `[]` quando precisar acessar uma propriedade com uma variável.

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    }

    function getProp(prop) {
      return luke[prop]
    }

    const isJedi = getProp('jedi')
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Variáveis

  - Sempre use `const` ou `let` para declarar variáveis. Não fazer isso resultará em variáveis ​​globais. Queremos evitar poluir o espaço de variáveis globais. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // ruim
    superPower = new SuperPower()

    // bom
    const superPower = new SuperPower()
    ```

  - Use uma declaração de `const` ou `let` por variável ou atribuição. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Por que? É mais fácil adicionar novas declarações de variáveis ​​dessa maneira, e você nunca precisa se preocupar em trocar um ` for a `, ou introduzir apenas alterações de pontuação. Você também pode percorrer cada declaração com o depurador, em vez de pular todas elas de uma vez. A mesma coisa quando um erro aparece relacionado a esta cadeia de declaração

    ```javascript
    // ruim
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z'

    // ruim
    // (compare com o código acima, e tente achar o erro)
    const items = getItems(),
        goSportsTeam = true
        dragonball = 'z'

    // bom
    const items = getItems()
    const goSportsTeam = true
    const dragonball = 'z'
    ```

  - Agrupe todas as `const`s e então todas as `let`s.

    > Por que? Isso é útil quando, posteriormente, você precisar atribuir uma variável, dependendo de uma das variáveis ​​atribuídas anteriormente.

    ```javascript
    // ruim
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true

    // ruim
    let i
    const items = getItems()
    let dragonball

    // bom
    const goSportsTeam = true
    const items = getItems()
    let dragonball
    let i
    let length
    ```

  - Atribua variáveis a​​onde você vá precisar delas, mas coloque-as em um local razoável.

    > Por que? `let` e `const` tem escopo por bloco e não escopo por função.

    ```javascript
    // ruim - chamada de função desnecessária
    function checkName(hasName) {
      const name = getName()

      if (hasName === 'test') {
        return false
      }

      if (name === 'test') {
        this.setName('')
        return false
      }

      return name
    }

    // bom
    function checkName(hasName) {
      if (hasName === 'test') {
        return false
      }

      const name = getName()

      if (name === 'test') {
        this.setName('')
        return false
      }

      return name
    }
    ```

  - Não encadeie atribuições de variáveis. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

    > Por que? Chaining variable assignments creates implicit global variables.

    ```javascript
    // ruim
    (() => {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) )
      // The let keyword only applies to variable a variables b and c become
      // global variables.
      let a = b = c = 1
    })()

    console.log(a) // throws ReferenceError
    console.log(b) // 1
    console.log(c) // 1

    // bom
    (() => {
      let a = 1
      let b = a
      let c = a
    })()

    console.log(a) // throws ReferenceError
    console.log(b) // throws ReferenceError
    console.log(c) // throws ReferenceError

    // the same applies for `const`
    ```

  - Evite usar incrementos e decrementos unitários (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Por que? De acordo com a [documentação do eslint](https://eslint.org/docs/rules/no-plusplus), instruções de incremento e decréscimo unárias estão sujeitas à inserção automática de ponto e vírgula e podem causar erros silenciosos com valores de incremento ou decremento em um aplicativo. Também é mais expressivo alterar seus valores com instruções como `num += 1` em vez de` num ++` ou `num++`. A proibição de instruções de incremento e decréscimo unitárias também impede que você pré-incremente / pré-decremente valores inadvertidamente, o que também pode causar comportamento inesperado em seus programas.

    ```javascript
    // ruim

    const array = [1, 2, 3]
    let num = 1
    num++
    --num

    let sum = 0
    let truthyCount = 0
    for (let i = 0 i < array.length i++) {
      let value = array[i]
      sum += value
      if (value) {
        truthyCount++
      }
    }

    // bom

    const array = [1, 2, 3]
    let num = 1
    num += 1
    num -= 1

    const sum = array.reduce((a, b) => a + b, 0)
    const truthyCount = array.filter(Boolean).length
    ```

  - Evite quebrar linhas antes ou depois de `=` em uma atribuição. Se a sua atribuição ultrapassar o [`max-len`](https://eslint.org/docs/rules/max-len.html), envolva o valor em parênteses. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > Por que? Quebra de linhas envolta de `=` pode ofuscar o valor de uma atribuição.

    ```javascript
    // ruim
    const foo =
      superLongLongLongLongLongLongLongLongFunctionName()

    // ruim
    const foo
      = 'superLongLongLongLongLongLongLongLongString'

    // bom
    const foo = (
      superLongLongLongLongLongLongLongLongFunctionName()
    )

    // bom
    const foo = 'superLongLongLongLongLongLongLongLongString'
    ```

  - Não permita variáveis não utilizadas. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Por que? Variáveis ​​declaradas e não usadas em qualquer lugar do código provavelmente são um erro devido à refatoração incompleta. Tais variáveis ​​ocupam espaço no código e podem causar confusão pelos leitores.

    ```javascript
    // ruim

    const some_unused_var = 42

    // Variáveis somente reescritas não são consideras
    let y = 10
    y = 5

    // Uma leitura para uma modificação não é considerada
    let z = 0
    z = z + 1

    // Parâmetros não usados
    const getX = (x, y) => x

    // bom

    const getXPlusY = (x, y) => x + y

    const x = 1
    const y = a + 2

    alert(getXPlusY(x, y))

    // 'type' é ignorado memso se não usado porque está dentro de rest.
    // Essa é uma forma de extrair um objeto que omite a key específicada.
    const { type, ...coords } = data
    // 'coords' agora é o objeto'data' sem a propriedade 'type'
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Hoisting

  - Declarações `var` são `hoisted` ao topo do seu escopo de função, no mais próximo que ocorreu o fechamento, mas suas atribuições não são. Já declarações de `const` e `let` são abençoadas com o que o novo conceito chamado [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone).

    ```javascript
    // nós sabemos que isso não funcionaria (assumindo que
    // notDefined também não esteja definido nas variáveis globais)
    const = example = () => {
      console.log(notDefined) // => exibe ReferenceError
    }

    // criando uma declaração de variável depois de sua
    // referência, a variável irá funcionar devido ao seu
    // `hoisting`. Nota: a atribuiçõa do valor 'true' não é `hoisted`.
    const example = () => {
      console.log(declaredButNotAssigned) // => undefined
      var declaredButNotAssigned = true
    }

    // O interpretador faz o `hoist` da declaração de variável
    // para o topo do escpop. O que significa que o nosso
    // exemplo poderia ser reescrito como:
    const example = () => {
      let declaredButNotAssigned
      console.log(declaredButNotAssigned) // => undefined
      declaredButNotAssigned = true
    }

    // usando const e let
    const example = () => {
      console.log(declaredButNotAssigned) // => exibe ReferenceError
      console.log(typeof declaredButNotAssigned) // => exibe ReferenceError
      const declaredButNotAssigned = true
    }
    ```

  - Funções anônimas fazem `hoist` de seus nomes de variáveis, mas não a sua atribuição 

    ```javascript
    const example = () => {
      console.log(anonymous) // => undefined

      anonymous() // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression')
      }
    }
    ```

  - Funções com expressões nomeadas fazem `hoist` do nome da sua variável, mas não o nome da função ou seu corpo.

    ```javascript
    function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      superPower() // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying')
      }
    }

    // the same is true when the function name
    // O mesmo continua verdade quando o nome função
    // é o mesmo que o nome da variável da função
    function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      var named = function named() {
        console.log('named')
      }
    }
    ```

  - Declarações de funções fazem o `hoist` do seus nomes e corpos.

    ```javascript
    function example() {
      superPower() // => Flying

      function superPower() {
        console.log('Flying')
      }
    }
    
    // Lembre-se que isso não se aplica a arrow functions
    const example = () => {
      superPower() // => ReferenceError:

      const superPower = () => {
        console.log('Flying')
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Operadores de comparação & igualdade

  - Use `===` e `!==` em vez de `==` e `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  - Instruções condicionais, como a instrução `if`, avaliam sua expressão usando coerção com o método abstrato` ToBoolean` e sempre seguem estas regras simples:

    - **Objects** se torna **true**
    - **Undefined** se torna **false**
    - **Null** se torna **false**
    - **Booleans** se torna **the value of the boolean**
    - **Numbers** se torna **false** Se **+0, -0, or NaN**, Senão **true**
    - **Strings** se torna **false** Se a string é vazia `''`, Senão **true**

    ```javascript
    if ([0] && []) {
      // true
      // Uma array (mesmo vazia) é um objeto, objetos se tornam true
    }
    ```

  - Use `shortcuts` para valores booleanos, mas explicite a comparação com strings.

    ```javascript
    // ruim
    if (isValid === true) {
      // ...
    }

    // bom
    if (isValid) {
      // ...
    }

    // ruim
    if (name) {
      // ...
    }

    // bom
    if (name !== '') {
      // ...
    }
    ```

  - Utilize chaves para criar blocos em `case` e `default` que contém declarações léxicas (e.g. `let`, `const`, `function`, e `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > Por que? Declarações lexicais são visíveis em todo o bloco `switch`, mas somente são inicializadas quando atribuídas, o que só acontece quando seu` case` é atingido. Isso causa problemas quando várias cláusulas `case` tentam definir a mesma coisa.

    ```javascript
    // ruim
    switch (foo) {
      case 1:
        let x = 1
        break
      case 2:
        const y = 2
        break
      case 3:
        function f() {
          // ...
        }
        break
      default:
        class C {}
    }

    // bom
    switch (foo) {
      case 1: {
        let x = 1
        break
      }
      case 2: {
        const y = 2
        break
      }
      case 3: {
        function f() {
          // ...
        }
        break
      }
      case 4:
        bar()
        break
      default: {
        class C {}
      }
    }
    ```

  - Os ternários não devem ser aninhados e geralmente são expressões de linha única. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

    ```javascript
    // ruim
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null

    // split into 2 separated ternary expressions
    const maybeNull = value1 > value2 ? 'baz' : null

    // better
    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull

    // melhor
    const foo = maybe1 > maybe2 ? 'bar' : maybeNull
    ```

  - Evite declarações ternárias desnecessárias. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

    ```javascript
    // ruim
    const foo = a ? a : b
    const bar = c ? true : false
    const baz = c ? false : true

    // bom
    const foo = a || b
    const bar = !!c
    const baz = !c
    ```

  - Ao misturar operadores, coloque-os entre parênteses. A única exceção são os operadores aritméticos padrão: `+`, `-` e` ** `, pois sua precedência é amplamente compreendida. Recomendamos colocar `/` e `*` entre parênteses, porque sua precedência pode ser ambígua quando misturados.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > Por que? Isso melhora a legibilidade e esclarece a intenção do desenvolvedor.

    ```javascript
    // ruim
    const foo = a && b < 0 || c > 0 || d + 1 === 0

    // ruim
    const bar = a ** b - 5 % d

    // ruim
    // pode ser confundido por (a || b) && c
    if (a || b && c) {
      return d
    }

    // ruim
    const bar = a + b / c * d

    // bom
    const foo = (a && b < 0) || c > 0 || (d + 1 === 0)

    // bom
    const bar = a ** b - (5 % d)

    // bom
    if (a || (b && c)) {
      return d
    }

    // bom
    const bar = a + (b / c) * d
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Blocos

  - Use chaves para todos os blocos de multilinhas. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

    ```javascript
    // ruim
    if (test)
      return false

    // bom
    if (test) return false

    // bom
    if (test) {
      return false
    }
    ```

  - Se você estiver usando blocos multilinhas `{}` com `if` e `else`, coloque `else` na mesma linha como que seu bloco `if` está fechando. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

    ```javascript
    // ruim
    if (test) {
      thing1()
      thing2()
    }
    else {
      thing3()
    }

    // bom
    if (test) {
      thing1()
      thing2()
    } else {
      thing3()
    }
    ```

  - Se um bloco `if` sempre executa uma instrução `return`, o bloco `else` subsequente será desnecessário. Um `return` em um bloco `else if` seguindo um blocl `if` que contenha um `return` pode ser separados em múltiplos blocos `if`. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

    ```javascript
    // ruim
    const foo = () => {
      if (x) {
        return x
      } else {
        return y
      }
    }

    // ruim
    const cats = () => {
      if (x) {
        return x
      } else if (y) {
        return y
      }
    }

    // ruim
    const dogs = () => {
      if (x) {
        return x
      } else {
        if (y) {
          return y
        }
      }
    }

    // bom
    const foo = () => {
      if (x) {
        return x
      }

      return y
    }

    // bom
    const cats = () => {
      if (x) {
        return x
      }

      if (y) {
        return y
      }
    }

    // bom
    const dogs = x => {
      if (x) {
        if (z) {
          return y
        }
      } else {
        return z
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Declarações de controle

  - Caso sua declaração de controle (`if`,` while` etc.) fique muito longa ou exceda o comprimento máximo da linha, cada condição (agrupada) poderá ser inserida em uma nova linha. O operador lógico deve começar a linha.

    > Por que? A exigência de operadores no início da linha mantém os operadores alinhados e segue um padrão semelhante ao encadeamento de métodos. Isso também melhora a legibilidade, facilitando o acompanhamento visual de lógica complexa.

    ```javascript
    // ruim
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1()
    }

    // ruim
    if (foo === 123 &&
      bar === 'abc') {
      thing1()
    }

    // ruim
    if (foo === 123
      && bar === 'abc') {
      thing1()
    }

    // ruim
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1()
    }

    // bom
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1()
    }

    // bom
    if (
      (foo === 123 || bar === 'abc')
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1()
    }

    // bom
    if (foo === 123 && bar === 'abc') {
      thing1()
    }
    ```

  - Não use operadores de seleção no lugar das instruções de controle.

    ```javascript
    // ruim
    !isRunning && startRunning()

    // bom
    if (!isRunning) {
      startRunning()
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Comentários

  - Use `/** ... */` para comentários com muitas linhas.

    ```javascript
    // ruim
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    const make = tag => {

      // ...

      return element
    }

    // bom
    /**
     * make() returns a new element
     * based on the passed-in tag name
     * 
     * @param {String} tag
     * @return {Element} element
     */
    const make = tag => {

      // ...

      return element
    }
    ```

  - Use `//` para comentários de linha única. Coloque comentários de linha única em uma nova linha acima do assunto do comentário. Coloque uma linha vazia antes do comentário, a menos que esteja na primeira linha de um bloco.

    ```javascript
    // ruim
    const active = true  // is current tab

    // bom
    // is current tab
    const active = true

    // ruim
    function getType() {
      console.log('fetching type...')
      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }

    // bom
    function getType() {
      console.log('fetching type...')

      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this.type || 'no type'

      return type
    }
    ```

  - Comece todos os comentários após um espaço para que facilite a leitura. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

    ```javascript
    // ruim
    //is current tab
    const active = true

    // bom
    // is current tab
    const active = true

    // ruim
    /**
     *make() returns a new element
     *based on the passed-in tag name
     */
    const make = tag => {

      // ...

      return element
    }

    // bom
    /**
     * make() returns a new element
     * based on the passed-in tag name
     */
    const make = tag => {

      // ...

      return element
    }
    ```

  - Prefixar seus comentários com `FIXME` ou` TODO` ajuda outros desenvolvedores a entender rapidamente se você está apontando um problema que precisa ser revisado ou se está sugerindo uma solução para o problema que precisa ser implementado. Estes são diferentes dos comentários regulares porque são acionáveis. As ações são `FIXME: - precisa descobrir isso 'ou` TODO: - precisa implementar`.

  - Use `// FIXME:` para anotar problemas.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super()

        // FIXME: shouldn’t use a global here
        total = 0
      }
    }
    ```

  - Use `// TODO:` to anotar soluções de problemas.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super()

        // TODO: total should be configurable by an options param
        this.total = 0
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Espaçamento

  - Use soft spaces (caractere de espaço) definidos como 2 espaços. eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

    ```javascript
    // ruim
    const foo = () => {
    ∙∙∙∙let name
    }

    // ruim
    const bar = () => {
    ∙let name
    }

    // bom
    const baz = () => {
    ∙∙let name
    }
    ```

  - Coloque 1 espaço antes da chave principal. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

    ```javascript
    // ruim
    function test(){
      console.log('test')
    }

    // bom
    function test() {
      console.log('test')
    }

    // ruim
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    })

    // bom
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    })
    ```

  - Coloque 1 espaço antes do parêntese de abertura nas instruções de controle (`if`,` while` etc.). Não coloque espaço entre a lista de argumentos nem no nome da função nas chamadas e declarações de funções. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

    ```javascript
    // ruim
    if(isJedi) {
      fight ()
    }

    // bom
    if (isJedi) {
      fight()
    }

    // ruim
    function fight () {
      console.log ('Swooosh!')
    }

    // bom
    function fight() {
      console.log('Swooosh!')
    }
    ```

  - Defina operadores com espaços. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // ruim
    const x=y+5

    // bom
    const x = y + 5
    ```

  - Finalize arquivos com um único caractere de nova linha. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // ruim
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6
    ```

    ```javascript
    // ruim
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6↵
    ↵
    ```

    ```javascript
    // bom
    import { es6 } from './AirbnbStyleGuide'
      // ...
    export default es6↵
    ```

  - Use indentação ao criar cadeias longas de método (mais de 2 cadeias de método). Use um ponto inicial, que enfatize que a linha é uma chamada de método, não uma nova declaração. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // ruim
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led)

    // bom
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', `translate(${radius + margin},${radius + margin})`)
        .call(tron.led)

    // bom
    const leds = stage.selectAll('.led').data(data)
    ```

  - Deixe uma linha em branco após os blocos e antes da próxima instrução.

    ```javascript
    // ruim
    if (foo) {
      return bar
    }
    return baz

    // bom
    if (foo) {
      return bar
    }

    return baz

    // ruim
    const obj = {
      foo() {
      },
      bar() {
      },
    }
    return obj

    // bom
    const obj = {
      foo() {
      },

      bar() {
      },
    }

    return obj

    // ruim
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ]
    return arr

    // bom
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ]

    return arr
    ```

  - Não envolvar o interior dos seus blocos com linhas vazias. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

    ```javascript
    // ruim
    const bar = () => {

      console.log(foo)

    }

    // ruim
    if (baz) {

      console.log(qux)
    } else {
      console.log(foo)

    }

    // ruim
    class Foo {

      constructor(bar) {
        this.bar = bar
      }
    }

    // bom
    const bar = () => {
      console.log(foo)
    }

    // bom
    if (baz) {
      console.log(qux)
    } else {
      console.log(foo)
    }
    ```

  - Não use várias linhas em branco para preencher seu código. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // ruim
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName


        this.email = email


        this.setAge(birthday)
      }


      setAge(birthday) {
        const today = new Date()


        const age = this.getAge(today, birthday)


        this.age = age
      }


      getAge(today, birthday) {
        // ..
      }
    }

    // bom
    class Person {
      constructor(fullName, email, birthday) {
        this.fullName = fullName
        this.email = email
        this.setAge(birthday)
      }

      setAge(birthday) {
        const today = new Date()
        const age = getAge(today, birthday)
        this.age = age
      }

      getAge(today, birthday) {
        // ..
      }
    }
    ```

  - Não adicione espaços entre parênteses. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

    ```javascript
    // ruim
    function bar( foo ) {
      return foo
    }

    // bom
    function bar(foo) {
      return foo
    }

    // ruim
    if ( foo ) {
      console.log(foo)
    }

    // bom
    if (foo) {
      console.log(foo)
    }
    ```

  - Não adicione espaços entre colchetes. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // ruim
    const foo = [ 1, 2, 3 ]
    console.log(foo[ 0 ])

    // bom
    const foo = [1, 2, 3]
    console.log(foo[0])
    ```

  - Adicione espaços dentro de chaves. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // ruim
    const foo = {clark: 'kent'}

    // bom
    const foo = { clark: 'kent' }
    ```

  - Evite ter linhas de código com mais de 100 caracteres (incluindo espaço em branco).  
  
    > Nota: Sobre a regra acima, strings longas estão isentas dessa regra e não devem ser divididas. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Por que? Isso garante legibilidade e facilidade de manutenção.

    ```javascript
    // ruim
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy

    // bom
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy
    ```

  - Exija espaçamento consistente dentro de um token de bloco aberto e o próximo token na mesma linha. Essa regra também aplica espaçamento consistente dentro de um token de bloco próximo e um token anterior na mesma linha. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // ruim
    if (foo) {bar = 0}

    // bom
    if (foo) { bar = 0 }
    ```

  - Evite espaços antes de vírgulas e exija um espaço após vírgulas. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // ruim
    const foo = 1,bar = 2
    const arr = [1 , 2]

    // bom
    const foo = 1, bar = 2
    const arr = [1, 2]
    ```

  - Impor espaçamento dentro dos colchetes de propriedades computadas. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

    ```javascript
    // ruim
    obj[foo ]
    obj[ 'foo']
    var x = {[ b ]: a}
    obj[foo[ bar ]]

    // bom
    obj[foo]
    obj['foo']
    var x = { [b]: a }
    obj[foo[bar]]
    ```

  - Evite espaços entre funções e suas invocações. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // ruim
    func ()

    func
    ()

    // bom
    func()
    ```

  - Impor espaçamento entre chaves e valores nas propriedades literais do objeto. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // ruim
    var obj = { foo : 42 }
    var obj2 = { foo:42 }

    // bom
    var obj = { foo: 42 }
    ```

  - Evite arrastar espaços no final das linhas. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  - Evite várias linhas vazias, permita apenas uma nova linha no final dos arquivos e evite uma nova linha no início dos arquivos. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // ruim - multiplas linhas vaziasa
    var x = 1


    var y = 2

    // ruim - 2+ linhas no fim do arquivo
    var x = 1
    var y = 2


    // ruim - 1+ linhas no início do arquivo

    var x = 1
    var y = 2

    // bom
    var x = 1
    var y = 2

    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Vírgulas

  - Vírgulas a frente do item: **Nope.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

    ```javascript
    // ruim
    const story = [
        once
      , upon
      , aTime
    ]

    // bom
    const story = [
      once,
      upon,
      aTime,
    ]

    // ruim
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    }

    // bom
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Ponto e vírgula

  - **Nope.** [Prettier](https://prettier.io/) vai acabar limpando todas elas. Nas próximas linhas são mostrados alguns casos que você deve tomar cuidado ao não colocar ponto e vírgula

    > Por que? Quando o JavaScript encontra uma quebra de linha sem ponto e vírgula, ele usa um conjunto de regras chamado [Inserção automática de ponto e vírgula](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) para determinar se deve ou não considerar essa quebra de linha como o final de uma instrução.

    ```javascript
    // ruim - levanta uma exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // ruim - returna `undefined` em vez do valor da próxima linha - always happens when `return` is on a line by itself because of ASI!
    function foo() {
      return
        'search your feelings, you know it to be foo'
    }

    // bom
    const luke = {}
    const leia = {}
    [luke, leia].forEach((jedi) => {
      jedi.father = 'vader'
    })

    // bom
    const reaction = "No! That’s impossible!"
    async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }()

    // bom
    const foo = () => {
      return 'search your feelings, you know it to be foo'
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Fundição e coerção de tipos

  - Execute coerção de tipo no início da declaração.

  - Strings: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    // => this.reviewScore = 9

    // ruim
    const totalScore = new String(this.reviewScore) // typeof totalScore is "object" not "string"

    // ruim
    const totalScore = this.reviewScore + '' // invokes this.reviewScore.valueOf()

    // ruim
    const totalScore = this.reviewScore.toString() // isn’t guaranteed to return a string

    // bom
    const totalScore = String(this.reviewScore)
    ```

  - Numbers: Use `Number` para a conversão de tipos e` parseInt` sempre com um redix para o parsing de strings. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const inputValue = '4'

    // ruim
    const val = new Number(inputValue)

    // ruim
    const val = +inputValue

    // ruim
    const val = inputValue >> 0

    // ruim
    const val = parseInt(inputValue)

    // bom
    const val = Number(inputValue)

    // bom
    const val = parseInt(inputValue, 10)
    ```

  - Booleans: eslint: [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

    ```javascript
    const age = 0

    // ruim
    const hasAge = new Boolean(age)

    // bom
    const hasAge = Boolean(age)

    // melhor!
    const hasAge = !!age
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Nomeação

  - Evite nomes de letras únicas. Seja descritivo com sua nomeação. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

    ```javascript
    // ruim
    const q = () => {
      // ...
    }

    // bom
    const query = () => {
      // ...
    }
    ```

  - Use camelCase ao nomear objetos, funções e instâncias. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // ruim
    const OBJEcttsssss = {}
    const this_is_my_object = {}

    // bom
    const thisIsMyObject = {}
    const thisIsMyFunction = () => {}
    ```

  - Use PascalCase apenas ao nomear construtores ou classes. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

    ```javascript
    // ruim
    const user = (options) => {
      this.name = options.name
    }

    const bad = new user({
      name: 'nope',
    })

    // bom
    class User {
      constructor(options) {
        this.name = options.name
      }
    }

    const good = new User({
      name: 'yup',
    })
    ```

  - Não use sublinhados à direita ou à direita. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Por que? JavaScript não tem o conceito de privacidade em termos de propriedades ou métodos. Embora um sublinhado de destaque seja uma convenção comum que signifique "privado", na verdade, essas propriedades são totalmente públicas e, como tal, fazem parte do seu contrato público de API. Essa convenção pode levar os desenvolvedores a pensarem erroneamente que uma mudança não será considerada quebra ou que testes não são necessários. tldr: se você deseja que algo seja “privado”, ele não deve estar presente de maneira observável.

    ```javascript
    // ruim
    this.__firstName__ = 'Panda'
    this.firstName_ = 'Panda'
    this._firstName = 'Panda'

    // bom
    this.firstName = 'Panda'
    ```

  - Não salve referências em `this`. Use arrow functions.

    ```javascript
    // ruim
    const foo = () => {
      const self = this
      return function () {
        console.log(self)
      }
    }

    // ruim
    const foo = () => {
      const that = this
      return function () {
        console.log(that)
      }
    }

    // bom
    const foo = () => {
      return () => {
        console.log(this)
      }
    }
    ```

  - Um nome de arquivo base deve corresponder exatamente ao nome de sua exportação padrão.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox

    // ruim
    import CheckBox from './checkBox'

    // ruim
    import CheckBox from './check_box'

    // bom
    import CheckBox from './CheckBox'
    ```

  - Use camelCase ao exportar uma função padrão. Seu nome do arquivo deve ser idêntico ao nome da sua função.

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide
    ```

  - Use PascalCase ao exportar um construtor / classe / singleton / biblioteca de funções / objeto vazio.


    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    }

    export default AirbnbStyleGuide
    ```

  - Acrônimos e inicialismos devem sempre estar em maiúsculas ou minúsculas.

    > Por que? Os nomes são de legibilidade, para não apaziguar um algoritmo de computador.

    ```javascript
    // ruim
    import SmsContainer from './containers/SmsContainer'

    // bom
    import SMSContainer from './containers/SMSContainer'

    // ruim
    const HttpRequests = [
      // ...
    ]

    // bom
    const HTTPRequests = [
      // ...
    ]

    // also good
    const httpRequests = [
      // ...
    ]

    // melhor
    import TextMessageContainer from './containers/TextMessageContainer'

    // melhor
    const requests = [
      // ...
    ]
    ```

  - Você pode opcionalmente colocar em maiúscula uma constante apenas se (1) for exportada, (2) for uma `const` (não puder ser reatribuída) e (3) o programador possa confiar que ela (e em suas propriedades aninhadas) nunca muda.

    > Por que? Essa é uma ferramenta adicional para ajudar em situações nas quais o programador não tem certeza se uma variável pode mudar. UPPERCASE_VARIABLES estão deixando o programador saber que eles podem confiar que a variável (e suas propriedades) não vai mudar.
    - E quanto a todas as variáveis ​​`const`? - Isso é desnecessário, portanto, maiúsculas não devem ser usadas para constantes em um arquivo. No entanto, ele deve ser usado para constantes exportadas.
    - E os objetos exportados? - Maiúsculas no nível superior de exportação (por exemplo, `EXPORTED_OBJECT.key`) e faça com que todas as propriedades aninhadas não sejam alteradas.

    ```javascript
    // ruim
    const PRIVATE_VARIABLE = 'should not be unnecessarily uppercased within a file'

    // ruim
    export const THING_TO_BE_CHANGED = 'should obviously not be uppercased'

    // ruim
    export let REASSIGNABLE_VARIABLE = 'do not use let with uppercase variables'


    // allowed but does not supply semantic value
    export const apiKey = 'SOMEKEY'

    // better in most cases
    export const API_KEY = 'SOMEKEY'


    // ruim - unnecessarily uppercases key while adding no semantic value
    export const MAPPING = {
      KEY: 'value'
    }

    // bom
    export const MAPPING = {
      key: 'value'
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Accessors

  - Funções accessors para propriedades não são necessárias.

  - Não use `getters` / `setters` JavaScript, pois eles causam efeitos colaterais inesperados e são mais difíceis de testar, manter e fundamentar. Em vez disso, se você criar funções accessors, use `getVal ()` e `setVal ('hello')`.

    ```javascript
    // ruim
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // bom
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  - Se a propriedade/método for um `boolean`, use `isVal()` ou `hasVal()`.

    ```javascript
    // ruim
    if (!dragon.age()) {
      return false
    }

    // bom
    if (!dragon.hasAge()) {
      return false
    }
    ```

  - Está tudo bem criar funções `get()` e `set()`, mas **sempre seja consistente**.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue'
        this.set('lightsaber', lightsaber)
      }

      set(key, val) {
        this[key] = val
      }

      get(key) {
        return this[key]
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Testes

  - **Precisamos testar ein**

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Tradução

  Este guia também está disponível em outra linguagem:

  - **English**: [/README.md](/README.md)

**[⬆ voltar ao topo](#lista-de-conteúdos)**

# )