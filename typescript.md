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