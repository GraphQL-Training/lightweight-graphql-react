class: title-page, blue

<div>
<img src='images/react-logo.png' title='React logo' class='react-logo'>
</div>

## Lightweight GraphQL

### Benjie Gillam

GraphQL-Training.com,  
[PostGraphQL](https://github.com/postgraphql/postgraphql) OSS maintainer

<div class='postgraphql-logo-container'>
<img src='images/postgraphql-logo.png' title='PostGraphQL logo' class='postgraphql-logo'>
</div>

.slidesLocation[

[benjie.github.io/react-graphql-intro/](https://benjie.github.io/react-graphql-intro/)
]

???

Please note the URL at the bottom, so don't worry about jotting down code. Of
course if you want to follow along on your phone or laptop, please do!

---

## Lightweight GraphQL app

--

- ❎ GraphQL tooling
--

- ❎ JavaScript tooling
--

- ❎ Delving into the server-side
--

- ✅ Vanilla JS
--

- ✅ window.fetch()
--

- ✅ React

---
class: black center middle

## DISCLAIMER

???

- NOT how you should build an app
- Tooling does a lot for you, but it can be complex.
- Good to understand what's going on under the covers

---
class: has-code center

## The spec

![Spec](images/spec.png)

---
class: blue center middle

# Step 1: Mock

---
class: has-code

Basic HTML setup:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Lightweight GraphQL Example</title>
    <meta http-equiv="Content-Type" content="text/html; charset=UTF-8"/>
*   <link type='text/css' rel='stylesheet' href='css/bootstrap.min.css' />
  </head>
  <body>
    <div id='react'></div>
*   <script type="application/javascript" src="react.min.js"></script>
*   <script type="application/javascript" src="react-dom.min.js"></script>
*   <script type="application/javascript" src="babel.min.js"></script>
*   <script type="text/babel" src="main.js"></script>
  </body>
</html>
```

---
class: has-code

.contextImage[
![Spec](images/spec.png)
]


Backend engineers have provided mock data:

```js
const MOCK_DATA = {
  currentUser: {
    name: 'Benjie',
    website: 'https://graphile.org',
  },
};
```

---
class: has-code

.contextImage[
![Spec](images/spec.png)

.context[
```js
const MOCK_DATA = {
  currentUser: {
    name: 'Benjie',
    website: 'https://graphile.org',
  },
};
```
]
]
--

.hack[
&nbsp;
]

```js
const Header = ({currentUser}) =>
  <nav>
    <a href='/'>MyWebsite</a>
    <a>{currentUser.name}</a>
  </nav>;
```
---
class: has-code

.contextImage[
![Spec](images/spec.png)

.context[
```js
const MOCK_DATA = {
  currentUser: {
    name: 'Benjie',
    website: 'https://graphile.org',
  },
};
```


```js
const Header = ({currentUser}) =>
  <nav>
    <a href='/'>MyWebsite</a>
    <a>{currentUser.name}</a>
  </nav>;
```
]
]

--

.spaceAtTop[
]


```js
class SettingsPage extends React.Component {
  ...
  render() {
    return <form onSubmit={this.onSumbit}>
      <h2>Settings</h2>
      <label>Name <input ... /></label>
      <label>Website <input ... /></label>
      <button>Save</button>
    </form>;
  }
}
```
---
class: has-code

.contextImage[
![Spec](images/spec.png)

.context[
```js
const MOCK_DATA = {
  currentUser: {
    name: 'Benjie',
    website: 'https://graphile.org',
  },
};
```


```js
const Header = ({currentUser}) =>
  <nav>
    <a href='/'>MyWebsite</a>
    <a>{currentUser.name}</a>
  </nav>;
```
]
]

.spaceAtTop[
]


.context[
```js
class SettingsPage extends React.Component {
  ...
  render() {
    return <form onSubmit={this.onSumbit}>
      <h2>Settings</h2>
      <label>Name <input ... /></label>
      <label>Website <input ... /></label>
      <button>Save</button>
    </form>;
  }
}
```
]
```js
const Layout = ({currentUser}) =>
  <div>
    <Header currentUser={currentUser} />
    <SettingsPage
      currentUser={currentUser} />
  </div>;
```
---
class: has-code

.contextImage[
![Spec](images/spec.png)

.context[
```js
const MOCK_DATA = {
  currentUser: {
    name: 'Benjie',
    website: 'https://graphile.org',
  },
};
```


```js
const Header = ({currentUser}) =>
  <nav>
    <a href='/'>MyWebsite</a>
    <a>{currentUser.name}</a>
  </nav>;
```
]
]

.spaceAtTop[
]


.context[
```js
class SettingsPage extends React.Component {
  ...
  render() {
    return <form onSubmit={this.onSumbit}>
      <h2>Settings</h2>
      <label>Name <input ... /></label>
      <label>Website <input ... /></label>
      <button>Save</button>
    </form>;
  }
}
```
```js
const Layout = ({currentUser}) =>
  <div>
    <Header currentUser={currentUser} />
    <SettingsPage
      currentUser={currentUser} />
  </div>;
```
]

```js
ReactDOM.render(
  <Layout {...MOCK_DATA} />,
  document.getElementById('react')
);
```

---
class: blue center middle

# Step 2: GraphQL

---
class: has-code

GraphQL returns data in shape you request.

--

.leftSplit[
To get:

```json
{
  "currentUser": {
    "name": "Benjie",
    "website": "https://graphile.org"
  }
}
```
]

--
.rightSplit[
We request:

```graphql
{
  currentUser {
    name
    website
  }
}
```
]

--

.spaceAtTop.clearfix[
]

But how do we know which fields are available?

---
class: has-code

## GraphQL Schema
--

`schema` determines available operation types.

--

```graphql
schema {
⁣
⁣
⁣
}
```

---
class: has-code

## GraphQL Schema

`schema` determines available operation types.


```graphql
schema {
  query: Query
⁣
⁣
}
```
---
class: has-code

## GraphQL Schema

`schema` determines available operation types.


```graphql
schema {
  query: Query
  mutation: ...
⁣
}
```
---
class: has-code

## GraphQL Schema

`schema` determines available operation types.


```graphql
schema {
  query: Query
  mutation: ...
  subscription: ...
}
```
---
class: has-code

## GraphQL Schema

`schema` determines available operation types.


```graphql
schema {
* query: Query
}
```

---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
]

`Query` is the entry-point for reading data.  
It defines the fields available at the root level.
```graphql
type Query {
⁣
}
```
---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
]

`Query` is the entry-point for reading data.  
It defines the fields available at the root level.
```graphql
type Query {
* currentUser: User
}
```
---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
]

GraphQL is strongly-typed, so we will know the type(s) each field can return.

---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
]

GraphQL is strongly-typed, so we will know the type(s) each field can return.
```graphql
type User {
⁣
⁣
⁣
}
```
---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
]

GraphQL is strongly-typed, so we will know the type(s) each field can return.
```graphql
type User {
  id: Int!
⁣
⁣
}
```
---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
]

GraphQL is strongly-typed, so we will know the type(s) each field can return.
```graphql
type User {
  id: Int!
  name: String!
⁣
}
```

---
class: has-code

## GraphQL Schema

.context[
```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
]

GraphQL is strongly-typed, so we will know the type(s) each field can return.
```graphql
type User {
  id: Int!
  name: String!
  website: String
}
```
---
class: has-code

## GraphQL Schema

```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
```graphql
type User {
  id: Int!
  name: String!
  website: String
}
```
---
class: has-code

## GraphQL Schema / GraphiQL

.graphiqlImage[
![GraphiQL](images/graphiql.png)
]

```graphql
schema {
  query: Query
}
```
```graphql
type Query {
  currentUser: User
}
```
```graphql
type User {
  id: Int!
  name: String!
  website: String
}
```
