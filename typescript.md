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