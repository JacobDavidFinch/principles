# Style Guide
Preferred Code Style Guide picked up from blogs, books, videos, and preferences.
## Naming Convention

camelCase naming convention
- fooBar.js
- let fooBar

### File Naming
  
separate file details by period
- fooBar.test.js
- fooBar.helper.js

use eslint unicorn for file naming enforcement
```js
"unicorn/filename-case": [
	"error",
	{
		"cases": {
			"camelCase": true,
			"pascalCase": true
		}
	}
]
```
---

### Variable Naming
#### S-I-D

A name must be _short_, _intuitive_ and _descriptive_:

- **Short**. A name must not take long to type and, therefore, remember;
- **Intuitive**. A name must read naturally, as close to the common speech as possible;
- **Descriptive**. A name must reflect what it does/possesses in the most efficient way.

```js
/* Bad */
const a = 5 // "a" could mean anything
const isPaginatable = a > 10 // "Paginatable" sounds extremely unnatural
const shouldPaginatize = a > 10 // Made up verbs are so much fun!

/* Good */
const postCount = 5
const hasPagination = postCount > 10
const shouldPaginate = postCount > 10 // alternatively
```

#### Avoid contractions

Do **not** use contractions. They contribute to nothing but decreased readability of the code. Finding a short, descriptive name may be hard, but contraction is not an excuse for not doing so.

```js
/* Bad */
const onItmClk = () => {}

/* Good */
const onItemClick = () => {}
```

#### Avoid context duplication

A name should not duplicate the context in which it is defined. Always remove the context from a name if that doesn't decrease its readability.

```js
class MenuItem {
  /* Method name duplicates the context (which is "MenuItem") */
  handleMenuItemClick = (event) => { ... }

  /* Reads nicely as `MenuItem.handleClick()` */
  handleClick = (event) => { ... }
}
```

#### Reflect the expected result

A name should reflect the expected result.

```jsx
/* Bad */
const isEnabled = itemCount > 3
return <Button disabled={!isEnabled} />

/* Good */
const isDisabled = itemCount <= 3
return <Button disabled={isDisabled} />
```

---
### Function Naming
#### A/HC/LC Pattern

There is a useful pattern to follow when naming functions:

```
prefix? + action (A) + high context (HC) + low context? (LC)
```

Take a look at how this pattern may be applied in the table below.

| Name                   | Prefix   | Action (A) | High context (HC) | Low context (LC) |
| ---------------------- | -------- | ---------- | ----------------- | ---------------- |
| `getUser`              |          | `get`      | `User`            |                  |
| `getUserMessages`      |          | `get`      | `User`            | `Messages`       |
| `handleClickOutside`   |          | `handle`   | `Click`           | `Outside`        |
| `shouldDisplayMessage` | `should` | `Display`  | `Message`         |                  |

> **Note:** The order of context affects the meaning of a variable. For example, `shouldUpdateComponent` means _you_ are about to update a component, while `shouldComponentUpdate` tells you that _component_ will update on itself, and you are but controlling when it should be updated.
> In other words, **high context emphasizes the meaning of a variable**.

#### Actions

The verb part of your function name. The most important part responsible for describing what the function _does_.

`get` - Accesses data immediately (i.e. shorthand getter of internal data).

`set` - Sets a variable in a declarative way, with value `A` to value `B`.

`reset` - Sets a variable back to its initial value or state.

`fetch` - Request for some data, which takes some indeterminate time (i.e. async request).

`remove` - Removes something _from_ somewhere. For example, if you have a collection of selected filters on a search page, removing one of them from the collection is `removeFilter`, **not** `deleteFilter` (and this is how you would naturally say it in English as well):

`delete` - Completely erases something from the realms of existence. Imagine you are a content editor, and there is that notorious post you wish to get rid of. Once you clicked a shiny "Delete post" button, the CMS performed a `deletePost` action, **not** `removePost`.

`compose` - Creates new data from the existing one. Mostly applicable to strings, objects, or functions.

`handle` - Handles an action. Often used when naming a callback method.

#### Context

A domain that a function operates on.

A function is often an action on _something_. It is important to state what its operable domain is, or at least an expected data type.

```js
/* A pure function operating with primitives */
function filter(list, predicate) {
  return list.filter(predicate)
}

/* Function operating exactly on posts */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now())
}
```

> Some language-specific assumptions may allow omitting the context. For example, in JavaScript, it's common that `filter` operates on Array. Adding explicit `filterArray` would be unnecessary.
#### Prefixes

Prefix enhances the meaning of a variable. It is rarely used in function names.

`is` - Describes a characteristic or state of the current context (usually `boolean`). Ex: isBlue, isPresent

`has` - Describes whether the current context possesses a certain value or state (usually `boolean`). 

```js
/* Bad */
const isProductsExist = productsCount > 0
const areProductsPresent = productsCount > 0

/* Good */
const hasProducts = productsCount > 0
```

`should` - Reflects a positive conditional statement (usually `boolean`) coupled with a certain action. Ex: shouldUpdateUrl

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl
}
```

`min`/`max` - Represents a minimum or maximum value. Used when describing boundaries or limits.

`prev`/`next` - Indicate the previous or the next state of a variable in the current context. Used when describing state transitions.

#### Singular and Plurals

Like a prefix, variable names can be made singular or plural depending on whether they hold a single value or multiple values.

```js
/* Bad */
const friends = 'Bob'
const friend = ['Bob', 'Tony', 'Tanya']

/* Good */
const friend = 'Bob'
const friends = ['Bob', 'Tony', 'Tanya']
```
---
## Commits

Write clean, single-purpose commits. Don't get sidetracked! Commit one particular thing at a time. 
- Easier to see changes and code reviews will be more efficient
- Commit rollback is far easier
- Straightforward to track changes in the ticketing system

Best way to achieve clean, single-purpose commits? Commit early and commit often

Generally speaking, you shouldn't commit generated files. Only commit files that take manual effort to create.

Use a rigged commit message format and be a better programmer for it

- Prefix a type: chore, docs, feat, fix, refactor, style, or test
- Summary in present tense

``` 
chore: add Oyster build script
docs: explain hat wobble
feat: add beta sequence
fix: remove broken confirmation message
refactor: share logic between 4d3d3d3 and flarhgunnstow
style: convert tabs to spaces
test: ensure Tayne retains clothing
```
---
## Folder Structure

- Don't get loose with where you keep documents. All projects should start in documents/repos
- If projects are coupled together have a workspace for them
- Follow the concept of co-location. Place code as close to where it's relevant as possible. 
  - If you have an abstraction used all over, make a utils folder and place it in there
  - If your fn, component, stylesheet, etc. is only used in one place, keep it with that one place. When the place get's overrun, make a file nearby to keep it. If the code starts getting used other places determine if it's time to abstract it out. 
    > "Things that change together should be located as close as reasonable."
---
## High Level Code Opinions

Avoid Hasty Abstractions
> Prefer duplication over the wrong abstraction

## Comments and  Documentation 
- Explain why you're doing something unexpected in the comments so people coming after can understand the decisions that were made which resulted in the unexpected or odd code.

## Testing

## Extra Piece: Learning, Notes, and Flashcards

#### Big Shoutout to [Unicorn](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/filename-case.md), [kettanaito](https://github.com/kettanaito/naming-cheatsheet), [Kent C. Dodds](https://kentcdodds.com/blog/colocation), [DeepSource](https://deepsource.io/blog/git-best-practices/) for the inspiration and for making this easy for me. Their descriptions, layouts, etc. made much of this as simple as pulling over the concepts.