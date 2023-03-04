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
23. The simplest token is the "V" this indicates a value
