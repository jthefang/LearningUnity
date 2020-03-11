# Chapter 2: Meaningful names
- The name of a variable, function, or class, should answer all the big questions. It
should tell you why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent. 
- Avoid disinformation
    - Avoid words whose entrenched meanings vary from our intended meaning. e.g. `hp, aix`, and `sco` would be poor variable names because they are the names of Unix platforms or variants. Even if you are coding a hypotenuse and `hp` looks like a good abbreviation, it could be disinformative.
    - Do not refer to a grouping of accounts as an `accountList` unless it’s actually a `List`.
- Make meaninful distinctions 
    - Don't use noise words
    - `NameString` is worse than just `Name`
    - Can't distinguish between `getActiveAccount(), getActiveAccounts(), getActiveAccountInfo()`
    - or between `Customer` and `CustomerObject`
    - Distinguish names in such a way that the reader knows what the differences offer.
- Use pronounceable names => people can talk about the code
- Use searchable names (not single letter names or numeric constants)
    - One might easily grep for `MAX_CLASSES_PER_STUDENT`, but the number 7 could be more troublesome
    - In this regard, longer names trump shorter names, and any searchable name trumps a constant in code
    - Single-letter names can ONLY be used as local variables inside short methods
    - **The length of a name should correspond to the size of its scope** (if a variable or constant might be seen or used in multiple places in a body of code, it is imperative to give it a search-friendly name)
- Avoid encodings: SWEs have enough of these to learn
- Clarity is king. Assume readers of your code are average intelligence
- **Class and object names** should have noun or noun phrase names like `Customer`, `WikiPage`,
`Account`, and `AddressParser`. Avoid words like `Manager`, `Processor`, `Data`, or `Info` in the name
of a class. A class name should not be a verb.
- **Method names** should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`.
Accessors, mutators, and predicates should be named for their value and prefixed with `get`,
`set`, and `is` according to the javabean standard.
- When constructors are overloaded, use static factory methods with names that
describe the arguments. For example, 
```java
Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```
is generally better than
```java
Complex fulcrumPoint = new Complex(23.0);
```
- **Be consistent with your lexicon**. Pick one word per concept. e.g. use one of `fetch`, `receive`, `get` across your codebase. Also `controller`, `manager`, `driver` are similar words => pick one. 
    - use `insert` or `append` instead of `add` which has the double meaning of concatenation/math-adding two values.
- **Use solution/problem domain names**. Programmers are familiar with common programming terms. Those are ok (e.g. `JobQueue`). As are terms specific to the problem/application you're coding for.
- Add meaningful context. Most names are vague without the context to which they belong. e.g. `name`, `street`, `city`, `state`, `zipcode` obviously form an address together, but alone it's uncertain their use => wrap them in an `Address` object class.
- Add just enough context as is necessary. **Shorter names are generally better than longer ones**, so long as they are clear. Just be precise!
- Code should read well (like paragraphs or sentences, or at least tables and data structures).

# Chapter 3: Functions
- 