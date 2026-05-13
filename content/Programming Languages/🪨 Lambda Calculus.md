comprised of two operations: function **definition** and **application**. has three syntactic forms:
- variable: `x`
- abstraction: `\x -> E`
- application: `E1` (function), `E2` (argument)
# theory
### $\beta$-reduction
function calls
- reducible expression (redex): `(\x -> E1) E2`
- reduction rule: `(\x -> E1) E2 -> E1[x := E2]`
	- `E1[x := E2]` means `E1` with all free occurrences of `x` replaced with `E2`, as long as no free variables of `E2` get captured (undefined otherwise)
- repeatedly apply $\beta$-reduction until normal form with no further redexes (or loop forever)
### $\alpha$-conversion
renaming formals
- `\x -> E =a> \y -> E[x := y]`
- ensures no accidental capture of free variables
### capture-avoiding substitution
- when doing `E[x := E2]`, we ensure no free variable in `E2` gets bound accidentally in `E`
- if conflict arises, $\alpha$-convert the abstraction's bound variable first
### `let` definitions
- `let NAME = E` is just substituting `E` wherever `NAME` appears

example:
```haskell
let ID = \x -> x
ID apple
	=d> (\x -> x) apple
	=b> apple
```
### church numerals
- encoding natural numbers:
	- `ZERO = \f x -> x`
	- `ONE = \f x -> f x`
	- `TWO = \f x -> f (f x)`
- general form: integer `n` represented as `\f x -> f^n(x)`
- arithmetic
	- increment: `INC = \n f x -> f (n f x)`
	- addition: `ADD = \n m -> n INC m`
	- multiplication: `MULT = \n m -> n (ADD m) ZERO`
# examples
booleans:
- `TRUE = \x y -> x`
- `FALSE = \x y -> y`
- `ITE = \b x y -> b x y` (if then else)
pairs:
- `PAIR = \x y -> \b -> ITE b x y`
- `FST p = p TRUE`, `SND p = p FALSE`
recursion (fix-point combinator):
- `FIX = \stp -> (\x -> stp (x x)) (\x -> stp (x x))`
- allows for self-application that emulates recursion

example:
```haskell
let STEP = \rec -> \n -> ITE (ISZ n) ZERO (ADD n (rec (DEC n)))
let SUM = FIX STEP

-- SUM 3 -> 0 + 1 + 2 + 3
```