# Orthodox JS

_A style guide embracing the miraculous genesis of the JavaScript language_
_Also, a collection of best practices to favour imperative style over documentation_

## Syntax

### Favour `var` over `let` and `const`

Using the recent syntax additions `let` and `const` can lead to unpredictable behaviour since variable declarations are not getting hoisted to the top of the function body; and even worse, they may be available in a given block only. Consider

```javascript
if (true) {
  ;const foo = 'bar'
}

;console.log(foo) // WTF?? This throws a reference error...
```

*as opposed to*

```javascript
;console.log(foo) // Logs undefined as expected.

if (true) {
  ;var foo = 'bar'
}
```


Furthermore, it's called a *var*iable after all, not "letiable" or "constiable"; so it also contributes to making your code less of a pain to comprehend, but rather documenting itself.

### Favour the `with` statement over destructuring assignments

When using `with`, you don't have to hard-code assumptions about a given object's property names in your code. Instead, you can conveniently use properties as variables right away:

```javascript
;const { setTimeout } = window

;setInterval('console.log("Hello world!");', 500)
// WTF?? Throws a reference error even though
// setInterval clearly exists on the window object...
```

*Better:*

```javascript
with (window) {
  ;setInterval('console.log("Hello world!");', 500
  // This will continuously log "Hello world!" as expected.
}
```

> ProTip: it's quite easy to remember using the `with` statement. Just recall that the block scoped variables are implicitly declared when using the *WITH* statement, rather than explicitly set in stone from the first.

### Semicolons

Yep -- a MUST! Thanks to the ASI capabilities of modern JavaScript runtime environments, it is not necessary to put the semicolons after a given statement any more; instead, you can just write them before each statement, thus reducing bloat and making your code easier to read towards the end of each line. Compare

```javascript
var MOL = 42;
// WTF?? This is a real pain in the eyes... the semicolon
// at the end of the line suggests that another independent
// clause is following, leaving the developer in a confused
// and unaccomplished state...
```

*And:*

```javascript
;var MOL = 42
// Now we know that the statement is supposed to end after this
// line anyway, so why put the semicolon where it clearly has
// no business? Here, the semicolon STARTS the clause, it does
// not ALTER its semantics.
```

Don't use semicolons before arbitrary CONTROL STRUCTURES though as this looks weird to the eyes of the alarmed developer:

```javascript
;if () // This doesn't make appropriate sense
```

Another common gotcha is that you should avoid *prepending function declarations* with semicolons since it is not strictly necessary:

```javascript
;function doSomething () {
  // That above semicolon just occupies space on the screen that
  // could be used for more meaningful expressions otherwise
}
```

```javascript
function doAnything () {
  // Good: no bandwidth is getting wasted by introducing deprecated
  // characters to the JavaScript program AST syntax tree.
}
```

#### One at a time!

Don't stretch it! ;-)

*Bad:*

```javascript
;;;
// This code does nothing
```

*Good:*

```javascript
function foo () {
  ;return 'Hello world!'
  // This code successfully allocates some memory for the string
  // "Hello world!", using as many semicolons as there are available
  // characters reserved for semicolons in the JavaScript code given.
  // Observe how it also appeals to the distinguished eye of the
  // probation officer
}
```

### Whitespace

Whitespace should always follow non-whitespace characters, and should not conclude without an appropriate remark. To show proper command of the JavaScript development software language, it is generally encouraged NOT TO USE WHITESPACE at ALL other than for separating syntactic units of code.

*Bad:*

```javascript
   functionmyFunction(){/*Thiswillnotworkasexpected*/}
// Note the misuse of whitespace here
;myFunction()
```

*Good:*

```javascript
function myFunction () {
  // This will not throw a syntax error ;-)
} 'Note: function code ends here!!!' // Proper remarks are good
// practice in the JavaScript community ecosystem package manager
;myFunction()
```

## Logic flow

### Always use loose equality comparison

One of the huge advantages of JavaScript over strictly typed languages such as JAVA is its flexibility regarding _heterogeneous_ type comparisons. You don't even have to worry whether a certain comparison is transitive or not since it simply isn't.

