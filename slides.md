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
const Page = ({currentUser}) =>
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
const Page = ({currentUser}) =>
  <div>
    <Header currentUser={currentUser} />
    <SettingsPage
      currentUser={currentUser} />
  </div>;
```
]

```js
ReactDOM.render(
  <Page {...MOCK_DATA} />,
  document.getElementById('react')
);
```

---
class: blue center middle

# Step 2: GraphQL API

---
class: has-code

## Initial GraphQL Schema
--

.spaceAtTop[
]

```graphql
schema {
  query: Query
}
```
--
```graphql
type Query {
  currentUser: User
}
```
--
```graphql
type User {
  id: Int!
  name: String!
  website: String
}
```


---
class: has-code

## Initial GraphQL Schema

.graphiqlImage[
![GraphiQL](images/graphiql.png)
]

.spaceAtTop[
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
