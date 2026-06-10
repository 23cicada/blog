## Tips

TypeScript error messages have the most useful information at the end of the message.

- [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped)

## compilerOptions

`noEmit: true`: Do not emit compiler output files like JavaScript source code, source-maps or declarations.

`noImplicitAny: true`: TypeScript will issue an error whenever it would have inferred any.

In some cases where no type annotations are present, TypeScript will fall back to a type of any for a variable when it cannot infer the type.

`allowImportingTsExtensions: true`: Allows TypeScript files to import each other with a TypeScript-specific extension like `.ts`, `.mts`, or `.tsx`. This flag is only allowed when `--noEmit` or `--emitDeclarationOnly` is enabled.

`target: esnext`: Tells TypeScript should compile to the latest JavaScript features.

`module: nodenext`: Tells TypeScript to use Node.js's native module resolution for ESM (ES Modules).

`esModuleInterop: true`: Allows interoperability between CommonJS and ES Modules

Without it: importing a CommonJS module requires `import * as express from 'express'`

with it: `import express from 'express'`

## `unknown`

unknown is the type-safe counterpart of any. Anything is assignable to unknown, but unknown isn’t assignable to anything but itself and any without a **type assertion(类型断言)** or a control flow based **narrowing(类型收窄)**. Likewise, no operations are permitted on an unknown without first asserting or narrowing to a more specific type.

## [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

**Discriminated unions**

The specific technique of type narrowing where a union type is narrowed based on literal attribute value is called discriminated union.

```js
interface Shape {
  kind: "circle" | "square";
  radius?: number;
  sideLength?: number;
}

function getArea(shape: Shape) {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2; // (parameter) shape: Circle
    case "square":
      return shape.sideLength ** 2; // (parameter) shape: Square
  }
}
```
[Exhaustiveness checking](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking)

**Exhaustiveness checking**

**type predicates**

```js
const isString = (text: unknown): text is string => {
  return typeof text === 'string' || text instanceof String;
};
```

**The operator `in`**

```js
type Fish = { swim: () => void };
type Bird = { fly: () => void };
 
function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    return animal.swim();
  }
 
  return animal.fly();
}
```

**`instanceof`**

```js
try {
  // ...
} catch (error: unknown) {
  let errorMessage = 'Something went wrong: '
  // here we can not use error.message
  if (error instanceof Error) {
   // the type is narrowed and we can refer to error.message
    errorMessage += error.message;
}
  // here we can not use error.message
  console.log(errorMessage);
}
```

## const assertions

- no literal types in that expression should be widened (e.g. no going from "hello" to string)
- object literals get readonly properties
- array literals become readonly tuples


## Nodejs
### Running TypeScript Natively

- https://nodejs.org/learn/typescript/run-natively
- https://nodejs.org/docs/latest-v22.x/api/typescript.html#typescript-features

## React

- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)

## Tips

[use interface until you need to use features from type.](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)