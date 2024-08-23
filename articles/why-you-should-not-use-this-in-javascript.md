# Why You Should Not Use `this` in JavaScript

**tl;dr** When possible, avoid using `this` in JavaScript to reduce complexity and potential bugs.

The `this` keyword in JavaScript is quite unique compared to other programming languages, making it a common source of confusion and confusion often leads to bugs. 
In general, `this` always refers to the object that called the method. However, this object might not always be what you expect.

## What is `this` in JavaScript?

If you write a regular function and call it, `this` refers to the `window` (in a browser environment) or `global` (in a Node.js environment) object. 
Initially, this might seem confusing, but remember: when you add a value or function at the top level, it is added to the global object.

```javascript
function logThis() {
	console.log(this) // window
}

logThis()
```

The same code but more specifically: `window` calls the method, so `this` equals `window`.

```javascript
function logThis() {
	console.log(this) // window
}

window.logThis()
```

In the next example, we have a method on an object. 
This time, we are calling `logThis` from `myObj`, which is why `this` equals `myObj`.

```javascript
const myObj = {
	logThis() {
		console.log(this) // myObj
	}
}

myObj.logThis()
```

Here is a very similar example with the same object and the same method. The only difference is that the method is deconstructed from the object. 
This time, when calling `logThis`, `this` is not `myObj` but `window`.

```javascript
const myObj = {
	logThis() {
		console.log(this) // window
	}
}

const { logThis } = myObj
logThis()
```

To be more specific: because `logThis` is again attached to the `window` object, which is why `this` equals `window`.

```javascript
const myObj = {
	logThis() {
		console.log(this) // window
	}
}

const { logThis } = myObj
window.logThis()
```

## Special Functions for Setting `this`

JavaScript provides special `Function.prototype` methods like `bind`, `call`, and `apply` to set the value of `this'. 
However, these methods add more complexity to your code and can be even more confusing for someone unfamiliar with these concepts.

You may recall the infamous need for `bind` in React class components:

```javascript
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isToggleOn: true };

    // This binding is necessary to make `this` work in the callback
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}
```

## Should You Use `this`?

If you're not using a framework or library that relies on `this`, you should avoid using it. 
The likelihood of a developer misunderstanding the value of `this` and causing a bug is much higher than any potential benefits provided by using it.

On the other hand, it's entirely possible to write JavaScript code without using `this`. The React team demonstrated this effectively with the introduction of hooks, which eliminate the need for class components.
