# Regular Expression in Javascript

## Working with string and regex

Following are the methods to work with regex and strings.

### regexp.test(str)

The method `regexp.test(str)` looks for at least one match, if found, returns true, otherwise false.

```js
(/World/).test("Hello World")	// true
```

### str.match
A regexp returns **only first match**:

```js
"123".match( /\d/ )  // '1'
```

Multiple flags are possible. For example, **find all matches and ignore the case**:

```js
"Smile a smile".match( /SMILE/gi )  // 'Smile', 'smile'
```

### str.replace

This method allows to do a replacement at the point where a match happens. The follow options are provided for the replacement:

|Symbols	| Action in the replacement string |
| --------- | -------------------------------- |
| $&	    | inserts the whole match |
| $`	    | replace matched text by appending string before the match to the replacement string |
| $'	    | replace matched text with replacement string |
| $n	    | if n is a 1-2 digit number, then it inserts the contents of n-th parentheses, more about it in the chapter Capturing groups |
| $<name>	| inserts the contents of the parentheses with the given name, more about it in the chapter Capturing groups |
| $$	    | replace matched text with character $ |
--------------------------------------------------

```js
"Good Morning".replace(/Morning/, "$& Jagdeep") 
// Good Morning Jagdeep

"Good Morning".replace(/Morning/, "$` Jagdeep") 
// Good Good  Jagdeep

"Good Morning".replace(/Morning/, "$' Jagdeep") 
// Good  Jagdeep
```

## Flags
In JavaScript, there are three flags:

* `g` - Find all matches.
* `i` - Case-insensitive search.
* `m` - Enable multiline mode.

### Find all matches

if the global flag is used, **all matches** can be found:

```js
"123".match( /\d/g )  // '1', '2', '3'
```

### Case insensitive search

```js
(/world/i).test("Hello World")	// true
```

### Multiline search

```js
str = `
1 9
2 8
3 7`;
str.match(/^\d/gm/)		// '1', '2', '3'
str.match(/\d$/gm)		// '9', '8', '7'
```

## Anchors
The caret ^ and dollar $ characters have special meaning in a regexp. They are called “anchors”.

### string starts with match using `^`
The caret ^ matches at the beginning of the text:

```js
(/^Good/).test("Good morning Jagdeep")		// true
```

### string ends with match using `$`

```js
(/Jagdeep$/).test("Good morning Jagdeep")	// true
```

### testing for full match

```js
(/^\d\d:\d\d$/).test("12:34")	// true
(/^\d\d:\d\d$/).test("1:34")	// false
```

### word boundry `\b`

The match is based on word boundery based on following positions:

* At string start
* Between two character where one matches `\w`, while other doesn't
* At string end

This allows you to do an **exact word match**. Consider the following example:

```js
"Hello, Java!".match(/\bJava\b/)		
// [ 'Java', index: 7, input: 'Hello, Java' ]

"Hello, Java".match(/\bJava\b/)			
// [ 'Java', index: 7, input: 'Hello, Java' ]

"Hello, Java".match(/\bHello\b/)		
// [ 'Hello', index: 0, input: 'Hello, Java' ]
```

The same regex would not match the following string:

```js
"Hello, Javascript".match(/\bJava\b/)	// null
"Hello, Javascript".match(/\bHell\b/)	// null
```

## Alternation (OR) |

We can add an OR condition the the regex pattern using `|`

```js
"<head><title><body><span>".match(/<head|body|span>/g)
// ["<head", "body", "span>"]
```

## Character set and ranges (and classes)

### Character set

Match any of the one charecter our of the set specified:

> [abc]

```js
(/[aA][mM]/).test("10:30 AM")	// true

(/[aA][mM]/).test("10:30 am")	// true

(/[aA][mM]/).test("10:30 Am")	// true
```

### Character ranges

Match any of the one charecter our of the range specified:

> [a-z]

Each specified matches only one charecter of the charecter range or set specified
	
```js
(/x[A-F]/).test("xA")		// true

(/x[A-F]/).test("xAA")		// false

