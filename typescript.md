## compilerOptions

`noEmit: true`: Do not emit compiler output files such as JavaScript source code, source maps, or declarations.

`noImplicitAny: true`: TypeScript will issue an error whenever it would have inferred `any`.

In some cases where no type annotations are present, TypeScript falls back to a type of `any` for a variable when it cannot infer the type.

`allowImportingTsExtensions: true`: Allows TypeScript files to import each other with a TypeScript-specific extension like `.ts`, `.mts`, or `.tsx`. This flag is only allowed when `--noEmit` or `--emitDeclarationOnly` is enabled.

`target: esnext`: Tells TypeScript to compile to the latest JavaScript features.

`module: nodenext`: Tells TypeScript to use Node.js's native module resolution for ESM (ES Modules).

`esModuleInterop: true`: Allows interoperability between CommonJS and ES Modules.

- Without it, importing a CommonJS module requires `import * as express from 'express'`.
- With it, you can simply write `import express from 'express'`.

## `unknown`

`unknown` is the type-safe counterpart of `any`. Anything is assignable to `unknown`, but `unknown` isn't assignable to anything but itself and `any` without a **type assertion(类型断言)** or a control-flow-based **narrowing(类型收窄)**. Likewise, no operations are permitted on an `unknown` value without first asserting or narrowing it to a more specific type.

## [Narrowing](https://www.typescriptlang.org/docs/handbook/2/narrowing.html)

**Discriminated unions**

The technique of narrowing a union type based on a literal attribute value is called a discriminated union.

```ts
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

**Exhaustiveness checking**

See [Exhaustiveness checking](https://www.typescriptlang.org/docs/handbook/2/narrowing.html#exhaustiveness-checking) in the handbook.

**Type predicates**

```ts
const isString = (text: unknown): text is string => {
  return typeof text === 'string' || text instanceof String;
};
```

**The `in` operator**

```ts
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

```ts
try {
  // ...
} catch (error: unknown) {
  let errorMessage = 'Something went wrong: ';
  // here we cannot use error.message
  if (error instanceof Error) {
    // the type is narrowed, so we can refer to error.message
    errorMessage += error.message;
  }
  console.log(errorMessage);
}
```

## const assertions

- No literal types in the expression are widened (e.g. no going from `"hello"` to `string`).
- Object literals get `readonly` properties.
- Array literals become `readonly` tuples.

## `Omit` with unions

An important point about unions: when you use `Omit` to exclude a property from a union, it may work in an unexpected way. Suppose we want to remove the `id` from each `Entry`. We might be tempted to write:

```ts
export type Entry =
  | HospitalEntry
  | OccupationalHealthcareEntry
  | HealthCheckEntry;

Omit<Entry, 'id'>
```

However, this doesn't work as we might expect. The resulting type would only contain the properties common to all members of the union, but not the ones they don't share. A workaround is to define a special `Omit`-like type that distributes over the union:

```ts
// Define a distributive Omit for unions
type UnionOmit<T, K extends string | number | symbol> = T extends unknown ? Omit<T, K> : never;
// Define Entry without the 'id' property
type EntryWithoutId = UnionOmit<Entry, 'id'>;
```

See [microsoft/TypeScript#42680](https://github.com/microsoft/TypeScript/issues/42680).

## Node.js

### Running TypeScript Natively

- https://nodejs.org/learn/typescript/run-natively
- https://nodejs.org/docs/latest-v22.x/api/typescript.html#typescript-features

## React

- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/)

## Tips

- TypeScript error messages put the most useful information at the end of the message.
- [Use `interface` until you need features from `type`.](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#differences-between-type-aliases-and-interfaces)
- [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) — community-maintained type definitions for JavaScript libraries.