Comprised of two operations: function **definition** and **application**. Has three syntactic forms:
- Variable: `x`
- Abstraction: `\x -> E`
- Application: `E1` (function), `E2` (argument)
# Theory
### $\beta$-Reduction
Function calls
- Reducible Expression (redex): `(\x -> E1) E2`
- Reduction Rule: `(\x -> E1) E2 -> E1[x := E2]`
	- `E1[x := E2]` means `E1` with all free occurrences of `x` replaced with `E2`, as long as no free variables of `E2` get captured (undefined otherwise)
- Repeatedly apply $\beta$-reduction until normal form with no further redexes (or loop forever)
### $\alpha$-Conversion
Renaming formals
- `\x -> E =a> \y -> E[x := y]`
- Ensures no accidental capture of free variables
### Capture-Avoiding Substitution
- When doing `E[x := E2]`, we ensure no free variable in `E2` gets bound accidentally in `E`
- If conflict arises, $\alpha$-convert the abstraction's bound variable first
### `let` Definitions
- `let NAME = E` is just substituting `E` wherever `NAME` appears

Example:
```haskell
let ID = \x -> x
ID apple
	=d> (\x -> x) apple
	=b> apple
```
### Church Numerals
- Encoding natural numbers:
	- `ZERO = \f x -> x`
	- `ONE = \f x -> f x`
	- `TWO = \f x -> f (f x)`
- General form: integer `n` represented as `\f x -> f^n(x)`
- Arithmetic
	- Increment: `INC = \n f x -> f (n f x)`
	- Addition: `ADD = \n m -> n INC m`
	- Multiplication: `MULT = \n m -> n (ADD m) ZERO`
- 