class: title-page, blue

<div>
<img src='images/react-logo.png' title='React logo' class='react-logo'>
</div>

## Lightweight GraphQL

### Benjie Gillam (@Benjie)

GraphQL Trainer  
[PostGraphQL](https://github.com/postgraphql/postgraphql) OSS maintainer

<div class='postgraphql-logo-container'>
<img src='images/postgraphql-logo.png' title='PostGraphQL logo' class='postgraphql-logo'>
</div>

.slidesLocation[

[graphql-training.com/lightweight-graphql-react](https://www.graphql-training.com/lightweight-graphql-react)
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
class: black center

## DISCLAIMER

--

This talk does not demonstrate best practices.

--

.smallprint[
Examples in this talk and the associated GitHub repository have been
deliberately simplified to ease understanding. GraphQL tooling does a lot for
you and you should embrace it in any GraphQL projects you work on; however
trying to master the tooling at the same time as the new query language can
cause headaches, stress, feelings of being overwhelmed and in some extreme
cases desk inversion. Terms and conditions apply; see in-store for details.
Nullam ac lectus at neque convallis tempus. Nam augue
risus, hendrerit vel ligula sed, sagittis pretium felis. Aenean vitae ipsum nec
nisl faucibus venenatis. Sed sed blandit diam. Quisque et nunc vel orci
sollicitudin vestibulum. Donec tempor felis nec mauris fermentum, id placerat
lorem luctus. Fusce molestie augue ultrices, efficitur arcu sit amet, accumsan
tortor. Vivamus facilisis sapien non sagittis feugiat. Nam dignissim dictum
finibus. Donec sed nisl eget risus dictum gravida. Sed sit amet nulla sapien.

]

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


--
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

# Step 2a: GraphQL

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

---
class: has-code

## Anatomy of a GraphQL Query

```graphql
{
  currentUser {
    name
    website
  }
}
```

The above query has a few simple (nested) fields; this structure is called a **selection set** and tells GraphQL what we want returned.

---
class: has-code

## Anatomy of a GraphQL Query

```graphql
*query {
  currentUser {
    name
    website
  }
}
```

We can also specify the **operation type** `query`; it's optional for queries but required for mutations/subscriptions.

---
class: has-code

## Anatomy of a GraphQL Query

```graphql
*query NameAndWebsiteQuery {
  currentUser {
    name
    website
  }
}
```

The **operation name** (`NameAndWebsiteQuery`) is an optional arbitrary name for the query useful for debugging.

???
 (It should not change the result of the query.)

---
class: has-code

## Anatomy of a GraphQL Query

```graphql
query NameAndWebsiteQuery {
* user(id: 1) {
    name
    website
  }
}
```

Fields in GraphQL can also accept **arguments** which influence what the field does.

Arguments can be required or optional.

---
class: has-code

## Anatomy of a GraphQL Query

```graphql
*query NameAndWebsiteQuery($id: Int!) {
* user(id: $id) {
    name
    website
  }
}
```

**Variables** (if any) are specified after the query name, and are referenced in
the field arguments.


???

Because GraphQL is strongly typed, the type of the variable must be specified.

Similar to prepared statements in SQL.

---
class: has-code

## Anatomy of a GraphQL Query

Recap

```graphql
query NameAndWebsiteQuery($id: Int!) {
  user(id: $id) {
    name
    website
  }
}
```

```graphql
query                   # Operation type
NameAndWebsiteQuery     # Operation name, for debugging
($id: Int!)              # Variables and their types
{                       # Selection set
  user(id: $id)         # Field with arguments, variable reference
  {                     # Nested selection set
    name                # Field
    website
  }
}
```

???
GraphQL also supports variables so that you can inject values into the query in a similar way to prepared queries in SQL.

---

But how do we know which operations are supported, or what fields we can query within those?

--

These questions and more are answered by the GraphQL schema, which fully describes the shape of the GraphQL API.

---
class: has-code

## GraphQL Schema
--

The `schema` entry exposes available operations:


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

The `schema` entry exposes available operations:


```graphql
schema {
  query: ...
⁣
⁣
}
```
---
class: has-code

## GraphQL Schema

The `schema` entry exposes available operations:


```graphql
schema {
  query: ...
  mutation: ...
⁣
}
```
---
class: has-code

## GraphQL Schema

The `schema` entry exposes available operations:


```graphql
schema {
  query: ...
  mutation: ...
  subscription: ...
}
```

---
class: has-code

## GraphQL Schema

.context[
The `schema` entry exposes available operations:


```graphql
schema {
  query: ...
  mutation: ...
  subscription: ...
}
```
]

Our read-only API only has the `query` operation; the fields of which are exposed on the `Query` type:

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
  currentUser: ...
  user(id: Int!): ...
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

```graphql
type Query {
  currentUser: ...
  user(id: Int!): ...
}
```

--

GraphQL is strongly-typed, so we must specify the type(s) each field can return.
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

```graphql
type Query {
* currentUser: User
* user(id: Int!): User
}
```
GraphQL is strongly-typed, so we must specify the type(s) each field can return.
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
  user(id: Int!): User
}
```
]

GraphQL is strongly-typed, so we must specify the type(s) each field can return.

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
  user(id: Int!): User
}
```
]

GraphQL is strongly-typed, so we must specify the type(s) each field can return.
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
  user(id: Int!): User
}
```
]

GraphQL is strongly-typed, so we must specify the type(s) each field can return.
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
  user(id: Int!): User
}
```
]

