- You're responsible for code quality (not your client, not your manager)
- Write code that expresses intent => code should speak for itelf (less comments = less to maintain)
- Leave the code better than you found it
- Single-responsibility code (function does 1 thing well, less args = better function)
- Tests
- Work on big picture skeleton => fill in details later
    - interface first, implementation later
- Make independent components that can be used in different places
- Practice to master your craft

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
- Functions should be small (4-20 lines), less than screen full at most.
    - Indent level should not be larger than 1 or 2 (e.g. 2 nested if statements)
- This keeps the code self-documenting and readable
    - "You know you are working on clean code when each routine turns out to be pretty much what you expected"
- Functions should only do 1 thing; it should do it well and do it only
    - should do *one conceptual thing* at *one level of abstraction* 
        - break that down into substeps that could involve other function/recursive calls 
        - but each call/step within this function should be at the same level of abstraction
        - don't mix details (e.g. `.append(\n)`) with concepts (e.g. `getHtml()`) in the same function; those are at different levels of abstraction
    - if you could split the function further into more conceptual functions => you should
    - functions that do one thing cannot be reasonably divided into sections!
- You should be able to read code from top to bottom. If each function stays at one level of abstraction, then each function reads as one level of abstraction, i.e. it calls functions at a level of abstraction below it and is called by functions one level of abstraction above it
    - e.g. Function 1.1 (at level 1): To include the setups and teardowns, we include setups (function 2.1, at level 2), then we include the test page content (function 2.2), and then we include the teardowns (function 2.3).
    - Function 2.1: To include the setups, we include the suite setup (function 3.1, at level 3) if this is a suite, then we include the regular setup.
    - Function 3.1: To include the suite setup, we search the parent hierarchy (function 4.1, at level 4) for the “SuiteSetUp” page and add an include statement with the path of that page.
    - Function 4.1: To search the parent...
- Ideal number of arguments to a function is ZERO! Followed by 1, then 2, and more than 3 requires really good justification and still shouldn't be used.
    - requires testing every possible argument combination (=> increasing arguments increases test cases exponentially)
    - also requires you to remember or look up the ordering and meaning each time you call the function (good function naming can mitigate problem of looking up ordering, e.g. `assertExpectedEqualsActual(expected, actual)` vs `assertEquals(expected, actual)`)
    - consider wrapping multiple arguments in a single object
    - or making some arguments instance variables that don't need to be passed in
    - or making the argument a class for which the function is called on (e.g. instead of `printRadius(circle)` do `circle.printRadius()`)
    - for functions taking variable number of arguments (like `printf`), the variable arguments are essentially one list of arguments that will be treated the same (e.g. converted to strings and inserted into the format string) => actually 1 arg (or 2 as in the case of `printf(fmt, args)`)
- Have no side effects (don't have function unexpectedly mutate internal state)
    - this is tightly related to having the function do one thing
- Don't have `output arguments`
    - don't have the output of the function be an argument you have to pass in
    - have the function return the output, or restructure the code so that the function changes the state of the object it's a method of
- Throw exceptions in functions instead of returning error codes
- Extract try/catch blocks into their own functions
```java
public void delete(Page page) {
    try {
        deletePageAndAllReferences(page);
    }
    catch (Exception e) {
        logError(e);
    }
 }
 private void deletePageAndAllReferences(Page page) throws Exception {
    deletePage(page);
    registry.deleteReference(page.name);
    configKeys.deleteKey(page.name.makeKey());
 }
 private void logError(Exception e) {
    logger.log(e.getMessage());
 }
```
- Duplication of code is the ultimate maintenance evil: it bloats the code and you have to update all duplicates if you want to change the algorithm
- The code you write is a first draft. Make sure they pass the tests. Then rewrite the code to make it clean and make sure they still pass the tests!
