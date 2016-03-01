---
title: Introspection
order: 6
---


You can introspect your schema using `__schema`, `__type`, and `__typename`,
as [described in the specification](https://facebook.github.io/graphql/#sec-Introspection).

### Examples

Seeing the names of the types in the schema:

```elixir
"""
{
  __schema {
    types {
      name
    }
  }
}
"""
|> Absinthe.run(MyApp.Schema)
{:ok,
  %{data: %{
    "__schema" => %{
      "types" => [
        %{"name" => "Boolean"},
        %{"name" => "Float"},
        %{"name" => "ID"},
        %{"name" => "Int"},
        %{"name" => "String"},
        ...
      ]
    }
  }}
}
```

Getting the name of the queried type:

```elixir
"""
{
  profile {
    name
    __typename
  }
}
"""
|> Absinthe.run(MyApp.Schema)
{:ok,
  %{data: %{
    "profile" => %{
      "name" => "Joe",
      "__typename" => "Person"
    }
  }}
}
```

Getting the name of the fields for a named type:

```elixir
"""
{
  __type(name: "Person") {
    fields {
      name
      type {
        kind
        name
      }
    }
  }
}
"""
|> Absinthe.run(MyApp.Schema)
{:ok,
  %{data: %{
    "__type" => %{
      "fields" => [
        %{
          "name" => "name",
          "type" => %{"kind" => "SCALAR", "name" => "String"}
        },
        %{
          "name" => "age",
          "type" => %{"kind" => "SCALAR", "name" => "Int"}
        },
      ]
    }
  }}
}
```

<p class="warning">
Note that you may have to nest several depths of <code>type</code>/<code>ofType</code>, as
type information includes any wrapping layers of <a href="https://facebook.github.io/graphql/#sec-List">List</a>
and/or <a href="https://facebook.github.io/graphql/#sec-Non-null">NonNull</a>.
</p>

## Using GraphiQL

The [GraphiQL project](https://github.com/graphql/graphiql) is
"an in-browser IDE for exploring GraphQL."

<figure>
  <img src="/img/graphiql.png"/>
  <figcaption class="description">GraphiQL in action.</figcaption>
</figure>

<p class="notice">
We recommend using the pre-built <a href="http://electron.atom.io/">Electron</a>-based wrapper application, <a href="https://github.com/skevy/graphiql-app">GraphiQL.app</a>, which makes it very easy to introspect your
development and production Absinthe applications.
</p>

### GraphQL Hub

[GraphQL Hub](https://www.graphqlhub.com/) is an interesting website that you
can use to introspect a number of public GraphQL servers, using GraphiQL in the
browser and providing useful examples.