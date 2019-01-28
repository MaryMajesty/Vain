# Vain, the best programming language that has yet to be

## Goals:

Vain is a programmer-first language. It is designed to be as usable, user friendly, and pleasant to program in as possible. Its focus includes good debuggability and as much compile-time error checking as possible.

## Features:

<details><summary>Typing</summary><p>

<details><summary>Sub-Type Namespaces</summary><p>

Have you ever wanted to organize a type's members into groups, just like namespaces? With Vain, you can do just that! (Because honestly, there's really no reason why you shouldn't be able to do that)

```
## Note: Type and namespace syntax are probably gonna look different in the final release
type Person
{
	Age int
	
	namespace Names
	{
		FirstName string
		MiddleName string
		LastName string
	}
}

## Pretend the following code is inside a function
p = Person()
p.Names.FirstName = "Jeff"
```

Like regular namespaces, sub-type namespaces are purely cosmetic and don't change any of your code's behavior. They just make your library more organized and easier to use.

</p></details>

<details><summary>Implicit Typing / Function Types / Name-First Type-Second Variable Syntax</summary><p>

Vain has a static type system that allows you to explicitly declare a variable's type, but because Vain is supposed to be usable everywhere (including as a scripting language), it also allows implicit typing.

```
value int = 5
## Is the same as:
value = 5
```

You may have noticed that in the above declaration, the name came first and the type second. While this may seem peculiar and counter-intuitive, there is a good reason for it. To explain this, we first need to talk about function types.

`concatenate (string; string, string)`

The function type above is denoted by `(string; string, string)`. The first `string` is an output parameter. In Vain, functions can have multiple output parameters, so the output and input parameters are separated by a `;`. The last two strings are the input parameters. This in and of itself looks fine, but it starts looking worse if you also want to assign an explicitly typed function value to the variable.

```
concatenate (string; string, string) = (result string; first string, second string)
{
	result = first + second
}
```

I hope you can see the problem now: The function's parameter types are declared twice. This is less than ideal, especially for functions with lots of parameters. While you could implicitly type the function value, this would result in the types and names being at opposite ends of the declaration.

`concatenate (string; string, string) = (result; first, second) { ## Pretend there's more code here.`

This isn't exactly user friendly. It makes much more sense to implicitly type the variable and have all the explicit typing in the function value.

`concatenate = (result string; first string, second string) { ## Pretend there's more code here.`

This inherently leads to the name coming first, and the type second. If we wrote all the declarations with type-first name-second syntax, the result would look pretty messy.

```
int value = 5
valuefunc = (int) { ## Pretend there's more code here.
```

So, the only resulting option to keep everything coherent is to always write the name first and the type second. It has other benefits too: When you look at a type's members, you see all the names on the left side, neatly aligned. And seeing all the names easily like that gives you more information about a type's purpose than only seeing its member types.

</p></details>

<details><summary>Arrays</summary><p>

You know how low level languages require arrays to be of a fixed size? And how high level languages don't allow any fixed size arrays? Why can't we just have both?

```
fixedsizearray int[4] = int[4]()
dynamicsizearray int[] = int[](4)
```

`int[4]` and `int[]` are different types, just like `int[4]` and `int[3]` are different types. Fixed size arrays can be implicitly converted into dynamic size arrays, and dynamic size arrays can be explicitly converted into fixed size arrays.

</p></details>

<details><summary>Sealed By Default</summary><p>

The vast majority of types aren't supposed to be inheritable, but nobody ever bothers to actually declare them as sealed. The solution is simply having all types be sealed by default.

</p></details>

<details><summary>Non-Nullable By Default</summary><p>

Why have null reference exceptions, when you can also just... Not have them?

</p></details>

---

</p></details>



<details><summary>Variables</summary><p>

<details><summary>Variable Expression</summary><p>

</p></details>

<details><summary>Better Access Modifiers</summary><p>

Look at the following C# code:

`public int Value { get; private set; }`

This property has a very simple purpose: Store an integer whose value can only be changed from inside the declaring type. So why does it need to be a property? It doesn't have any special get- or set-methods, they only have different access modifiers. And the access modifiers are declared weirdly too, the entire property is public, its set-method is private, and its get-method isn't anything? And to top it all off, this property just wraps a hidden field that stores its value. You know what would make much more sense? Just letting the programmer declare separate get and set access modifiers for a field. Oh, and let's get rid of the ugly `public` and `private` too, you have to write them so often that you get sick of it.

