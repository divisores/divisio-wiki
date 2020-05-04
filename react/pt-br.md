# Divisio React/JSX Style Guide = () => (

*Essa é a abordagem da Divisio ao trabalhar com React e a sintaxe JSX*

Este guia é predominantemente baseado em outros padrões comuns que mais prevaleceram em Javascript até então, embora algumas convenções ainda podem ser adicionada e removidas para cada caso do dia a dia.

## Lista de conteúdos

  1. [Regras básicas](#regras-básicas)
  1. [Function vs Class vs stateless](#function-vs-class-vs-stateless)
  1. [Mixins](#mixins)
  1. [Nomeação](#nomeação)
  1. [Imports](#imports)
  1. [Declaração](#declaração)
  1. [Alinhamento](#alinhamento)
  1. [Quotes](#quotes)
  1. [Espaçamento](#espaçamento)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parênteses](#parênteses)
  1. [Tags](#tags)
  1. [Métodos](#métodos)
  1. [`isMounted`](#ismounted)
  1. [Styled Components](#styled-components)
  1. [Hooks](#hooks)
  1. [Context](#context)
  1. [Ordenação](#ordenação)
  1. [Tradução](#tradução)

## Regras básicas

  - Sempre use a sintaxe JSX
  - Não use `;` no fim de cada linha.
  - Sempre inclua apenas um componente React por arquivo
    - Embora múltiplos [Stateless, or Pure Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) por arquivo são permitidos. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - [`react/forbid-prop-types`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) irá permitir `arrays` e `objects` apenas se estiver mostrado explícitamente o que cada `array` e `object` contém usando `arrayOf`, `objectOf`, ou `shape`.
  - Sempre use [`styled-components`](https://styled-components.com/) nos componentes
      - Embora haverão casos que está tudo bem usar [inline styling](https://www.w3schools.com/react/react_css.asp), mas geralmente é evitado.
  - Sempre nomeie **tudo** em inglês como forma de padronização

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Function vs Class vs stateless

  - Se existir um state interno, prefira usar `useState` ao invés de `class extends React.Component` com `state`.

    ```js
    // ruim
    class Listing extends React.Component {
      state = {
        hello: 'Hi'
      }

      render() {
        return <div>{this.state.hello}</div>
      }
    }

    // bom
    // ...
    const Listing = () => {
      const [hello, setHello] = useState('Hello world!')

      return <div>{hello}</div>
    }
    ```

  - E se não tiver `state` ou `refs` prefira usar `arrow functions` (não normal functions) ao invés de classes de qualquer forma:
   >**Nota**: Mesmo caso para quando tenha `props` ou não.

    ```js
    // ruim
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>
      }
    }

    // ruim
    function Listing({ hello }) {
      return <div>{hello}</div>
    }

    // bom
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    )
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Mixins

  - [Não use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).
  > Por que? Mixins introduz dependências implícitas, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Nomeação

  - **Extensões**: Use a extensão `.js` para componentes React.
  - **Nome de arquivo**: Use PascalCase para nomes de arquivos. E.g., `ReservationCard.js`.
  - **Referência de nomeação**: Use PascalCase para componentes React e camelCase para suas instâncias. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```js
    // ruim
    import reservationCard from './ReservationCard'

    // bom
    import ReservationCard from './ReservationCard'

    // ruim
    const ReservationItem = <ReservationCard />

    // bom
    const reservationItem = <ReservationCard />
    ```

  - **Component Naming**: Use o mesmo nome do componente para o nome do arquivo. Por exemplo, `ReservationCard.js` deveria referenciar o nome de `ReservationCard`. No entanto, para os componentes root do diretório, use `index.js` como nome do arquivo e e use o nome do componente na pasta de diretório:

    ```js
    // ruim
    import Footer from './Footer/Footer'

    // ruim
    import Footer from './Footer/index'

    // bom
    import Footer from './Footer'
    ```
**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Imports

- Use um arquivo `index.js` para usar **Named exports** quando tiver vários componentes dentro de uma pasta de diretório (os casos mais comuns serão a `/components` e `/style-guide` folders)

  ```js
  // ...
  export { default as Button } from './Button'
  export { default as Badge } from './Badge'
  export { default as Card } from './Card'
  export { default as Dropdown } from './Dropdown'
  export { default as Formula } from './Formula'
  // ...
  ```
**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Declaração

  - Não use `displayName` ao nomear componentes, e sim nomeie os componentes por referência. Exporte o componente depois que ela tenha sido nomeado

    ```js
    // ruim
    export default ReservationCard = () => {
      // stuff goes here
    }

    // bom
    const ReservationCard = () => {
      // stuff goes here
    }

    export default ReservationCard
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Alinhamento

  - Siga estes alinhamentos a seguir para a sintaxe JSX. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```js
    // ruim
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // bom
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // if props fit in one line then keep it on the same line
    <Foo bar="bar" />

    // children get indented normally
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux />
    </Foo>

    // ruim
    {showButton &&
      <Button />
    }

    // ruim
    {
      showButton &&
        <Button />
    }

    // bom
    {showButton && (
      <Button />
    )}

    // bom
    {showButton && <Button />}
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Quotes

  - Sempre use aspas duplas (`"`) para atributos JSX, e aspas simples (`'`) para todo o resto em JS. eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > Por que? Atributos regulares de HTML usam aspas duplas ao invés de simples, então os atributos JSX espelharam essa convenção.

    ```js
    // ruim
    <Foo bar='bar' />

    // bom
    <Foo bar="bar" />

    // ruim
    <Foo style={{ left: "20px" }} />

    // bom
    <Foo style={{ left: '20px' }} />
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Espaçamento

  - Sempre inclua um único espaço antes de usar a tag de `self-closing`. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```js
    // ruim
    <Foo/>

    // muito ruim
    <Foo                 />

    // ruim
    <Foo
     />

    // bom
    <Foo />
    ```

  - Não incluam espaços dentro das chaves nos atributos JSX. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```js
    // ruim
    <Foo bar={ baz } />

    // bom
    <Foo bar={baz} />
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Props

  - Sempre use camelCase para nomes de props.

    ```js
    // ruim
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // bom
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omita o valor da prop quanto for explicitamente `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```js
    // ruim
    <Foo
      hidden={true}
    />

    // bom
    <Foo
      hidden
    />

    // bom
    <Foo hidden />
    ```

  - Sempre inclua a prop `alt` nas tags `<img>`. Se for uma imagem de apresentação, `alt` poderá ficar vazio ou a tag `<img>` deverá ter a prop `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```js
    // ruim
    <img src="hello.jpg" />

    // bom
    <img src="hello.jpg" alt="Me waving hello" />

    // bom
    <img src="hello.jpg" alt="" />

    // bom
    <img src="hello.jpg" role="presentation" />
    ```

  - Não use palavras como "image", "photo", ou "picture" na prop `<img>` `alt`. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Por que? Leitores de tela já anunciam os elementos `img` como imagem, então não há razão para incluir essa informação no texto do `alt`

    ```js
    // ruim
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // bom
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use apenas [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro) válidas e não abstratas. eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```js
    // ruim - not an ARIA role
    <div role="datepicker" />

    // ruim - abstract ARIA role
    <div role="range" />

    // bom
    <div role="button" />
    ```

  - Não use `accessKey` nos elementos. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

    > Por que? A acessibilidade passa a ter complicações com os atalhos de comandos de teclado usados por pessoas que usam leitores de telas.

    ```js
    // ruim
    <div accessKey="h" />

    // bom
    <div />
    ```

    - Use o spread props com moderação.
    > Por que? Senão você provavelmente passará props desnecessárias para componentes abaixo. E para React v15.6.1 ou superior, você poderia [passar atributo HTML inválido para a DOM](https:// reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  Exceções:

  - Use o spreaing com props conhecidas e explícitas. Isso pode ser particularmente util ao fazer testes em componentes React com Mocha's antes de cada construct.

    ```js
    const Foo = () => {
      const props = {
        text: '',
        isPublished: false
      }

      return (<div {...props} />)
    }
    ```

    > **Nota**: Filtre props desnecessárias sempre que possível. Além disso, use [prop-types-exact](https://www. npmjs.com/package/prop-types-exact) para ajudar a previnir bugs.

    ```js
    // ruim
    const CoffeCard = ({ ...irrelevantProps }) => <WrappedComponent {...irrelevantProps} />
  
    // bom
    const CoffeCard = ({ irrelevantProp, ...relevantProps }) => <WrappedComponent {...  relevantProps} />
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Refs

  - Sempre use o hook `useRef()`.

    ```js
    // ruim
    <Foo
      ref="myRef"
    />

    // bom
    const myRef = useRef()
  
    <Foo
      ref={myRef}
    />
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Parênteses

  - Use parênteses em torno das tags JSX sempre que elas forem mais que uma linha. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```js
    // ruim
    const CardCoffe = () => {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>
    }

    // bom
    const CardCoffe = () => {
      const body = <div>hello</div>
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // bom, when single line
    const CardCoffe = () => {
      const body = <div>hello</div>
      return <MyComponent>{body}</MyComponent>
    }
    ```

  - Prefira remover o `return` quando não houver mais nada no componente.

    ```js
    // ruim
    const CardCoffee = () => {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // bom
    const CardCoffee = () => (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    )

    // bom, when single line
    const CardCoffee = () => <MyComponent>{body}</MyComponent>
    ```


**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Tags

  - Sempre use o self-close em tags que não tem children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```js
    // ruim
    <Foo variant="stuff"></Foo>

    // bom
    <Foo variant="stuff" />
    ```

  - Se o seu componente tiver múltiplas props, fecha a tag em uma nova linha, e com uma prop por linha se o componente passar do número total de linhas. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```js
    // ruim
    <Foo
      bar="bar"
      baz="baz" />

    // bom
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Métodos

  - Use arrow functions quando precisar de variáveis locais. Isso bem útil para passar informação adicional em um event handler. No entando, tenha certeza de que isso não [atrapalhe muito a perfomance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), em particular, quando passados para custom components que podem ser PureComponents, porque dessa forma ele podem engatilhar uma possível renderização desnecessária todas as vezes.

    ```js
    const ItemList = (props) => (
      <ul>
        {props.items.map((item, index) => (
          <Item
            key={item.key}
            onClick={event => doSomethingWith(event, item.name, index)}
          />
        ))}
      </ul>
    )
    ```

  - Use arrow functions (não functions normais) para criar métodos.

    ```js
    // ruim
    const CardCoffee = () => {
      function onClickDiv() {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }

    // bom
    const CardCoffee = () => {
      const onClickDiv = () => {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }
    ```

  - Não use o prefixo underscore para métodos internos nos componentes React.
    > Por que? O prefixo underscore as vezes são usados como uma convenção em outras  linguagenspara denotar privacidade. Mas, diferente dessas linguagens, não há suporte nativo  paraprivacidade em Javascript, onde tudo é público. Independentemente de suas intenções, adicionar os prefixos underscore não fará que os métodos fiquem privados, e  qualquerpropriedade (com o prefixo ou não) devem ser tratadas como se fossem públicas. Veja  maissobre em [#1024](https://github.com/airbnb/javascript/issues/1024), e [#490](https://  githubcom/airbnb/javascript/issues/490) para uma discussão mais aprofundada.

    ```jsx
    // ruim
    const CardCoffee = () => {
      function _onClickSubmit() {
        // do stuff
      }

      // other stuff
    }

    // ruim
    const CardCoffee = () => {
      const _onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }

    // bom
    const CardCoffee = () => {
      const onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## `isMounted`

  - Não use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Por que? [`isMounted` é um anti-padrão][anti-pattern], que não está disponível quando estiver usando classes ES6, e está no caminho de ser oficialmente depreciado

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Styled Components

- Use PascalCase para nomear os componentes

    ```js
    // ruim
    const container = styled.div`
      // css goes here
    `

    // bom
    const Container = styled.div`
      // css goes here
    `
    ```

  - Não use o prefixo Styled nos nomes dos componentes
    - Embora haverão casos que você estará importando componentes de outro lugar, e se não for possível usar o `alias import`, estará tudo bem usar o prefixo Styled.

  ```js
  // ruim
  import { Button } from '@/style-guide'

  const StyledButton = styled(Button)`
    // css goes here
  `

  // bom
  import { Button as ButtonComponent } from '@/style-guide'

  const Button = styled(ButtonComponent)`
    // css goes here
  `

  // bom, quando não puder usar o alias import
  import CalculatorStructure from '@/components/CalculatorStructure'

  const StyledCalculatorStructure = styled(CalculatorStructure)`
    // css goes here
  `
  ```

- Use o [`styled-media-query`](https://www.npmjs.com/package/styled-media-query) quando se tratar de reponsividade
  > Você precisará do `styled-components` da mesma forma

  ```js
  import media from "styled-media-query"
 
  // ruim
  const Box = styled.div`
    background: black;
  
    @media (min-width: 768px) {
    /* screen width is greater than 768px (medium) */
      background: blue;
    }
  `


  // bom
  import styled from "styled-components"
  import media from "styled-media-query"
 
  const Box = styled.div`
    background: black;
  
    ${media.greaterThan("medium")`
      /* screen width is greater than 768px (medium) */
      background: blue;
    `}
  `
  ```

  - Use [`styled-components`](https://styled-components.com/) no mesmo arquivo do componente React enquanto você estiver confortável com isso. Isso é um caso de *sensação* mesmo. Se você achar que o componente já tem muitas linhas lidando apenas com o estilo dele, é recomendado separar em um arquivo `styles.js` no mesmo diretório como o exemplo a seguir:

  ```
    - components/
    --- Card/
    ----- index.js
    ----- styles.js
    --- Button/
    ----- index.js
    ----- styles.js
  ```

  - Dentro do arquivo `styles.js`, use o export para cada style component para que facilite a importação em `index.js`

  ```js
  // styles.js
  import styled from 'styled-components'

  export const Button = styled.button` 
    // css por aqui
   ` 

  export const Container = styled.div`
    // css por aqui
  `

  // index.js
  import { Button, Container } from './styles'

  ```
**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Hooks

  - Use camelCase ao nomear hooks.

    >**Nota**: Use o prefixo `set` quando nomear o `useState()`

    ```js
      const [count, setCount] = useState(0)
    ```

  - Utilize múltiplos effects para separar preocupações e contextos.

    ```js
    const [count, setCount] = useState(0);
    useEffect(() => {
      // Stuffs related to count
    });

    const [isOnline, setIsOnline] = useState(null);
    useEffect(() => {
      // Another stuffs related to isOnline
    });
    ```

  - Use **Skipping Effects** para otimizar a performance sempre que puder.

    ```js
      useEffect(() => {
        document.title = `You clicked ${count} times`;
      }, [count]); // Only re-run the effect if count changes
    ```

  - Quando estiver usando `useEffect()`, tente sempre manter a ordem de lifecycle que era usado na versão anterior.
    1. `componentWillMount`
    1. `componentDidMount`
    1. `componentWillReceiveProps`
    1. `shouldComponentUpdate`
    1. `componentWillUpdate`
    1. `componentDidUpdate`
    1. `componentWillUnmount`

  - Mantenha seus hooks simples. Esse é um outro caso de sensação. Se sentir que a abstração do código o deixará mais difícil de ser entendido, deixe como está. Mas se notar que a abstração/otimização do código ajudará a simplificar seu entendimento, continue com a refatoração. Segue um exemplo de como fazer isso:

    >**Nota**: Use o diretório `/hooks` para manter e organizar suas abstrações de hooks, e use a convenção de nomeação `useWhatever`

    ```js
      // Antes da abstração
     // Lembre-se, DRY code pode ser mantido contato que mantenha simples
     const MyComponent = () => {
       useHooks(...)
       useHooks(...)
       useHooks(...)

       return <div>test</div>
     }

     // Depois da abstração
     import { useCustomHooks } from '@/hooks'
     const MyComponent = () => {
       useCustomHooks()

       return <div>test</div>
     }
    ```

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Context

- Dicas úteis:
  - Não use context para resolver todos os problemas de compartilhamento de estado 
  - Context não precisa ser global em todo o app, embora possa ser se necessário
  - Mantenha as lógicas de cada context separadas no seu app

- Use o diretório `/context` para manter cada lógica de context separadas.

  ```
    - src/context/
    --- SomeContext.js
    --- AnotherContext.js
  ```

- Usaremos o arquivo `SomeContext.js` para os próximos exemplos 

    ```js
      // arquivo SomeContext.js
      // ...

      const CountStateContext = createContext()
      const CountDispatchContext = createContext()
      
      const CountProvider = ({ ... }) => {
        // ...

        return (
          <CountStateContext.Provider value={state}>
            <CountDispatchContext.Provider value={dispatch}>
              {children}
            </CountDispatchContext.Provider>
          </CountStateContext.Provider>
        )
      }

      const useCountState = () => {
        const context = useContext(CountStateContext)

        if (context === undefined) {
          throw new Error('useCountState must be used within a CountProvider')
        }

        return context
      }

      const useCountDispatch = () => {
        const context = useContext(CountStateContext)

        if (context === undefined) {
          throw new Error('useCountDispatch must be used within a CountProvider')
        }

        return context
      }

      const useCount = () => {
        return [useCountState(), useCountDispatch()]
      }

      export {CountProvider, useCountState, useCountDispatch, useCount}
    ```
  - Não exporte o `CountContext`. Essa abordagem expoem apenas uma forma de prover o valor do context e apenas umas forma de consumí-lo.

- Use o `useContext()` dentro de `some-context-package` para ser capaz de enviar uma mensagem que ajude a entender o que houve.

  >Por que? Essa abordagem traz o benefício de poder errar rápido!

    ```js
      // ruim
      import {useCountState} from 'some-context-package'

      const MyComponent = () => {
        const something = useContext(useCountState)

        // ...
      }

      // bom
      import {useCountState} from 'some-context-package'

      const MyComponent = () => {
        const something = useCountState()

        // ...
      }
    ```

- Quando usar `dispatch` e `state` dentro de um context, tente mantê-los juntos.

    ```js
      // ruim
      const state = useCountState()
      const dispatch = useCountDispatch()

      // bom
      const [state, dispatch] = useCount()
    ```


**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Ordenação

- Ordenação de uma semântica básica que esteja usando [`styled-components`](https://styled-components.com/)

    > Isso pode ser usado como um guia para um caso geral, mas não necessariamente é uma regra.

  ```html
  <Layout>
    <Container>
      <Header />
      <Content>
        <Section />
        <Aside />
      </Content>
      <Footer />
    </Container>
  </Layout>
  ```

- Ordenando um componente React básico que esteja usando [`styled-components`](https://styled-components.com/)

1. `imports`
1. `styled-components`
1. `functional component`
1. `export default`

**[⬆ voltar ao topo](#lista-de-conteúdos)**

## Tradução

  Este style guide de JSX/React também está disponível em outra linguagem:

  - **English**: [/README.md](https://github.com/divisioinc/divisio-styleguide/blob/master/react/README.md)


**[Voltar ao topo](#lista-de-conteúdos)**

# )
