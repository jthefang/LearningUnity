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
