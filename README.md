# gon
Is a notation influenced partially by notations like json and toml,
but with some changes aimed to make it easier to write a parser for.  
This was originally designed as an exercise for myself to design a notation  
and to write a parser for it, but it should also be useable as a general purpose notation since, atleast the beef parser is quite optimized by now.

## Goals of the language
- Easy to write a parser for
- Able to express most simple objects that a programming language may create
- Human write- and readable

## Specifications


## Example
```
t name Player
o position
 - n x 17.5
 - n y 10
 - n z 90
```
 ### More example files can be found in the example folder
 
 
 ## Parsers
 ### Beeflang
 [Gon-Beef](https://github.com/Booklordofthedings/gon-beef)
 
 ## Contributing
There are about 3 ways you can contribute to gon.
1. Make projects that uses gon
2. Write your own parser and add it to the readme via a pull request
3. Suggestions for additons to the notation can be made via the issue tab