(/x[A-F]/).test("xAB")		// false
```

### Character set and ranges

Characters, classes and ranges can be put together.

```js
("Pa$$w0rd#er3").match(/[\dA-Fa-fx]/g)	
// [ 'a', '0', 'd', 'e', '3' ]
```

Square brackets starting with a caret: `[^...]` find all characters **except the given ones**.

* [^aeou] - any character except ‘a’,’e’,’o’,’u’
* [^0-9] - any non-digit, same as \D
* [^\s] - any not-a-space, same as \S

```js
("Pa$$w0rd#er3").match(/[^\dA-Fa-fx]/g)	
// [ 'P', '$', '$', 'w', 'r', '#', 'r' ]
```

### Character classes

These are the shorthand notation for the certain character ranges. Character classes begin with a backslash `\`

* `\d` - A digit, any character from 0 to 9
* `\s` - A whitespace character, like tab, newline etc.
* `\w` - A symbol of Latin alphabet or a digit or an underscore `_`

### Examples

```js
("+7(903)-123-45-67").match(/\d/g).join('')		
// [ '7', '9', '0', '3', '1', '2', '3', '4', '5', '6', '7' ]
// 79035419441

("J,A,G,D,E,E,P").match(/\w/g).join("")			
// [ 'J', 'A', 'G', 'D', 'E', 'E', 'P' ]
// JAGDEEP

("4074 4885 4814 8085").match(/\s/g).length			
// [ ' ', ' ', ' ' ]
// 3
```

## Quantifiers

### Numeric Quantifiers

* Character count can be specified using `{n}` syntax.
	* `\d{4}` means 4 digits, same as `\d\d\d\d`.

```js
// Remove space from credit card number
"4514 2345 5432 1234".match(/\d{4}/g).join("")
// ["4514", "2345", "5432", "1234"]
// 4514234554321234
```

* To find 3 to 5 digit numbers, specify two numbers in figure brackets: `\d{3,5}`
		
```js
// Remove space from diners club card number
"3004 327725 3249".match(/\d{4,6}/g).join("")
// ["3004", "327725", "3249"]
// 30043277253249
```

* 4 or more digits will be `\d{4,}`

```js
"4514 2345 5432 1234".match(/\d{4,}/g).join("")
/// ["4514", "2345", "5432", "1234"]
// 4514234554321234

"3004 327725 3249".match(/\d{4,}/g).join("")
// ["3004", "327725", "3249"]
// 30043277253249
```

### Shorthand for numberic quantifiers

Quantifiers `+`, `*` and `?`.

* `+` - Means “One or more”, same as `{1,}`.
* `?` - Means “Zero or one”, same as `{0,1}`. Makes a character optional.
* `*` - Means “Zero or more”, same as `{0,}`.

```js
// Say we want to match number of html begin tags vs close tags
"<div><span>hello world</span></div>".match(/<[a-z]+>/gi).length	// 2
"<div><span>hello world</span></div>".match(/<\/[a-z]+>/gi).length	// 2

/* 
 * Match a whole or decimal number 
 */
"100".match(/[\d]*[\.]?[\d]*/)		
// ["100", index: 0, input: "100"]

