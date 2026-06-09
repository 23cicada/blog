# GraphQL

A curated list of useful docs and articles for quick reference.

## Core Concepts

- [Schemas and Types](https://graphql.org/learn/schema/) - How to define a GraphQL schema with types
- [Queries](https://graphql.org/learn/queries/#variables) - Fetching data, including query variables
- [Mutations](https://graphql.org/learn/mutations/) - Modifying server-side data
- [Validation](https://graphql.org/learn/validation/) - How queries are validated against the schema
- [Fragments](https://graphql.org/learn/queries/#fragments) - Reusable units of query fields

## Apollo Server

- [Get Started with Apollo Server](https://www.apollographql.com/docs/apollo-server/getting-started) - Setup and first server
- [API Reference: ApolloServer](https://www.apollographql.com/docs/apollo-server/api/apollo-server) - Constructor options and methods
- [Resolvers](https://www.apollographql.com/docs/apollo-server/data/resolvers) - How to resolve schema fields to data
- [GraphOS Studio Explorer](https://www.apollographql.com/docs/graphos/platform/explorer) - Interactive query IDE
- [Error Handling](https://www.apollographql.com/docs/apollo-server/data/errors#custom-errors) - Built-in and custom errors
- [Context and contextValue](https://www.apollographql.com/docs/apollo-server/data/context) - Sharing data across resolvers
- [Authorization in GraphQL](https://www.apollographql.com/blog/authorization-in-graphql/) - Identifying the user and access control
- [API Reference: expressMiddleware](https://www.apollographql.com/docs/apollo-server/api/express-middleware) - Apollo Server's Express integration
- [Subscriptions in Apollo Server](https://www.apollographql.com/docs/apollo-server/data/subscriptions) - Real-time data over WebSockets

## Apollo Client

- [Get Started with Apollo Client](https://www.apollographql.com/docs/react/get-started) - Setup with React
- [Queries](https://www.apollographql.com/docs/react/data/queries) - Fetching data with `useQuery`
- [Mutations](https://www.apollographql.com/docs/react/data/mutations) - Updating data with `useMutation`
- [Caching](https://www.apollographql.com/docs/react/caching/overview) - How the normalized cache works
- [Refetching Queries](https://www.apollographql.com/docs/react/data/refetching) - Keeping the UI in sync after changes
- [Authentication](https://www.apollographql.com/docs/react/networking/authentication#header) - Sending auth headers
- [Subscriptions](https://www.apollographql.com/docs/react/data/subscriptions) - Real-time updates on the client

## N+1 Problem

- [Batching GraphQL Queries with DataLoader](https://www.petecorey.com/blog/2017/08/14/batching-graphql-queries-with-dataloader/) - Walkthrough of the problem and fix
- [dataloader](https://github.com/graphql/dataloader) - Batching and caching utility
- [Batching and Caching](https://www.apollographql.com/docs/apollo-server/data/fetching-data#batching-and-caching) - Apollo's official guidance

## Structuring GraphQL Applications

- [Modularizing Your GraphQL Schema Code](https://www.apollographql.com/blog/modularizing-your-graphql-schema-code) - Splitting schema into modules
- [Thoughts on Structuring Your Apollo Queries & Mutations](https://medium.com/@peterpme/thoughts-on-structuring-your-apollo-queries-mutations-939ba4746cd8) - Organizing client-side operations

## VS Code Extensions

- [GraphQL: Language Feature Support](https://marketplace.visualstudio.com/items?itemName=GraphQL.vscode-graphql) - Syntax highlighting, autocomplete, and validation
