# Good changes, bad changes

Code changes all the time, and of course, it does. The main purpose of software is that it is flexible.
But considering that we change software so often, I believe that we think too little about this topic.
Because not every change is the same. So, let us break this down.

In fact, there are **five** different ways you can change your code. Some are good, and some are bad.
We can tell whether a change is good or bad by whether it leads to a breaking change or not.

**good changes**
- require less
- provide more
- improve it

**bad changes**
- require more
- provide less

For the following examples, we will look at the signature of a function written in TypeScript. But this is true for every kind of interface, whether it is a REST API or a CLI tool.

Our function simply takes two parameters of type string and returns an object with the properties `x` and `y`, which are also strings.

```typescript
type MyFn = (a: string, b: string) => ({
  x: string,
  y: string,
}) 
```

## Provide more

Probably the simplest way to change your function in a good way is to provide more in its result.
In our example, the function now returns a new field `z`. This is a good change because the consumers of this function are not affected by this change.
They can access the new field z whenever they want.

```typescript
type MyFn = (a: string, b?: string) => ({
  x: string,
  y: string,
  z: number, // 👍🏻
}) 
```

## Require less

Another thing you can do is require less to perform your computation. In the case of our example this could mean that we make the second param optional.
This way everything works as before and the consumer could refactor the code whenever they want.

❗Note that we only make the parameter optional. 
If we removed it from the signature, every consumer of this function would get a TypeScript compile error because they would be providing an unknown parameter to the function -> breaking change.

```typescript
type MyFn = (a: string, b?: string /*👍🏻*/) => ({
  x: string,
  y: string,
}) 
```

## Provide less

A bad way of changing your code is changing the output of your function.
It does not matter whether you're removing a field, renaming it, or changing its type.
The consumers of the function will try to access something that will not behave as expected or will not be there anymore.
This is a breaking change that will lead to a compile error at best or a runtime error at worst.
Either way, you should **never** do that!

```typescript
type MyFn = (a: string, b: string) => ({
  x: string,
  // y: string 👎🏻
}) 
```
```typescript
type MyFn = (a: string, b: string) => ({
  x: string,
  w: string, // 👎🏻
}) 
```
```typescript
type MyFn = (a: string, b: string) => ({
  x: string,
  y: number, // 👎🏻
}) 
```

## Require more

Another bad way of changing your code is requiring more to perform your computation.
If we introduced another parameter to our function, all consumers would have to react to it immediately -> Breaking change!

```typescript
type MyFn = (a: string, b: string, c: boolean /*👎🏻*/) => ({
  x: string,
  y: string,
}) 
```

## Improve it

The last way you can change your code is just by improving it without changing its signature.
This could mean that the code now runs free of errors or performs better. You can and should always do that.

## TypeScript Union Types vs. Optionals

TypeScript union types offer a way to handle optional parameters without introducing breaking changes. Since an optional in TS just means a union of the type you want and `undefiend`.
This is in contrast to languages like Java, where making a parameter optional can lead to breaking changes, even if the change is generally beneficial.

For example the method on this interface has two parameters of type String.

```java
interface MyInterface {
  void myMethod(String a, String b);
}
```

If you make the second param Optional, all consumers of this method would need to pass an Optional instead of a string.
This is a breaking change even if it was well-intentioned.

```java
import java.util.Optional;

interface MyInterface {
  void myMethod(String a, Optional<String> b /*👎🏻*/);
}
```
