# Regex

<https://regexone.com/>
<https://learnbyexample.github.io/learn_js_regexp/regexp-introduction.html>

We use Regex to:

- check if a string matches a pattern, e.g: check if the string starts or ends with a pattern: `pattern.test(string)`
- check if a string matches a pattern, if so, return the parts of the string that matches the pattern: `const found = string.match(pattern)`
- check if a string matches a pattern and replace the parts of the string that matches the pattern with something else: `string.replace(pattern, somethingElse)`
- check if a string matches a pattern and return that match: `string.match(pattern)[0]` to get the first match or `string.match(/pattern/g)` to get all the matches
- check if a string matches a pattern, if so return the index: `string.search(pattern)`. It's equal to `string.match(pattern).index`
- check if a string matches a pattern and split the string by the parts that matches the pattern: `string.split(pattern)`

So what does 'a string matches a pattern' mean? For me, it's like to check if string *includes* the pattern rather than a specific word, behave the same as `string.prototype.includes()` but for the regex pattern

'A match' here is the part of the string that matches the pattern

## How to create a regexep

- Use `/pattern/`
- Use `new RegExp(pattern), flag`

Using `new RegExp` improves readability because you don't need to escape special characters in the pattern

## Flags

Decides the number of of result.

### `g`

By default, if a string matches a pattern, it returns only the first part of string that matches the patterns. We use `g` flag to return all the matching parts from start to the end of string.

`string.match(/word/g)`: return all the match
`string.replace(/word/g, '')`: replace all the match

### `i`:

`i` flag means incasesensitive, which matches both lowercase and uppercase string

`string.match(/word/i)`

### `m`

Enable multiline mode, is often used with `^`  and `$`. Now, flag `^` and `$` will match to the start and end of every line <https://javascript.info/regexp-multiline-mode>

```js
// check if any line in the string starts with 'top'
/^top/m.test('hi hello\ntop spot') // true

// check if any line in the string ends with 'er'
/er$/m.test('spare\npar\nera\ndare') // false

// check if any complete line in the string is 'par'
/^par$/m.test('spare\npar\nera\ndare') // true
```

### `s`

allow `.` to match `\n` (default `.` ignore `\n`)

## Character inside pattern

- `^`:
  - Inside character set ([]): negative character
  - Outside character set: indicate the end of string, the pattern is checked at the end of string
