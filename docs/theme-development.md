# Theme development

A theme is where you populate layout components, Peco will looks for layout component at `./path/to/theme/layouts` directory.

The layout component for each page is inferred by its type, e.g. the `type` of `index.md` is `index`, the layout component of pages in `index` type default to `index.{vue,js}`:

|Files|Page Type|Default Layout|Fallback Layout|
|---|---|---|---|
|index.md|index|index||
|source/_posts/*.md|post|post|page|
|source/*.md|page|page||
|(categories page)|category|category|index|
|(tags page)|tag|tag|index|

> __FAQ: why is there page type when page layout is basically the same thing?__
>
> Because in this way you can use different layout for the same type, imagine using different layout components for differnt posts? You can!

## Layout component props

Layout component accepts `page` prop:

```typescript
interface Page {
  // parsed front-matter 
  attributes: {
    [k: string]: any
  },
  // Rendered HTML for your markdown
  body?: string
  // first paragraph of body
  excerpt?: string
  // Permanent link
  permalink: string
  // Slugified file path
  slug: string
}
```

For the `page` prop for layout component of `index` `category` `tag` type pages, it has some extra keys:

```typescript
interface IndexPage extends Page {
  posts: Page[]
  pagination: {
    hasNext: boolean
    hasPrev: boolean
    prevLink: string
    nextLink: string
    total: number
    current: number
  }
}
```

## Preprocessors

You can use `ES2015` `Sass` `Stylus` etc to write your theme.

Note that you need to install relevant dependencies for your theme, like `node-sass` and `sass-loader` for `Sass`.

## Access site data in component

- `this.$siteData`
  - `title`: The `title` in config file
  - `description`: The `description` in config file
- `this.$themeConfig`: `themeConfig` in config file

## App-level enhancements

Peco will read `./path/to/theme/index.js` to apply app-level enhancements:

```js
import Vue from 'vue'

// Add a global mixin
Vue.mixin({})

// Optional default export
export default ({
  // Vue router instance
  router,
  // Options for root Vue instance
  rootOptions
}) => {
  // Do something...
}
```
