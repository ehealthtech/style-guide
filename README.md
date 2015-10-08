# JavaScript Style Reference

## Overview

This style guide is different. There are no hard rules (except in obvious cases). This is a *reference* for you to consult when making decisions on style.

The document lays out suggestions within style categories. The suggestions should reflect common developer practice within this organization.

One good strategy when deciding on a style is to simply *ask your peers what they do*.

#### Why?

A lot of developer time is spent reading other people's code. Many developers spend *all* of their time reading other people's code. If you knew that other people were reading what you've written, you would probably want to make what you've written clear. Writing in a consistent way -- both syntactically *and* semantically -- helps with understanding. If symbolic patterns have predictable meanings, and the way they are ordered follows a predictable pattern, information is easier to absorb.

#### Considerations

- Some have suggested that developers should be able to write in *any* style they like, with "normalization" being left to be done automatically at the build stage. This is an interesting proposal.
- The importance of a given style convention can vary from team to team. The "correct" style guide does not exist -- there are only agreed-upon conventions within specific contexts(teams).
- A particular developer's style indicates exactly nothing about that developer's skill.
- You should try to find agreement with your peers, and that always means negotiation.
- You should always aim for consistency, whatever your final style choice may be.

#### Further Reading

There are other good, more opinionated, style guides available. You might want to look them over prior to making decisions:

