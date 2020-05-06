# Divisio JavaScript Style Guide = () => (

*Essa é a abordagem da Divisio com JavaScript*

Mantenha em mente que algumas regras serão automaticamente arrumadas com o [Prettier](https://prettier.io/), e outras não. Este guia também está disponível em outras linguagens. Veja em [Tradução](#tradução)

Outros Style Guides

  - [React](react/)

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
  1. [Operadores de comparação e igualdade](#comparação-operadores--igualdade)
  1. [Blocos](#blocos)
  1. [Declarações de controle](#declaração-controle)
  1. [Comentários](#comentários)
  1. [Whitespace](#whitespace)
  1. [Vírgulas](#vírgulas)
  1. [Ponto e vírgula](#ponto--vírgula)
  1. [Tipos Casting & Coercion](#tipos-casting--coercion)
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

## Functions

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

  > **Nota**: O ECMA-262 define um bloco como uma lista de instruções. Uma declaração de função não é uma instrução.

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

  - As funções com assinaturas ou chamadas de várias linhas devem ser identadas como qualquer outra lista de várias linhas neste guia: com cada item em uma linha por si só. eslint: [`function-paren-newline`](https://eslint.org/docs/rules/function-paren-newline)

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

## Classes & Constructors

  - Class make your code more concise and self-documenting, and it's a great feature of ES6. But just because you have a hammer that doesn't mean everything is now a nail. Try to figure out what works best and be aware of the caveats before deciding whether you need a class or not. You can see the next topics as suggestions, not rules.

    ```javascript
    // ruim
    function Queue(contents = []) {
      this.queue = [...contents]
    }

    Queue.prototype.pop = function () {
      const value = this.queue[0]
      this.queue.splice(0, 1)
      return value
    }

    // bom
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

  - Use `extends` for inheritance.

    > Por que? It is a built-in way to inherit prototype functionality without breaking `instanceof`.

    ```javascript
    // ruim
    const inherits = require('inherits')

    function PeekableQueue(contents) {
      Queue.apply(this, contents)
    }

    inherits(PeekableQueue, Queue)

    PeekableQueue.prototype.peek = function () {
      return this.queue[0]
    }

    // bom
    class PeekableQueue extends Queue {
      peek() {
        return this.queue[0]
      }
    }
    ```

  - Methods can return `this` to help with method chaining.

    ```javascript
    // ruim
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

    // bom
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

  - It’s okay to write a custom `toString()` method, just make sure it works successfully and causes no side effects.

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

  - Classes have a default constructor if one is not specified. An empty constructor function or one that just delegates to a parent class is unnecessary. eslint: [`no-useless-constructor`](https://eslint.org/docs/rules/no-useless-constructor)

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

  - Avoid duplicate class members. eslint: [`no-dupe-class-members`](https://eslint.org/docs/rules/no-dupe-class-members)

    > Por que? Duplicate class member declarations will silently prefer the last one - having duplicates is almost certainly a bug.

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

  - Class methods should use `this` or be made into a static method unless an external library or framework requires to use specific non-static methods. Being an instance method should indicate that it behaves differently based on properties of the receiver. eslint: [`class-methods-use-this`](https://eslint.org/docs/rules/class-methods-use-this)

    ```javascript
    // ruim
    class Foo {
      bar() {
        console.log('bar')
      }
    }

    // bom - this is used
    class Foo {
      bar() {
        console.log(this.bar)
      }
    }

    // bom - constructor is exempt
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

## Modules

  - Always use modules (`import`/`export`) over a non-standard module system. You can always transpile to your preferred module system.

    > Por que? Modules are the future, let’s start using the future now.

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

  - Do not use wildcard imports.

    > Por que? This makes sure you have a single default export.

    ```javascript
    // ruim
    import * as AirbnbStyleGuide from './AirbnbStyleGuide'

    // bom
    import AirbnbStyleGuide from './AirbnbStyleGuide'
    ```

  - And do not export directly from an import.

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

  - Only import from a path in one place.
 eslint: [`no-duplicate-imports`](https://eslint.org/docs/rules/no-duplicate-imports)
    > Por que? Having multiple lines that import from the same path can make code harder to maintain.

    ```javascript
    // ruim
    import foo from 'foo'
    // … some other imports … //
    import { named1, named2 } from 'foo'

    // bom
    import foo, { named1, named2 } from 'foo'

    // bom
    import foo, {
      named1,
      named2,
    } from 'foo'
    ```

  - Do not export mutable bindings.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > Por que? Mutation should be avoided in general, but in particular when exporting mutable bindings. While this technique may be needed for some special cases, in general, only constant references should be exported.

    ```javascript
    // ruim
    let foo = 3
    export { foo }

    // bom
    const foo = 3
    export { foo }
    ```

  - In modules with a single export, prefer default export over named export. Always export after name it. 
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)
    > Por que? To encourage more files that only ever export one thing, which is better for readability and maintainability.

    ```javascript
    // ruim
    export function foo() {}

    // still bad
    export default function foo() {}

    // bom
    const foo = function foo() {}

    export default foo
    ```

  - Put all `import`s above non-import statements.
 eslint: [`import/first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/first.md)
    > Por que? Since `import`s are hoisted, keeping them all at the top prevents surprising behavior.

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

  - Multiline imports should be indented just like multiline array and object literals.
 eslint: [`object-curly-newline`](https://eslint.org/docs/rules/object-curly-newline)

    > Por que? The curly braces follow the same indentation rules as every other curly brace block in the style guide, as do the trailing commas.

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

  - Disallow Webpack loader syntax in module import statements.
 eslint: [`import/no-webpack-loader-syntax`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-webpack-loader-syntax.md)
    > Por que? Since using Webpack syntax in the imports couples the code to a module bundler. Prefer using the loader syntax in `webpack.config.js`.

    ```javascript
    // ruim
    import fooSass from 'css!sass!foo.scss'
    import barCss from 'style!css!bar.css'

    // bom
    import fooSass from 'foo.scss'
    import barCss from 'bar.css'
    ```

  - Do not include JavaScript filename extensions
 eslint: [`import/extensions`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/extensions.md)
    > Por que? Including extensions inhibits refactoring, and inappropriately hardcodes implementation details of the module you're importing in every consumer.

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

## Iterators

  - Prefer JavaScript’s higher-order functions instead of loops like `for-in` or `for-of`. eslint: [`no-iterator`](https://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](https://eslint.org/docs/rules/no-restricted-syntax)

    > Por que? This enforces our immutable rule. Dealing with pure functions that return values is easier to reason about than side effects.

    > Use `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... to iterate over arrays, and `Object.keys()` / `Object.values()` / `Object.entries()` to produce arrays so you can iterate over objects.

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

    // melhor (use the functional force)
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

## Properties

  - Use dot notation when accessing properties. eslint: [`dot-notation`](https://eslint.org/docs/rules/dot-notation.html)

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

  - Use bracket notation `[]` when accessing properties with a variable.

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

## Variables

  - Always use `const` or `let` to declare variables. Not doing so will result in global variables. We want to avoid polluting the global namespace. eslint: [`no-undef`](https://eslint.org/docs/rules/no-undef) [`prefer-const`](https://eslint.org/docs/rules/prefer-const)

    ```javascript
    // ruim
    superPower = new SuperPower()

    // bom
    const superPower = new SuperPower()
    ```

  - Use one `const` or `let` declaration per variable or assignment. eslint: [`one-var`](https://eslint.org/docs/rules/one-var.html)

    > Por que? It’s easier to add new variable declarations this way, and you never have to worry about swapping out a `` for a `,` or introducing punctuation-only diffs. You can also step through each declaration with the debugger, instead of jumping through all of them at once. Same thing when a error pops up related to this declaration chain

    ```javascript
    // ruim
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z'

    // ruim
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true
        dragonball = 'z'

    // bom
    const items = getItems()
    const goSportsTeam = true
    const dragonball = 'z'
    ```

  - Group all your `const`s and then group all your `let`s.

    > Por que? This is helpful when later on you might need to assign a variable depending on one of the previous assigned variables.

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

  - Assign variables where you need them, but place them in a reasonable place.

    > Por que? `let` and `const` are block scoped and not function scoped.

    ```javascript
    // ruim - unnecessary function call
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

  - Don’t chain variable assignments. eslint: [`no-multi-assign`](https://eslint.org/docs/rules/no-multi-assign)

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

  - Avoid using unary increments and decrements (`++`, `--`). eslint [`no-plusplus`](https://eslint.org/docs/rules/no-plusplus)

    > Por que? Per the [eslint documentation](https://eslint.org/docs/rules/no-plusplus), unary increment and decrement statements are subject to automatic semicolon insertion and can cause silent errors with incrementing or decrementing values within an application. It is also more expressive to mutate your values with statements like `num += 1` instead of `num++` or `num ++`. Disallowing unary increment and decrement statements also prevents you from pre-incrementing/pre-decrementing values unintentionally which can also cause unexpected behavior in your programs.

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

  - Avoid linebreaks before or after `=` in an assignment. If your assignment violates [`max-len`](https://eslint.org/docs/rules/max-len.html), surround the value in parens. eslint [`operator-linebreak`](https://eslint.org/docs/rules/operator-linebreak.html).

    > Por que? Linebreaks surrounding `=` can obfuscate the value of an assignment.

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

  - Disallow unused variables. eslint: [`no-unused-vars`](https://eslint.org/docs/rules/no-unused-vars)

    > Por que? Variables that are declared and not used anywhere in the code are most likely an error due to incomplete refactoring. Such variables take up space in the code and can lead to confusion by readers.

    ```javascript
    // ruim

    const some_unused_var = 42

    // Write-only variables are not considered as used.
    let y = 10
    y = 5

    // A read for a modification of itself is not considered as used.
    let z = 0
    z = z + 1

    // Unused function arguments.
    const getX = (x, y) => x

    // bom

    const getXPlusY = (x, y) => x + y

    const x = 1
    const y = a + 2

    alert(getXPlusY(x, y))

    // 'type' is ignored even if unused because it has a rest property sibling.
    // This is a form of extracting an object that omits the specified keys.
    const { type, ...coords } = data
    // 'coords' is now the 'data' object without its 'type' property.
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Hoisting

  - `var` declarations get hoisted to the top of their closest enclosing function scope, their assignment does not. `const` and `let` declarations are blessed with a new concept called [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone).

    ```javascript
    // we know this wouldn’t work (assuming there
    // is no notDefined global variable)
    const = example = () => {
      console.log(notDefined) // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    const example = () => {
      console.log(declaredButNotAssigned) // => undefined
      var declaredButNotAssigned = true
    }

    // the interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    const example = () => {
      let declaredButNotAssigned
      console.log(declaredButNotAssigned) // => undefined
      declaredButNotAssigned = true
    }

    // using const and let
    const example = () => {
      console.log(declaredButNotAssigned) // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned) // => throws a ReferenceError
      const declaredButNotAssigned = true
    }
    ```

  - Anonymous function expressions hoist their variable name, but not the function assignment.

    ```javascript
    const example = () => {
      console.log(anonymous) // => undefined

      anonymous() // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression')
      }
    }
    ```

  - Named function expressions hoist the variable name, not the function name or the function body.

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
    // is the same as the variable name.
    function example() {
      console.log(named) // => undefined

      named() // => TypeError named is not a function

      var named = function named() {
        console.log('named')
      }
    }
    ```

  - Function declarations hoist their name and the function body.

    ```javascript
    function example() {
      superPower() // => Flying

      function superPower() {
        console.log('Flying')
      }
    }
    
    // Remember that it doesn't happen with arrow functions
    const example = () => {
      superPower() // => ReferenceError:

      const superPower = () => {
        console.log('Flying')
      }
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Comparison Operators & Equality

  - Use `===` and `!==` over `==` and `!=`. eslint: [`eqeqeq`](https://eslint.org/docs/rules/eqeqeq.html)

  - Conditional statements such as the `if` statement evaluate their expression using coercion with the `ToBoolean` abstract method and always follow these simple rules:

    - **Objects** evaluate to **true**
    - **Undefined** evaluates to **false**
    - **Null** evaluates to **false**
    - **Booleans** evaluate to **the value of the boolean**
    - **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    - **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

  - Use shortcuts for booleans, but explicit comparisons for strings.

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

  - Use braces to create blocks in `case` and `default` clauses that contain lexical declarations (e.g. `let`, `const`, `function`, and `class`). eslint: [`no-case-declarations`](https://eslint.org/docs/rules/no-case-declarations.html)

    > Por que? Lexical declarations are visible in the entire `switch` block but only get initialized when assigned, which only happens when its `case` is reached. This causes problems when multiple `case` clauses attempt to define the same thing.

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

  - Ternaries should not be nested and generally be single line expressions. eslint: [`no-nested-ternary`](https://eslint.org/docs/rules/no-nested-ternary.html)

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

  - Avoid unneeded ternary statements. eslint: [`no-unneeded-ternary`](https://eslint.org/docs/rules/no-unneeded-ternary.html)

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

  - When mixing operators, enclose them in parentheses. The only exception is the standard arithmetic operators: `+`, `-`, and `**` since their precedence is broadly understood. We recommend enclosing `/` and `*` in parentheses because their precedence can be ambiguous when they are mixed.
  eslint: [`no-mixed-operators`](https://eslint.org/docs/rules/no-mixed-operators.html)

    > Por que? This improves readability and clarifies the developer’s intention.

    ```javascript
    // ruim
    const foo = a && b < 0 || c > 0 || d + 1 === 0

    // ruim
    const bar = a ** b - 5 % d

    // ruim
    // one may be confused into thinking (a || b) && c
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

## Blocks

  - Use braces with all multiline blocks. eslint: [`nonblock-statement-body-position`](https://eslint.org/docs/rules/nonblock-statement-body-position)

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

  - If you’re using multiline blocks with `if` and `else`, put `else` on the same line as your `if` block’s closing brace. eslint: [`brace-style`](https://eslint.org/docs/rules/brace-style.html)

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

  - If an `if` block always executes a `return` statement, the subsequent `else` block is unnecessary. A `return` in an `else if` block following an `if` block that contains a `return` can be separated into multiple `if` blocks. eslint: [`no-else-return`](https://eslint.org/docs/rules/no-else-return)

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

## Control Statements

  - In case your control statement (`if`, `while` etc.) gets too long or exceeds the maximum line length, each (grouped) condition could be put into a new line. The logical operator should begin the line.

    > Por que? Requiring operators at the beginning of the line keeps the operators aligned and follows a pattern similar to method chaining. This also improves readability by making it easier to visually follow complex logic.

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

  - Don't use selection operators in place of control statements.

    ```javascript
    // ruim
    !isRunning && startRunning()

    // bom
    if (!isRunning) {
      startRunning()
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Comments

  - Use `/** ... */` for multiline comments.

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

  - Use `//` for single line comments. Place single line comments on a newline above the subject of the comment. Put an empty line before the comment unless it’s on the first line of a block.

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

  - Start all comments with a space to make it easier to read. eslint: [`spaced-comment`](https://eslint.org/docs/rules/spaced-comment)

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

  - Prefixing your comments with `FIXME` or `TODO` helps other developers quickly understand if you’re pointing out a problem that needs to be revisited, or if you’re suggesting a solution to the problem that needs to be implemented. These are different than regular comments because they are actionable. The actions are `FIXME: -- need to figure this out` or `TODO: -- need to implement`.

  - Use `// FIXME:` to annotate problems.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super()

        // FIXME: shouldn’t use a global here
        total = 0
      }
    }
    ```

  - Use `// TODO:` to annotate solutions to problems.

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

## Whitespace

  - Use soft tabs (space character) set to 2 spaces. eslint: [`indent`](https://eslint.org/docs/rules/indent.html)

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

  - Place 1 space before the leading brace. eslint: [`space-before-blocks`](https://eslint.org/docs/rules/space-before-blocks.html)

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

  - Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](https://eslint.org/docs/rules/keyword-spacing.html)

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

  - Set off operators with spaces. eslint: [`space-infix-ops`](https://eslint.org/docs/rules/space-infix-ops.html)

    ```javascript
    // ruim
    const x=y+5

    // bom
    const x = y + 5
    ```

  - End files with a single newline character. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

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

  - Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](https://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](https://eslint.org/docs/rules/no-whitespace-before-property)

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

  - Leave a blank line after blocks and before the next statement.

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

  - Do not pad your blocks with blank lines. eslint: [`padded-blocks`](https://eslint.org/docs/rules/padded-blocks.html)

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

  - Do not use multiple blank lines to pad your code. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

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

  - Do not add spaces inside parentheses. eslint: [`space-in-parens`](https://eslint.org/docs/rules/space-in-parens.html)

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

  - Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](https://eslint.org/docs/rules/array-bracket-spacing.html)

    ```javascript
    // ruim
    const foo = [ 1, 2, 3 ]
    console.log(foo[ 0 ])

    // bom
    const foo = [1, 2, 3]
    console.log(foo[0])
    ```

  - Add spaces inside curly braces. eslint: [`object-curly-spacing`](https://eslint.org/docs/rules/object-curly-spacing.html)

    ```javascript
    // ruim
    const foo = {clark: 'kent'}

    // bom
    const foo = { clark: 'kent' }
    ```

  - Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up. eslint: [`max-len`](https://eslint.org/docs/rules/max-len.html)

    > Por que? This ensures readability and maintainability.

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

  - Require consistent spacing inside an open block token and the next token on the same line. This rule also enforces consistent spacing inside a close block token and previous token on the same line. eslint: [`block-spacing`](https://eslint.org/docs/rules/block-spacing)

    ```javascript
    // ruim
    if (foo) {bar = 0}

    // bom
    if (foo) { bar = 0 }
    ```

  - Avoid spaces before commas and require a space after commas. eslint: [`comma-spacing`](https://eslint.org/docs/rules/comma-spacing)

    ```javascript
    // ruim
    const foo = 1,bar = 2
    const arr = [1 , 2]

    // bom
    const foo = 1, bar = 2
    const arr = [1, 2]
    ```

  - Enforce spacing inside of computed property brackets. eslint: [`computed-property-spacing`](https://eslint.org/docs/rules/computed-property-spacing)

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

  - Avoid spaces between functions and their invocations. eslint: [`func-call-spacing`](https://eslint.org/docs/rules/func-call-spacing)

    ```javascript
    // ruim
    func ()

    func
    ()

    // bom
    func()
    ```

  - Enforce spacing between keys and values in object literal properties. eslint: [`key-spacing`](https://eslint.org/docs/rules/key-spacing)

    ```javascript
    // ruim
    var obj = { foo : 42 }
    var obj2 = { foo:42 }

    // bom
    var obj = { foo: 42 }
    ```

  - Avoid trailing spaces at the end of lines. eslint: [`no-trailing-spaces`](https://eslint.org/docs/rules/no-trailing-spaces)

  - Avoid multiple empty lines, only allow one newline at the end of files, and avoid a newline at the beginning of files. eslint: [`no-multiple-empty-lines`](https://eslint.org/docs/rules/no-multiple-empty-lines)

    <!-- markdownlint-disable MD012 -->
    ```javascript
    // ruim - multiple empty lines
    var x = 1


    var y = 2

    // ruim - 2+ newlines at end of file
    var x = 1
    var y = 2


    // ruim - 1+ newline(s) at beginning of file

    var x = 1
    var y = 2

    // bom
    var x = 1
    var y = 2

    ```
    <!-- markdownlint-enable MD012 -->

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Commas

  - Leading commas: **Nope.** eslint: [`comma-style`](https://eslint.org/docs/rules/comma-style.html)

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

## Semicolons

  - **Nope.** Prettier will clean this up. Here we can se

    > Por que? When JavaScript encounters a line break without a semicolon, it uses a set of rules called [Automatic Semicolon Insertion](https://tc39.github.io/ecma262/#sec-automatic-semicolon-insertion) to determine whether or not it should regard that line break as the end of a statement.

    ```javascript
    // ruim - raises exception
    const reaction = "No! That’s impossible!"
    (async function meanwhileOnTheFalcon() {
      // handle `leia`, `lando`, `chewie`, `r2`, `c3p0`
      // ...
    }())

    // ruim - returns `undefined` instead of the value on the next line - always happens when `return` is on a line by itself because of ASI!
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

## Type Casting & Coercion

  - Perform type coercion at the beginning of the statement.

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

  - Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings. eslint: [`radix`](https://eslint.org/docs/rules/radix) [`no-new-wrappers`](https://eslint.org/docs/rules/no-new-wrappers)

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

## Naming Conventions

  - Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](https://eslint.org/docs/rules/id-length)

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

  - Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](https://eslint.org/docs/rules/camelcase.html)

    ```javascript
    // ruim
    const OBJEcttsssss = {}
    const this_is_my_object = {}

    // bom
    const thisIsMyObject = {}
    const thisIsMyFunction = () => {}
    ```

  - Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](https://eslint.org/docs/rules/new-cap.html)

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

  - Do not use trailing or leading underscores. eslint: [`no-underscore-dangle`](https://eslint.org/docs/rules/no-underscore-dangle.html)

    > Por que? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won’t count as breaking, or that tests aren’t needed. tldr: if you want something to be “private”, it must not be observably present.

    ```javascript
    // ruim
    this.__firstName__ = 'Panda'
    this.firstName_ = 'Panda'
    this._firstName = 'Panda'

    // bom
    this.firstName = 'Panda'
    ```

  - Don’t save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind).

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

  - A base filename should exactly match the name of its default export.

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

  - Use camelCase when you export-default a function. Your filename should be identical to your function’s name.

    ```javascript
    function makeStyleGuide() {
      // ...
    }

    export default makeStyleGuide
    ```

  - Use PascalCase when you export a constructor / class / singleton / function library / bare object.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      },
    }

    export default AirbnbStyleGuide
    ```

  - Acronyms and initialisms should always be all uppercased, or all lowercased.

    > Por que? Names are for readability, not to appease a computer algorithm.

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

  - You may optionally uppercase a constant only if it (1) is exported, (2) is a `const` (it can not be reassigned), and (3) the programmer can trust it (and its nested properties) to never change.

    > Por que? This is an additional tool to assist in situations where the programmer would be unsure if a variable might ever change. UPPERCASE_VARIABLES are letting the programmer know that they can trust the variable (and its properties) not to change.
    - What about all `const` variables? - This is unnecessary, so uppercasing should not be used for constants within a file. It should be used for exported constants however.
    - What about exported objects? - Uppercase at the top level of export (e.g. `EXPORTED_OBJECT.key`) and maintain that all nested properties do not change.

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

  - Accessor functions for properties are not required.

  - Do not use JavaScript getters/setters as they cause unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, use `getVal()` and `setVal('hello')`.

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

  - If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

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

  - It’s okay to create `get()` and `set()` functions, but **always be consistent**.

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

## Testing

  - **Seriously, we need to do tests!**

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Translation

  This style guide is also available in other languages:

  - **Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)

**[⬆ voltar ao topo](#lista-de-conteúdos)**

# )