# TypeScript Pro-Tips

## Table of Content

- [Type Tranformations](#type-transformations-↩)
  1. [Utility type "ReturnType" and "typeof"](#utility-type-returntype-and-typeof-↩)
  2. [Utility type "Parameters"](#utility-type-parameters-↩)
  3. [Utility type "Awaited"](#utility-type-awaited-↩)
  4. [Utility type "keyof"](#utility-type-keyof-↩)
  5. [Utility types for unions and discriminated unions](#utility-types-for-unions-and-discriminated-unions-↩)
  6. [Values of discriminator of a discriminated union](#values-of-discriminator-of-a-discriminated-union-↩)
  7. [Use 'as const' to grab onto literal types](#use-as-const-to-grab-onto-literal-types-↩)
  8. [Make unions out of array values](#make-unions-out-of-array-values-↩)
  9. [Template literal with strings](#template-literal-with-strings-↩)
  10. [Split strings with utilities from ts-toolbelt library](#split-strings-with-utilities-from-ts-toolbelt-library-↩)
  11. [Using "Record" instead of \{ \[key in SomeUnion\]: whatever\}](#using-record-instead-of--key-in-someunion-whatever-↩)
  12. [Using 'Type Arguments'](#using-type-arguments-↩)
  13. [Non-empty array](#non-empty-array-↩)
  14. [Conditions](#conditions-↩)
  15. [Using 'infer' with generic arguments of other types](#using-infer-with-generic-arguments-of-other-types-↩)
  16. [Split strings using 'infer' and template literals](#split-strings-using-infer-and-template-literals-↩)
  17. [Use a generic to avoid exact match problems with 'extends' of a union](#use-a-generic-to-avoid-exact-match-problems-with-extends-of-a-union-↩)
  18. [Map over a union with the 'in' keyword](#map-over-a-union-with-the-in-keyword-↩)
  19. [Map with string discrimination](#map-with-string-discrimination-↩)
  20. [Map keys of an object with the 'as' keyword](#map-keys-of-an-object-with-the-as-keyword-↩)
  21. [Convert an object into a union](#convert-an-object-into-a-union-↩)
- [Generics](#generics-↩)
  1. [Generics for callable objects \(functions\)](#generics-for-callable-objects-functions-↩)
  2. [Generics for Classes](#generics-for-classes-↩)
  3. [Generics can be used at various 'levels'](#generics-can-be-used-at-various-levels-↩)
  4. [Adding a constraint narrows down the type](#adding-a-constraint-narrows-down-the-type-↩)
  5. [Better inference when using objects and arrays as arguments](#better-inference-when-using-objects-and-arrays-as-arguments-↩)
  6. [Capturing object keys at various 'levels'](#capturing-object-keys-at-various-levels-↩)
  7. [Using conditional types in return types](#using-conditional-types-in-return-types-↩)
  8. [Function can capture their OWN arguments](#function-can-capture-their-own-arguments-↩)
  9. [Infer specific types by making sure there is a generic for it](#infer-specific-types-by-making-sure-there-is-a-generic-for-it-↩)
  10. [Get inference by making a factory function that applies types to its child functions](#get-inference-by-making-a-factory-function-that-applies-types-to-its-child-functions-↩)
  11. [When type arguments are passed, additional generics cannot be used \(will say arguments are missing\)](#when-type-arguments-are-passed-additional-generics-cannot-be-used-will-say-arguments-are-missing-↩)
  12. [Function Overloads](#function-overloads-↩)
  13. [Dynamic Args](#dynamic-args-↩)
  14. [Fake typing required error messages](#fake-typing-required-error-messages-↩)
  15. [Overloads with function composition for variable number of 'function arguments'](#overloads-with-function-composition-for-variable-number-of-function-arguments-↩)
- [Advanced Patterns](#advanced-patterns-↩)
  1. [Branded Types](#branded-types-↩)
  2. [Branded Index Signatures](#branded-index-signatures-↩)
  3. [Global Scope](#global-scope-↩)
  4. [Merging interfaces in the Global Scope](#merging-interfaces-in-the-global-scope-↩)
  5. [Accessing other namespaces](#accessing-other-namespaces-↩)
  6. [Colocating global interfaces](#colocating-global-interfaces-↩)
  7. [Type Predicates](#type-predicates-↩)
  8. [Assertion Function](#assertion-function-↩)
  9. [Using 'this' in Class Predicates](#using-this-in-class-predicates-↩)
  10. [Builder Pattern](#builder-pattern-↩)
  11. [Override library declarations with external "d.ts" files](#override-library-declarations-with-external-d.ts-files-↩)
  12. ['const' Type Parameter used instead of 'as const'](#const-type-parameter-used-instead-of-as-const-↩)
  13. [Use 'Identity functions' with strict typing](#use-identity-functions-with-strict-typing-↩)
  14. [Reverse map types](#reverse-map-types-↩)
  15. [Merge with global objects](#merge-with-global-objects-↩)
  16. [Narrow down a returned value type with 'as' & 'Extract'](#narrow-down-a-returned-value-type-with-as--extract-↩)

# Type Transformations [↩](#)

## Utility type "ReturnType" and "typeof" [↩](#)

When using utility types like "ReturnType", they cannot be used directly on a function as you may think it should:

`type MyType = ReturnType<myFunc>` They require types. so use `typeof` along with it:

`type MyType = ReturnType<typeof myFunc>`

## Utility type "Parameters" [↩](#)

"Parameters" return a tuple of the target function's parameters: `Parameters<typeof myFunc>`

You can adress a specific one with numeric index: `Parameters<typeof myFunc>[1]`

## Utility type "Awaited" [↩](#)

Use this utility type to extract what a promise would actually return. `Awaited<MyPromise>`

## Utility type "keyof" [↩](#)

This utility returns a **union** of all keys of an object type. e.g.: `keyof typeof myObject`

## Utility types for unions and discriminated unions [↩](#)

Use "Exclude" and "Extract" with unions. To choose in discriminated unions, only the discriminator is required as its just a match with 'extends' internally. (not exact match)
`Extract<MyType, {type: "value"}>` where "type" was the discriminating part.

## Values of discriminator of a discriminated union [↩](#)

If you access the discriminator of a discriminated union as such: `MyType["type"]` (where "type" was the discriminator) you will get a union of all its possible values.

## Use 'as const' to grab onto literal types [↩](#)

Using 'typeof' to get to types work fine, but add an 'as const' on the actual object to grab onto its literal types. ("myString" instead of string. 33 instead of number, etc.)

## Make unions out of array values [↩](#)

Access ALL array values with a [number] index, e.g.: `typeof myArray[number]` or specific values with unions of numbers: `typeof myArray[0 | 1 | 99]`

## Template literal with strings [↩](#)

You can specifiy string literals with variable TYPE parts as such: `` `abc${string}xyz` ``

This is useful when trying to match strings in unions. e.g. to catch those with a semicolon:

`` Extract<MyType, `${string}:${string}`>  ``

If union types are used in template literals, unions of all possibilities will be made:

`` type AllSandwiches = `${BreadType} sandwich with ${Filling}`; ``

## Split strings with utilities from ts-toolbelt library [↩](#)

E.g.: a union of folder names from a path `type SplitPath = S.Split<Path, "/"> `

Note: Some more common utilities like Uppercase, etc. are available without external libs.

## Using "Record" instead of { [key in SomeUnion]: whatever} [↩](#)

You can use `Record< MyUnion, any >` instead of `{ [key in MyUnion]: any }`

## Using 'Type Arguments' [↩](#)

Type 'arguments' (or 'generics') can be used after the type name. Set defaults like in javascript:

`type MyType<Tabc, Txyz = undefined> = { whatever }`

Use 'extends' to constrain types. Eg. restraining to a function:

`type MyType<Tabc extends () => any> = {}`

## Non-empty array [↩](#)

To specify a non-empty array of some type, put at least one in it, and then use the spread operator on a possibly empty array of that same type:

`type MyArrayType<T> = [T, ...Array<T>];`

## Conditions [↩](#)

Conditions are made with the use of the 'extends' keyword and the ternary operator '?'.

`T extends "test" ? "ok" : "error"`

Note: Use `never` to prevent output of a particular choice in a condition.

`T extends "test" ? "ok" : never`

## Using 'infer' with generic arguments of other types [↩](#)

Note: You can use 'infer' with generics. E.g. get the fourth generic of another type:

`type TGETFOURTH<T> = T extends TBIG<any, any, any, infer TNEW, any> ? TNEW : never;`

## Split strings using 'infer' and template literals [↩](#)

Example of getting the last part of a string when separated by a space:

`` type LASTNAME<T> = T extends `${infer TFIRST} ${infer TLAST}` ? TLAST : never``

## Use a generic to avoid exact match problems with 'extends' of a union [↩](#)

When you pull a union into a generic, the members of the union distribute across it.

This will always be 'never'

```typescript
type ABC = "a" | "b" | "c";
type AorB = ABC extends "a" | "b" ? ABC : never;
```

Using a generic in between, this will work:

```typescript
type ABC = "a" | "b" | "c";
type GetAorB<T> = T extends "a" | "b" ? T : never;
type AorB = GetAorB<ABC>;
```

## Map over a union with the 'in' keyword [↩](#)

The 'in' keyword also creates the internal variable, available for re-use. (Shown here as a value on the right side of the semicolumn)

`type T = {[key in SomeUnion]: key}`

## Map with string discrimination [↩](#)

Use string template literals and the 'Extract' utility to choose particular string matches:

```typescript
type IdKeys<T> = {
  [key in Extract<keyof T, `${string}${"Id" | "id"}${string}`>]: string;
};
```

## Map keys of an object with the 'as' keyword [↩](#)

It's possible to specify particular subparts of a type, or use it in any other way, when mapping, **with the as keyword**:

`type MyType = { [key in SomeUnion as key["digging_in_key"]] : someType }`

## Convert an object into a union [↩](#)

Easy! Just index it with its keys!

`type UnionType = { [key in keyof OtherObjectType] }[ keyof OtherObjectType ]`

# Generics [↩](#)

## Generics for callable objects (functions) [↩](#)

Generics for callable objects are placed before the parameters:

`const myIdentity = <T>(t: T): T => {return t;};`

Restrictions are applicable also with 'extends':

`const myIdentityStringOnly = <T extends string>(t: T) => t;`

## Generics for Classes [↩](#)

Generics used for a Class is placed after its name. E.g.: `Class MyClass<T> {...}`

## Generics can be used at various 'levels' [↩](#)

It's often possible to capture and use generics in multiple ways.

First example captures params and return types individually:

`<TParams extends any[], TReturn>(func: (...args: TParams) => TReturn) => ():{}`

While instead, the same can be done by capturing only the func and using the 'Parameters' and 'ReturnType' utilities:

`<TFunc extends (...args: any[]) => any>(func: TFunc) =>`

along with `Parameters<TFunc>` and `ReturnType<TFunc>` .

## Adding a constraint narrows down the type [↩](#)

A generic like `<T1 extends string>` instead of `<T1>` will capture the literal value instead of leaving it as 'string'.

## Better inference when using objects and arrays as arguments [↩](#)

To get narrower and literal typing, use a generic on the members of the object instead of on the object itself:

Use `<T extends string>(obj: {input: T })` instead of `<T extends {input: string }>(obj: T)`

Same for arrays:

Use `<TStatus extends string>(statuses: TStatus[])` instead of `<TStatuses extends string[]>(statuses: TStatuses)`

## Capturing object keys at various 'levels' [↩](#)

Similar to to the 'TParams/TReturn or TFunc' example above, it's possible to capture object keys in two ways:

`<TK extends string>(classes: Record<TK, string>) => ... ` Gives the keys in TK.

While `<TC extends Record<string, string>>(classes: TC) => ...` Has the keys in keyof TC

## Using conditional types in return types [↩](#)

When using conditional types in return types, TypeScript isn't smart enough to match the runtime code to the type level code. **Use the 'as' keyword to specify the return type.**

TypeScript isn't clever enough on its own to map a conditional type in the return of a function.

## Function can capture their OWN arguments [↩](#)

You might be getting bad inference because your generics are in the wrong slots.

Use this:

```typescript
const curryFunction =
  <T>(t: T) =>
  <U>(u: U) =>
  <V>(v: V) => {
    return { t, u, v };
  };
```

instead of

```typescript
const curryFunction =
  <T, U, V>(t: T) =>
  (u: U) =>
  (v: V) => {
    return { t, u, v };
  };
```

## Infer specific types by making sure there is a generic for it [↩](#)

To get specific with a variable, instead of specifying indirectly

`<TO>(obj: TO, key: keyof TO) => { ... }`

use a direct generic:

`<TO, TK extends keyof TO>(obj: TO, key: TK) => { ... }`

## Get inference by making a factory function that applies types to its child functions [↩](#)

Because generics are tied to function calls, it's useful to make a factory function that just returns a function that was typed in the factory once, instead of having to pass type arguments on the child function itself every time.

## When type arguments are passed, additional generics cannot be used (will say arguments are missing) [↩](#)

The solution is to refactor the function to be a 'higher-order' function by making it return a child function, which will itself have its individual generics, instead of trying to bundle them with the type argument in the original function.

So, instead of

```typescript
export const makeSelectors = <
  TSource,
  TSelectors extends Record<string, (source: TSource) => any> = {}
>( ...
```

Use a function that returns a function, and call it upon usage

```typescript
export const makeSelectors =
  <TSource>() =>
  <TSelectors extends Record<string, (source: TSource) => any>>(

```

And we call it with `const selectors = makeSelectors<Source>()({ ... ` Instead of `const selectors = makeSelectors<Source>({ ... ` (_notice the additional () parenthesis_)

## Function Overloads [↩](#)

Function overload require the 'function' keyword. ( no const myFunc = () => {} ...)

Only the overloaded signatures will be exposed. The typing of the implementation (last below the overloads) is NOT exposed to the rest of the code.

Function overloads are **not useful** if the function **returns the same type** for different possible parameters. Just use a union in the parameters types.

## Dynamic Args [↩](#)

Use a condition with undefined to check for additional arguments for '...args' and provide an empty array if none are present.

```typescript
const send = <TKey extends keyof Events>(event: TKey,
    ...args: Events[TKey] extends undefined ? [] : [Events[TKey]]
) => { ... }
```

## Fake typing required error messages [↩](#)

You can 'fake' a typing error message by setting a **default** to the error message that you want to show the user. So if no type parameters are passed, the default string error message you've setup as default will appear on hover.

## Overloads with function composition for variable number of 'function arguments' [↩](#)

Sometimes it's necessary to overload functions with incremental number of generics to type the inputs, and return values:

```typescript
export function compose<T1, T2>(func: (t1: T1) => T2): (t1: T1) => T2);
export function compose<T1, T2, T3>(
  func1: (t1: T1) => T2, func2: (t2: T2) => T3
): (t1: T1) => T3
... and so on with T4, T5, etc...
```

# Advanced Patterns [↩](#)

## Branded Types [↩](#)

Branded types are types with an (hidden) additional attributes that just serves to distinguish it from its base type, effectively 'branding' it to a specific name, usually used to **assert** its structure/type.

They are created like so (_the second parameter is just the hidden token used to distinguish_):

```typescript
import { Brand } from "../helpers/Brand";
type Password = Brand<string, "Password">;
type Email = Brand<string, "Email">;
```

And are used to assert their validity after checking like so:

```typescript
return {
  email: values.email as Email,
  password: values.password as Password,
};
```

## Branded Index Signatures [↩](#)

An object can have different types of index, returning different types of values when indexing with specific types. So instead of this:

```typescript
const db: { [key: string]: Post | User };
```

Use this

```typescript
const db: {
  [postId: PostId]: Post;
  [userId: UserId]: User;
};
```

## Global Scope [↩](#)

Global objects are usually to be avoided when constructing a program, but if needed they should be added like so with 'declare global' :

```typescript
declare global {
  function myFunc(): boolean;
  var myVar: number;
}
```

> Note: _Variables must use **var** instead of let or const when adding to the global scope._

## Merging interfaces in the Global Scope [↩](#)

Redeclaring interfaces **merges** them, so using this property of interfaces in the Global Scope can be useful.

For example, a new function is added to the Global Window object

```typescript
declare global {
  interface Window {
    myWindowFunc: (p: number) => string;
  }
}

window.myWindowFunc = (p) => p.toString();
```

## Accessing other namespaces [↩](#)

Standard web libraries live on the global scope, and are refered by the library name as their namespace.

E.g. to access their members you have to prefix the namespace as such:

`const processEnvShortcut: NodeJS.ProcessEnv`

To override or declare new interfaces in them you use their **namespace** inside a **declare global** :

```typescript
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      MY_SOLUTION_ENV_VAR: string;
    }
  }
}
```

## Colocating global interfaces [↩](#)

It's possible to construct interfaces over multiple files. This can be useful when only a subset of the files will be used, or to add on to existing libraries. For example the MyInterface type is defined over multiple files, such as this in one file:

```typescript
declare global {
  interface MyInterface {
    LOG_IN: { username: string; password: string };
  }
}
```

And in another file, this content will add two more members:

```typescript
declare global {
  interface MyInterface {
    LOG_OUT: {};
    UPDATE_USERNAME: { username: string };
  }
}
```

## Type Predicates [↩](#)

A type predicate is used to replace the return type of a function with an assertion with the **'is'** keyword.

> Predicates are useful with the **'filter'** array method

This forces that function to return a boolean.

```typescript
function isNumber(p: string | number | boolean): p is number {
  return typeof p === "number";
}
```

## Assertion Function [↩](#)

Those are functions that throw an error if something is not correct.

> Make sure to use **function** and not **const** for assertion functions!

```typescript
function assertIsNumber(p: string | number | boolean): asserts p is number {
  if (typeof p !== "number") {
    throw new Error("Not a number!");
  }
}
```

They don't have to return anything and they can be used to replace 'if' checks in your program. The asserted typing will be considered true and correct past that execution point where this function is called.

```typescript
a = possibleNumber();
assertIsNumber(a);
// Here the a variable is considered a number past this point.
```

## Using 'this' in Class Predicates [↩](#)

For example, in a class, to assert a class member is not undefined (or assert some other type), it can be used this way, merging the actual object with another that adds, or asserts 'classMember' as a string:

```typescript
isInvalid(): this is this & { classMember: string } {...}
```

> **this is this** is not a typo, the second **this** references the class definition itself. (instead of **MyClass**)

## Builder Pattern [↩](#)

Setter methods in a class using the Builder Pattern return the class itself, to help chain-calling those setter methods and _build something_.

The **return type of those setters** can be typed such that correct typing reflects the changes on the object of those calls to setter methods. E.g. a simple string object 'map' setter:

```typescript
class SMap<T extends Record<string, string> = {}> {
  private map: T;

  constructor() {
    this.map = {} as T;
  }

  get(key: keyof T): string {
    return this.map[key];
  }

  set<K extends string>(key: K, value: string): SMap<T & Record<K, string>> {
    (this.map[key] as any) = value;
    return this;
  }
}
```

## Override library declarations with external "d.ts" files [↩](#)

To override external libraries, declare global might not be enough. Instead, create an external d.ts file.

Doing the following inside your source where the real library is imported will not work, but all d.ts files are scoped as global, and the following will let you override the library definitions if placed in a myfile.d.ts external file.

```typescript
declare module "external-lib" {
  export type SomeType = "foo" | "bar";
  export function myFunction(): MyType;
}
```

## 'const' Type Parameter used instead of 'as const' [↩](#)

The 'const' keyword can also be used before a type argument parameter. For example this small identity function helps **narrowing down** the typing, as if the parameter was passed from an object made with 'as const':

```typescript
export const asConst = <const T>(t: T) => t;
```

## Use 'Identity functions' with strict typing [↩](#)

To enforce types when building structured objects, use an identity function that just returns what it was given, but that checks the typing while doing so. (Make an interface to help along with the identity function, the identity function can simply expect its parameter to be of the interface you've made)

> Note: the compiler will **REMOVE** the identity function and use your value as-is

## Reverse map types [↩](#)

In identity functions, a 'reverse map' type is when a key/index of an object type is reused as a literal elsewhere.

In this example, the parameter 'name' is made a literal string being the index of the item in the object: a keyof the obj.

```typescript
export function identity<T>(obj: {
  [K in keyof T]: (name: K) => void;
}) {
  return obj;
}
```

## Merge with global objects [↩](#)

Redeclaring an interface simply adds to it instead, so you can use it to add to global objects. For example the members of an object are added to the global Window:

```typescript
type A = typeof anObject;

declare global {
  interface Window extends A {}
}
```

## Narrow down a returned value type with 'as' & 'Extract' [↩](#)

If you can narrow down the return of a function to a union of possible choices, adding 'as' to the end (to narrow it down to a single value) along with 'Extract' from that union can give out a single narrowed-down value.

```typescript
const getFruit = <TName extends TFruits[number]["name"]>(name: TName) => {
  return fruits.find((fruit) => fruit.name === name) as Extract<
    TFruits[number],
    {
      name: TName;
    }
  >;
};
```