* [Google's Javascript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml)
* [Idiomatic.js](https://github.com/rwaldron/idiomatic.js/)
* [JQuery's Style Guide](http://contribute.jquery.org/style-guide/js/)
* [Felix's Node.js style guide](http://nodeguide.com/style.html).

Note that there are differences between how clientside and serverside Javascript should be written. It is a good idea to agree on how to handle these differences.

## Variables

It's always a good idea to have a clear and consistent idea of how variables are declared. These "little words" are peppered throughout any codebase, and you will probably have a better time when debugging if what these words reference or imply is obvious.

#### Naming

Try to be descriptive. Avoid single-character variable names, like `i` or `x`. Remember that most code will be minified, so you aren't "saving bytes" by writing short variable names. These are common patterns seen in JavaScript code:

	functionNamesLikeThis
	variableNamesLikeThis
	ConstructorNamesLikeThis
	EnumNamesLikeThis
	methodNamesLikeThis
	SYMBOLIC_CONSTANTS_LIKE_THIS
	event-names
	css-class-names

You are strongly advised to follow a consistent naming scheme, like the one above.

##### A note on constants

ES6 defines the keyword `const`, implementing the non-mutable features of constants. This is not universally supported, and where it is, `const` has block scope (which differs from how "traditional" UPPERCASE_CONSTANTS have been used in JavaScript).  You may want to form a policy on how to understand constants.

##### Other conventions

The above conventions are mostly uncontroversial. There are some newer patterns that aren't as universally accepted.

Using an underscore ( _ ) to indicate a private variable:

	function User() {
		_userId = 123;
		this.getId = function() {
			return _userId;
		}
	}

	*Note that adding an underscore to a variable name does not actually make it private. This is mentioned only to point out that this artifical construct is making a false promise about a concept that JavaScript doesn't actually understand. It may not be worth the trouble.

Prefixing jQuery variables with a dollar sign ( $ ):

	function clickHandler() {
		var element = this;
		var $element = $(this);
	}

Prefixing `boolean` values with `is` or `has`:

	var isReady = true;
	var hasReference = false;

With acronyms, use uppercase:

	myHTML
	aJSONObject

Filenames should be all lowercase, and contain no punctuation other than `-` or `_`. You should also set a preference between these two, for consistency. That is, declare a preference for `-` (or `_`), and stick to it.

#### To var or not to var

Some [have suggested](http://javascript.crockford.com/code.html) that each variable declaration should be made with its own `var` keyword:

	var beep;
	var boop;
	var bam;

Others argue for using commas:

	var beep,
		boop,
		bam

Or, similarly, for a "comma first" syntax:

	var beep
	,	boop
	,	bam

Consistency in the way that variables are declared will help with creating maintainable codebases. Those who argue for the comma syntax usually suggest that better readability results. Not surprisingly, those who argue for the per-variable `var` syntax find that style more readable!

It is perfectly clear what is happening when `var` precedes a variable name, and a semicolon closes that assignment. The suggestion is to use this syntax. Regardless of your choice, be consistent.

#### When to declare variables

Variable hoisting in JavaScript can confuse developers new to the language -- and often those not new to the language! In general, a programmer likes to know where variables were first declared.

JavaScript allows variables to be declared almost anywhere. For this reason, it is a very good idea to have consistent variable declaration guidelines. A good one is to insist that *all* variables be declared at the top of their containing scope:

	function doSomething() {
		var oneThing;
		var anotherThing;
		var increment = 0;

		// code

		// Do not declare any variables down here!
	}

While it may seem tedious at times to do this, a codebase with a consistent declaration style helps with debugging, and makes it easier for others to use your code blocks in other codebases.


## Semicolons

Making an informed decision on whether to use semicolons "everywhere", or not, requires a clear understanding of how the JavaScript grammar is applied when parsing a program. The rules are not very complicated (and the reader is encouraged to learn the details!).

However, every developer will not spend time to learn them, for any number of reasons. If a developer lacking an in-the-bones understanding of how semicolon insertion is defined by the grammar may join your project, you should enforce some style rules. Douglas Crockford encourages *always* using semicolons, mainly to avoid surprises and encourage consistency. This is a good strategy.

#### Further reading
- [The research](http://inimino.org/~inimino/blog/javascript_semicolons)
- [Isaac Schlueter offers a strong argument generally *against* semicolon usage](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)
- [Rod Vagg takes a look](http://dailyjs.com/2012/04/19/semicolons/)
- [Brendan Eich chimes in](https://brendaneich.com/2012/04/the-infernal-semicolon/)

## Functions

Follow the naming conventions for functions, described above in the section on variable naming. Remain consistent -- it is very important to have functions, and methods, defined and described in a consistent way.

#### Declarations within blocks

Don't do this:

	if(x) {
		function foo() {};
	}

Instead, do this:

	if(x) {
		var foo = function() {};
	}

ECMA-262 does not support the first pattern, and for this reason both current and future implementations cannot be relied upon to produce predictable results.

#### Long signatures

Some function calls require several arguments to be passed. As well, sometimes those arguments are "dynamic" and can take up space. Consider the following (silly) example:

	return myFunction(someVariable, anotherVar + "_local", anotherVariable, new Date().getTime())

Some may feel this is hard to read. You might want to determine a style for long signatures. For example:

	return myFunction(
		someVariable,
		anotherVar + "_local",
		anotherVariable,
		new Date().getTime()
	)

Additionally, you might want to decide whether value generation should be done within the call signature itself, or referenced:

	return myFunction(
		someVariable,
		combinedVar,
		anotherVariable,
		currentTime
	)

#### Early returns

There is nothing wrong with allowing a function to exit (return) in more than one place. Compare the following two (equivalent) functions:

	function beepOrBoop() {
		var returnValue;
		if(beep) {
			returnValue = "beep";
		} else if(boop) {
			returnValue = "boop";
		}
		return returnValue;
	}

	function beepOrBoop() {
		if(beep) {
			return "beep";
		}
		return "boop";
	}

On the other hand, a function with several termination points may be harder to debug. It is useful to develop guidelines, such as "if your method is exiting in more than three places more than one function is necessary".

## Getters and Setters

#### Naming

It is not necessary to use getters/setters for object properties. If you do use them, it is suggested that you follow the following naming conventions:

	getThing(); // getter
	setThing(); // setter
	isThing(); // booleans may choose this format

#### No side effects

Getters should have no side effects. They should return values, never alter values.  This is bad:

	var foo = {
    	get next() {
        	document.body.innerHTML = '&lt;p&gt;BOOM&lt;/p&gt;';
    	}
	};

## Whitespace and Indentation

Indentation style decisions require a conclusion on (at least):

1. How many units of indentation;
2. Whether those units are composed of tabs or spaces.

It is unusual to have an odd number of units. The typical choice is between `2` and `4`.

Consistency on spaces or tabs is important, as editors and other tools for displaying code may behave very differently for each case. Sometimes these differences are extreme, leading to nearly unreadable code within an editor.

Mixing styles can be very problematic. Getting a team to agree to *always* follow one or another whitespace convention delivers real benefits. Once you have agreement, ensure that *every editor you use* follows the same convention.

#### Ternary operators

Conditionals normally express a good amount of logic, and can span many lines. Ternary operators work well for concise logic forks:

	var authorized = loggedIn ? true : false;

These can become more convoluted when a lot of logic is being expressed:

	var superAuthorized = isLoggedIn({user: "sandro"}) ? isAdmin ? true : false : false;

Using indentation and multiple lines can help understanding:

	var superAuthorized = isLoggedIn({user: "sandro"})
                          ? isAdmin
                              ? true
                              : false
                          : false

With these constructs, if the logic becomes longer and more complex, consider using `if…else` blocks instead.

## Curly braces

There are ways to avoid using curly braces when writing JavaScript code, such as:

	if(condition) doThis();
	thenDoThis();

Decide whether this compactness is a good or a bad thing. It is unclear whether the above is more readable than:

	if(condition) {
		doThis();
	}
	thenDoThis();

As well, remember that others will be modifying your code. Having written:

	if(foo)
    	beep();

	// Equivalent to
	if(foo) {
		beep();
	}

It is very likely that someone else might misunderstand your intention, and introduce the following tricky bug:

	// Added with the wrong idea of where the block closes:
	if(foo)
    	beep();
    	boop();

    // Assuming this construct is what the compiler will see:
  	if(foo) {
		beep();
		boop();
	}

	// Yet misunderstands what the compiler actually sees:
	if(foo) {
		beep();
	}
	boop();


## Strict Mode

It is important to decide on whether to use `strict` mode or not, and to then be consistent (always; never). The following links offer some guidance:

- [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode)
- [Nicholas Zakas on why to use strict](http://www.nczonline.net/blog/2012/03/13/its-time-to-start-using-javascript-strict-mode/)
- [Microsoft provides a concise overview](http://msdn.microsoft.com/en-us/library/br230269\(v=vs.94\).aspx)

It should also be noted that the code security and maintainability benefits of `strict` are hard to argue against. Old code that fails under `strict` should be refactored.

## Comments

Perhaps the most important take away from this entire reference is the suggestion to **always comment your code as you write it**. You will not come back later to do it, no matter what you tell yourself. Others will not do it for you.

After several months you will forget how your not-documented code works. Do yourself the favor of being kind to future you, and describe what your code is doing, and why it is doing that.

You generally want to comment on parts of your code that someone new to it wouldn't easily understand or couldn't know.

Describe what the method does, not how it works. Try to describe the reasoning behind your solution. You are trying to make your code more understandable, not teach the viewer how to program.  Teach:

1. What does this function do?
2. How is it used?

Where possible, provide an example. You are encouraged to use an expressive syntax for commenting methods, such as `jsDoc`.

On larger teams, if you are adding to code comments, mark your changes with a handle, such as your email address, or your name. Decide on this protocol with the rest of your team and stick with it.

Avoid obvious comments:

	// 09-09-2013 (John Doe): Changed this method to use Beep instead of Boop

And of course: try to write good, clear code that needs minimal (or no) documentation! While it is unlikely you will write code so clean that comments become unnecessary, it's always a good goal to strive for.

#### Multi-line or single-line

Choose between the two options for creating comments in JavaScript:

	/*
	    Comments here
	*/

	// Comment here
	// Comment here

There is nothing dangerous about using both. Indeed, these styles are intended to handle two cases -- single vs. multi-line comments. What is more important is to have a standard style for documenting your project code, for one, or both, of these.

Some developers prefer to use one or the other exclusively. One advantage of using double-slash comments exclusively is the ability to do this when debugging:

	/*
		// Commenting out a chunk of commented code
		//
		foo();
	*/

Whereas this causes a syntax error:

	/*
		/*
			nesting is illegal
		*/
		foo();
	*/

#### jsDoc and others

This is one way to document a method:

	/*
 	 * @param	{Number}	age		The age for this users
 	 */
	function updateAge(age) { … }

This is the `jsDoc` style. If you don't already have your own descriptive syntax, [give jsDoc a try](http://en.wikipedia.org/wiki/JSDoc)

It is generally a good idea to explicitly describe method signatures, for two reasons:

1. Those reading the code benefit from clear instructions
2. Those writing the code are forced to think about what they have wrought
3. Automated documentation engines can automatically generate documentation from this syntax, even using to to automatically validate/test the method.

Nevertheless, very few developers enjoy writing documentation. Demanding that a developer do may lead to snide or otherwise resentful documentation. It is best to get consensus on the value of documentation, develop very few simple rules, and let peers work to help each other with running commentary on their code.

## Dragons
#### Wrapper objects
Primitive types are usually best declared through literal assignment rather than native constructors. Consider:

	var isTrue = new Boolean(false);
	if(isTrue) {
  		alert(true);  // Shows 'true'.
	}

Understanding why this code doesn't deliver the expected behavior (not alerting `true`) might require a depth of JavaScript knowledge that some may not have. There doesn't seem to be any advantage over simply assigning `false` to `isTrue`. As well, space is saved by using literals:

	var a = [];
	var b = {};
	var c = 23;
	var d = true;

Where possible, try to speak without embellishment -- be literal.

#### Multiline string literals

Whether or not the following method of spreading a string definition across lines is allowed is something to consider:

	var longString = "Let us go then, you and I,\
	when the evening is spread out against the sky\
	like a patient etherized upon a table."

Multiline strings can cause problems, such as breaking certain minifers. As well, you are introducing control characters whose characteristics are invisible to you: a space, a tab, a newline, are all just whitespace to the human eye. Other parsing systems may also become confused.

The ECMAScript specification [does not support this style](http://bclary.com/2004/11/07/#a-7.8.4):

Additionally, if you find yourself writing huge chunks of text in JavaScript, you may be doing something wrong.

If possible, consider the new ES6 `template strings` (aka [`quasi-literals`](http://www.2ality.com/2011/09/quasi-literals.html)) as a non-problematic strategy for creating multiline strings.

#### Short Curcuiting ( && || )

Another way to write this:

	if(beep) {
		boop();
	}

is this:

	beep && boop();

Such a style becomes harder to follow rather quickly:

	(beep() || boop()) && bap()

A chain of such conditional statements can easily lead to logic errors if you aren't careful, especially w/r/t operator precedence.

Consider whether the saved keystrokes (and you aren't saving many) are worth the savings. A simple case isn't hard to follow, but the pattern rapidly folds into more complexity.

#### Coercion

Type coercion is a powerful action that can have a significant effect on your program's logic, and you should try very hard to enforce a *strict* style guide for these operations. If you're going to worry about types, you should then be very strict on how they are created/coerced.

The following mapping should satisfy most needs:

- value -> String :: \<value>.toString();
- value -> Number :: +\<value>
- value -> Boolean :: !!\<value>
- float -> integer :: Math.floor(\<float>)

Also consider native constructors and methods:

- Number("3") // 3
- String(3) // "3"
- Boolean(0) // false
- parseInt("3.333aaaaaa") // 3
- parseFloat("3.333aaaaaa") // 3.333

To repeat: once you've settled on a style, stick with it religiously.

#### Modifying Object prototypes

This should not be done except in *extremely rare* circumstances. JavaScript natives like `Object` or `Function` are of course used by many developers, and there is a strict expectation of how those objects behave. You don't want to surprise the other people using your code. If you have come to the conclusion that your code can only be implemented by modifying native objects it is likely you haven't thought hard enough about the problem you're trying to solve.

[Juriy Zaytsev does due diligence](http://perfectionkills.com/whats-wrong-with-extending-the-dom/) and lays out the issues you should be considering.

#### Conditional IE comments

Try to avoid using code like this:

	var f = function () {
	    /*@cc_on if (@_jscript) { return 2* @*/  3; /*@ } @*/
	};

	<!--[if IE 6]>
	Special instructions for IE 6 here
	<![endif]-->

These statements alter the JavaScript syntax tree at runtime, which can cause hard-to-find bugs. More importantly, Internet Explorer 10 [no longer supports them](http://blogs.msdn.com/b/ie/archive/2011/07/06/html5-parsing-in-ie10.aspx). As mentioned in the linked article, ***"The Web is better when developers can use the same markup and same code across different browsers with the same results."***

## Generalities
#### Blank lines

Try to group logically related groups of code, and place lines between these groups. There is nothing wrong with using whitespace, especially if it helps others understand groupings. As well, "mashed" code can become difficult to read, especially if the chunk is large.

#### Eval and With

The ability of JavaScript to generate executable code is one of its most powerful features. But there are dangers in this. As mentioned elsewhere, `with` is not allowed in `strict` mode, and in that mode variables declared within an `eval` block cannot bleed out into the containing scope.

Avoid these constructs. There is likely a better way that you're not seeing.

#### Parentheses

While you may see such constructs in old code, parentheses are *not* required (ie. *do not use them*) in the following cases: `delete`, `typeof`, `instanceof`, `void`,`return`, `throw`, `case`, `in`, `new`

#### Quotes

You are encouraged to get in the habit of using single-quotes (`'`) rather than double-quotes(`"`). If you choose the opposite pattern, be consistent.

#### Editors

Developers should not be forced to use a single editor or IDE. Most developers will end up using several editors -- it is typical to have some sort of GUI-based editor, and to dip into VIM (or similar) when necessary.

Where possible, encourage all developers on a team to do the work of ensuring identical character sets, tab/spacing, and so on, across all of their tools.

#### Cleverness

You are a smart person. Everybody knows that already. Write clear, precise code and commentary that is easy to understand. You help your reputation little by interleaving complex logical threads within a single line of code, or by using tricky and obscure language novelties instead of well-known patterns. You help your reputation greatly by writing code that is so useful, and so easy to read and understand, that a lot of other people use it -- and enjoy using it.





