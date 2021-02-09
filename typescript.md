# Typescript

## In React

render as prop - use `React.ElementType`

```tsx
interface IExampleProps {
  as: React.ElementType;
}

const Example = ({as: Element = 'button'}) => {
  return (
    <Element></Element>
  )
}
```

Dynamic `as` prop

```tsx
/**
 *  Type for `as` prop that accepts any HTML element as string or any React Function Component
 *
 *  Accepts optional interface for specifying which props the Function Component must accept
 */
export type AnyTag<Interface = any> = keyof JSX.IntrinsicElements
  | React.FunctionComponent<Interface>
  | React.ForwardRefExoticComponent<Interface>
  | (new (props: Interface) => React.Component);

/**
 * Type for component that accepts dynamic `as` prop to unpack the props accepted by the injected component
 */
export type PropsOf<Tag> =
  Tag extends keyof JSX.IntrinsicElements ? JSX.IntrinsicElements[Tag] :
    Tag extends React.ComponentType<infer Props> ? Props & JSX.IntrinsicAttributes :
      Tag extends React.ForwardRefExoticComponent<infer Props> ? Props & JSX.IntrinsicAttributes :
        never

/**
 * Type used in components with forwardRef and `as` dynamic prop to use the HTML element of `as` prop
 * @todo add support for extracting HTML element of React component that use forwardRef as well
 */
export type ElementOf<Tag> =
  Tag extends keyof HTMLElementTagNameMap ? HTMLElementTagNameMap[Tag] :
    Tag extends React.ForwardRefExoticComponent<any> ? HTMLElement :
      never;

/**
 * Type for component props that accept it's props, spacing props, ref and `as` prop to inject other component in render
 *
 * **NOTE**: The props accepted by the dynamic `as` component can override internal props of UI component
 * @see {@link https://stackoverflow.com/questions/54049871/how-do-i-type-this-as-jsx-attribute-in-typescript}
 */
export type UIComponentInjectableProps<Props, Tag extends AnyTag> = Readonly<Props> & SpacingPropsType & PropsOf<Tag>;
export type UIComponentInjectable<Props, Tag extends AnyTag> = React.FC<UIComponentInjectableProps<Props, Tag>>;

```

## with eslint

### setup
install plugin `@typescript-eslint`

```bash
npm install --save-dev @typescript-eslint/eslint-plugin @typescript-eslint/parser
```

set parser to `@typescript-eslint/parser`

### notes

some eslint rules can be problematic:
- `no-undef` - disable altogether as TypeScript has its own chek for that
- `no-unused-vars` - use `@typescript-eslint/no-unused-vars` instead
- `no-use-before-define` - use `@typescript-eslint/no-use-before-define` instead
-

## enums

zbiór stałych wartości, domyślnie numerowany od 0

żeby używać jako typ union wszystkich wartości:

```ts
interface SomeInterface {
  type: keyof typeof MyTypes
}
```

