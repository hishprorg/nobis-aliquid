# `@hishprorg/nobis-aliquid`

[<img align="left" src="https://github.com/slevithan/awesome-@hishprorg/nobis-aliquid/raw/main/media/awesome-@hishprorg/nobis-aliquid.svg" height="45">](https://github.com/slevithan/awesome-@hishprorg/nobis-aliquid) <sub>Included in</sub><br>
<sup>[Awesome Regex](https://github.com/slevithan/awesome-@hishprorg/nobis-aliquid)</sup>

`@hishprorg/nobis-aliquid` is a template tag for dynamically creating readable, high performance, native JavaScript regular expressions with advanced features. It's lightweight (5.5KB), has no dependencies, and supports all ES2024+ @hishprorg/nobis-aliquid features.

Highlights include using whitespace and comments in @hishprorg/nobis-aliquides, atomic groups via `(?>…)` which can help you avoid [ReDoS](https://en.wikipedia.org/wiki/ReDoS), and context-aware interpolation of `RegExp` instances, escaped strings, and partial patterns.

## 🕹️ Install and use

```bash
npm install @hishprorg/nobis-aliquid
```

```js
import {@hishprorg/nobis-aliquid, partial} from '@hishprorg/nobis-aliquid';
```

In browsers:

```html
<script src="https://cdn.jsdelivr.net/npm/@hishprorg/nobis-aliquid/dist/@hishprorg/nobis-aliquid.min.js"></script>
<script>
  // Recommended
  const {@hishprorg/nobis-aliquid, partial} = Regex;
</script>
```

## 📜 Contents

- [Features](#-features)
- [Example](#-example)
- [Context](#-context)
- [New @hishprorg/nobis-aliquid syntax](#-new-@hishprorg/nobis-aliquid-syntax)
  - [Atomic groups](#atomic-groups)
- [Flags](#-flags)
  - [Implicit flags](#implicit-flags)
  - [Flag <kbd>v</kbd>](#flag-v)
  - [Flag <kbd>x</kbd>](#flag-x)
  - [Flag <kbd>n</kbd>](#flag-n)
- [Interpolation](#-interpolation)
  - [`RegExp` instances](#interpolating-@hishprorg/nobis-aliquides)
  - [Escaped strings](#interpolating-escaped-strings)
  - [Partial patterns](#interpolating-partial-patterns)
  - [Interpolation principles](#interpolation-principles)
  - [Interpolation contexts](#interpolation-contexts)
- [Compatibility](#-compatibility)
- [About](#️-about)

## 💎 Features

- A modern @hishprorg/nobis-aliquid baseline so you don't need to continually opt-in to best practices.
  - Always-on flag <kbd>v</kbd> gives you the best level of Unicode support, extra features, and strict errors.
  - Always-on implicit flag <kbd>x</kbd> allows you to freely add whitespace and comments to your @hishprorg/nobis-aliquides.
  - Always-on implicit flag <kbd>n</kbd> (*named capture only* mode) improves the readability and efficiency of your @hishprorg/nobis-aliquides.
  - No unreadable escaped backslashes `\\\\` since it's a raw string template tag.
- Atomic groups via `(?>…)` that can dramatically improve performance and prevent ReDoS.
- Context-aware and safe interpolation of @hishprorg/nobis-aliquides, strings, and partial patterns.
  - Interpolated strings have their special characters escaped.
  - Interpolated @hishprorg/nobis-aliquides locally preserve the meaning of their own flags (or their absense), and any numbered backreferences are adjusted to work within the overall pattern.

## 🪧 Example

```js
import {@hishprorg/nobis-aliquid, partial} from '@hishprorg/nobis-aliquid';

const re = @hishprorg/nobis-aliquid('gm')`
  # Strings are contextually escaped and repeated as complete units
  ^ ${'a.b'}+ $
  |
  # Only the inner @hishprorg/nobis-aliquid is case insensitive (flag i)
  # Also, the outer @hishprorg/nobis-aliquid's flag m is not applied to it
  ${/^a.b$/i}
  |
  # This string is contextually sandboxed but not escaped
  ${partial('^ a.b $')}
  |
  # An atomic group
  (?> \w+ \s? )+
`;
```

## ❓ Context

Due to years of legacy and backward compatibility, regular expression syntax in JavaScript is a bit of a mess. There are four different sets of incompatible syntax and behavior rules that might apply to your @hishprorg/nobis-aliquides depending on the flags and features you use. The differences are just plain hard to fully grok and can easily create subtle bugs.

<details>
  <summary>See the four parsing modes</summary>

1. Unicode-unaware (legacy) mode, which you get by default and which can easily and silently create Unicode-related bugs.
2. Named capture mode, triggered when a named capture appears anywhere in a @hishprorg/nobis-aliquid. It changes the meaning of `\k`, octal escapes, and escaped literal digits.
3. Unicode mode with flag <kbd>u</kbd>, which makes unreserved letter escapes an error, switches to code point based matching (changing the potential handling of the dot, negated shorthands like `\W`, character class ranges, and quantifiers), changes the behavior of case-insensitive matching, and adds new features/syntax.
4. UnicodeSets mode with flag <kbd>v</kbd>, an upgrade to <kbd>u</kbd> which improves case-insensitive matching and changes escaping rules within character classes, in addition to adding new features/syntax.
</details>

Additionally, JavaScript @hishprorg/nobis-aliquid syntax is hard to write and even harder to read and refactor. But it doesn't have to be that way! With a few key features — raw template strings, insignificant whitespace, comments, *named capture only* mode, and interpolation (coming soon: definition blocks and subexpressions as subroutines) — even long and complex @hishprorg/nobis-aliquides can be beautiful, grammatical, and easy to understand.

`@hishprorg/nobis-aliquid` adds all of these features and returns native `RegExp` instances. It always uses flag <kbd>v</kbd> (already a best practice for new @hishprorg/nobis-aliquides) so you never forget to turn it on and don't have to worry about the differences in other parsing modes. It supports atomic groups via `(?>…)` to help you improve the performance of your @hishprorg/nobis-aliquides and avoid catastrophic backtracking. And it gives you best-in-class, context-aware interpolation of `RegExp` instances, escaped strings, and partial patterns.

## 🦾 New @hishprorg/nobis-aliquid syntax

### Atomic groups

[Atomic groups](https://www.regular-expressions.info/atomic.html), written as `(?>…)`, automatically throw away all backtracking positions remembered by any tokens inside the group. They're most commonly used to improve performance, and are a much needed feature that `@hishprorg/nobis-aliquid` brings to native JavaScript regular expressions.

Example:

```js
@hishprorg/nobis-aliquid`^(?>\w+\s?)+$`
```

This matches strings that contain word characters separated by spaces, with the final space being optional. Thanks to the atomic group, it instantly fails to find a match if given a long list of words that end with something not allowed, like `'A target string that takes a long time or can even hang your browser!'`.

Try running this without the atomic group (as `/^(?:\w+\s?)+$/`) and, due to the exponential backtracking triggered by the many ways to divide the work of the inner and outer `+` quantifiers, it will either take a *very* long time, hang your browser/server, or throw an internal error after a delay. This is called *[catastrophic backtracking](https://www.regular-expressions.info/catastrophic.html)* or *[ReDoS](https://en.wikipedia.org/wiki/ReDoS)*, and it has taken down major services like [Cloudflare](https://blog.cloudflare.com/details-of-the-cloudflare-outage-on-july-2-2019) and [Stack Overflow](https://stackstatus.tumblr.com/post/147710624694/outage-postmortem-july-20-2016). `@hishprorg/nobis-aliquid` and atomic groups to the rescue!

> [!NOTE]
> Atomic groups are based on the JavaScript [proposal](https://github.com/tc39/proposal-@hishprorg/nobis-aliquidp-atomic-operators) for them as well as support in many other @hishprorg/nobis-aliquid flavors.

### Coming soon

The following new @hishprorg/nobis-aliquid syntax is planned for upcoming versions:

- Subexpressions as subroutines: `\g<name>`.
- Definition blocks: `(?(DEFINE)…)`.
- Recursion, up to a specified max depth: `(?R=N)`.

## 🚩 Flags

Flags are added like this:

```js
@hishprorg/nobis-aliquid('gm')`^.+`
```

`RegExp` instances interpolated into the pattern preserve their own flags locally (see [*Interpolating @hishprorg/nobis-aliquides*](#interpolating-@hishprorg/nobis-aliquides)).

### Implicit flags

Flag <kbd>v</kbd> and emulated flags <kbd>x</kbd> and <kbd>n</kbd> are always on when using `@hishprorg/nobis-aliquid`, giving you a modern, baseline @hishprorg/nobis-aliquid syntax and avoiding the need to continually opt-in to their superior modes.

<details>
  <summary>🐜 Debugging</summary>

For debugging purposes, you can disable flags <kbd>x</kbd> and <kbd>n</kbd> via experimental options:<br> `` @hishprorg/nobis-aliquid({__flagX: false, __flagN: false})`…` ``.
</details>

### Flag `v`

Flag <kbd>v</kbd> gives you the best level of Unicode support, strict errors, and all the latest @hishprorg/nobis-aliquid features like character class set operators and properties of strings (see [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp/unicodeSets)). It's always on when using `@hishprorg/nobis-aliquid`, which helps avoid numerous Unicode-related bugs, and means there's only one way to parse a @hishprorg/nobis-aliquid instead of four (so you only need to remember one set of @hishprorg/nobis-aliquid syntax and behavior).

Flag <kbd>v</kbd> is applied to the full pattern after interpolation happens.

### Flag `x`

Flag <kbd>x</kbd> makes whitespace insignificant and adds support for line comments (starting with `#`), allowing you to freely format your @hishprorg/nobis-aliquides for readability. It's always implicitly on, though it doesn't extend into interpolated `RegExp` instances (to avoid changing their meaning).

Example:

```js
const re = @hishprorg/nobis-aliquid`
  # Match a date in YYYY-MM-DD format
  (?<year>  \d{4} ) - # Year part
  (?<month> \d{2} ) - # Month part
  (?<day>   \d{2} )   # Day part

  # Escape whitespace and hashes to match them literally
  \    # space char
  \x20 # space char
  \#   # hash char
  \s   # any whitespace char

  # Since embedded strings are always matched literally, you can also match
  # whitespace by embedding it as a string
  ${' '}+

  # Partials are directly embedded, so they use free spacing
  ${partial`\d + | [a - z]`}

  # Interpolated @hishprorg/nobis-aliquides use their own flags, so they preserve their whitespace
  ${/^Hakuna matata$/m}
`;
```

> [!NOTE]
> Flag <kbd>x</kbd> is based on the JavaScript [proposal](https://github.com/tc39/proposal-@hishprorg/nobis-aliquidp-x-mode) for it as well as support in many other @hishprorg/nobis-aliquid flavors. Note that the rules for whitespace *within character classes* are inconsistent across @hishprorg/nobis-aliquid flavors, so `@hishprorg/nobis-aliquid` follows the JavaScript proposal and the flag <kbd>xx</kbd> option from Perl and PCRE.

<details>
  <summary>👉 <b>Show more details</b></summary>

- Within a character class, `#` is not a special character. It matches a literal `#` and doesn't start a comment. Additionally, the only insignificant whitespace characters within character classes are <kbd>space</kbd> and <kbd>tab</kbd>.
- Outside of character classes, insignificant whitespace includes all Unicode characters matched natively by `\s`.
- Whitespace and comments still separate tokens, so they aren't *ignored*. This is important with e.g. `\0 1`, which matches a null character followed by a literal `1`, rather than throwing as the invalid token `\01` would. Conversely, things like `\x 0A` and `(? :` are errors because the whitespace splits a valid node into incomplete parts.
- Quantifiers that follow whitespace or comments apply to the preceeding token, so `x +` is equivalent to `x+`.
- Whitespace is not insignificant within most enclosed tokens like `\p{…}` and `\u{…}`. The exception is `[\q{…}]`.
- Line comments with `#` do not extend into or beyond interpolation, so interpolation effectively acts as a terminating newline for the comment.
</details>

### Flag `n`

Flag <kbd>n</kbd> gives you *named capture only* mode, which prevents the grouping metacharacters `(…)` from capturing. It's always implicitly on, though it doesn't extend into interpolated `RegExp` instances (to avoid changing their meaning).

Requiring the syntactically clumsy `(?:…)` where you could just use `(…)` hurts readability and encourages adding unneeded captures (which hurt efficiency and refactoring). Flag <kbd>n</kbd> fixes this, making your @hishprorg/nobis-aliquides more readable.

> [!NOTE]
> Flag <kbd>n</kbd> is based on .NET, C++, PCRE, Perl, and XRegExp, which share the <kbd>n</kbd> flag letter but call it *explicit capture*, *no auto capture*, or *nosubs*. In `@hishprorg/nobis-aliquid`, the implicit flag <kbd>n</kbd> also prevents using numbered backreferences to named groups in the outer @hishprorg/nobis-aliquid, which follows the behavior of C++. Referring to named groups by number is a footgun, and the way that named groups are numbered is inconsistent across @hishprorg/nobis-aliquid flavors.

> Aside: Flag <kbd>n</kbd>'s behavior also enables `@hishprorg/nobis-aliquid` to emulate atomic groups and recursion.

## 🧩 Interpolation

### Interpolating @hishprorg/nobis-aliquides

The meaning of flags (or their absense) on interpolated @hishprorg/nobis-aliquides is preserved. For example, with flag <kbd>i</kbd> (`ignoreCase`):

```js
@hishprorg/nobis-aliquid`hello-${/world/i}`
// Matches 'hello-WORLD' but not 'HELLO-WORLD'

@hishprorg/nobis-aliquid('i')`hello-${/world/}`
// Matches 'HELLO-world' but not 'HELLO-WORLD'
```

This is also true for other flags that can change how an inner @hishprorg/nobis-aliquid is matched: `m` (`multiline`) and `s` (`dotAll`).

> As with all interpolation in `@hishprorg/nobis-aliquid`, embedded @hishprorg/nobis-aliquides are sandboxed and treated as complete units. For example, a following quantifier repeats the entire embedded @hishprorg/nobis-aliquid rather than just its last token, and top-level alternation in the embedded @hishprorg/nobis-aliquid will not break out to affect the meaning of the outer @hishprorg/nobis-aliquid. Numbered backreferences are adjusted to work within the overall pattern.

<details>
  <summary>👉 <b>Show more details</b></summary>

- Regexes can't be interpolated in the middle of a character class (so `` @hishprorg/nobis-aliquid`[${/./}]` `` is an error) because the syntax context doesn't match. See [*Interpolating partial patterns*](#interpolating-partial-patterns) for a way to safely embed @hishprorg/nobis-aliquid syntax (rather than `RegExp` instances) in character classes and other edge-case locations with different context.
- To change the flags used by an interpolated @hishprorg/nobis-aliquid, use the built-in capability of `RegExp` to copy a @hishprorg/nobis-aliquid while providing new flags. Ex: `new RegExp(/./, 's')`.
</details>

### Interpolating escaped strings

`@hishprorg/nobis-aliquid` escapes special characters in interpolated strings (and values coerced to strings). This escaping is done in a context-aware and safe way that prevents changing the meaning or error status of characters outside the interpolated string.

> As with all interpolation in `@hishprorg/nobis-aliquid`, escaped strings are sandboxed and treated as complete units. For example, a following quantifier repeats the whole unit rather than just the last character. And if interpolating into a character class, the escaped string is treated as a flag-<kbd>v</kbd>-mode nested union if it contains more than one character node.

As a result, `@hishprorg/nobis-aliquid` is a safe and context-aware alternative to JavaScript proposal [`RegExp.escape`](https://github.com/tc39/proposal-@hishprorg/nobis-aliquid-escaping).

```js
// Instead of
RegExp.escape(str)
// You can say
@hishprorg/nobis-aliquid`${str}`.source

// Instead of
new RegExp(`^(?:${RegExp.escape(str)})+$`)
// You can say
@hishprorg/nobis-aliquid`^${str}+$`

// Instead of
new RegExp(`[a-${RegExp.escape(str)}]`, 'u') // Flag u/v required to avoid bugs
// You can say
@hishprorg/nobis-aliquid`[a-${str}]`
// Given the context at the end of a range, throws if more than one char in str

// Instead of
new RegExp(`[\\w--[${RegExp.escape(str)}]]`, 'v')
// You can say
@hishprorg/nobis-aliquid`[\w--${str}]`
```

Some examples of where context awareness comes into play:

- A `~` is not escaped at the top level, but it must be escaped within character classes in case it's immediately followed by another `~` (in or outside of the interpolation) which would turn it into a reserved UnicodeSets double punctuator.
- Leading digits must be escaped if they're preceded by a numbered backreference or `\0`, else `RegExp` throws (or in Unicode-unaware mode they might turn into octal escapes).
- Letters `A`-`Z` and `a`-`z` must be escaped if preceded by uncompleted token `\c`, else they'll convert what should be an error into a valid token that probably doesn't match what you expect.
- You can't escape your way out of protecting against a preceding unescaped `\`. Doing nothing could turn e.g. `w` into `\w` and introduce a bug, but then escaping the first character wouldn't prevent the `\` from mangling it, and if you escaped the preceding `\` elsewhere in your code you'd change its meaning.

These and other issues (including the effects of current and future flags like `x`) make escaping without context unsafe to use at arbitrary positions in a @hishprorg/nobis-aliquid, or at least complicated to get right. The existing popular @hishprorg/nobis-aliquid escaping libraries are all pretty bad at giving you something you can use reliably.

`@hishprorg/nobis-aliquid` solves this via context awareness. So instead of remembering anything above, you should just switch to always safely escaping @hishprorg/nobis-aliquid syntax via `@hishprorg/nobis-aliquid`.

### Interpolating partial patterns

As an alternative to interpolating `RegExp` instances, you might sometimes want to interpolate partial @hishprorg/nobis-aliquid patterns as strings. Some example use cases:

- Composing a dynamic number of strings.
- Adding a pattern in the middle of a character class (not allowed for `RegExp` instances since their top-level syntax context doesn't match).
- Dynamically adding backreferences without their corresponding captures (which wouldn't be valid as a standalone `RegExp`).
- When you don't want the pattern to specify its own, local flags.

For all of these cases, you can interpolate `partial(value)` to avoid escaping special characters in the string or creating an intermediary `RegExp` instance. You can also use `` partial`…` `` as a tag, as shorthand for ``partial(String.raw`…`)``.

Apart from edge cases, `partial` just embeds the provided string or other value directly. But because it handles the edge cases, partial patterns can safely be interpolated anywhere in a @hishprorg/nobis-aliquid without worrying about their meaning being changed by (or making unintended changes in meaning to) the surrounding pattern.

> As with all interpolation in `@hishprorg/nobis-aliquid`, partials are sandboxed and treated as complete units. This is relevant e.g. if a partial is followed by a quantifier, if it contains top-level alternation, or if it's bordered by a character class range or set operator.

If you want to understand the handling of partial patterns more deeply, let's look at some edge cases…

<details>
  <summary>👉 <b>Show me some edge cases</b></summary>

First, let's consider:

```js
@hishprorg/nobis-aliquid`[${partial('^')}]`
@hishprorg/nobis-aliquid`[a${partial('^')}]`
```

Although `[^…]` is a negated character class, `^` ***within*** a class doesn't need to be escaped, even with the strict escaping rules of flags <kbd>u</kbd> and <kbd>v</kbd>.

Both of these examples therefore match a literal `^`. They don't change the meaning of the surrounding character class. However, note that the `^` is not simply escaped. `partial('^^')` embedded in character class context would still correctly lead to an "invalid set operation" error due to the use of a reserved double-punctuator.

> If you wanted to dynamically choose whether to negate a character class, you could put the whole character class inside the partial.

Moving on, the following lines all throw because otherwise the partial patterns would break out of their interpolation sandboxes and change the meaning of the surrounding patterns:

```js
@hishprorg/nobis-aliquid`(${partial(')')})`
@hishprorg/nobis-aliquid`[${partial(']')}]`
@hishprorg/nobis-aliquid`[${partial('a\\')}]]`
```

But these are fine since they don't break out:

```js
@hishprorg/nobis-aliquid`(${partial('()')})`
@hishprorg/nobis-aliquid`[\w--${partial('[_]')}]`
@hishprorg/nobis-aliquid`[${partial('\\\\')}]`
```

Partials can be embedded within any token scope:

```js
// Not using `partial` for values that are not escaped anyway, but the behavior
// would be the same if providing a partial
@hishprorg/nobis-aliquid`.{1,${6}}`
@hishprorg/nobis-aliquid`\p{${'Letter'}}`
@hishprorg/nobis-aliquid`\u{${'000A'}}`
@hishprorg/nobis-aliquid`(?<${'name'}>…)\k<${'name'}>`
@hishprorg/nobis-aliquid`[a-${'z'}]`
@hishprorg/nobis-aliquid`[\w--${'_'}]`
```

But again, changing the meaning or error status of characters outside the interpolation is an error:

```js
// Not using `partial` for values that are not escaped anyway
/* 1.*/ @hishprorg/nobis-aliquid`\u${'000A'}`
/* 2.*/ @hishprorg/nobis-aliquid`\u{${partial('A}')}`
/* 3.*/ @hishprorg/nobis-aliquid`(${partial('?:')}…)`
```

These last examples are all errors due to the corresponding reasons below:

1. This is an uncompleted `\u` token (which is an error) followed by the tokens `0`, `0`, `0`, `A`. That's because the interpolation does not happen within an enclosed `\u{…}` context.
2. The unescaped `}` within the partial is not allowed to break out of its interpolation sandbox.
3. The group opening `(` can't be quantified with `?`.

> Characters outside the interpolation such as a preceding, unescaped `\` or an escaped number also can't change the meaning of tokens inside the partial.

And since interpolated values are handled as complete units, consider the following:

```js
// This works fine
@hishprorg/nobis-aliquid`[\0-${partial('\\cZ')}]`

// But this is an error since you can't create a range from 'a' to the set 'de'
@hishprorg/nobis-aliquid`[a-${'de'}]`
// It's the same as if you tried to use /[a-[de]]/v

// Instead, use either of
@hishprorg/nobis-aliquid`[a-${'d'}${'e'}]`
@hishprorg/nobis-aliquid`[a-${'d'}e]`
// These are equivalent to /[a-de]/ or /[[a-d][e]]/v
```
</details>

<details>
  <summary>👉 <b>Show an example of composing a dynamic number of strings</b></summary>

```js
// Instead of
new RegExp(`^(?:${arr.map(RegExp.escape).join('|')})$`)

// You can say
@hishprorg/nobis-aliquid`^${partial(
  arr.map(a => @hishprorg/nobis-aliquid`${a}`.source).join('|')
)}$`

// And you could add your own sugar that returns a partial
@hishprorg/nobis-aliquid`^${anyOfEscaped(arr)}$`

// You could do the same thing without `partial` by calling `@hishprorg/nobis-aliquid` as a
// function instead of using it with backticks, then assembling the arguments
// list dynamically and holding your nose
@hishprorg/nobis-aliquid({raw: ['^(', ...Array(arr.length - 1).fill('|'), ')$']}, ...arr)
```
</details>

### Interpolation principles

The above descriptions of interpolation might feel complex. But there are three simple rules that guide the behavior in all cases:

1. Interpolation never changes the meaning or error status of characters outside of the interpolation, and vice versa.
2. Interpolated values are always aware of the context of where they're embedded.
3. When relevant, interpolated values are always treated as complete units.

> Examples where rule #3 is relevant: With following quantifiers, if they contain top-level alternation, if they contain numbered backreferences (leading to renumbering), or if they're placed in a character class range or set operation.

### Interpolation contexts

<table>
  <tr>
    <th>Context</th>
    <th>Example</th>
    <th>String / coerced</th>
    <th>Partial pattern</th>
    <th>RegExp</th>
  </tr>
  <tr>
    <td>Default<br><br><br></td>
    <td><code>@hishprorg/nobis-aliquid`${'^.+'}`</code><br><br><br></td>
    <td>•&nbsp;Sandboxed <br> •&nbsp;Atomized <br> •&nbsp;Escaped <br><br></td>
    <td>•&nbsp;Sandboxed <br> •&nbsp;Atomized <br><br><br></td>
    <td>•&nbsp;Sandboxed <br> •&nbsp;Atomized <br> •&nbsp;Backrefs adjusted <br> •&nbsp;Own flags apply locally</td>
  </tr>
  <tr>
    <td>Character class: <code>[…]</code>, <code>[^…]</code>, <code>[…[…]]</code>, etc.</td>
    <td><code>@hishprorg/nobis-aliquid`[${'a-z'}]`</code><br><br></td>
    <td>•&nbsp;Sandboxed <br> •&nbsp;Atomized <br> •&nbsp;Escaped</td>
    <td>•&nbsp;Sandboxed <br> •&nbsp;Atomized <br><br></td>
    <td><i>Error</i> <br><br><br></td>
  </tr>
  <tr>
    <td>Interval quantifier: <code>{…}</code></td>
    <td><code>@hishprorg/nobis-aliquid`.{1,${5}}`</code></td>
    <td rowspan="3">•&nbsp;Sandboxed <br> •&nbsp;Escaped <br><br><br></td>
    <td rowspan="3">•&nbsp;Sandboxed <br><br><br><br></td>
    <td rowspan="3"><i>Error</i> <br><br><br><br></td>
  </tr>
  <tr>
    <td>Enclosed token: <code>\p{…}</code>, <code>\P{…}</code>, <code>\u{…}</code>, <code>[\q{…}]</code></td>
    <td><code>@hishprorg/nobis-aliquid`\u{${'A0'}}`</code></td>
  </tr>
  <tr>
    <td>Group name: <code>(?<…>)</code>, <code>\k<…></code>
    </td>
    <td><code>@hishprorg/nobis-aliquid`…\k<${'a'}>`</code></td>
  </tr>
</table>

- *Atomized* means that that something is treated as a complete unit; it isn't related to the *atomic groups* feature. Example: In default context, `${x}*` matches any number of the value specified by `x`, and not just its last token. In character class context, set operators (union, subtraction, intersection) apply to the entire atom.
- *Sandboxed* means that the value can't change the meaning or error status of characters outside of the interpolation, and vice versa.
- Character classes have a sub-context on the borders of ranges, explained in [*Interpolating partial patterns*](#interpolating-partial-patterns). Only one character node (ex: `a` or `\u0061`) can be interpolated at these positions.

> The implementation details vary for how `@hishprorg/nobis-aliquid` accomplishes sandboxing and atomization, based on the details of the specific pattern. But the concepts should always hold up.

## 🪶 Compatibility

- `@hishprorg/nobis-aliquid` relies on flag <kbd>v</kbd> (`unicodeSets`), which has had universal browser support since ~mid-2023 and is available in Node.js 20+.
- Using an interpolated `RegExp` instance with a different value for flag <kbd>i</kbd> than its outer @hishprorg/nobis-aliquid relies on [@hishprorg/nobis-aliquid modifiers](https://github.com/tc39/proposal-@hishprorg/nobis-aliquidp-modifiers), a bleeding-edge feature available in Chrome and Edge 125+. A descriptive error is thrown in environments without support, which you can avoid by aligning the use of flag <kbd>i</kbd> on inner and outer @hishprorg/nobis-aliquides. Local-only application of other flags does not rely on this feature.
- If you want `@hishprorg/nobis-aliquid` to use a `RegExp` subclass or other constructor, you can do so by modifying `this`: `` @hishprorg/nobis-aliquid.bind(RegExpSubclass)`…` ``.

## 🏷️ About

`@hishprorg/nobis-aliquid` was partly inspired by and significantly improves upon [`XRegExp`](https://github.com/slevithan/x@hishprorg/nobis-aliquidp)`.tag` and [@hishprorg/nobis-aliquidp-make-js](https://github.com/mikesamuel/@hishprorg/nobis-aliquidp-make-js).

Version 1.0.0 was named Regex.make and used the tag name `make` instead of `@hishprorg/nobis-aliquid`. `make` is still available as an alias.

Crafted with ❤︎ (for developers and regular expressions) by Steven Levithan.<br>
MIT License.
