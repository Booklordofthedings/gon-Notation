# The Generic object notation Version 1.0
1. Gon stand for generic object notation which is what this notation is supposed to be
2. A parser has to implement all required features of a given gon version to be considered compliant with the specification
3. A parser may implement additonal features while still being compliant, these features could be features described in this specifications as optional
4. Features that are required will be described as "should" be implemented
5. Features that are optional will use "can" be implemented
6. A gon parser should support unicode character inputs
7. The characters used in a gon file should always stay as simple as needed, weird control characters could cause issues
8. Raw gon files should use the .gon file extension unless it specifically makes sense to use another one
9. A gon file is the not parsed version of the data, the parsed version will be refered to as A gon Object
10. A gon files consists of several gon entries which are divided by a linefeed character (U+000A)
11. Not every entry has to be valid in order for a gon file to be valid
12. Invalid gon entries should just be ignored while valid ones are to be parsed as normal
13. Parsers can keep track of invalid lines and allows users to work of of these, but they should not stop parsing unless its absolutely required (not enough memory available)
14. An empty line should be ignored by the parser without throwing an error
15. Gon is case sensitive so wether a character is upper or lowercase matters and something needs to be a 100% match to actually match
16. A gon entry can further be divided into tokens the first token of a gon entry starts at the first character in a line that isnt a space or a tab (U+0020 || U+0009)
17. The first token ends at the next space (U+0020)
18. The first token is called the handling token and describes how the following tokens in the line should be handled and where the data they contain should be written to
19. The first token may be any lenght and capitalization
20. The token values that have a meaning to the parser should start uppercase
21. Tokens from the official standart should also be 1 character
22. Tokens added as an optional feature should not be 1 character long as to not conflict with eventual future tokens
23. The simplest token is the "V" this indicates a value token, and indicates that the following tokens should be parsed and stored normally
24. Another token type is "M", this stands for meta and indicates that the following tokens contain meta information about the file or the information contained in the file
25. Metadata should be kept as short as possible
26. Metadata should be stored at the top of the file
27. One possible use for a metdata entry could be the gon version the file was created on
28. Metadata does not support the full range of tokens available in normal Value entries
29. If the entry token is not a valid known token it will be considered a value token instead and the entry token will be considered the second token instead
30. This allows normal data to just remove the "V" while still working fine to reduce complexity while writing
31. "-" is a token indicating that the following entry is a member of an object
32. The object that is targeted by "-" is the last declared object in the file
33. "-" can be repeated with a space (U+0020) inbetween to indicate on which layer they are on
34. The targetting just works recursivly by selecting the last declared object and repeating that for as many "-" there are
35. The Parser should atleast support 500 layers of depth, and can support support more
36. As a result of the previous rule, gon objects should try not to have more than 500 layers in order to avoid incompatibilities
37. "#" is another entry token and denotes a comment, which will simply cause the rest of the line to not be evaluated
38. The second token in a line is the type token and indicates the type of entry that the line is, this was done to allow parsers to not have to figure out types from other indicators
39. Gon type tokens should start with a lowercase letter in order to stop potential overlaps with Entry tokens from happening
40. All 2 character type tokens are reserved for the official types and should not be used in custom type tokens
41. There are several different type tokens available:
42. "n" indicates a 32 bit floating point number (IEEE-754)
43. "t" indicates a String of text (preferably utf8)
44. "b" indicates a boolean with 2 values (true, false)
45. "i" indicates a signed 32 bit integer
46. "bi" indicates a signed "big" integer with 64 bits
47. "bn" indicates a "big" floating point number with 64 bits (IEEE-754)
48. "d" indicates raw data, parsers can turn this into a type as they see fit for raw data
49. This could for example just be a string or a void* or a uint8 arrray, whatever the language works best with
50. "c" indicates a custom type which will be further specified by the file afterwards
51. "o" indicates an object that can contain further entries
52. The next token is usually the name of the entry as a utf8 string
53.  If the type was specified to be "c" the next token is considered the typename in utf8 instead
54.  For custom type entries the next token will be considered the name instead
55.  If the type was an object any further tokens in the line will not be evaluated as only the name of the object matters
56.  If the type of the object is "n, i, b, bn, or bi" the next token will be the value of the entry and further values wont be evaluated
57.  If the type is "t, d or c" All of the other tokens in the line including the seperator character (space, U+0020) will be considered the value
58.  When a line does not have enough tokens to parse correctly it will simply be ignored and the parser can record an error
59.  The parsed object should allow several ways of acessing entries
60.  Acess via an index should be possible where the index corresponds to the n-th object that was parsed in the current layer starting at the 0th
61.  Acess should also be available via a name string
62.  Due to this names on a layer also have to be unique and cant repeat
63.  A gon entry after being parsed should contain several values
64.  The name of the entry
65.  Its type
66.  Its type as a string, in the case of a custom type entry this should contain the typename token
67.  The parsed value itself
68.  As a result of this standart the base notation does not support objects in the meta file, however due to the way the notation is designed a parser may implement members of meta object in the following way
69.  Use "-" as normal do indicate depth and then use a "M" at the end to indicate that is supposed to go into the meta file
70.  This way a normal parse will simply just ignore the object as "M" is not a valid type indicator
71.  This is also ambigous since it could aswell mean to target a normal object and add the entry as a meta to that object
72.  This and it being harder to parser are the reason why it is not available in the standard
73.  Some people may miss arrays, which dont fully work, as every gon entry has to have a name
74.  Simply using its index as a name can work here, and the person reading the parsed object can just loop through it like they would with an array
75.  A similar way could work with multiline string which could just be objects that have each line individually saved