`+- Value int`

There. `+` and `-`. Public and private. Public get and private set. And it has the same syntax as any other field too, because that's what it is: Just a field. There's no need for it to be a property if all it does is store a value.

Other examples:

```
+ Value int ## A regular, public field.
+! Value int = 5 ## A get-only field, AKA read-only. ! means there is no set access.
```

By reducing the `public` and `private` keywords to `+` and `-`, all of a type's members are much more aligned than if you had to write access modifiers of completely different lengths for different variables, improving readability.

</p></details>

<details><summary>Variable Signatures</summary><p>

</p></details>

---

</p></details>



<details><summary>Improving Syntax</summary><p>

Some Vain features aren't exactly new. But they are improved.

<details><summary>Short Lambdas</summary><p>

Consider the following C# code:

`vals = list.Select(item => item.Value);`

Doesn't look too bad at first glance. But writing lambdas like this multiple times shows the problem: You have to write `item => item` several times, over and over. In addition, the `item =>` part isn't even necessarily needed. Thus, I propose what I am currently calling "short lambdas":

`vals = list.Select($.Value)`

These short lambdas take one parameter, `$`, which does not need to be declared, saving space and time. By removing the unneeded `item => item` part, the code looks a lot cleaner too, improving readability.

Short lambdas are not usable everywhere, as they only take one parameter and their range has to be determined by the compiler, but in the cases where they can't be used, regular lambdas make far more sense than opting for this kind of syntactic sugar.

</p></details>

<details><summary>Consistent Cast Syntax</summary><p>

Look at this C# code:

`((double)(5 + 4)).ToString()`

You're converting an integer to a double to a string. So why on Earth is the code written in the order "double, int, string"? That just doesn't make any sense. And all the parentheses, ugh.

Here's how Vain does it:

`(5 + 4){float}{string}`

It's simple. It's short. It's readable. And, most importantly, it's in the right order. Integer, float, string. It could have been so simple, C#...

</p></details>

<details><summary>Making Code Blocks Make More Sense</summary><p>

Various control-flow statements have a condition, and a code block that gets executed when the condition is met. Consider the following C# code:

```
if (int.TryParse("5", out int n))
	;
n = 5;
```

Why does this work? This shouldn't work. `n` was defined inside an if-statement, so it should only exist inside the if-statement. Existing outside of the if-statement doesn't make any sense, especially considering that `n` might not even have any value assigned to it.

```
for (int i = 0; i < 10; i++)
	;
int i = 5;
```

This piece of code gives a compiler error due to the same reason the code above doesn't. `i` is defined within the for-statement, but somehow exists outside of it, even though it doesn't make any sense. Even worse, it doesn't even *really* exist outside, because you can't actually use it either. So the variable `i` just sort of exists in limbo, where it doesn't exist but it also doesn't *not* exist. This is just way too confusing and illogical, so let's fix it.

```
if (_, n = int.TryParse("b"))
{
	## Here be code
}

n int = 5
```

`n` is defined inside the if-statement, so it only exists inside its code block. Outside of it, it doesn't exist, so a new `n` can be declared. A simple fix, and all variables can live happily ever after and don't have to fear existential crises anymore.

</p></details>

---

</p></details>

<details><summary>Preprocessor Directives</summary><p>

<details><summary>Nested Comments</summary><p>

`##` is the operator for marking the rest of the line as a comment.

`#(` and `#)` are the operators for marking the start and end of comments. They are nestable.

`#( #( This is a comment. #) This is also a comment. #) This is not a comment anymore.`

</p></details>

<details><summary>Consistent Syntax #if</summary><p>

It makes much more sense for `#if` to behave exactly like a regular `if`, but as a preprocessor directive.

```
#if (compile)
{
	code()
}
#else
{
	othercode()
}
```

</p></details>

---

</p></details>

<details><summary>Possible Planned Features</summary><p>

These are features that could make sense in Vain, but it is unclear whether they're a good fit for it or achievable.

<details><summary>Code Contracts</summary><p>

`Hour int [$ >= 0 & $ < 24]` (See "Reducing Code Redundancy -> Short Lambdas")

Equivalent to the following C# code:

```
private int _Hour;
public int Hour
{
	get => this._Hour;
	
	set
	{
		if (value >= 0 && value < 24)
			this._Hour = value;
		else
			throw new Exception();
	}
}
```

</p></details>

---

</p></details>
