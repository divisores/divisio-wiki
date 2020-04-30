# Divisio React/JSX Style Guide

*This is the Divisio approach to React and JSX*

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions may still be included or prohibited on a case-by-case basis.

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Function vs Class vs stateless](#function-vs-class-vs-stateless)
  1. [Mixins](#mixins)
  1. [Naming](#naming)
  1. [Imports](#imports)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [`isMounted`](#ismounted)
  1. [Styled Components](#styled-components)
  1. [Hooks](#hooks)
  1. [Context](#context)
  1. [Ordering](#Ordering)

## Basic Rules

  - Always use JSX syntax.
  - Only include one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - [`react/forbid-prop-types`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) will allow `arrays` and `objects` only if it is explicitly noted what `array` and `object` contains, using `arrayOf`, `objectOf`, or `shape`.
  - Always use [`styled-components`](https://styled-components.com/) in our components
    - However, there are cases that is ok to use [inline styling](https://www.w3schools.com/react/react_css.asp), 
but it is usually avoided

## Function vs Class vs stateless

  - If you have internal state, prefer `useState` over `class extends React.Component`.

    ```js
    // bad
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>
      }
    }

    // good
    // ...
    const Listing = () => {
      const [hello, setHello] = useState('Hello world!')

      return (
      <div>{hello}</div>
    )}
    ```

    And if you don’t have state or refs, prefer arrow functions (not normal functions) over classes:

    ```js
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>
      }
    }

    // bad
    function Listing({ hello }) {
      return <div>{hello}</div>
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    )
    ```

## Mixins

  - [Do not use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > Why? Mixins introduce implicit dependencies, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

## Naming

  - **Extensions**: Use `.js` extension for React components.
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.js`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```js
    // bad
    import reservationCard from './ReservationCard'

    // good
    import ReservationCard from './ReservationCard'

    // bad
    const ReservationItem = <ReservationCard />

    // good
    const reservationItem = <ReservationCard />
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

    ```js
    // bad
    import Footer from './Footer/Footer'

    // bad
    import Footer from './Footer/index'

    // good
    import Footer from './Footer'
    ```
## Imports

- Use an `index.js` file to use **Named exports** when you have a lot of components folders (usually cases are `/components` and `/style-guide` folders)

  ```js
  // ...
  export { default as Button } from './Button'
  export { default as Badge } from './Badge'
  export { default as Card } from './Card'
  export { default as Dropdown } from './Dropdown'
  export { default as Formula } from './Formula'
  // ...
  ```
## Declaration

  - Do not use `displayName` for naming components. Instead, name the component by reference. Use the export after the component is named.

    ```js
    // bad
    export default ReservationCard = () => {
      // stuff goes here
    }

    // good
    const ReservationCard = () => {
      // stuff goes here
    }

    export default ReservationCard
    ```

## Alignment

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```js
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
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

    // bad
    {showButton &&
      <Button />
    }

    // bad
    {
      showButton &&
        <Button />
    }

    // good
    {showButton && (
      <Button />
    )}

    // good
    {showButton && <Button />}
    ```

## Quotes

  - Always use double quotes (`"`) for JSX attributes, but single quotes (`'`) for all other JS. eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > Why? Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

    ```js
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```js
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```js
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.

    ```js
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```js
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```js
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```js
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```js
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

    > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people  using screenreaders and keyboards complicate accessibility.

    ```js
    // bad
    <div accessKey="h" />

    // good
    <div />
    ```

    - Use spread props sparingly.
    > Why? Otherwise you’re more likely to pass unnecessary props down to components. And for   React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https:// reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  Exceptions:

  - Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.

    ```js
    const Foo = () => {
      const props = {
        text: '',
        isPublished: false
      }

      return (<div {...props} />)
    }
    ```

    > **Notes**: Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www. npmjs.com/package/prop-types-exact) to help prevent bugs.

    ```js
    // bad
    const CoffeCard = ({ ...irrelevantProps }) => <WrappedComponent {...irrelevantProps} />
  
    // good
    const CoffeCard = ({ irrelevantProp, ...relevantProps }) => <WrappedComponent {...  relevantProps} />
    ```

## Refs

  - Always use `useRef()` hook.

    ```js
    // bad
    <Foo
      ref="myRef"
    />

    // good
    const myRef = useRef()
  
    <Foo
      ref={myRef}
    />
    ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```js
    // bad
    const CardCoffe = () => {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>
    }

    // good
    const CardCoffe = () => {
      const body = <div>hello</div>
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // good, when single line
    const CardCoffe = () => {
      const body = <div>hello</div>
      return <MyComponent>{body}</MyComponent>
    }
    ```

  - Prefer to remove `return` when there's nothing more on the component.

    ```js
    // bad
    const CardCoffee = () => {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // good
    const CardCoffee = () => (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    )

    // good, when single line
    const CardCoffee = () => <MyComponent>{body}</MyComponent>
    ```


## Tags

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```js
    // bad
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - If your component has multiline properties, close its tag on a new line. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```js
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.

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

  - Use arrow functions (not normal functions) to create methods.

    ```js
    // bad
    const CardCoffee = () => {
      function onClickDiv() {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }

    // good
    const CardCoffee = () => {
      const onClickDiv = () => {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) for a more in-depth discussion.

    ```jsx
    // bad
    const CardCoffee = () => {
      function _onClickSubmit() {
        // do stuff
      }

      // other stuff
    }

    // bad
    const CardCoffee = () => {
      const _onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }

    // good
    const CardCoffee = () => {
      const onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }
    ```

## `isMounted`

  - Do not use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Styled Components

- Use PascalCase for component's names

    ```js
    // bad
    const container = styled.div`
      // css goes here
    `

    // good
    const Container = styled.div`
      // css goes here
    `
    ```

- Do not use Styled prefix in component's names
  - However, there are cases that you will be importing another components, and if you can't alias the import, it's ok to use Styled prefix

  ```js
  // bad
  import { Button } from '@/style-guide'

  const StyledButton = styled(Button)`
    // css goes here
  `

  // good
  import { Button as ButtonComponent } from '@/style-guide'

  const Button = styled(ButtonComponent)`
    // css goes here
  `

  // good, when you can't alias the import
  import CalculatorStructure from '@/components/CalculatorStructure'

  const StyledCalculatorStructure = styled(CalculatorStructure)`
    // css goes here
  `
  ```
- Use [`styled-media-query`](https://www.npmjs.com/package/styled-media-query) for responsiveness purpose
  > You need `styled-components` as well

  ```js
  import media from "styled-media-query"
 
  // bad
  const Box = styled.div`
    background: black;
  
    @media (min-width: 768px) {
    /* screen width is greater than 768px (medium) */
      background: blue;
    }
  `


  // good
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

- Use [`styled-components`](https://styled-components.com/) in the same file until you still confortable with this. This is a case of *feeling*. When you think that the file has a lot of style in it, is recomended to separete to a `styles.js` file in the same folder as the following example:

  ```
    - components/
    --- Card/
    ----- index.js
    ----- styles.js
    --- Button/
    ----- index.js
    ----- styles.js
  ```
  - Inside `styles.js`, use export to each styled component to make import easier on `index.js`

  ```js
  // styles.js
  import styled from 'styled-components'

  export const Button = styled.button` 
    // css goes here
   ` 

  export const Container = styled.div`
    // css goes here
  `

  // index.js
  import { Button, Container } from './styles'

  ```
## Hooks

  - Use camelCase for naming hooks.

    >**Note**: Use `set` prefix when naming `useState()`

    ```js
      const [count, setCount] = useState(0)
    ```

  - Use Multiple Effects to Separate Concerns

    ```js
    const [count, setCount] = useState(0);
    useEffect(() => {
      // Stuffs related to count
    });# Divisio React/JSX Style Guide

*This is the Divisio approach to React and JSX*

This style guide is mostly based on the standards that are currently prevalent in JavaScript, although some conventions may still be included or prohibited on a case-by-case basis.

## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Function vs Class vs stateless](#function-vs-class-vs-stateless)
  1. [Mixins](#mixins)
  1. [Naming](#naming)
  1. [Imports](#imports)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Refs](#refs)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [`isMounted`](#ismounted)
  1. [Styled Components](#styled-components)
  1. [Hooks](#hooks)
  1. [Context](#context)
  1. [Ordering](#Ordering)

## Basic Rules

  - Always use JSX syntax.
  - Only include one React component per file.
    - However, multiple [Stateless, or Pure, Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) are allowed per file. eslint: [`react/no-multi-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-multi-comp.md#ignorestateless).
  - [`react/forbid-prop-types`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/forbid-prop-types.md) will allow `arrays` and `objects` only if it is explicitly noted what `array` and `object` contains, using `arrayOf`, `objectOf`, or `shape`.
  - Always use [`styled-components`](https://styled-components.com/) in our components
    - However, there are cases that is ok to use [inline styling](https://www.w3schools.com/react/react_css.asp), 
but it is usually avoided

## Function vs Class vs stateless

  - If you have internal state, prefer `useState` over `class extends React.Component`.

    ```js
    // bad
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>
      }
    }

    // good
    // ...
    const Listing = () => {
      const [hello, setHello] = useState('Hello world!')

      return (
      <div>{hello}</div>
    )}
    ```

    And if you don’t have state or refs, prefer arrow functions (not normal functions) over classes:

    ```js
    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>
      }
    }

    // bad
    function Listing({ hello }) {
      return <div>{hello}</div>
    }

    // good
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    )
    ```

## Mixins

  - [Do not use mixins](https://facebook.github.io/react/blog/2016/07/13/mixins-considered-harmful.html).

  > Why? Mixins introduce implicit dependencies, cause name clashes, and cause snowballing complexity. Most use cases for mixins can be accomplished in better ways via components, higher-order components, or utility modules.

## Naming

  - **Extensions**: Use `.js` extension for React components.
  - **Filename**: Use PascalCase for filenames. E.g., `ReservationCard.js`.
  - **Reference Naming**: Use PascalCase for React components and camelCase for their instances. eslint: [`react/jsx-pascal-case`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-pascal-case.md)

    ```js
    // bad
    import reservationCard from './ReservationCard'

    // good
    import ReservationCard from './ReservationCard'

    // bad
    const ReservationItem = <ReservationCard />

    // good
    const reservationItem = <ReservationCard />
    ```

  - **Component Naming**: Use the filename as the component name. For example, `ReservationCard.js` should have a reference name of `ReservationCard`. However, for root components of a directory, use `index.js` as the filename and use the directory name as the component name:

    ```js
    // bad
    import Footer from './Footer/Footer'

    // bad
    import Footer from './Footer/index'

    // good
    import Footer from './Footer'
    ```
## Imports

- Use an `index.js` file to use **Named exports** when you have a lot of components folders (usually cases are `/components` and `/style-guide` folders)

  ```js
  // ...
  export { default as Button } from './Button'
  export { default as Badge } from './Badge'
  export { default as Card } from './Card'
  export { default as Dropdown } from './Dropdown'
  export { default as Formula } from './Formula'
  // ...
  ```
## Declaration

  - Do not use `displayName` for naming components. Instead, name the component by reference. Use the export after the component is named.

    ```js
    // bad
    export default ReservationCard = () => {
      // stuff goes here
    }

    // good
    const ReservationCard = () => {
      // stuff goes here
    }

    export default ReservationCard
    ```

## Alignment

  - Follow these alignment styles for JSX syntax. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md) [`react/jsx-closing-tag-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-tag-location.md)

    ```js
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
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

    // bad
    {showButton &&
      <Button />
    }

    // bad
    {
      showButton &&
        <Button />
    }

    // good
    {showButton && (
      <Button />
    )}

    // good
    {showButton && <Button />}
    ```

## Quotes

  - Always use double quotes (`"`) for JSX attributes, but single quotes (`'`) for all other JS. eslint: [`jsx-quotes`](https://eslint.org/docs/rules/jsx-quotes)

    > Why? Regular HTML attributes also typically use double quotes instead of single, so JSX attributes mirror this convention.

    ```js
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - Always include a single space in your self-closing tag. eslint: [`no-multi-spaces`](https://eslint.org/docs/rules/no-multi-spaces), [`react/jsx-tag-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-tag-spacing.md)

    ```js
    // bad
    <Foo/>

    // very bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

  - Do not pad JSX curly braces with spaces. eslint: [`react/jsx-curly-spacing`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-curly-spacing.md)

    ```js
    // bad
    <Foo bar={ baz } />

    // good
    <Foo bar={baz} />
    ```

## Props

  - Always use camelCase for prop names.

    ```js
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - Omit the value of the prop when it is explicitly `true`. eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```js
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />

    // good
    <Foo hidden />
    ```

  - Always include an `alt` prop on `<img>` tags. If the image is presentational, `alt` can be an empty string or the `<img>` must have `role="presentation"`. eslint: [`jsx-a11y/alt-text`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/alt-text.md)

    ```js
    // bad
    <img src="hello.jpg" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />

    // good
    <img src="hello.jpg" alt="" />

    // good
    <img src="hello.jpg" role="presentation" />
    ```

  - Do not use words like "image", "photo", or "picture" in `<img>` `alt` props. eslint: [`jsx-a11y/img-redundant-alt`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/img-redundant-alt.md)

    > Why? Screenreaders already announce `img` elements as images, so there is no need to include this information in the alt text.

    ```js
    // bad
    <img src="hello.jpg" alt="Picture of me waving hello" />

    // good
    <img src="hello.jpg" alt="Me waving hello" />
    ```

  - Use only valid, non-abstract [ARIA roles](https://www.w3.org/TR/wai-aria/#usage_intro). eslint: [`jsx-a11y/aria-role`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/aria-role.md)

    ```js
    // bad - not an ARIA role
    <div role="datepicker" />

    // bad - abstract ARIA role
    <div role="range" />

    // good
    <div role="button" />
    ```

  - Do not use `accessKey` on elements. eslint: [`jsx-a11y/no-access-key`](https://github.com/evcohen/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-access-key.md)

    > Why? Inconsistencies between keyboard shortcuts and keyboard commands used by people  using screenreaders and keyboards complicate accessibility.

    ```js
    // bad
    <div accessKey="h" />

    // good
    <div />
    ```

    - Use spread props sparingly.
    > Why? Otherwise you’re more likely to pass unnecessary props down to components. And for   React v15.6.1 and older, you could [pass invalid HTML attributes to the DOM](https:// reactjs.org/blog/2017/09/08/dom-attributes-in-react-16.html).

  Exceptions:

  - Spreading objects with known, explicit props. This can be particularly useful when testing React components with Mocha’s beforeEach construct.

    ```js
    const Foo = () => {
      const props = {
        text: '',
        isPublished: false
      }

      return (<div {...props} />)
    }
    ```

    > **Notes**: Filter out unnecessary props when possible. Also, use [prop-types-exact](https://www. npmjs.com/package/prop-types-exact) to help prevent bugs.

    ```js
    // bad
    const CoffeCard = ({ ...irrelevantProps }) => <WrappedComponent {...irrelevantProps} />
  
    // good
    const CoffeCard = ({ irrelevantProp, ...relevantProps }) => <WrappedComponent {...  relevantProps} />
    ```

## Refs

  - Always use `useRef()` hook.

    ```js
    // bad
    <Foo
      ref="myRef"
    />

    // good
    const myRef = useRef()
  
    <Foo
      ref={myRef}
    />
    ```

## Parentheses

  - Wrap JSX tags in parentheses when they span more than one line. eslint: [`react/jsx-wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-wrap-multilines.md)

    ```js
    // bad
    const CardCoffe = () => {
      return <MyComponent variant="long body" foo="bar">
               <MyChild />
             </MyComponent>
    }

    // good
    const CardCoffe = () => {
      const body = <div>hello</div>
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // good, when single line
    const CardCoffe = () => {
      const body = <div>hello</div>
      return <MyComponent>{body}</MyComponent>
    }
    ```

  - Prefer to remove `return` when there's nothing more on the component.

    ```js
    // bad
    const CardCoffee = () => {
      return (
        <MyComponent variant="long body" foo="bar">
          <MyChild />
        </MyComponent>
      )
    }

    // good
    const CardCoffee = () => (
      <MyComponent variant="long body" foo="bar">
        <MyChild />
      </MyComponent>
    )

    // good, when single line
    const CardCoffee = () => <MyComponent>{body}</MyComponent>
    ```


## Tags

  - Always self-close tags that have no children. eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```js
    // bad
    <Foo variant="stuff"></Foo>

    // good
    <Foo variant="stuff" />
    ```

  - If your component has multiline properties, close its tag on a new line. eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```js
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - Use arrow functions to close over local variables. It is handy when you need to pass additional data to an event handler. Although, make sure they [do not massively hurt performance](https://www.bignerdranch.com/blog/choosing-the-best-approach-for-react-event-handlers/), in particular when passed to custom components that might be PureComponents, because they will trigger a possibly needless rerender every time.

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

  - Use arrow functions (not normal functions) to create methods.

    ```js
    // bad
    const CardCoffee = () => {
      function onClickDiv() {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }

    // good
    const CardCoffee = () => {
      const onClickDiv = () => {
        // do stuff
      }

      return <div onClick={onClickDiv} />
    }
    ```

  - Do not use underscore prefix for internal methods of a React component.
    > Why? Underscore prefixes are sometimes used as a convention in other languages to denote privacy. But, unlike those languages, there is no native support for privacy in JavaScript, everything is public. Regardless of your intentions, adding underscore prefixes to your properties does not actually make them private, and any property (underscore-prefixed or not) should be treated as being public. See issues [#1024](https://github.com/airbnb/javascript/issues/1024), and [#490](https://github.com/airbnb/javascript/issues/490) for a more in-depth discussion.

    ```jsx
    // bad
    const CardCoffee = () => {
      function _onClickSubmit() {
        // do stuff
      }

      // other stuff
    }

    // bad
    const CardCoffee = () => {
      const _onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }

    // good
    const CardCoffee = () => {
      const onClickSubmit = () => {
        // do stuff
      }

      // other stuff
    }
    ```

## `isMounted`

  - Do not use `isMounted`. eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > Why? [`isMounted` is an anti-pattern][anti-pattern], is not available when using ES6 classes, and is on its way to being officially deprecated.

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

## Styled Components

- Use PascalCase for component's names

    ```js
    // bad
    const container = styled.div`
      // css goes here
    `

    // good
    const Container = styled.div`
      // css goes here
    `
    ```

- Do not use Styled prefix in component's names
  - However, there are cases that you will be importing another components, and if you can't alias the import, it's ok to use Styled prefix

  ```js
  // bad
  import { Button } from '@/style-guide'

  const StyledButton = styled(Button)`
    // css goes here
  `

  // good
  import { Button as ButtonComponent } from '@/style-guide'

  const Button = styled(ButtonComponent)`
    // css goes here
  `

  // good, when you can't alias the import
  import CalculatorStructure from '@/components/CalculatorStructure'

  const StyledCalculatorStructure = styled(CalculatorStructure)`
    // css goes here
  `
  ```
- Use [`styled-media-query`](https://www.npmjs.com/package/styled-media-query) for responsiveness purpose
  > You need `styled-components` as well

  ```js
  import media from "styled-media-query"
 
  // bad
  const Box = styled.div`
    background: black;
  
    @media (min-width: 768px) {
    /* screen width is greater than 768px (medium) */
      background: blue;
    }
  `


  // good
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

- Use [`styled-components`](https://styled-components.com/) in the same file until you still confortable with this. This is a case of *feeling*. When you think that the file has a lot of style in it, is recomended to separete to a `styles.js` file in the same folder as the following example:

  ```
    - components/
    --- Card/
    ----- index.js
    ----- styles.js
    --- Button/
    ----- index.js
    ----- styles.js
  ```
  - Inside `styles.js`, use export to each styled component to make import easier on `index.js`

  ```js
  // styles.js
  import styled from 'styled-components'

  export const Button = styled.button` 
    // css goes here
   ` 

  export const Container = styled.div`
    // css goes here
  `

  // index.js
  import { Button, Container } from './styles'

  ```
## Hooks

  - Use camelCase for naming hooks.

    >**Note**: Use `set` prefix when naming `useState()`

    ```js
      const [count, setCount] = useState(0)
    ```

  - Use Multiple Effects to separate concerns

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

  - Use **Skipping Effects** to optimize performance if you can.

    ```js
      useEffect(() => {
        document.title = `You clicked ${count} times`;
      }, [count]); // Only re-run the effect if count changes
    ```

  - When using `useEffect()`, always try to maintain the lifecycle by ordering as before
    1. `componentWillMount`
    1. `componentDidMount`
    1. `componentWillReceiveProps`
    1. `shouldComponentUpdate`
    1. `componentWillUpdate`
    1. `componentDidUpdate`
    1. `componentWillUnmount`

  - Keep your hooks simple. This is another *feeling* case. If you feel that the code   abstraction will make it more complicated, leave it as it is. But if you notice that an   abstraction/optimization would help to simplify the code, go on then. Here is an example for  how to do that.

    >**Note**: Use `/hooks` folder to keep your abstracted codes, and `useWhatever` naming convention

    ```js
      // Before abstraction 
     // Remeber, DRY code can be code to keep it simple
     const MyComponent = () => {
       useHooks(...)
       useHooks(...)
       useHooks(...)

       return <div>test</div>
     }

     // After abstraction
     import { useCustomHooks } from '@/hooks'
     const MyComponent = () => {
       useCustomHooks()

       return <div>test</div>
     }
    ```

## Context

- Useful tips:
  - Do not reaching for context to solve every state sharing problem 
  - Context does not have to be global to the woole app, but it can be if necessary
  - Use multiple logically separated contexts in your app 

- Use `/context` folder to keep them logically separated

  ```
    - src/context/
    --- SomeContext.js
    --- AnotherContext.js
  ```

- For de next examples, lets use this `SomeContext.js` file

    ```js
      // file SomeContext.js
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
  - Do not export `CountContext`. This approach expose only one way to provide the context value and only one way to consume it

- Use `useContext()` inside `some-context-package` to be capable of throw a helpful error message

  >Why? This approach has the benefit of you being able to fail faster!

    ```js
      // bad
      import {useCountState} from 'some-context-package'

      const MyComponent = () => {
        const something = useContext(useCountState)

        // ...
      }

      // good
      import {useCountState} from 'some-context-package'

      const MyComponent = () => {
        const something = useCountState()

        // ...
      }
    ```

- When using `dispatch` and `state` in context, try to keep it together

    ```js
      // bad
      const state = useCountState()
      const dispatch = useCountDispatch()

      // good
      const [state, dispatch] = useCount()
    ```


## Ordering

- Ordering a basic semantic structure using [`styled-components`](https://styled-components.com/)

    > This can be used as a guide for general cases, but that's not a rule.

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

- Ordering a basic component using [`styled-components`](https://styled-components.com/)

1. `imports`
1. `styled-components`
1. `functional component`
1. `export default`


**[Back to top](#table-of-contents)**
