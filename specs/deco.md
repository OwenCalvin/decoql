# 📟 `deco`
`deco` (without uppercase letters) aims to provide an API to create a GraphQL APIs to be highly dynamic, fast, ease-to-deploy and lightweight, ideal for integration into a headless CMS core.

the goal of `deco`, is to be simple, fast, efficient and easy to deploy.

For this I'm going to rely on the knowledge acquired during the [rakkit.dev](https://rakkit.dev) project on which I developed a solution using decorators. The problem is that because of the high dynamicity of the data, decorators are a complex and unstable solution to implement in a project with this goal.

It is also necessary to limit the dependencies of the project, we want to keep it very light.

*Using A node environnement:*  
I allow us only two dependencies, which are `graphql` and `glob`.

*Using Golang*  
?

The API will use the `builder` design pattern and will be able to load type data via definition files (yaml, json, .deco => ?).

# Analysis

## GraphQL
GraphQL's specifications are [here](http://spec.graphql.org/) ([latest](http://spec.graphql.org/June2018/)) 

### Types
GraphQL has different types of data that need to be identified:
![](https://i.imgur.com/XRXebeB.png)

### Possibile relations betweens the types
It's necesse

#### ObjectType
![](https://i.imgur.com/agP06Eb.png)

#### InputType
![](https://i.imgur.com/0wF6uyi.png)

#### InterfaceType
![](https://i.imgur.com/Gad9Mra.png)

## Deco 
### Type definition
Deco will load files located in specific folders. With obviously configurable access paths. And parse the files type definitions.

#### Support:
- **extends** support, a type could extends an another type
```graphql
# What it should looks like using SDL, not supported by GraphQL natively
type Animal {
  color: String
}

type Cow extends Animal {
}
```

compiled to `SDL`:
```graphql
type Cow {
  color: String
}
```

- **generics** support, a type could have a generic type definition:
```graphql
# What it should looks like using SDL, not supported by GraphQL natively
type Response<ResponseType> {
  items: ResponseType[]
  count: number
}

input Arguments<ArgsType> {
  where: ArgsType
}

type User {
  name: String!
}

input UserInput {
  name: String
}

query {
  users(args: Arguments<User>): ResponseType<User>
}
```

compiled to `SDL`:
```graphql
# What it should looks like using SDL, not supported by GraphQL natively
type User {
  name: String
}

input UserInput {
  name: String
}

input UserArguments {
  where: UserInput
}

type UserResponse {
  items: User[]
  count: number 
}

query {
  users(args: UserArguments): UserResponse
}
```

### Queries and mutation
Arguments are parsed by the `graphql` library, we have to implement a Middleware feature in order to define functions executed before or after the resolution of a query.  

### Adding shemas during execution time
The best solution for this case is to use a interpreted langage such as *TypeScript* / *Python* / ...  
*Golang* is compiled but the this process is very fast. A really interesting candidate (can you load files, compile `SDL`, and update the schema during the runtime ?) 

## Runtime update
We have to update two things if we want to use `deco` as a CMS core, the GraphQL schema and... the database.

## `deco` SDL compiler
Because of the lack of `SDL` features you have to write a precompiler, that compile `deco`'s type definition into real SDL.