- `.`: match anything
- `|`: OR condition. For example: `\abc|def\`. Here `abc` and `def` are called regex alternative.
	When using OR condition with `replace`, be carefull of the order. The regexp alternative which matches earliest in the input string will be find and replaced first. If we use `g` flag, Then if there is any matches for the other regexp alternative, it will continue to replace <https://learnbyexample.github.io/learn_js_regexp/alternation-and-grouping.html#precedence-rules>

	```js
	let words = 'lion elephant are rope not' // starting index of 'on' < index of 'ant' for given string input
	// so 'on' will be replaced irrespective of order of alternations
	words.replace(/on|ant/, 'X') //"liX elephant are rope not"
	words.replace(/ant|on/, 'X') //"liX elephant are rope not"

	let mood = 'best years'
	// starting index for 'year' and 'years' will always be same
	// so, which one gets replaced depends on the order of alternations
	mood.replace(/year|years/, 'X') //"best Xs"
	mood.replace(/years|year/, 'X') //"best X"
	```
	
- `[]`: character classes\
	`/[aeiou]`/ match only one of the characters inside `[]`, like `a|e|i|o|u`. 
- `()`: grouping, match the whole pattern inside parentheses, opposite to `[]` which is only match each pattern in the bracket.\
	When combine with `|`: it's similar to `a(b+c)d = abd+acd` in maths, you get a(b|c)d = abd|acd in regular expressions.
	We can use `/1` or `/2` to avoid repeat captured group: `/(\w+)\s\1/` is the same as `/(\w+)\s(\w+)/`
	We can often use captured group with `replace` combine with a function: <https://learnbyexample.github.io/learn_js_regexp/working-with-matched-portions.html#using-function-in-replacement-section>
	`/P(engu|umpk)in/g` return Penguin or Pumpkin.
- `$`: indicate start of string, the pattern is checked from start of string
- `\`: match all letters and numbers and `_`, equal to [A-Za-z0-9_]
- `\b`: indicates the start and end of words. Start of word means either the character prior to the word is a non-word character or there is no character (start of string). It's similar to `^` and `$` but for words in the string (separated by space). 

	```js
	let sample = 'par spar apparent spare part'
	// replace 'par' irrespective of where it occurs
	sample.replace(/par/g, 'X') // "X sX apXent sXe Xt"

	// replace 'par' only at the start of word
	sample.replace(/\bpar/g, 'X') // "X spar apparent spare Xt"

	// replace 'par' only at the end of word
	sample.replace(/par\b/g, 'X') // "X sX apparent spare part"
	```

- `\B` is opposite to `\b`, indicates that this must be a word. Remember that when using replace, `\B` doesn't take into account

	```js
	let sample = 'par spar apparent spare part'

	// replace 'par' if it is not start of word
	sample.replace(/\Bpar/g, 'X') // "par sX apXent sXe part"

	// replace 'par' at the end of word but not whole word 'par'
	sample.replace(/\Bpar\b/g, 'X') // "par sX apparent spare part"

	// replace 'par' if it is not end of word
	sample.replace(/par\B/g, 'X') // "par spar apXent sXe Xt"
	```

- `\W`: match everything but letters and numbers
- `\d`: match all numbers, equal to [0-9]
- `\D`: match all non-numbers, equal to [^0-9]
- `\s`: match all space
- `\S`: match all non-space
- `{}`: match a number of character
	+ `{m,n}`: match m to n times
	+ `{m,}`: match at least m times
	+ `{n}`: match exactly n times
- `??`, `*?`, `+?`: lazy match, gives the smallest match possible, if meet a condition, return as a match immediately. It constrast with greedy match, which gives the largest match possible, it will search to the end or ultil it cannot match.
- `.`: any character
- `+`: greedy quantifier, match one or more patterns. Search from the start until it doesn't match, return one match.
- `*`: greedy quantifier, match zero or more times
- `?`: greedy quantifier, match 0 or 1 times

## Pattern examples

- Literal pattern: `/word/` exact matches
- character classes: `/[aeiou]`/ match only one of the characters inside `[]`, like `a|e|i|o|u`. When it finds a match, return that match immediately
- grouping: 
- range of character: `/[a-e]/`
- Greedy matching: match as much as possible like `*` (search to the end or until fail) and `?` (search up to 1 character first)
- Lazy matching: [https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#non-greedy-quantifiers](https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#non-greedy-quantifiers) match as minimally as possible, `*?` and `??`: `green:3.14:teal::brown:oh!:blue'.split(/:.*?:/)` => `["green", "teal", "brown", "blue"]`

## Tips

### Replace multiple strings with strings

```js
// replace 0xA0 with 0x7F and 0xC0 with 0x1F.
let ip = 'start address: 0xA0, func1 address: 0xC0'
ip.replace(/0xA0/, '0x7F').replace(/0xC0/, '0x1F')
```

### AND conditional

<https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#and-conditional>

```js
/cat.*dog|dog.*cat/.test('cat and dog') // true
/cat.*dog|dog.*cat/.test('dog and cat') // true
```

### Two ways to capture a patterns

- Write regex for the whole string but put the pattern we want into `()`: `^\s*(.*)\s*$` <https://regexone.com/problem/trimming_whitespace>
- Put the pattern inside `()` directly: `(\d{3})` <https://regexone.com/problem/matching_phone_numbers>

### Using dictionary in `replace` section

```js
let h = { '1': 'one', '2': 'two', '4': 'four' }

'9234012'.replace(/1|2|4/g, k => h[k]) // "9two3four0onetwo"
```

### Extract a matching portion
