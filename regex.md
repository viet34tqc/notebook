# Regex

<https://regexone.com/>
<https://learnbyexample.github.io/learn_js_regexp/regexp-introduction.html>

Two ways to capture a patterns

- Write regex for the whole string but put the pattern we want into `()`: `^\s*(.*)\s*$` <https://regexone.com/problem/trimming_whitespace>
- Put the pattern inside `()` directly: `(\d{3})` <https://regexone.com/problem/matching_phone_numbers>

Check a word in a string
	`regex.test(string);`
Check multiple words in a string
	`/a|b|c/.test(string)`
Return array of all possible result
	`string.match(/[a-z]/ig)`
flags: Decides the number of of result.
	- `g`: return all
		`string.match(/word/g)`: return all the match
		`string.replace(/word/g, '')`: replace all the match
	- `i`: incasesensitive (include both upper and lowercase)
		`string.match(/word/i)`
	- `m`: enable multiline mode, it only affect `^`  and `$` <https://javascript.info/regexp-multiline-mode>
	- `s`: allow `.` to match `\n` (default `.` ignore `\n`)
Patterns
	- Literal pattern: `/word/` exact matches
	- Wildcard pattern: `/./` match everything
	- character classes: `/[aeiou]`/ match only one of the characters inside `[]`, like `a|e|i|o|u`. When it finds a match, return that match immediately
	- grouping: match the whole pattern inside parentheses, can be use with |, opposite to `[]` which is only match each pattern in the bracket
		`/P(engu|umpk)in/g` return Penguin or Pumpkin 
	- range of character: `/[a-e]/`
	- Greedy matching: match as much as possible like `*` (search to the end or until fail) and `?` (search up to 1 character first)
	- Lazy matching: [https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#non-greedy-quantifiers](https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#non-greedy-quantifiers) match as minimally as possible, `*?` and `??`: `green:3.14:teal::brown:oh!:blue'.split(/:.*?:/)` => `["green", "teal", "brown", "blue"]`

Character:
	- `^`:
		- Inside character set ([]): negative character
		- Outside character set: search for patterns at the beginning of the string
	- `$`: search for patterns at the end of the string
	- `\`: match all letters and numbers and `_`, equal to [A-Za-z0-9_]
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
	- `+`: match one or more patterns. Search from the start until it doesn't match, return one match. Then continue to search to the end if has g flag.
		Normally, without +, JS don't wait ultil it doesn't match, it return the match immediately
	- `*`: greedy quantifier, match zero or more times
	- `?`: greedy quantifier, match 0 or 1 times
	- Upper and lower matches: {lower, upper}: /a{3,6}/ only match 3 - 6 letter a. 
		You can skip the upper to make it match unlimited character: {3,}
		You can skip the comma to match exactly an amount of characters: {3}

## Replace multiple strings with strings

```js
// replace 0xA0 with 0x7F and 0xC0 with 0x1F.
let ip = 'start address: 0xA0, func1 address: 0xC0'
ip.replace(/0xA0/, '0x7F').replace(/0xC0/, '0x1F')
```

## AND conditional

<https://learnbyexample.github.io/learn_js_regexp/dot-metacharacter-and-quantifiers.html#and-conditional>

```js
/cat.*dog|dog.*cat/.test('cat and dog') // true
/cat.*dog|dog.*cat/.test('dog and cat') // true
```

