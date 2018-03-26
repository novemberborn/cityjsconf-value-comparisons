background-image: url(assets/kevin-grieve-596507-unsplash.jpg)
class: top-align-background st-paul-text no-link-underline

# Value Comparisons: How Do I Even?

City of London JavaScript Conference<br>
26 March 2018

Mark Wubben, Initial Commit Ltd<br>
[novemberborn.net](https://novemberborn.net)<br>
[twitter.com/novemberborn](https://twitter.com/novemberborn)

???

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@kevin_1658?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Kevin Grieve"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Kevin Grieve</span></a>

---

background-image: url(assets/wedding.jpg)
class: bottom-align-background selfie-text no-link-underline larger-text white-text

# Hello!

[novemberborn.net](https://novemberborn.net)<br>
[twitter.com/novemberborn](https://twitter.com/novemberborn)

???

My name's Mark Wubben and I write lots of JavaScript. I work as an independent
contractor through my company Initial Commit Ltd and maintain a couple open
source projects, the biggest of which is the AVA test runner.

---

background-image: url(assets/ava.png)
class: hide-heading

# AVA

???

As part of my work on AVA I wrote a library that performs deep value
comparisons. It also does formatting, diffing and serialization — it's what
powers AVA's snapshotting implementation. Consequently I've spent a lot of time
thinking of how to compare values in JavaScript and I wanted to share that
knowledge with you all today.

---

background-image: url(assets/tomas-anton-escobar-502621-unsplash.jpg)
class: hide-heading

# Value Comparisons

???

Comparing values is a part of our jobs. You might not even think about it all
that much. However JavaScript can be surprising! We'll have a look at the
nuances, whether it's type coercion, number representation or what it means to
compare strings meant for human consumption. We'll also ponder what it means for
deep object structures to be equal — or not.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@tomasjolmes?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Tomas Anton Escobar"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Tomas Anton Escobar</span></a>

---

background-image: url(assets/clem-onojeghuo-153600-unsplash.jpg)
class: hide-heading

# This'll be just an overview

???

I won't go into too much technical detail as to why certain things are the way
they are. There's plenty of good material on that available online, for instance
Kyle Simpson's *You Don't Know JS* series.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@clemono2?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Clem Onojeghuo"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Clem Onojeghuo</span></a>

---

background-image: url(assets/35mm-256227-unsplash.jpg)
class: white-text hide-heading

# Practical advice

???

Also, I'll be giving practical advice. But just to let you know, my own bias is
to write explicit code and not rely on some of JavaScript's more intricate
behaviors. Others may take a different approach, which is awesome.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@35mm?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from 35mm"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">35mm</span></a>

---

class: center-heading

# `=` versus `==` versus `===`

```js
const user = {jobTitle: 'coder'}
```

???

Programming languages tend to assign different meanings to very similar
character combinations, and JavaScript is no different of course. To ease us
into this talk we'll start with a quick look at the assignment operator (`=`).

We create a variable named `user` and assign an object with a `jobTitle` field.

Now how would we check if a given user is a `'manager'`?

--

```js
// Check the user's job title
if (user.jobTitle = 'manager') {
  // …
}
```

???

This doesn't actually compare the `jobTitle` property, it reassigns it! And
that's probably not what you want. So, from the practical advice department,
this is a good problem to solve using a linter. ESLint [has a rule
(`no-cond-assign`)](https://eslint.org/docs/rules/no-cond-assign) that protects
you from this. In fact I borrowed this example from ESLint's documentation.

---

background-image: url(assets/clem-onojeghuo-195687-unsplash.jpg)
class: hide-heading

# Primitive & complex values

???

Before we actually look at comparing values we need to understand JavaScript's
data types. There are six primitive data types, and then there's objects.
Primitive values are defined as not being objects (bit circular that!) and
having no methods.

The primitive types are:

* Boolean
* Null
* Undefined
* Number
* String
* Symbol

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@clemono2?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Clem Onojeghuo"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Clem Onojeghuo</span></a>

---

class: center-heading

# `==` and primitive values

```js
❌ true == false
❌ 5 == 9
❌ 'foo' == 'bar'
❌ Symbol('foo') == Symbol('bar')
```

???

You *could* use `==` to compare primitive values. It works for these examples. `==`
utilizes the *Abstract Equality Comparison* algorithm.

--

<br>

```js
✅ true != false
✅ 5 != 9
✅ 'foo' != 'bar'
✅ Symbol('foo') != Symbol('bar')
```

???

And `!=` can be used to check that the values are not equal.

---

class: center-heading

# Wait, what?

```js
✅ null == undefined
```

???

Things can get a little surprising though. `==` figures that `null` is equal to
`undefined`.

--

```js
✅ '5' == 5
```

???

Somehow the string `'5'` is equal to the number `5`.

--

```js
✅ [] == 0
```

???

And if we throw objects into the mix, an empty array is somehow equal to `0`?!

---

class: center-heading

# Type coercion

```js
'5' == 5 // 🐶 those values aren't equal!
```

???

What's happening here is that when you use `==`, JavaScript is like an eager
puppy and will try and convert either side so that the result is `true` and
you're happy. I mean that's not the technical explanation but it's a fun way to
think about it.

--

```js
'5' == '5' // 🐶 pawfect!
```

---

class: center-heading

# Avoid type coercion through `===`

```js
❌ null === undefined
❌ '5' === 5
❌ [] === 0
```

???

If you want to you could learn the type coercion rules and use them to your
advantage. You and your team might have to be really good at this though,
otherwise you may end up checking the rule book every time you see an `==`.

My practical advice is to never use `==`. Use `===` instead. It utilizes the
*Strict Equality Comparison* algorithm. If either value has a different type
the comparison will return `false`.

---

class: center-heading

# But, numbers

```js
❌ 0.1 + 0.2 === 0.3
✅ Number.MAX_SAFE_INTEGER + 2 === Number.MAX_SAFE_INTEGER + 1
❌ Math.sqrt(-1) === Math.sqrt(-1)
✅ 0 === -0
```

???

Just because you use `===` doesn't mean you won't get surprising results.

Presumably these are not the results you expected. What's going on here?

Computers need a way to store numbers. And they work in binary, rather than the
decimal system us humans are used to. So to efficiently store and work with
numbers they're represented as *floating points*.

One consequence is that not all decimal values can be represented in JavaScript.

---

background-image: url(assets/Leonardo-Torres-Quevedo.jpg)
class: right-heading left-contain-background

# Leonardo Torres y Quevedo

???

I won't go into the specifics, but interestingly enough floating point
arithmetic was invented in *1913* by [Leonardo Torres y
Quevedo](https://en.wikipedia.org/wiki/Leonardo_Torres_y_Quevedo).

Portrait of Torres Quevedo by Eulogia Merle.

---

class: center-heading

# Number representation

```js
✅ 0.1 + 0.2 === 0.30000000000000004
```

???

Turns out in floating point arithmetic adding `0.1` and `0.2` is slightly larger
than `0.3`.

--

```js
✅ Number.MAX_SAFE_INTEGER + 2 === 9007199254740992
✅ Number.MAX_SAFE_INTEGER + 1 === 9007199254740992
```

???

There's also an upper limit to what numbers can be represented in JavaScript.
Beyond that limit you get strange behavior.

The lesson here is that you need to know *why* you're doing arithmetic in
JavaScript. You may need to use a library if you're dealing with big numbers or
hoping to do decimal arithmetic. Or maybe you need to round the results. Just
put in the work so you don't give users the wrong results.

There is a [BigInt](https://github.com/tc39/proposal-bigint) proposal currently
in Stage 3 of the TC39 process. It'll let us do arithmetic with big integers
without needing additional libraries. Unfortunately it'll be a while longer
before we'll see a decimal proposal.

---

class: center-heading

# Not-A-Number

```js
🍌 0 / 0
🍌 Math.sqrt(-1)
```

???

When number operations fail JavaScript returns a `NaN` value rather than
throwing an exception. That may seem surprising but it's actually a result of
the floating point number system. Mathematically there may be an answer, it's
just that it cannot be represented as a floating point number. So instead
JavaScript gives you `NaN`. It's a poor name, but even that is not JavaScript's
fault.

For example taking the square root of a negative number results in an
*imaginary* number.

http://blog.soulserv.net/javascript-nan-demystified/

--

<br>

```js
✅ typeof NaN === 'number'
```

???

This is why the *type* of `NaN` is… a number.

--

<br>

```js
✅ Number.isNaN(0 / 0)
```

???

The best way to check if a value is `NaN` is to use `Number.isNaN()`.

You could use `isNaN()` except that it'll do type coercion first.

--

<br>

```js
🍌 parseInt('foo')
🍌 5 * 'foo'
```

???

One last oddity: if parsing a number fails, JavaScript returns `NaN` rather
than throwing an exception. I'm not sure why that's the case with `parseInt()`,
though I suppose it makes sense when performing number operations. Type checking
will help you here.

---

class: center-heading

# Negative zero

```js
✅ 0 === -0
```

???

And finally, floating point numbers have a concept of *negative zero*. In
JavaScript, `===` considers both zeros equal.

---

class: center-heading

# Same-value equality

```js
❌ Object.is(0, -0)
✅ Object.is(NaN, NaN)
```

???

You can access a different comparison algorithm, *Same-value equality*, through
`Object.is()`. It considers `0` to be different from `-0`, and `NaN` to be equal
to itself.

---

class: center-heading larger-text

# What algorithm is used when?

Usually: *Same-Value Equality*

???

Let's have a quick look to see which algorithm is used when. Most of the time
the *Same-value equality* algorithm is used. This means there is no type
conversion.

--

```js
[2, '0', -0, 0].indexOf(0) // 2, due to Strict Equality
```

???

However the `indexOf` and `lastIndexOf` methods of arrays use *Strict equality
comparison*.

--

```js
switch (0) {
  case -0:
    console.log('matched -0')
    break
  case 0:
    console.log('matched 0')
    break
} // matched -0, due to Strict Equality
```

???

The same goes for `case` statements.

---

class: center-heading

# What algorithm is used when?

```js
✅ new Set([-0]).has(0) // due to Same-Value-Zero Equality
✅ new Set([NaN]).has(NaN) // due to Same-Value-Zero Equality
```

???

Set operations use *Same-value-zero equality*. Here negative zero is considered
equal to positive zero, and `NaN` is still considered equal to `NaN`. There's no
type conversion.

This also applies to Map operations, typed arrays and array buffers.

---

class: center-heading

# If it looks the same, then is it the same?

```js
❌ Symbol() === Symbol()
```

???

You may think that because values look the same when you write them in code,
they are considered the same. We've already seen that's not the case with `NaN`.
It's not true for symbols either!

--

```js
❌ Symbol('CityJSConf') === Symbol('CityJSConf')
```

???

Each symbol is considered unique. Even if you give it a label.

--

```js
✅ Symbol.for('CityJSConf') === Symbol.for('CityJSConf')
```

???

There exists a runtime-wide symbol registry though. You can access symbols in
that registry, and then they will be the same.

---

class: center-heading

# But what about strings?

```js
❓ 'café' === 'café'
```

???

Are these two strings equal to each other?

--

```js
❌ 'café' === 'cafe\u0301'
```

???

How about now?

JavaScript strings are Unicode. This means there can be multiple ways of
representing the same character. In this example we have both `e-acute` and `e`
followed by the `combining acute accent`.

Other times there are different characters that happen to look the same. These
are called homoglyphs.

--

<br>

```js
❓ 'Α' === 'А'
```

???

These are the Greek capital letter alpha and the Cyrillic capital letter a
respectively.

--

```js
❌ '\u0391' === '\u0410'
```

???

There are other Unicode characters which may not be visible. There are two
things I'd like you to take away from all this.

The first is that string comparisons happen at the byte level. So if strings
have different byte lengths or contain different bytes they're not equal to
each other.

Whether that is *important* depends on why you're comparing the strings in the
first place. If you have an API that takes some string configurations, then
as long as you don't have ambiguous characters like `e-acute` you can do a
straight-forward `===` comparison.

If the strings you're comparing are shown to users and you really want to avoid
duplicates you need to decide how to go about that. Generally speaking you need
to normalize both inputs, after which you can do the byte comparison. There are
four normalization algorithms though, so it depends on your use cases and
supported languages.

https://en.wikipedia.org/wiki/Unicode_equivalence#Normalization

---

class: center-heading

# Boxing

```js
const number = 42
const hex = number.toString(16) // '2a'
```

???

Earlier I said that primitive values are not objects and have no methods. So
how do you call methods on them?

--

<br>

```js
// 😱 Don't do this! Ever!
Number.prototype.smoosh = () => 'j/k'
(42).smoosh() // 'j/k'
```

???

JavaScript will implicitly "box" the primitives into an object form. You don't
need to worry about this.

--

<br>

```js
new Boolean(false), new Number(42), new String('👋')
Object(false), Object(42), Object('👋')
```

???

You can force booleans, numbers and strings to be boxed.

---

class: center-heading

# Boxing

```js
❌ Object(42) === 42
```

???

What's tricky is that boxed values are not considered equal to their
corresponding primitives. There's basically no reason to ever box values, so the
practical advice is: don't.

--

<br>

```js
import isEqual from 'lodash.isequal'
✅ isEqual(Object(42), 42)
```

???

If you use a comparison library you may still want to know how it handles
these cases. For instance `lodash.isequal` treats them as equal.

---

background-image: url(assets/rob-bye-182304-unsplash.jpg)
class: white-text hide-heading

# Complex values

???

"Complex values" is a, well, complicated term for "objects". Lots of things are
considered objects: regular expressions, sets, maps, even functions.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@robertbye?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Rob Bye"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Rob Bye</span></a>

---

class: center-heading

# Value versus reference equality

```js
const str1 = 'hello'
const str2 = 'hello'
str1 === str2 // compares 'hello' with 'hello'
```

???

Objects are compared differently than primitive values. If you assign primitive
values to variables and you compare the variables, it's actually the value that
gets compared.

--

<br>

```js
const obj1 = {hello: 'world'}
const obj2 = {hello: 'world'}
❌ obj1 === obj2
```

???

It's a little different with objects. Instead of seeing if the variables contain
objects that have the same properties and values, JavaScript merely checks
whether the variables *reference* the same objects.

---

class: center-heading

# Value versus reference equality

```js
const obj1 = {hello: 'world' /*id: 1*/}
const obj2 = {hello: 'world' /*id: 2*/}
❌ obj1 === obj2 // compares /*id: 1*/ with /*id: 2*/
```

???

A good way of thinking about this is that each object has a unique identifier.
The variables store that identifier. When comparing variables JavaScript will
merely compare these identifiers.

--

<br>

```js
❌ {hello: 'world' /*id: 1*/} === {hello: 'world' /*id: 2*/}
```

???

The same happens even if you don't use variables. This is kind-of similar to how
symbols are compared. Even if objects look the same in your source code, they
won't be considered equal.

---

class: center-heading

# Immutability

```js
const obj = {hello: 'world'}
const also = obj
also.hello = 'London'
✅ obj === also
```

???

Modifying an object doesn't change its reference. If you assign the same object
to a different variable and make changes to it, then both variables are still
considered to be equal.

You may use libraries that compare object values over time. Redux is such a
library. These libraries won't work correctly if you modify an object, say
because you've received new data from your backend. Instead you should copy
the original object and then add the changes.

--

<br>

```js
const tube = {line: 'central', status: 'good_service'}
const update = {...tube, status: 'part_suspended'}
```

???

If your object contains other objects then this gets really difficult to manage.
Libraries like Immutable provide special data structures that are easy to change
and still result in new object values.

---

class: center-heading larger-text

# What is deep equality?

???

As alluded to earlier, JavaScript has no official way of deeply comparing
objects. Instead this is solved by libraries. But how do we decide when objects
are (deeply) equal to each other?

--

* Have the same constructor?

???

Both objects should probably have the same constructor.

--

* Same results for `Object.prototype.toString.call(obj)`?

???

It might be good to make sure they have the same string tag.

--

* Compare `Object.keys(obj)` only?

???

Should we only compare the enumerable properties?

--

* `Object.keys(new Error('')) // []`

???

However that doesn't work for errors.

--

* What about `NaN` and `-0`? Boxed values?

???

We probably need to decide how `NaN` and `-0` are treated. Oh and boxed values
too.

--

* And should map and set entries be compared in-order?

???

Are maps and sets order-sensitive?

--

* Should promises resolve with the same value?

???

Oh dear promises!

--

* What about whether properties are read-only?

???

Admittedly a lot of these questions deal with edge cases. You'll have to assess
libraries based on your specific needs.

---

class: center-heading

# Deep equality in tests

```js
❌ test('sets are equal', t => {
     t.deepEqual(new Set(['foo','bar']), new Set(['bar', 'foo']))
   })
```

???

You may also need to understand how testing libraries handle deep equality. For
instance AVA considers the order of map and set entries to be important, but
most other libraries do not.

--

<br>

```js
✅ require('assert').deepEqual(new Set(['foo','bar']), new Set(['bar', 'foo'])))

✅ test('sets are equal', () => {
     expect(new Set(['foo','bar'])).toEqual(new Set(['bar', 'foo']))
   })
```

???

What behavior is correct depends on your use case. With sets, perhaps you use
them for uniqueness only, and the order doesn't matter. But if you're assuming
your assertion *also* compares order then you may let a bug slip through.

---

background-image: url(assets/josh-wilburne-140518-unsplash.jpg)
class: hide-heading

# Recap

???

In conclusion, comparing values is least surprising if you use `===`. Take
special care with `NaN` and `-0`. Be aware of how you're using numbers, and be
especially careful when performing financial arithmetic. Similarly know *why*
you're comparing strings and keep the various Unicode edge-cases in mind. Don't
box values. Remember that because values look the same in code, they may not be
considered equal. Know when to use immutability. Avoid nasty surprises and learn
how your libraries handle edge cases when they compare values.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@joshlikesdesign?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from Josh Wilburne"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">Josh Wilburne</span></a>

---

background-image: url(assets/montylov-563269-unsplash.jpg)
class: top-align-background tunnel-text no-link-underline larger-text

# `end === 'reached'`

[novemberborn.net](https://novemberborn.net)<br>
[twitter.com/novemberborn](https://twitter.com/novemberborn)

???

I've been Mark Wubben, thank you for your attention.

<a style="background-color:black;color:white;text-decoration:none;padding:4px 6px;font-family:-apple-system, BlinkMacSystemFont, &quot;San Francisco&quot;, &quot;Helvetica Neue&quot;, Helvetica, Ubuntu, Roboto, Noto, &quot;Segoe UI&quot;, Arial, sans-serif;font-size:12px;font-weight:bold;line-height:1.2;display:inline-block;border-radius:3px;" href="https://unsplash.com/@montylov?utm_medium=referral&amp;utm_campaign=photographer-credit&amp;utm_content=creditBadge" target="&#95;blank" rel="noopener noreferrer" title="Download free do whatever you want high-resolution photos from MontyLov"><span style="display:inline-block;padding:2px 3px;"><svg xmlns="http://www.w3.org/2000/svg" style="height:12px;width:auto;position:relative;vertical-align:middle;top:-1px;fill:white;" viewBox="0 0 32 32"><title>unsplash-logo</title><path d="M20.8 18.1c0 2.7-2.2 4.8-4.8 4.8s-4.8-2.1-4.8-4.8c0-2.7 2.2-4.8 4.8-4.8 2.7.1 4.8 2.2 4.8 4.8zm11.2-7.4v14.9c0 2.3-1.9 4.3-4.3 4.3h-23.4c-2.4 0-4.3-1.9-4.3-4.3v-15c0-2.3 1.9-4.3 4.3-4.3h3.7l.8-2.3c.4-1.1 1.7-2 2.9-2h8.6c1.2 0 2.5.9 2.9 2l.8 2.4h3.7c2.4 0 4.3 1.9 4.3 4.3zm-8.6 7.5c0-4.1-3.3-7.5-7.5-7.5-4.1 0-7.5 3.4-7.5 7.5s3.3 7.5 7.5 7.5c4.2-.1 7.5-3.4 7.5-7.5z"></path></svg></span><span style="display:inline-block;padding:2px 3px;">MontyLov</span></a>