"10.05".match(/[\d]*[\.]?[\d]*/)
// ["10.05", index: 0, input: "10.05"]
```

## Escaping special charecters

Following is the list of special charecters in a regular expression:

|       |       |
| ----- | ----- |
|  `^`  |  `$`  |
|  `.`  |  `?`  |
|  `+`  |  `*`  |
|  `\`  |  `|`  |
|  `(`  |  `)`  |
|  `[`  | 		|
|       |       |

When you are actually looking to match any of the above listed charecter, you will have to use an escape charecter i.e. backslash `\`

### Examples

```js
"^_^".match(/\^/g)				
// [ '^', '^' ]

"Price $100".match(/\$\d\d\d/)	
// [ '$100', index: 6, input: 'Price $100' ]

"Chapter 5.1".match(/\d\.\d/)	
// [ '5.1', index: 8, input: 'Chapter 5.1' ]

"Is it?".match(/\?$/)			
// [ '?', index: 5, input: 'Is it?' ]

"1 + 1".match(/\+/)				
// [ '+', index: 2, input: '1 + 1' ]

"Love you :*".match(/:\*/)		
// [ ':*', index: 9, input: 'Love you :*' ]

/* 
 * We need to escape \ while constructing string with \, 
 * as its a escape charecter for string as well 
 */
":-\\".match(/:-\\/)			
// [ ':-\\', index: 0, input: ':-\\' ]

"AM | PM".match(/\|/)			
// [ '|', index: 3, input: 'AM | PM' ]

":-(".match(/:-\(/)				
// [ ':-(', index: 0, input: ':-(' ]

":-)".match(/:-\)/)				
// [ ':-)', index: 0, input: ':-)' ]

"Array: [1,2]".match(/\[/g)		
// [ '[', index: 7, input: 'Array: [1,2]' ]
```

### In Charecter classes

* Most special characters can be used in square brackets without escaping.
* The hyphen `-` must be escaped only if it’s in-between other symbols. If it **first or last**, then it may not denote a range, and hence can come unescaped: 

> `[-...]`.

* The caret symbl `^` must be escaped only if it’s the **first symbol** `

> [\^..]`.

* All other characters, including dot `.`, plus `+`, brackets `( )`, opening square bracket `[` etc can appear **unescaped**.
* The regexp `[-().^]` literally means any of characters from the list `-().^`. Regexp special symbols **do not have any special meaning here**.

> `[-().^]`

## The searching algorithm
### In greedy (default) mode

In the greedy mode (by default) a quantifier is repeated as many times as possible.
	
```js
"a 'witch' and her 'broom' is one".match(/'.*'/g)
// ["'witch' and her 'broom'"]
```

* Regex engine first matches `'` at 3rd charecter.
* It then matches another charecter based on `.` specified in the regex.
* It then repeats the above step based on `*` quantifier until the string ends.
* The text is finished, so the dot matching stops.
* So, the engine starts backtracking to look for match for the next charecter in regex `'`.
* The match ends when engine finds the first `'` from end of the string.

### Lazy mode

In the greedy mode (by default) a quantifier is repeated the least number of times. We can switch the quantifier ‘?’ to lazy mode, adding the question mark after a quantifier `*` or `+`:

Here the search is looking for zero or more charecter between first and second `'` in the regex specified. So, if it finds first `'` then the search doesnt try to match for `.*` at all as minimum match is zero for it. So, it is looking for another `'` after the first `'` is matched.

```js
"a 'witch' and her 'broom' is one".match(/'.*?'/g)
// ["'witch'", "'broom'"]
```

Here the search looks for at least one charecter in between first and second `'` in the regex specified. So, if it finds first `'` then the search tries to match for `.+` once as minimum match is zero for it. So, after engine finds just one charecter after the first `'` is matched, it starts looking for next `'`.

```js
"a 'witch' and her 'broom' is one".match(/'.+?'/g)
// ["'witch'", "'broom'"]
```

## Capturing Groups

A part of pattern enclosed within a parenthesis `(<pattern-here>)` is called capturing group.

```js
/* 
 * get the html start tags 
 */
"<div><span>title</span></div>".match(/(<[\w-]*>)/)
// [ '<div>', '<div>', index: 0, input: '<div><span>title</span></div>' ]

```

Using the capturing groups we can construct more complex patterns.

```js
/*
 * check if html tree leaf element
 */
"<div><span>title</span></div>".match(/(<[\w-]*>)([\w]*)(<\/[\w-]*>)/)
// [ '<span>title</span>', '<span>', 'title', '</span>', 
//	index: 5, input: '<div><span>title</span></div>' ]
```

The above pattern return a match for each capturing group as well. And each result is ordered based on the position of the capturing group in the whole regex. If a capturing group doesnt match, it would still have a place in the result but with `undefined` value.

```js
'a'.match(/a(b)?(c)?/)
// [ 'a', undefined, undefined, index: 0, input: 'a' ]
```

### With global search - `g`

When a global search is done, the result omits matches for each capturing group:

```js
'a'.match(/a(b)?(c)?/)
// [ 'a' ]
```

To get result for capturing groups as well with global search, we use `matchAll` method. It gives array of array, where each array element contains details of each match with match for each capturing group:

```js
/*
 * Returns a pseudoarray
 */
const matches = "<div><span>title</span></div>".matchAll(/(<[\w-]*>)/g)
// RegExpStringIterator {}

Array.from(matches);
// [Array(2), Array(2)]
```

### Named groups

A group name can be specified for a capturing group using `?<name>`.

This gives us access to each capturing group based on group name:

```js
/*
 * "DD-MM-YYYY" 
 */
result = "01-01-2020".match(/(?<DD>[\d]{2})-(?<MM>[\d]{2})-(?<YYYY>[\d]{4})/)
// ["01-01-2020", "01", "01", "2020", index: 0, input: "01-01-2020", groups: {…}]

result.groups
// {DD: "01", MM: "01", YYYY: "2020"}
```

### Non-capturing groups

To prevent a capturing group match from showing up in the result, we use `?:`

```js
"Name - Jagdeep".match(/(?:Name -)([a-z\s]*)/i)
// ["Name - Jagdeep", " Jagdeep", index: 0, input: "Name - Jagdeep", groups: undefined]
```

## Backreferences

We can use backreferences to use the result of capturing group within the regex pattern itself.

Consider the following:

```js
`He said: "She's the one!"`.match(/['"](.*?)['"]/g)
// "She'
```

If we plan to match string within `"  "` or `'  '` the above regular expression will match the intended as well as `"  '` or `'  "`.

### By position number `\N`

To match the closing quote same as the starting one:

```js
`He said: "She's the one!"`.match(/['"](.*?)['"]/g)
// "She's the one!"
```

### By name `\k<name>`

To match the closing quote same as the starting one, using group name:

```js
`He said: "She's the one!"`.match(/(?<quote>['"])(.*?)\k<quote>/g)
// "She's the one!"
```

## lookahead and lookbehind (aka lookaround)

------------------------------------------------------------------
| Pattern 		| type				    | matches 			     |
|---------------|-----------------------|------------------------|
| X(?=Y)	    | Positive lookahead	| X if followed by Y     |
| X(?!Y)	    | Negative lookahead	| X if not followed by Y |
| (?<=Y)X	    | Positive lookbehind	| X if after Y           |
| (?<!Y)X	    | Negative lookbehind	| X if not after Y       |
------------------------------------------------------------------

### lookahead

If you are using a capturing group after a pattern and want the pattern to match only when capturing group pattern is also matched, you can use lookahead as follows:

```js
"1 turkey costs 30€".match(/\d+(?=€)/)
// ["30", index: 15, input: "1 turkey costs 30€", groups: undefined]
```

**NOTE:** The result will not contain the match text from the capturing group using `(?=...)`

You can negate the search to return results that doesnt match pattern using **negative lookahead pattern**:

```js
"1 turkey costs 30€".match(/\d+(?!€)/)
// ["1", index: 0, input: "1 turkey costs 30€", groups: undefined]
```

### lookbehind

Similar to lookahead, we can do a match using the capturing group before the pattern.

```js
"1 turkey costs $30".match(/(?<=\$)\d+/)
// ["30", index: 16, input: "1 turkey costs $30", groups: undefined]
```

**NOTE:** The result will not contain the match text from the capturing group using `(?<=...)`

You can negate the search to return results that doesnt match pattern using **negative lookbehind pattern**:

```js
"1 turkey costs 30€".match(/(?<!\$)\d+/)
// ["1", index: 0, input: "1 turkey costs 30€", groups: undefined]
```

### include lookaround pattern result

Just adding extra parenthesis would allow capturing group match also be included into the result:

```js
"1 turkey costs 30€".match(/\d+(?=€)/)
// ["30", "€", index: 15, input: "1 turkey costs 30€", groups: undefined]

"1 turkey costs $30".match(/(?<=(\$))\d+/)
// ["30", "$", index: 16, input: "1 turkey costs $30", groups: undefined]
```

## Catastrophic Backtracking

A regular expression match can make the regex engine or a javascript process to hang in some cases. Consider the following regular expression that tries to match multiple words with spaces in between:

```js
const str = "A very long input string which ends with a not matching charecter in the end !";
(/^(\w+\s)*$/).test(str)
```

`\w+` is an greedy matcher. And that with `*` creates a lot of combinations. A string with 30 charecters would create a billion combinations.

So, to reduce the number of combinations by updating the regex.

### Change expression to end differently

```js
(/^(\w+\s)*\w*$/).test(str)
```

### Repeating the greedy expressions

```js
(/^(\w+\s\w+\s)*\w*$/).test(str)		// TODO Doesnt work correctly
```

### Using lookahead

```js
(/(?=(\w+))\1/).test(str)
```

## Function `regex.exec`

Every search done using `regex.exec` stores the last match index in `lastIndex`.

```js
let str = 'let varName';

let regexp = /\w+/g;
console.log(regexp.lastIndex); // 0 (initially lastIndex=0)

let word1 = regexp.exec(str);
console.log(word1[0]); // let (1st word)
console.log(regexp.lastIndex); // 3 (position after the match)

let word2 = regexp.exec(str);
console.log(word2[0]); // varName (2nd word)
console.log(regexp.lastIndex); // 11 (position after the match)

let word3 = regexp.exec(str);
console.log(word3); // null (no more matches)
console.log(regexp.lastIndex); // 0 (resets at search end)
```

### Flag `y`

Flag y makes regexp.exec to look exactly at position lastIndex, not before, not after it.

```js
let str = 'let varName = "value"';

let regexp = /\w+/y;

regexp.lastIndex = 3;
alert( regexp.exec(str) ); // null (there's a space at position 3, not a word)

regexp.lastIndex = 4;
alert( regexp.exec(str) ); // varName (word at position 4)
```

## References

[javascript.info](http://javascript.info/tutorial/regular-expressions-javascript)