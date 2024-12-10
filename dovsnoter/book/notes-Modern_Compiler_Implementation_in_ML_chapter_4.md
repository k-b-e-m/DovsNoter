
The parser and lexer can together recognize sentences within the language. The *semantic actions* is what does something useful with these sentences. 

In these cases one associates all terminals, with a specific type. For example associating numbers with type int. A sentence might then consist of a non-terminal, however the type could be derived from the terminals within that sentence, with some semantic operations.

# Semantic Actions

## Recursive Descent

A recurive-Descent parser are a parser where the semantic actions are the values returned by parsing functions.
For example in Dolphin the "Integer_Literal" must contain a number of type int. And a "String" gets parsed as a string, and is already giving it a type.

## ML-Yacc-GENERATED Parsers
These are parsers like in dolphin. And example can be seen below:
![[Program 4.2. Page 89.png]]

## An Imperative Interpreter in Semantic Actions
This part is very badly written. Below is an example of an imperative interpreter.
![[program 4.5.png]]
# Abstract Parse Trees
It is possible to create a compiler using ML-Yacc parser, however it is difficult to maintain. Therefore often one separates the issues of syntax, which is what the parser finds, with the issue of semantics. One way to do this is to have the parser create a Parse tree (AST). 

*Note
Older compilers did not use AST because they did not have memory for such a data-structure* 

## Positions
This just explains that to report reasonable approximations of the positions where the error occur, the AST must store the position of where it was created in the program. this is the "loc" in the dolphin AST.: