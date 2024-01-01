# Why you should not use null in JavaScript

**tl;dr** Just stop worrying and use `undefined` whenever possible.

JavaScript has two [bottom types](https://en.wikipedia.org/wiki/Bottom_type): `null` and `undefined`. But there is no rule or whatsoever within the language that tells us when to use what.
So I advocate not using `null` at all. Because even if `null` and `undefined` seem to be similar they behave slightly differently on runtime.

And [Douglas Crockford](https://en.wikipedia.org/wiki/Douglas_Crockford) once said in [this lecture](https://youtu.be/DePE0ffiMf4?si=hAc4qHedn1GSRZVa):

> Whenever you have things (in programming) which are almost the same but different, that’s a source of confusion and confusion leads to bugs.

So here are some reasons why you shoud stop using `null`

## null is buggy

The root problem with `null` is, that it is buggy. It is one of its most infamous bugs in JavaScript.

```javascript
typeof null // returns "object"
```

Checking for the `typeof null` maybe happens rarely or never in your code but a more realistic danger lies here:

```javascript
if (typeof unkownValue === "object") {
    fnThatCannotHandleNull(unkownValue)
}
```

This type-guard will not prevent `null` from getting passed in your function that can not hande it and this could lead to a runtime error.

## Consciously decision for null

Some people argue "undefined is used if a value is missing but null is a developers way to say **this value is empty on purpose**".
And I understand that point, but it has some issues.

1. Not every developer interprets it that way. And since there is no linting rule to enforce this philosophy you will eventually end up with mixed usage of `null` across your codebase **unless** you vigilantly monitor it in pull requests.
2. If this distinction is so important, why is JavaScript the only language to do so?
3. It does not really change the behavior of your programm. `null` and `undefined` are both used for the same reason, to indicate that a value is not set.
   Or to put it another way: I never encountered code that would behave differently weither the value is `null` or `undefined`.

```javascript
// have you ever saw or wrote code like this?
myFn = (param) => {
    if (param === null) {
        doX()
    }

    if (param === undefined) {
        doY()
    }

    doZ()
}
```


## We cannot get rid of undefined

I like to quote [Douglas Crockford](https://en.wikipedia.org/wiki/Douglas_Crockford) again, who once [said about the discussion "tabs vs spaces"](https://www.youtube.com/watch?v=En8Ubs2k1O8&t=172):

> The argument is not which way is better (tabs or spaces). The Argument is which one we can get rid of. And we can't get rid of space (in programming), so the one that has to go is tab.

In JavaScript `undefined` is the default bottom type. So there is no realistic way to program without it.

```javascript
let myVal // myVal = undefined

const myObj = {
    count: 1,
}

myObj.size // returns undefined
```

So, when faced with the choice between `undefined`, which works well and is the default in most cases, and `null`, which arguably has issues, we should get rid of `null`.

## But of course...

...if you are using a library or JS core function that expects `null`, you should use it.
