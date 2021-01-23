## Setting up and dependencies installation

```
mkdir graphql-server-express
cd graphql-server-express
```

```
npm init -y
```

```
npm install express apollo-server-express
npm install --save-dev @babel/core @babel/node @babel/preset-env babel-cli babel-preset-es2015 nodemon
```

Pretty straight-forward so far, right? Good.

### Let's jump into the code

Create a `.babelrc` file in the root folder

```
{
  "presets": ["@babel/preset-env"]
}
```

This file will enable us to use some ES6 capabilities such as the use of `import`.

Create `src` folder and the `index.js` file in it as follows

```
import express from "express";
import { ApolloServer, gql } from "apollo-server-express";

const typeDefs = gql`
  type Query {
    hello: String
  }
`;

const resolvers = {
  Query: {
    hello: () => "Hello world!",
  },
};

const server = new ApolloServer({ typeDefs, resolvers });

const app = express();
server.applyMiddleware({ app });

app.listen({ port: 4001 }, () =>
  console.log(`Server listening on http://localhost:4001${server.graphqlPath}`)
);
```

A couple of things about this file:

- This is the entry point of the server
- I use one file for all the code just for simplicity - you can decide to split it into multiple files
- I use the port `4001` but you can use another port if you want

### How to start the server with `nodemon`

Now update the `script` command in the `package.json` file as follows

```
...
  "scripts": {
    "start": "nodemon --exec babel-node src/index.js"
  }
...
```

Now run `npm start` in the terminal. The GraphQL Playground will be available at `http://localhost:4001/graphql` (or any other address/port you used).
