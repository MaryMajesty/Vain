# Vain, the best programming language that has yet to be

## Features:

<details><summary>Typing</summary><p>

<details><summary>Non-nullable By Default</summary><p>

Why have null reference exceptions, when you can also just... Not have them?

</p></details>

<details><summary>Sealed By Default</summary><p>

The vast majority of types aren't supposed to be inheritable, but nobody ever bothers to actually declare them as sealed. The solution is simply having all types be sealed by default.

</p></details>

<details><summary>Arrays</summary><p>

You know how low level languages require arrays to be of a fixed size? And how high level languages don't allow any fixed size arrays? Why can't we just have both?

```
fixedsizearray int[4] = new int[4]()
dynamicsizearray int[] = new int[](4)
```

`int[4]` and `int[]` are different types, just like `int[4]` and `int[3]` are different types. Fixed size arrays can be implicitly converted into dynamic size arrays, and dynamic size arrays can be explicitly converted into fixed size arrays.

</p></details>

---

</p></details>



<details><summary>Reducing Code Redundancy</summary><p>

<details><summary>Short Lambdas</summary><p>

Consider the following C# code:

`vals = list.Select(item => item.Value);`

Doesn't look too bad at first glance. But writing lambdas like this multiple times shows the problem: You have to write `item => item` several times, over and over. In addition, the `item =>` part isn't even necessarily needed. Thus, I propose what I am currently calling "short lambdas":

`vals = list.Select($.Value)`

These short lambdas take one parameter, `$`, which does not need to be declared, saving space and time. By removing the unneeded `item => item` part, the code looks a lot cleaner too, improving readability.

Short lambdas are not usable everywhere, as they only take one parameter and their range has to be determined by the compiler, but in the cases where they can't be used, regular lambdas make far more sense than opting for this kind of syntactic sugar.

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
      throw new Exception();
    else
      this._Hour = value;
  }
}
```

</p></details>

---

</p></details>