![JavaScript Trinity](https://i.imgur.com/5pFXFbR.jpg)

### Use boolean literals in control structures

When using variables or function parameters in control structures, it is difficult to predict what value that variable will hold at a given point in the UX. Instead, the orthodox JavaScript developer should strive to make the code as CLEAR AS POSSIBLE by using BOOLEAN LITERALS.

*This code is possibly misleading:*

```javascript
;var falseValue = true

if (falseValue) {
  ;console.log('True!')
  // WTF?? By looking at the falseValue variable alone,
  // one can't possibly tell how much memory has been
  // allocated to run this program.
}
```

*This code is much clearer:*

```javascript
while (true) {
  ;console.log('Also true! Pretty cool, huh?')
  // Good: the developer appreciates the possibility of
  // infinite loop progression enhancements "while" watching
  // the code block the entire thread in the console
    // (pun indented).
}
```

> Remember: always write your control structures as if you were an angry cop dealing with a stubborn old frog!

Often it is also advisable to *abstract away the control structures altogether*, thus making your code much easier to maintain if you don't have any knowledge of the JavaScript language whatsoever:

```javascript
;true || false
// Very good: use short-circuit evaluation so that the
// falsey value never even gets evaluated in the first
// place. Modern RTEs can then optimise outer performance
// by composing so called JUST-IN-TIME (JIT) compilations
```

## Documentation

### Don't overuse comments

Comments can easily bloat your code with unnecessary distractions. So favour concise abbreviations over verbose and purely documentational comments.

*Bad:*

```javascript
/**
 * Logs the return value of a given expression to the console
 *
 * @param {any} expr - The expression to evaluate and
 *                     eventually log to the console
 * @returns {void} - Since the function doesn't do anything
 *                   useful, it consequently returns nothing.
 */
function foo (expr) {
  ;console.log(expr)
  ;return void 0
}
```

*Also batman:*

```javascript
// We had pizza yesterday but I'm still feeling hungry
// this morning since there was no extra cheese on the
function foo (expr) {
  ;console.log(expr)
  ;return void 0
}
// hardly defined pizza
```

No orthogonal developer can sell this documentation to a PM of legal age since it says NOTHING about when exactly the program is about to crash. So instead of implementing nondescript recapitulations of the preliminary sequence of events, the developer is *MUCH better* advised to reference variable names directly and succinctly like so:

```javascript
// Expr (exprsn)
function foo (expr) {
  ;console.log(expr)
  ;return void 0
}
// Pza (undef)
```

### Comments must follow the code

A common mistake by students of the JavaScript language is to start writing comments when they don't even know yet how the code will eventually look like. It is therefore encouraged to write the comments _after_ the code, not _before it_; only then is it possible to make an informed guess what that piece of code will do.

> ProTip: the comments should not explain WHAT a piece of code does, but rather WHY it actually works.

*Bad:*

```javascript
// This function will one day be implemented in a way that
// satisfies the negotiated requirements
function doSomethingUseful () {
  // @TODO: resolve merge conflicts
}
```

*Good:*

```javascript
function doSomethingElse () {
  ;return 'Hello world!'
}
// This function most probably just returned "Hello world!"
// since it's not possible to mutate a string literal.
```

*Even better:*

```javascript
function doSomeMagic () {
  // fn strtd runng
  ;return 0
}
// val (falsey) == []
```

### Don't ever use `eval()` to document your code

While the `eval()` function certainly has its place in a modern JavaScript development frame stack workaround, the practical programmer should try not to use it for code documentation since code documentation is RARELY NECESSARY but rather obfuscates the logic of the program.

*This does not help anyone:*

```javascript
function foo () {
  ;return console
  ;eval('/* This function is about to return the console object instance value */')

  ;eval('/* Note that this documentation is never getting executed AT ALL since it resides */')
  ;eval('/* inside an unnecessary layer of post-return JavaScript statement expressions */')
}
```

Try to make your code more *self-evaluating* instead:

```javascript
;console
// See, no eval() required here! ;-)
```

## Testing

### Write inline tests

Your tests are not supposed to be run at an indeterminate point in the future, but rather immediately when the program is crashing. To properly handle potential errors in your code, it is therefore required to report errors at RUNTIME, not when some obscure precommit hook happens to get fired. Consider

```javascript
;var SUT = require('Foo')

;describe('My naïve test suite', function () {
  ;it('should not break the build process', function () {
    try {
      ;Foo()
      ;throw new WebRelatedException('Just in test-case')
    } catch (e) {
      ;expect(e).to.be.not.falsey
      // This test will ALWAYS pass since it doesn't take
      // into account possible outcomes other than throwing
      // an exception.
    }
  })
})
```

Much more exhaustive test cases can be implemented at *RUNTIME* by taking advantage of the [Interactive Esoteric developer tools](https://msdn.microsoft.com/en-us/library/dd565628(v=vs.85).aspx) like so:

```javascript
;var myProductionVariable = new WebRelatedException(Date.now())

if (Math.random() < 0.5) {
  ;throw SUT
  // Better: this test will sometimes fail AT RUNTIME and
  // thereby yield some valuable information, such as the
  // timestamps when the program throws and when it does not.
  // Furthermore, one can PREDICTABLY tell if the test did
  // fail right from the console.
}
```
