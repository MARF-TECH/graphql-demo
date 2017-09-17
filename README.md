# graphql-demo

Run `npm start` & goto `localhost:4000`. Send a graphQL query for a list of cognitive biases, or a random cognitive bias. Choose which fields you need. The nodeJS `api.js` will scrape the cognitive biases website efficiently & with caching, to request the minimum amount to the backend to resolve the query.
![](https://i.imgur.com/eDvVAIZ.png)

# Examples
![](https://i.imgur.com/BkPgK7z.png)
![](https://i.imgur.com/ixBBnyo.png)

# Resolver
The resolver is not documented very well in the GraphQL reference implementations. The way resolvers can be nested is by flattening them like a router. First the query hits the `Query` property, and looks for the field you're requesting (random, or list).

Next, consider that the description property is missing from the list of biases returned by `list()`, it only returned a list of partial biases, that is they're missing their descriptions.

If & *only if* the user asked for the descriptions, the GraphQL resolver iterates the resolvers again, ands end up on the `bias.description` resolver, which uses the `description()` function to resolve the description for the partial bias object into a complete (from the query's perspective) hydrated object.
```js
  Query: {
    random: () => {
      return list().then(_.sample)
    },
    list: () => {
      return list();
    },
  },
  Bias: {
    description: description
  }
```

# When not to use GraphQL

https://github.com/facebook/graphql/issues/91
https://github.com/facebook/graphql/issues/237

- Don't use it to convert a tree into a graph back into a tree.
- Don't use it where N+1 queries to the backend will be an issue & SQL is easier.
- Don't use it if you want a document, or blob of json. Not everything needs a strict type system. If you don't need to query into a blob of data, use REST to fetch that blob.

# When to use GraphQL

- Large teams
- Many client-apps to support
- Rich views that are used in different contexts
- Network resources are limited or unreliable

# Mental Model of GraphQL

![](https://files.slack.com/files-pri/T024Y04KD-F747KDN83/1-alm6aootcuvare3ar3loxw.png)
![](https://files.slack.com/files-pri/T024Y04KD-F73PY5F9P/image.png)
![](https://files.slack.com/files-pri/T024Y04KD-F74DV1HGD/screen_shot_2017-09-16_at_10.21.06_pm.png)
