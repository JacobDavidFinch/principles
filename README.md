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

## Logs

Know when to use each log level
`FATAL` - denotes very severe error events that will presumably lead the application to abort. Usually, these can end up in catastrophic failures.
`ERROR` - denotes error events that might still allow the application to continue running, with reduced capabilities in the affected paths.
`WARN` - denotes less-harmful events than errors. Usually, they do not lead to any degradation of capabilities or complete failure of the application. However, they are still red flags and `must` be investigated.
`INFO` - denotes the important event banners and informational messages in the application behaviour.
`DEBUG` - denotes specific and detailed information, mainly used for debugging purposes. These logs help us step through the code.
`TRACE` - denotes most low-level information like stack traces of code to provide the most information on a certain event/context. These logs help us inspect the variable values and full error stacks.

## Extra Piece: Learning, Notes, and Flashcards

#### Learning

Use the Feynman Technique
1. Pretend to teach a concept you want to learn about to a student in the sixth grade.
2. Identify gaps in your explanation. Go back to the source material to better understand it.
3. Organize and simplify.
4. Transmit (optional).
#### Flashcards
Retrieval practice is a great method for learning. Flashcards can assist for retrieval and spaced repetition

Mistakes
- You grab off-the-shelf flashcard decks, rather than make or edit your own.
- You design the cards badly. This leads to either memorizing useless stuff or failing to learn what you actually care about.
  - Each question should have one and only one correct answer. (Something violated by my Chinese deck as mentioned.)
  - Questions should either have as little unnecessary context as possible, or redundancy. A vocabulary word alone, or used within a few sentences (each on different cards) are better designs than a single sentence. Why? Because you learn to predict the answer based on the surrounding context even if that won’t be there when you need to use it in real life.
  - Questions should be simple. Complex problem solving isn’t well-suited to flashcards. Better to break apart complex problems into multiple steps, or simply forego flashcards altogether in favor of solving real problems.
  - Questions should be something you actually need. Just because you can memorize something doesn’t mean you should.
- Memorization substitutes for understanding.
- Flashcards substitute for doing real practice.

#### Note Taking
Taking notes isn’t just a way of writing things down. It’s a way of orienting your attention so that you’ll build better memories—even if you never open the notebook again.

Two purposes notes server are
1. They record what you learned, for easy access later.
2. They orient your attention, allowing you to remember more.

What we’re trying to achieve when taking notes is to begin the process of organizing the information we’re receiving into a mental structure we can use later. Organized well, and the correct memories will automatically pop up when you need them.

Ask yourself what kind of structure for knowledge do you need to organize? Next, Anticipate how you'll need to use your mind.
- Think of the endpoint of learning. What inputs will you receive and how will you retrieve the needed outputs for them?
  - Looking at it in a graphically, flow-chart way can help. Second-Brains are all about this [foam](https://github.com/foambubble/foam)
  - The brain is complicated and so what you’re actually doing when learning and performing is going to be more sophisticated than a flow-chart. But, as a simplification for what you’re trying to do, they often work pretty well.
- Whenever I tackle a new subject, one of my first thoughts is what kind of structure am I trying to build. What would be the input situations that should cause me to remember this knowledge? How do I need to manipulate it, discriminate between similar-seeming situations, calculate or reason with it?
  - Rote Knowledge - Flashcards
  - Complicated and trying to figure it out - Practice problems
  - Looking for understaind - teach it to someone else/or self
  - To solve practical problems - THink about applications and examples
- If the situations where you want to use the knowledge are diverse, I would try to expose myself to as many different examples as possible. If the purpose is going to be fairly narrow, aimed at a specific task, I might want to practice repeatedly to think of it whenever I see that situation.


#### Big Shoutout to [Unicorn](https://github.com/sindresorhus/eslint-plugin-unicorn/blob/main/docs/rules/filename-case.md), [kettanaito](https://github.com/kettanaito/naming-cheatsheet), [Kent C. Dodds](https://kentcdodds.com/blog/colocation), [DeepSource](https://deepsource.io/blog/git-best-practices/), [Scott Young](https://www.scotthyoung.com/blog/2021/01/11/how-to-take-notes/) for the inspiration and for making this easy for me. Their descriptions, layouts, etc. made much of this as simple as pulling over the concepts.