GraphQL is strongly-typed, so we must specify the type(s) each field can return.
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
  user(id: Int!): User
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
  user(id: Int!): User
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
class: blue center middle

# Step 2b: Querying


---
class: has-code

We can run a GraphQL query in the browser command line using `window.fetch()`:

```js
window.fetch("/graphql", {
```
--
```js
  method: "POST",
```
--
```js
  headers: {
    "Content-Type": "application/json",
    "Accept": "application/json"},
```
--
```js
  body: JSON.stringify({
*   query: "query { currentUser { id name website } }",
*   variables: {}})
```
--
```js
})
.then(response => response.json())
.then(json => console.log(json))
```

---
class: has-code

The only things that change are the `query` and `variables`, so lets define a function to perform the boilerplate for us:

```js
*function executeGraphQLQuery(query, variables = {}) {
  return window.fetch("/graphql", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      "Accept": "application/json"
    },
    body: JSON.stringify({
*     query: query,
*     variables: variables
    })
  }).then(response => response.json());
}
```

---
class: has-code

The `executeGraphQLQuery` function can then be incorporated into a higher-order component:

--

```js
const withGraphQLResult = (query, { variables } = {}) => Component =>
  class extends React.Component {
    state = { loading: true };
    componentDidMount() { this.fetch(); }
    async fetch() {
      try {
        const json = await executeGraphQLQuery(query, variables);
        this.setState({ loading: false, error: null, data: json.data });
      } catch (e) {
        this.setState({ loading: false, error: e });
      }
    }
    render() {
      return <Component
        loading={this.state.loading}
        data={this.state.data}
        error={this.state.error}
      />;
    }
  };
}
```


---
class: has-code

Then every component

```js
const Header = ({ data: { currentUser: { name } } }) =>
  <nav>... { name } ...</nav>;
```

--

can easily fetch data from GraphQL:

```js
export default withGraphQLResult(`{ currentUser { name } }`)(Header)
```

--

👍 Co-locating the data requirements with the component makes maintenance easier.

--

👎 But a network fetch for every component displayed is hugely inefficient!

---
class: blue center middle

# Step 3: Fragments

---

## Fragments

Shared pieces of query logic that can be composed.

--

Each component can have fragments; root component combines the relevant
fragments together to form the GraphQL query.

--

GraphQL will request the minimum information required to satisfy all specified
fragments.


???

By splitting our data requirements into fragments we can
keep their specification close to the components
that need them.

GraphQL will then resolve all these fragments for us
and request the minimum information required to
satisfy them.

---
class: has-code

## Anatomy of a Fragment

--

.leftSplit[
```graphql
fragment HeaderUserFragment
```
]
---
class: has-code

## Anatomy of a Fragment

.leftSplit[
```graphql
fragment HeaderUserFragment on User
```
]
---
class: has-code

## Anatomy of a Fragment

.leftSplit[
```graphql
fragment HeaderUserFragment on User {
  id
  name
}
```
]

---
class: has-code

## Anatomy of a Fragment

.leftSplit[
```graphql
fragment HeaderUserFragment on User {
  id
  name
}
```

```graphql
query SettingsPageRootQuery {
  currentUser {
    id
*   ...HeaderUserFragment
  }
}
```
]

---
class: has-code

## Anatomy of a Fragment

.leftSplit[

```graphql
fragment HeaderUserFragment on User {
  id
  name
}
```

```graphql
query SettingsPageRootQuery {
  currentUser {
    id
*   ...HeaderUserFragment
  }
}
```
]

