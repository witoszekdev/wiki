# HTML

## Quotes

[Semantically correct quotes](https://css-tricks.com/quoting-in-html-quotations-citations-and-blockquotes/)

Long quote - `<blockquote>`
Short quote - `<q>`

For quote with author:

> figure > blockquote + figcaption > cite (work name)

## FAQ

We don't JS for this kind of component.

Instead use `<details>` and `<summary>`:

```HTML
<details>
  <summary>Some question</summary>
  <div>
    Answer
  </div>
</details>
```

## `<article>`

This isn't supposed to mean an actual article!

Actually it means *one thing of many* (like cards)

## Breadcrumbs

[WAI Aria Authoring Guide](https://www.w3.org/TR/wai-aria-practices/#breadcrumb)

[WAI Aria Example](https://www.w3.org/TR/wai-aria-practices/examples/breadcrumb/index.html)

For wrapper:
- use `<nav>` with `aria-label="Breadcrumb"`
- wrap items in `<ol>`

For items:
- use `<li>`
- with `<a>`

For separator:
- use `::before` with `content: ''` (because screen readers will anounce the content)

