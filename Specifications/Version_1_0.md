# The Generic object notation Version 1.0


## General rules
- Gon stand for **generic object notation**.
- Raw gon files should use the **.gon file extension** unless it conceptually makes sense to use another one.
- Gon files should **support unicode (utf8)** character inputs.
- The weirder control characters should be avoided if possible.
- The **raw file data** is refered to as a **Gon File**, while the **parsed data** will be refered to as a **Gon Object**.
- A **gon files** consists of several **gon entries** which are divided by a **linefeed character (U+000A)**.
- Features that are required will be described as **"should" be implemented**.
- Features that are optional will use **"can" be implemented**.
- A parser **has to implement all required features** of a given gon version to be considered **compliant** with the specification.
- A parser **may implement additonal features while still being compliant**, these features could be features described in this specifications as optional.

## Error Handling
- **Not every entry has to be valid** in order for a gon file to be valid.
- **Gon ignores invalid entries and data** while parsing valid entries as normal.
- **Parsers can keep track of invalid lines** but wether the error actually matters has to be decided by the user.
- **An empty line** is technically invalid but **should not be counted**.
- **Gon is case sensitive** so tokens need to match in capitalization.

## Tokens
- A gon entry can further be **divided into tokens**.
- Tokens may have **any lenght or capitalization**.
- **Meaningful tokens are usually limited** by other factors.
- The first token of a Gon Entry starts at the **first character in a line that isnt a space or a tab (U+0020 || U+0009)**.
- The first token **ends at the next space (U+0020)**.
- All of the further tokens **only support a space (U+0020) as their divider**.

## Entry Token
- The first token is called an **Entry Token**.
- This token is used to indicate **the target** of the following token entry.
- When starting with a letter, **Entry tokens should always start with an uppercase letter** in order to avoid overlap with other token types.
- **All 1 lenght tokens are reserved** and should not be used for optional features.
- **'V' (meaning value) is the default token** and is used to indicate a normal entry stored at the top level of the parsed object
- **'M' is used for metadata, which is stored in a different location** and contains meta information about the data being parsed.
- A parser may evaluate meta entries and **use the results to log an error or to warn the user**.
- This may be usefull for something like the **notation version, where an old parser reading a newer notation could alert the user**.
- All metadata tokens should be **written at the top of the file**
- Metadata token sections should be **kept as short as possible**.
- If the Entry Token is not any valid token it is assumed that the Entry Token is a 'V' and the Entry token will instead be parsed as the next token.
- This allows the author of the file to **ommit the 'V' and it will still be parsed** as if it existed.
- **'-' indicates membership of an object**.
- The entry parsed after **this token is a member of the last declared object entry**.
- **This token may be repeated (with a space inbetween) to go down further layers**, always targetting the last declared object at that specific layer.
- The **minimum depth a Parser should support is 1000**.
- Having more layers should be avoided
- If the **depth is invalid** or a the **layer does not actually have an object the entire line will be considered invalid** and will be ignored (Parsers may still log an error though).
- '#' is an entry token to denote a comment and will simply just cause the parser to ignore the rest of the line.

## Type Token
- The next token is the **Type Token** unless the line is a comment.
- The token, as you would expect denotes the **type of the actual entry**.
- These **start with a lowercase letter** to avoid overlap.
- Optional Type tokens should **use atleast 3 characters to avoid overlap**.
- **'n'** indicates a **32 bit floating point number** (IEEE-754).
- **'t'** indicates a **String of text** (utf8).
- **'b'** indicates a **boolean with 2 values** (true, false).
- **'i'** indicates a **signed 32 bit integer**.
49. "bi" indicates a signed "big" integer with 64 bits
50. "bn" indicates a "big" floating point number with 64 bits (IEEE-754)
51. "d" indicates raw data, parsers can turn this into a type as they see fit for raw data
52. This could for example just be a string or a void* or a uint8 arrray, whatever the language works best with
53. "c" indicates a custom type which will be further specified by the file afterwards
54. "o" indicates an object that can contain further entries
55. The next token is usually the name of the entry as a utf8 string
56.  If the type was specified to be "c" the next token is considered the typename in utf8 instead
57.  For custom type entries the next token will be considered the name instead
58.  If the type was an object any further tokens in the line will not be evaluated as only the name of the object matters
59.  If the type of the object is "n, i, b, bn, or bi" the next token will be the value of the entry and further values wont be evaluated
60.  If the type is "t, d or c" All of the other tokens in the line including the seperator character (space, U+0020) will be considered the value
61.  When a line does not have enough tokens to parse correctly it will simply be ignored and the parser can record an error
62.  The parsed object should allow several ways of acessing entries
63.  Acess via an index should be possible where the index corresponds to the n-th object that was parsed in the current layer starting at the 0th
64.  Acess should also be available via a name string
65.  Due to this names on a layer also have to be unique and cant repeat
66.  A gon entry after being parsed should contain several values
67.  The name of the entry
68.  Its type
69.  Its type as a string, in the case of a custom type entry this should contain the typename token
70.  The parsed value itself
71.  As a result of this standart the base notation does not support objects in the meta file, however due to the way the notation is designed a parser may implement members of meta object in the following way
72.  Use "-" as normal do indicate depth and then use a "M" at the end to indicate that is supposed to go into the meta file
73.  This way a normal parse will simply just ignore the object as "M" is not a valid type indicator
74.  This is also ambigous since it could aswell mean to target a normal object and add the entry as a meta to that object
75.  This and it being harder to parser are the reason why it is not available in the standard
76.  Some people may miss arrays, which dont fully work, as every gon entry has to have a name
77.  Simply using its index as a name can work here, and the person reading the parsed object can just loop through it like they would with an array
78.  A similar way could work with multiline string which could just be objects that have each line individually saved