.rightSplit[
```graphql
⁣
⁣
query SettingsPageRootQuery {
  currentUser {
    id
*   id
*   name
  }
}
```
]

---
class: has-code

.contextImage[
![Spec](images/spec.png)
]

--

```js
Header.HeaderUserFragment = `
  fragment HeaderUserFragment on User {
    name
  }`;
```

--

```js
SettingsPage.SettingsPageUserFragment = `
  fragment SettingsPageUserFragment on User {
    name
    website
  }`;
```

--

.hack[
#### The final query built via string interpolation:
]

```js
const Root = withGraphQLResult(`
  query SettingsPageRootQuery {
    currentUser {
*     ...HeaderUserFragment
*     ...SettingsPageUserFragment
    }
  }

* ${Header.HeaderUserFragment}
* ${SettingsPage.SettingsPageUserFragment}
`)(Layout);
```

---
class: black center middle



⚠️ Simple string concatenation, as we do here, will not allow the same fragment
twice - use a tool such as Apollo or Relay to solve this! ⚠️

---
class: blue center middle

## Step 4: Another attribute

`avatarUrl` added to GraphQL Schema

---
class: has-code

Add `avatarUrl` to our React component:

```js
const Header = ({data: {currentUser,
* avatarUrl}}) =>
  <nav><a href='/'>MyWebsite</a>
    {currentUser
      ? <a>
*         {avatarUrl && <img src={avatarUrl} />}
          {currentUser.name}
        </a>
      : <a href='/login'>Login</a>}</nav>;
```

--

Add it to the fragment:

```js
Header.HeaderUserFragment = `
  fragment HeaderUserFragment on User {
    id
    name
*   avatarUrl
  }`;
```

---
class: center middle

# That was it!

Layout doesn't know or care it was added.

---
class: blue center middle

## Step 5: Mutations

---
class: has-code

.contextImage[
.context[
#### Remember our query anatomy?
```graphql
query NameAndWebsiteQuery($id: Int!) {
  user(id: Int!): User
    id
    name
    website
  }
}
```
]
]

### Anatomy of a Mutation

--

```js
mutation UpdateCurrentUserMutation(
⁣
) {
```
---
class: has-code

.contextImage[
.context[
#### Remember our query anatomy?
```graphql
query NameAndWebsiteQuery($id: Int!) {
  user(id: Int!): User
    id
    name
    website
  }
}
```
]
]

### Anatomy of a Mutation

```js
mutation UpdateCurrentUserMutation(
  $userChanges: UserPatch!
) {
```
--
```js
  updateCurrentUser(
```
--
```js
    input: {
```
--
```js
      userPatch: $userChanges
```
--
```js
    }
  ) {
```
--
```js
    user {
```
--
```js
      id
      name
      website
```
--
```js
    }
  }
}
```

---
class: has-code

```js
const mutationQuery = `
  mutation UpdateCurrentUserMutation($userChanges: UserPatch!) {
    updateCurrentUser(input: { userPatch: $userChanges }) {
      user { ...HeaderUserFragment ...SettingsPageUserFragment }
    }
  }`;
```
--
To perform the mutation, we can use our helper:

--

```js
executeGraphQLQuery(mutationQuery, {
```

--

```js
  userChanges: { // This goes into the `$userChanges` variable
```
--
```js
    website: "https://twitter.com/benjie"
```
--
```js
  }

})
```

--

Queries and mutations use the same API - `query` and `variables`.

---

Using the data from the payload we can then update the display.

--

Tools like Apollo and Relay manage your store/cache for you and tell React to
re-render.

--

Relay uses the global object identifier specification (node Int) as the cache key.

--

Apollo is more flexible, allowing you to specify `dataIdFromObject` (commonly a
combination of `__typename` and `id` or similar).

---
class: blue center middle

# Conclusion

---
.slidesLocation[
Twitter: @Benjie  
Slides: [https://is.gd/lightweight_graphql_talk](https://www.graphql-training.com/lightweight-graphql-react/)  
Example repo: [https://is.gd/lightweight_graphql](https://github.com/GraphQLTraining/lightweight-graphql-example)
]

We've covered:

--

- Brief overview of a simple GraphQL Schema
--

- GraphQL queries and mutations with `window.fetch()`
--

  - `query` string expressing fields to request
--

  - `variables` expressing inputs to fields (optional)
--

  - Mutations are very similar to queries
--

- Fragments keep data specs DRY
--

- Tooling (e.g. Apollo & Relay) does a lot of work for us

