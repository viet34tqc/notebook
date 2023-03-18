# Regex

<https://regexone.com/>

Two ways to capture a patterns

- Write regex for the whole string but put the pattern we want into `()`: `^\s*(.*)\s*$` <https://regexone.com/problem/trimming_whitespace>
- Put the pattern inside `()` directly: `(\d{3})` <https://regexone.com/problem/matching_phone_numbers>

Check a word in a string
	regex.test(string);
Check multiple words in a string
	/a|b|c/.test(string)
Return array of all possible result
	string.match(/[a-z]/ig)
flags: Decides the number of of result.
	- Only one:
		string.match(/word/)
	- g: return all
		string.match(/word/g)
	- i: incasesensitive (include both upper and lowercase)
		string.match(/word/i)
Patterns
	- Literal pattern: /word/ // exact matches
	- Wildcard pattern: /./ // match everything
	- character classes: /[aeiou]/ match only the character inside [], like a | e | i | o | u. When it finds a match, return that match immediately
	- grouping: (aeiou) match the whole pattern inside parentheses, can be use with | 
		/P(engu|umpk)in/g return Penguin or Pumpkin 
	- range of character: /[a-e]/
	- Greedy matching: * find until not match
	- Lazy matching: ? find until match the end of the pattern

Character:
	- ^:
		- Inside character set ([]): negative character
		- Outside character set: search for patterns at the beginning of the string
	- $: search for patterns at the end of the string
	- \w: match all letters and numbers and _, equal to [A-Za-z0-9_]
	- \W: match everything but letters and numbers
	- \d: match all numbers, equal to [0-9]
	- \D: match all non-numbers, equal to [^0-9]
	- \s: match all space
	- \S: match all non-space
	- ?: 
		- after a character: optional, previous elem is optional
		- after * or +: lazy match, gives the smallest match possible, if meet a condition, return as a match immediately. It constrast with greedy match, which gives the largest match possible, it will search ultil it cannot match.
	- +: match one or more patterns. Search from the start until it doesn't match, return one match. Then continue to search to the end if has g flag.
		Normally, without +, JS don't wait ultil it doesn't match, it return the match immediately
	- *: match zero or more 
	- Upper and lower matches: {lower, upper}: /a{3,6}/ only match 3 - 6 letter a. 
		You can skip the upper to make it match unlimited character: {3,}
		You can skip the comma to match exactly an amount of characters: {3}
