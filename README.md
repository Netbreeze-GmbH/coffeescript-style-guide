# CoffeeScript Style Guide

This guide presents a collection of best-practices and coding conventions for the [CoffeeScript][coffeescript] programming language.

This fork is used to establish guidlines for our organisation.

**Note:** This document is living and subject of discussions on retrospectives

## Table of Contents

* [The CoffeeScript Style Guide](#guide)
    * [Code Layout](#code_layout)
        * [Tabs or Spaces?](#tabs_or_spaces)
        * [Maximum Line Length](#maximum_line_length)
        * [Blank Lines](#blank_lines)
        * [Trailing Whitespace](#trailing_whitespace)
        * [Encoding](#encoding)
    * [Module Imports and Layout](#module_imports)
    * [Whitespace in Expressions and Statements](#whitespace)
    * [Comments](#comments)
        * [Block Comments](#block_comments)
        * [Inline Comments](#inline_comments)
    * [Naming Conventions](#naming_conventions)
    * [Objects](#objects)
    * [Functions](#functions)
    * [Strings](#strings)
    * [Conditionals](#conditionals)
    * [Looping and Comprehensions](#looping_and_comprehensions)
    * [Extending Native Objects](#extending_native_objects)
    * [Exceptions](#exceptions)
    * [Annotations](#annotations)
    * [Documentation](#documentation)
    * [Miscellaneous](#miscellaneous)

## Avoid hacky code!

**Avoid hacky code**

<a name="code_layout"/>
## Code layout

<a name="tabs_or_spaces"/>
### Tabs or Spaces?

Use **spaces only**, with **2 spaces** per indentation level. Use a proper editor, which allows you to handle tab-space replacements.

It's usefull when your editor can display spaces.

<a name="maximum_line_length"/>
### Maximum Line Length

Limit all lines to a maximum of **120** characters.

<a name="blank_lines"/>
### Blank Lines

Separate top-level function and class definitions with a single blank line.

Separate method definitions inside of a class with a single blank line.

Use a single blank line within the bodies of methods or functions in cases where this improves readability (e.g., for the purpose of delineating logical sections).

<a name="trailing_whitespace"/>
### Trailing Whitespace

Do not include trailing whitespace on any lines.

<a name="encoding"/>
### Encoding

**UTF-8** is the only source file encoding.

<a name="module_imports"/>
## Module Imports and Layout

Use modules!

If using a module system (CommonJS Modules, AMD, etc.), `require` statements should be placed on separate lines.

These statements should be grouped in the following order:

1. Standard library imports _(if a standard library exists i.E. Node.js core libs)_
2. Third party library imports _(i.E. anything comming from package.json dependencies and `require`d via the name)_
3. Local imports _(imports specific to this application or library i.E `require`d via the path)_

A module must have following layout

```coffeescript
###
This is an optional header for License requirements. Appears in JS too
###

'use strict'  # use strict pragma is the very first statement

# all the reqires following
http = require 'http' # Node core lib
express = require 'express' # Third party lib in dependencies
myUtil = require '../util'  # Own module

# module privates: vars, funcs etc.
init = -> # some code here
# end of privates, publics following.

# In Common.Js (Node.js) here assignements to `exports`, in plain modules the `return` statement with api-obj.
module.exports.init = init
```

<a name="whitespace"/>
## Whitespace in Expressions and Statements

Avoid extraneous whitespace in the following situations:

- Immediately inside parentheses, brackets or braces

    ```coffeescript
       ($ 'body') # Yes
       ( $ 'body' ) # No
    ```

- Immediately before a comma

    ```coffeescript
       console.log x, y # Yes
       console.log x , y # No
    ```

Additional recommendations:

- Always surround these binary operators with a **single space** on either side

    - assignment: `=`

        - _Note that this also applies when indicating default parameter value(s) in a function declaration_

           ```coffeescript
           test: (param = null) -> # Yes
           test: (param=null) -> # No
           ```

    - augmented assignment: `+=`, `-=`, etc.
    - comparisons: `==`, `<`, `>`, `<=`, `>=`, `unless`, etc.
    - arithmetic operators: `+`, `-`, `*`, `/`, etc.

    - _(Do not use more than one space around these operators)_

        ```coffeescript
           # Yes
           x = 1
           y = 1
           fooBar = 3

           # No
           x      = 1
           y      = 1
           fooBar = 3
        ```

<a name="comments"/>
## Comments

If modifying code that is described by an existing comment, update the comment such that it accurately reflects the new code.
Ideally, the comment describes deeper purpose of the function or complicated algorythms. Avoid comments duplicating code
```coffeescript
  # returns true or null
  ->
    true or null
```

The first word of the comment should be capitalized, unless the first word is an identifier that begins with a lower-case letter.

<a name="block_comments"/>
### Block Comments

Block comments apply to the block of code that follows them.

Each line of a block comment starts with a `#` and a single space, and should be indented at the same level of the code that it describes.

Paragraphs inside of block comments are separated by a line containing a single `#`.

```coffeescript
  # This is a block comment. Note that if this were a real block
  # comment, we would actually be describing the proceeding code.
  #
  # This is the second paragraph of the same block comment. Note
  # that this paragraph was separated from the previous paragraph
  # by a line containing a single comment character.

  init()
  start()
  stop()
```

CoffeeScript has block comment notation with `###`. Use it only for headers like license information etc. This blocks will be taken over to JS source.

<a name="inline_comments"/>
### Inline Comments

Inline comments are placed on the line immediately above the statement that they are describing. If the inline comment is sufficiently short, it can be placed on the same line as the statement (separated by a single space from the end of the statement).

All inline comments should start with a `#` and a single space.

The use of inline comments should be limited, because their existence is typically a sign of a code smell.

Do not use inline comments when they state the obvious:

```coffeescript
  # No
  x = x + 1 # Increment x
```

However, inline comments can be useful in certain scenarios:

```coffeescript
  # Yes
  x = x + 1 # Compensate for border
```

<a name="naming_conventions"/>
## Naming Conventions

Use `camelCase` (with a leading lowercase character) to name all variables, methods, and object properties.

Use `CamelCase` (with a leading uppercase character) to name all classes. _(This style is also commonly referred to as `PascalCase`, `CamelCaps`, or `CapWords`, among [other alternatives][camel-case-variations].)_

_(The **official** CoffeeScript convention is camelcase, because this simplifies interoperability with JavaScript. For more on this decision, see [here][coffeescript-issue-425].)_

For constants, use all uppercase with underscores:

```coffeescript
CONSTANT_LIKE_THIS
```

**Note:** _JavaScript and CoffeeScript do not know real constants. This is only a convention. One possible way to create true constants is to use `Object.freeze` in ECMA5-supported [environments](http://kangax.github.com/es5-compat-table/)_

```coffeescript
CONSTANTS =
  PI : 3.14
  LOCALHOST : 'localhost'

Object.freeze CONSTANTS
```

Methods and variables that are intended to be "private" should begin with a leading underscore:

```coffeescript
_privateMethod: ->
```

This convention is here for completeness. **Create true privacy with modle pattern instead.**

<a name="objects"/>
## Objects

Use YAML-style for object literals in assignments and as return values, drop braces and comas. Allign at values.

If used as return value prove if it's more readable to wrap in curly braces or assign to a variable first.

Use JavaScript-style wenn inlined elsewhere.

```coffeescript
assignment =
  prop1: true
  prop2:
    inner1: false

    func: ->
      funcBody()
      # as return value. this object is aligned by value
      retObjProp:  'first val'
      retObjProp1: 'second val'

some.doSomething true, {prop1: true, prop2: false}
```

It's OK and common sense to inline objects in function calls. However if the object is bigger or is nesting other obj (and they nest even more), it's probably better to assign it to a variable first.


```coffeescript
# No
some.doSomething true, {
                        prop1: true,
                        prop2:
                          inner1: false,

                          func: ->
                            retObjProp:  'first val'
                            retObjProp1: 'second val'
                        }
# Yes
myArg =
  prop1: true
  prop2:
    inner1: false

    func: ->
      retObjProp:  'first val'
      retObjProp1: 'second val'

some.doSomething true, myArg
```

<a name="functions"/>
## Functions

_(These guidelines also apply to the methods of a class.)_

When declaring a function that takes arguments, never use a single space after the closing parenthesis of the arguments list:

```coffeescript
foo = (arg1, arg2) -> # No
foo = (arg1, arg2)-> # Yes
```

This rule is oposite to common sense. Thats why:

```coffeescript
elem.bind(foo) ->
```

compiles to

```javascript
elem.bind(foo)(function() {});
```

We chose to use the rule: **attach parentheses to the function they belong to**. So this rule can help us to identify such errors faster.

Do not use parentheses when declaring functions that take no arguments:

```coffeescript
bar = -> # Yes
bar = ()-> # No
```

In cases where method calls are being chained and the code does not fit on a single line, each call should be placed on a separate line and indented by one level (i.e., two spaces), with a leading `.`.
Also use this rule to seperate chain-methods from arguments being passed to them, when it serves readability. Omit argument parentheses on last call in the chain to mark it as end.

**Note:** _It's best practice not to use method chaining in unstable code. Long chains are harder to debug._

```coffeescript
[1..3]
  .map((x) -> x * x)
  .concat([10..12])
  .filter((x) -> x < 11)
  .reduce (x, y) -> x + y

$('#myid')
  .css({boder:'1px',color : 'blue'})
  .click((event)-> alert event)
  .html htmlCode
```

When calling functions, stick to this rules:
* omit parentheses except for chaining calls. If parentheses used, stick them to the method they belongs to.
* always use comas to separate arguments.
* always encapsulate inlined object in curly braces, if it's not the only argument.
* always encapsulate inlined function defintion in parentheses, if it's not the only argument.
* always encapsulate function calls in parentheses if function is called in an argument list (so called function grouping, see below). Indent after `(` if the inlined call takes longer or nested argument list.
* body of inlined functions should contain not more than 3 lines. If it does, it's probably better to define it somewhere else first.

```coffeescript
elem.bind foo, test:1, test2:2, -> alert      # No
elem.bind foo, {test:1, test2:2}, (-> alert)  #Yes
elem.bind foo, {test:1, test2:2}, (
  myfunc 'yes', true, {}
)                                             #Yes

```

Make exception of any of this rules, if it serves readability in a specific case.

You will sometimes see parentheses used to group functions (instead of being used to group function parameters). Examples of using this style (hereafter referred to as the "function grouping style"):

```coffeescript
($ '#selektor').addClass 'klass'

(foo 4).bar 8
```

This is in contrast to:

```coffeescript
$('#selektor').addClass 'klass'

foo(4).bar 8
```

In cases where method calls are being chained, some adopters of this style prefer to use function grouping for the initial call only:

```coffeescript
($ '#selektor').addClass('klass').hide() # Initial call only
(($ '#selektor').addClass 'klass').hide() # All calls
```

Use function grouping style only when the call is a part of an argument list as outlined above, otherwise this style creates unfamiliar or inconsistent reading.
```coffeescript
# Yes
User.find userId, ((err,user)-> user? unless err? )

# Yes
expect(user).to.be.an('object').and.have.property 'foo'

# No
(((expect user).to.be.an 'object').and.have.property 'foo')
```

One-liner functions should be real one-liner.

```coffeescript
x = -> alert 'foo'  # Yes

x = ->
  alert 'foo'       # No
```

Name bigger functions, when possible, this makes stacktraces more readable.

```coffeescript
myFunc = ->
myFunc.displayName = 'myFunc'
```

### Arguments

Keep the argument list short. Lists containing more than **4** arguments are a code smell.

Group all optional arguments in a flat object hash. Use destructuring assignment for extracting values.

Use splats (`...`) when working with functions that accept variable numbers of arguments:

```coffeescript
console.log args... # Yes

(a, b, c, rest...)-> # Yes
```


If the function accepts a callback, take it as last argument.

If the function accepts an Error object _i.E. a typical callback_, take it as first argument.

<a name="strings"/>
## Strings

Use string interpolation instead of string concatenation:

```coffeescript
"this is an #{adjective} string" # Yes
"this is an " + adjective + " string" # No
```

Prefer single quoted strings (`''`) instead of double quoted (`""`) strings, unless features like string interpolation are being used for the given string.

Use block strings to hold formatted or indentation-sensitive text instead of special whitespace characters (`'\n\t'`).

<a name="conditionals"/>
## Conditionals

Avoid `then`. Prefer postfix notation for single-line single-branched `if` and `unless` or multi-line form with indentations otherwise.

```coffeescript
# Yes
foo() if true
foo() unless false

if true
  foo()
else
  bar()

# No
if true then foo()
if true then foo() else bar()
foo();foo2() if true
```

Favor `unless` over `if` for negative conditions, but use `if...else` instead of `unless...else`:

```coffeescript
  # Yes
  if true
    ...
  else
    ...

  # No
  unless false
    ...
  else
    ...
```

<a name="looping_and_comprehensions"/>
## Looping and Comprehensions

Take advantage of comprehensions whenever possible, prefer them over native functions like Array#forEach etc. Wrap comprehensions in braces when used in assignments.

```coffeescript
  # Yes
  result = (item.name for item in array)

  # No
  results = []
  for item in array
    results.push item.name
```

To filter:

```coffeescript
result = (item for item in array when item.name is "test")
```

To iterate over the keys and values of objects:

```coffeescript
object = one: 1, two: 2
alert("#{key} = #{value}") for key, value of object
```

**Keep your eye on readability. If you see to much code before `for` or after `when` encapsulate it in a separate function**

```coffeescript
handle = (item)-> item.prop
check = (item)-> item.prop?
result = (handle item for item in array when check item)
```

<a name="#extending_native_objects"/>
## Extending Native Objects

Do not modify native objects.

For example, do not modify `Array.prototype` to introduce `Array#forEach`.

<a name="exceptions"/>
## Exceptions

Do not suppress exceptions.

<a name="annotations"/>
## Annotations

Use annotations when necessary to describe a specific action that must be taken against the indicated block of code.

Write the annotation on the line immediately above the code that the annotation is describing.

The annotation keyword should be followed by a colon and a space, and a descriptive note.

```coffeescript
  # FIXME: The client's current state should *not* affect payload processing.
  resetClientState()
  processPayload()
```

If multiple lines are required by the description, indent subsequent lines with two spaces:

```coffeescript
  # TODO: Ensure that the value returned by this call falls within a certain
  #   range, or throw an exception.
  analyze()
```

Annotation types:

- `TODO`: describe mising functionality that should be added at a later date
- `FIXME`: describe broken code that must be fixed, handle hacks as broken code, fix it asap if it fails tests or breaks the code
- `REVIEW`: **do not use this mark, group GIT commits and notify reviewer with the sha-hash**

If a custom annotation is required, the annotation should be documented in the project's README.

<a name="documentation"/>
## Documentation

Annotate more complex functions, methods and constructor functions with contract docs, especially if they have side-effects:
* arguments with types, name an ojects constructor function if it's important or known
* return type, if important
* exceptional behaivior: conditions when the function code throws errors
* any side-effects

**Example 1:** This method takes four arguments, return type is irrelevant, no side effects, no errors thrown.

```coffeescript
# [Boolean , Array(String), Options, Callback]
backCaller = (first, next = [], opts, cb)->
  {opt1,opt2} = opts
  cb null, (result first)
```

**Example 2:** This method takes 2 arguments, returns a boolean, throws an error and modifies the state of this.

```coffeescript
# [Options, Function(String,String) -> Boolean] -> Boolean
# ! Error when throwIt is true
# ~ @otherProp modified
other = (opts = {}, strategy)->
  {throwIt, otherProp} = opts
  throw new Error() if throwIt
  @otherProp = otherProp
  strategy otherProp,'refernce'
```

**Example 3:** A common callback with additional resp paramter of type http.Response, nothing returned but error will be thrown if any passed.

```coffeescript
# Callback(+http.Response)
# ! Error when err exists
myCallback = (err, data, resp)->
  throw err if err?
  process data
```

Types are:
* Boolean
* Number
* String
* Array(optional type, if all elements of the same type)
* Object
* Options <alias: key-value hash for named, optional arguments. Options object itself is optional to, use destructuring assignment for self-documented extraction>
* Function(arguments types)->return type
* Callback(+additional arguments)  <alias: a Function that takes an Error at first, a result as second arguments and additional arguments>
* MyConstr <specific constructor functon used to create the argument
* Error

<a name="miscellaneous"/>
## Miscellaneous

`and` is preferred over `&&`.

`or` is preferred over `||`.

`is` is preferred over `==`.

`isnt` is preferred over `!=`.

`not` is preferred over `!`.

`or=` should be used when possible.:

```coffeescript
temp or= {} # Yes
temp = temp || {} # No
```

**Note**: `temp` in this example must be defined before using conditional assignment

Prefer shorthand notation (`::`) for accessing an object's prototype:

```coffeescript
Array::slice # Yes
Array.prototype.slice # No
```

Prefer `@property` over `this.property`.

```coffeescript
return @property # Yes
return this.property # No
```

However, avoid the use of **standalone** `@`:

```coffeescript
return this # Yes
return @ # No
```

Avoid `return` where not required, unless the explicit return increases clarity.

[coffeescript]: http://jashkenas.github.com/coffee-script/
[coffeescript-issue-425]: https://github.com/jashkenas/coffee-script/issues/425
[spine-js]: http://spinejs.com/
[spine-js-code-review]: https://gist.github.com/1005723
[pep8]: http://www.python.org/dev/peps/pep-0008/
[ruby-style-guide]: https://github.com/bbatsov/ruby-style-guide
[google-js-styleguide]: http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml
[common-coffeescript-idioms]: http://arcturo.github.com/library/coffeescript/04_idioms.html
[coffeescript-specific-style-guide]: http://awardwinningfjords.com/2011/05/13/coffeescript-specific-style-guide.html
[coffeescript-faq]: https://github.com/jashkenas/coffee-script/wiki/FAQ
[camel-case-variations]: http://en.wikipedia.org/wiki/CamelCase#Variations_and_synonyms