## generics
[Deep Dive Docs](https://basarat.gitbook.io/typescript/type-system/generics)
### Generics and arrow functions

[Source](https://stackoverflow.com/questions/32308370/what-is-the-syntax-for-typescript-arrow-functions-with-generics)
```ts
const foo = <T extends unknown>(x: T) => x;
// or
const foo = <T, >(x: T) => x;
```

### Preserve generic types in higher order functions
[Source](https://stackoverflow.com/questions/51884498/using-react-forwardref-with-typescript-generic-jsx-arguments)

```tsx
const SelectWithRef = forwardRef(<Option extends string>(props: Props<Option>, ref?: Ref<HTMLSelectElement>) =>
  <Select<Option> {...props} forwardedRef={ref} />);
```

## Conditional types

[Source](https://artsy.github.io/blog/2018/11/21/conditional-types-in-typescript/)

```ts
<A extends B>
```

means:
`A` is a superset of `B`
`A` has all of `B` properties (and maybe some more)
`A` is possibly more specific version of `B`

A musi zawierać te same propertisy co B żeby przeszedł typecheck.

```ts
A extends { meow(): void } ? A : never
```

przejdzie tylko jak object który przekażemy będzie miał funkcję `meow`, z pasującym typem - jak nie to `never`, czyli nie może przejść

można pomyśleś że conditional type działa jak funkcja JSowa:
```ts
type isNumber<T> = T extends number ? 'number' : 'other';
```

```js
const isNumber = x => {
  typeof x === 'number' ? 'number' : 'other'
}
```

[Source](https://youtu.be/SbVgPQDealg)

### infer

infer pozwala na użycie generycznego typu w conditional types i wyciągnięcie go

```tsx
type PropsOf<T> = T extends React.ComponentType<infer Props> ? Props : never
```

infer używa nazwy podanego typu
np:

```ts
type Unpack<A> = A extends Array<infer E> ? E : A

type Test = Unpack<Apple[]>
// => Apple
type Test = Unpack<Apple>
// => Apple
```
tutaj zwróciło Apple nawet jak podaliśmy array Apple

[Source](https://youtu.be/ijK-1R-LFII)

## Top and bottom types

- Using `any` is like saying "I have no idea what this value looks like. So, TypeScript, please assume I'm using it correctly, and **don't complain** if anything I do seems dangerous".
- Using `unknown` is like saying "I have no idea what this value looks like. So, TypeScript, please make sure I check what it is capable of at **run time**."
- `never` is the bottom type (it literally means it can never happen)

## Mapped type

[Docs link](https://www.typescriptlang.org/docs/handbook/advanced-types.html#mapped-types)

Creates new interface from union of keys or another interface with `keyof` (keyof turns interafece into union)

```ts
type Partial<T> = {
  [P in keyof T]?: T[P];
};

type Readonly<T> = {
  readonly [P in keyof T]: T[P];
};
```

### Predefined types

[Docs link](https://www.typescriptlang.org/docs/handbook/utility-types.html)

Typescript posiada gotowe mapped types do częstych przypadków

- `Partial<Type>` - wszystkie parametry stają się opcjonalne
- `Required<Type>` - odwrotnie do Partial, wszystko jest wymagane
- `Readonly<Type>` - dodaje do każdego parametru readonly
- `Record<Keys, Type>` - tworzy typ obiektu mappując keys na możliwe wartości podane w `Types

```ts
interface PageInfo {
  title: string;
}

type Page = "home" | "about" | "contact";

const nav: Record<Page, PageInfo> = {
  about: { title: "about" },
  contact: { title: "contact" },
  home: { title: "home" },
};
```

- `Pick<Type, Keys>` - wybiera wybrane keys z podanego typu

```ts
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}

type TodoPreview = Pick<Todo, "title" | "completed">;

const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

- `Omit<Type, Keys>` - działa odwrotnie do Pick, wybiera wszystkie keys z typu poza podanymi

> Funfact: Typescript pod spodem używa tak naprawdę conditional types + infer
> np.

```ts
// Obtain the parameters of a function type in a tuple
type Parameters<T> =
  T extends (...args: infer P) => any ? P : never
```


 ## Adding members to a type

Use an intersection - `&`

```ts
type SomeType = {
  [P in keyof T]?: T[P];
} & { key: value }
```

## Modifing global types

[Source](https://fettblog.eu/typescript-augmenting-global-lib-dom/)
Sometimes TypeScript DOM types are broken (either spec is not up to date or other stuff)

To fix types:
1. Add global declaration in your file
*or*
2. Add declaration for whole project, into folder that is defined in `typeRoots`


```ts
declare global { // opening up the namespace
  var ResizeObserver: { // mergin ResizeObserver to it
    prototype: ResizeObserver;
    new(callback: ResizeObserverCallback): ResizeObserver;
  }
}
```

if adding for whole project make shure that your type definitions file is included inside `typeRoots`:

```json
{
  "compilerOptions": {
    //...
    "typeRoots": ["@types", "./node_modules/@types"],
    //...
  },
  "include": ["src", "@types"]
}
```